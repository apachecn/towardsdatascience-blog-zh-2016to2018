# 使用 Mockito 模仿同一个测试类中的方法

> 原文：<https://towardsdatascience.com/mocking-a-method-in-the-same-test-class-using-mockito-b8f997916109?source=collection_archive---------0----------------------->

![](img/69ed1e1330631bf4579472763d424f09.png)

在测试驱动开发(TDD)中，单元测试是隐含实现质量的子部分。在使用 junit 进行单元测试时，您会遇到想要模拟类的地方。当您调用具有外部通信(如数据库调用或 rest 调用)的类的方法时，就完成了模仿。通过模仿，您可以显式地定义方法的返回值，而无需实际执行方法的步骤。在这篇文章中，我将讨论在你编写测试用例的同一个测试类中模仿方法。

假设您有一个 Person 类，它有外部通信并相应地返回值。

```
 public class Person {

  private String name;
  private int age; public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public boolean runInGround(String location) {
    if(location.equals("ground")) {
      System.*out*.println("The person runs in the " + location);
      return true;
    } else {
      System.*out*.println("The person doesn't run in the " +   location);
      return false;
    }

  }

  public boolean isPlay() {

    if(this.runInGround("ground")) {
      System.*out*.println("The person plays.");
      return true;
    }
    else {
      System.*out*.println("The person doesn't play");
      return false;
    }
  }
}
```

出于解释的目的，让我们假设 runInGround(字符串位置)方法通过检查数据库值来返回值，这是一个外部通信。这个特殊的方法是在同一个 Person 类的 isPlay()方法中调用的，它影响 isPlay()方法的返回值。

**那么如何在不让 runInGround(字符串位置)执行并与数据库对话的情况下测试 isPlay()。**

我们可以模仿 PersonTest 类中的 *runInGround(字符串位置)*方法，如下所示。

```
 import org.junit.Assert;
import org.junit.Test;
import org.mockito.Mockito;

public class PersonTest{

  @Test
  public void playTest() {
    Person person = new Person("name", 15, "23435678V");

    Person person1 = Mockito.*spy*(person);

    Mockito.*doReturn*(true).when(person1).runInGround("ground");

    Assert.*assertEquals*(true, person1.isPlay());
  }
}
```

这里我们需要使用 Mockito.spy()来模拟我们正在测试的同一个类，而不是使用 mock(class)。然后我们可以如下模拟我们想要的方法。

```
Mockito.*doReturn*(true).when(person1).runInGround("ground");
```

希望这将有所帮助。用单元测试快乐编码:d。

**延伸阅读**

[1][http://www.vogella.com/tutorials/Mockito/article.html](http://www.vogella.com/tutorials/Mockito/article.html)

[https://dzone.com/articles/introduction-to-java-tdd](https://dzone.com/articles/introduction-to-java-tdd)