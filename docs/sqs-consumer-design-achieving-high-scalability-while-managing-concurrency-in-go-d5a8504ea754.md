# SQS 消费者设计:在 Go 中管理并发性的同时实现高可伸缩性

> 原文：<https://towardsdatascience.com/sqs-consumer-design-achieving-high-scalability-while-managing-concurrency-in-go-d5a8504ea754?source=collection_archive---------3----------------------->

![](img/9f4c9525f2de558a24c5e1ef042db35b.png)

Photo by [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/this-is-engineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

最近我不得不重新设计一个异步排队系统。在研究了几个选项之后，我选择了 AWS *简单队列服务(SQS)* 和*简单通知服务(SNS)* 。通过这两个服务的组合，我能够复制和改进以前的(失败的)RabbitMQ 设置。AWS 产品的伟大之处在于它们是为微服务而从头开始构建的。它们抽象了复杂的异步消息传递系统的大部分复杂性，从通过消息等待期处理水平扩展到死信队列和许多其他特性，开箱即用。

根据 [AWS 关于伸缩](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-throughput-horizontal-scaling-and-batching.html) **、**的建议，他们建议通过利用多线程在多个服务之间以及单个服务内进行水平伸缩。在这篇博文中，我们将重点讨论在一个服务中利用多线程使用 Golang 检索和处理来自 SQS 的消息的最佳方式。

在整个示例中，我们将处理一个包含 20，000 条消息的 AWS 队列。我们将测量 Golang 中管理并发工作者的三种不同技术，以及一个同步控制测试。我们将测量 3 分钟内消耗的项目总数，并取平均每分钟消耗的邮件数(mpm)。

所有示例都将使用 go aws-sdk 来长轮询 sqs 服务器。消息的成功接收将由一个基本处理程序处理，该处理程序在返回和完成请求之前向另一个微服务发出 http 请求。完成后，我们从队列中删除消息，这是 SQS 表达消息已被消费的方式。

# 控制

在这个例子中，我们将有一个控件。同步消费者

```
func (c *consumer) Consume() {
 for {
  output, err := receiveSQSMessages(c.QueueURL) 
  if err != nil {
   //log error
   continue
  } for _, m := range output.Messages {
    c.run(newMessage(m)) if err := h(m); err != nil {
      //log error
      continue
    }

    c.delete(m) //MESSAGE CONSUMED
  }
 }
}
```

对于同步消费者，我们得到的平均处理时间为 **160mpm**

# **基本并发**

让我们添加一些 goroutines。虽然 goroutines 很便宜，但我们还是想在一定程度上控制它们。随着时间的推移，允许一百万个 goroutines 产生可能会导致我们的程序耗尽内存，运行速度比我们控制的要慢。在本例中，我们将使用一个简单的原子计数器来跟踪和添加允许的工作人员总数的限制。我们使用原子包，因为并发使用它是安全的。虽然简单，[但不建议在基本应用中使用原子包](https://golang.org/src/sync/atomic/doc.go?s=275:550#L8)，因为它们的工作方式往往与预期不同。

**在下面的例子中，我们现在将允许 SQS 为每个请求批处理 10 条消息**

```
func (c *consumer) Consume() {
 c.workerPool = 50 // should be configurable
 c.workerCount = 0
 maxMessages := 10 for {
  if atomic.LoadInt32(&c.workerCount) < c.workerPool {
   output, err := receiveSQSMessages(c.QueueURL, maxMessages)
   if err != nil {
    continue
   } for _, m := range output.Messages {
     atomic.AddInt32(&c.workerCount, 1)
     go c.run(newMessage(m))
   }
  }
 }
}func (c *consumer) run(m *message) error {
  defer atomic.AddInt32(&c.workerCount, -1) if err := h(m); err != nil {
    return err
  }

  return c.delete(m) //MESSAGE CONSUMED
}
```

对于一个基本的并发消费者，我们得到的平均处理时间为**2700 MPM**。这是一个巨大的进步。

有趣的是，尽管我们允许一次运行 50 个 go routine，但是一次只能运行 2 个响应(总共 20 个 go routine)。这是因为处理消息的速度比长轮询从 SQS 检索消息的速度快。我们现在发现来自 SQS 的 http 请求是主要的瓶颈

# **工人池**

工作池是管理应用程序中 goroutines 的惯用方法。它们在内存和速度方面都更高效，并在所有准备好的工作人员之间合理地分配工作负载。在大多数情况下，这将是最终的解决方案(提示:它不是)

```
func (c *consumer) Consume() {
  maxMessages := 10
  jobs := make(chan *message)
  for w := 1; w <= c.workerPool; w++ {
    go c.worker(w, jobs)
  } for {
    output, err := retrieveSQSMessages(c.QueueURL, maxMessages)
    if err != nil {
     //log error
     continue
    } for _, m := range output.Messages {
      jobs <- newMessage(m)
    }
  }
}// worker is an always-on concurrent worker that will take tasks when they are added into the messages buffer
func (c *consumer) worker(id int, messages <-chan *message) {
 for m := range messages {
   if err := h(m); err != nil {
    //log error
    continue

   } c.delete(m) //MESSAGE CONSUMED
 }
}
```

首先，我们创建一个工作池池(本例中有 50 个)。这些工人正在寻找工作。消息通道上的范围是一个阻塞操作，因此工作者将处于休眠状态，直到有消息可用。如果您以不同的方式处理错误或消息处理程序，您可以在 *For* 循环中使用 *select* 以相同的方式处理各种选项。

此外，在使用者中发送消息也是一种阻塞操作。只有在有工作人员可用的情况下，它才会继续。这确保了如果没有可用的工作线程，使用者将不会继续检索消息。

这种方法的伟大之处在于，它在不同的工作人员之间实现了一种近乎循环的方式，并且在大多数情况下速度更快。

对于一个工人池消费者，我们得到的平均处理时间为 **2，766 mpm。**与前面的例子相比，这并不是一个巨大的改进。在这种情况下，虽然代码更加习惯和可靠，但我们仍然有相同的单点故障和瓶颈，因为对 AWS 的长轮询请求只是*不够快*。

# **最终解决方案**

下一个合乎逻辑的步骤是通过将 http 请求本身合并到 worker 中来消除瓶颈。我们脱离了工人池的概念，因为我们不再需要一个调度程序(即一个通过通道向工人池发送作业或工作负载的系统)。我们只需要一组 goroutines，它可以无限期地查询 sqs 服务器上的消息并处理它们。

```
func (c *consumer) Consume() {
 for w := 1; w <= c.workerPool; w++ {
  go c.worker(w)
 }
}func (c *consumer) worker(id int) {
 for {
  output, err := retrieveSQSMessages(c.QueueURL, maxMessages)
  if err != nil {
   continue
  } var wg sync.WaitGroup
  for _, message := range output.Messages {
   wg.Add(1)
   go func(m *message) {
     defer wg.Done()
     if err := h(m); err != nil {
       //log error
       continue
     }
     c.delete(m) //MESSAGE CONSUMED
   }(newMessage(m))

   wg.Wait()
  }
 }
}
```

在 **6750 mpm** 时，我们成功地将消息处理速度提高了 200%,比同步示例提高了 4000%。

在本例中，我们根据可配置的数量创建了一组工作线程，它们都负责自己的长轮询和消息处理。当收到一组消息时，waitgroup 用于同步一组额外的 goroutines，以便在再次查询之前管理消息负载

虽然这种解决方案偏离了工人池的优雅，但它消除了瓶颈和单点故障。最终结果是消费者的速度成倍提高，只需一个实例就可以完成 50 个实例。

该解决方案可以通过添加额外的工作线程来扩展 CPU 容量，也可以通过添加额外的消费者实例来无限扩展。如果成本是一个问题，可以在休眠期间缩减工作人员，并在有更多消息需要处理时自动增加工作人员

有别的解决方法吗？请留下评论

更新:我已经在这里开源了我用来创建这个解决方案的库[https://github.com/qhenkart/gosqs](https://github.com/qhenkart/gosqs)