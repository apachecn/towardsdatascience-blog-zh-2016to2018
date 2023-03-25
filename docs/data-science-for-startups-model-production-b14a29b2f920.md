# 创业公司的数据科学:模型生产

> 原文：<https://towardsdatascience.com/data-science-for-startups-model-production-b14a29b2f920?source=collection_archive---------0----------------------->

![](img/69c61ba5c4f9ea645a7d34c5fcfabf36.png)

Source: cwizner at pixabay.com

我正在进行的关于在创业公司建立数据科学学科系列的第七部分。你可以在 [*简介*](/data-science-for-startups-introduction-80d022a18aec) *中找到所有帖子的链接，还有一本基于这个系列的关于* [*亚马逊*](https://www.amazon.com/dp/1983057975) *的书。*

数据科学家可以为初创公司提供价值的一个重要方式是构建可用于改进产品的数据产品。从模型训练到模型部署的转变意味着学习一套全新的工具来构建生产系统。模型的产品化意味着数据科学团队需要支持运营问题，以维护一个活跃的系统，而不仅仅是输出一个报告或模型的规范。

我用来缓解这种过渡的一种方法是托管工具，如 Google Dataflow，它为将模型投入生产提供了一种托管和可扩展的解决方案。本文中讨论的大多数方法都使用无服务器方法，因为它通常比手动配置服务器更适合初创公司。使用像 Dataflow 这样的工具还可以使数据科学家与工程团队更紧密地合作，因为它可以建立一个暂存环境，其中数据管道的一部分可以在部署之前进行测试。初创公司的大多数早期数据科学家也可能通过构建数据产品来扮演 ML 工程师的角色。

数据科学家应该拥有缩放模型所需的工具，而不是依赖工程团队将模型规范转化为生产系统。我过去完成这项工作的方法之一是使用预测模型标记语言( [PMML](https://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) )和谷歌的[云数据流](https://github.com/GoogleCloudPlatform/DataflowJavaSDK)。以下是我推荐的构建和部署模型的工作流程:

1.  用 R 或 Python 训练离线模型
2.  将模型翻译到 PMML
3.  使用数据流作业获取 PMML 模型用于生产

这种方法使数据科学家能够在本地处理用于训练模型的采样数据集，然后在完整的数据集上使用生成的模型规范。第三步可能需要工程团队的一些初始支持，但只需要设置一次。使用这种方法意味着数据科学家可以使用 PMML 支持的任何预测模型，利用托管数据流服务意味着团队无需担心维护基础架构。

在这篇文章中，我将讨论几种不同的模型产品化方法。首先，我讨论了如何在 R 中训练一个模型并将规格说明输出到 PMML。接下来，我提供了两种模型部署的例子:批处理和实时。最后，我将讨论一些我见过的团队用于产品化模型的定制方法。

## 构建模型规范

为了建立一个预测模型，我们将再次使用出生率公共数据集。在这篇文章中，我们将建立一个线性回归模型来预测出生体重。用于执行模型构建和导出过程的完整笔记本可在线[获得](https://github.com/bgweber/WindfallData/blob/master/natality/TrainNatality.ipynb)。脚本的第一部分从 BigQuery 下载数据，并将结果存储在数据框中。

```
**library**(bigrquery)
project <- "gcp_project_id"sql <- "
 SELECT year, plurality, apgar_5min, mother_age, father_age,    
       gestation_weeks, ever_born, mother_married, weight_pounds
 FROM `bigquery-public-data.samples.natality`
 order by rand() 
 LIMIT 10000 
"df <- query_exec(sql, project = project, use_legacy_sql = FALSE)
```

接下来，我们训练一个线性回归模型来预测出生体重，并计算误差指标:

```
lm <- lm(weight_pounds ~ ., data = df)
summary(lm)cor(df$weight_pounds, predict(lm, df))
mean(abs(df$weight_pounds - predict(lm, df))) sqrt(mean(abs(df$weight_pounds - predict(lm, df)^2)))
```

这会产生以下结果:

*   相关系数:0.335
*   平均误差:0.928
*   RMSE: 6.825

模型性能相当弱，可以探索其他算法和功能来改进它。由于这篇文章的目标是专注于将模型产品化，因此经过训练的模型就足够了。

下一步是将训练好的模型翻译成 PMML。 [r2pmml](https://github.com/jpmml/r2pmml) R 包和 [jpmml-r](https://github.com/jpmml/jpmml-r) 工具使这一过程变得简单，并支持多种不同的算法。第一个库将 R 模型对象直接转换成 PMML 文件，而第二个库需要将模型对象保存到 RDS 文件，然后运行命令行工具。我们使用第一个库直接进行翻译:

```
**library**(r2pmml)
r2pmml(lm, "natality.pmml")
```

这段代码生成了下面的 [pmml 文件](https://github.com/bgweber/WindfallData/blob/master/natality/natality.pmml)。PMML 文件格式指定用于模型的数据字段、要执行的计算类型(回归)以及模型的结构。在这种情况下，模型的结构是一组系数，定义如下:

```
<RegressionTable intercept="7.5619">   
  <NumericPredictor name="year" coefficient="3.6683E-4"/>   
  <NumericPreda ictor name="plurality" coefficient="-2.0459"/>   
  <NumericPredictor name="apgar_5min" coefficient="9.4773E-5"/>   
  ...
  <NumericPredictor name="mother_married" coefficient="0.2784"/>  
</RegressionTable>
```

我们现在有了一个模型规范，可以将其产品化并应用于我们的整个数据集。

## 批量部署

在批处理部署中，模型被应用于大量记录，结果被保存以备后用。这与实时方法不同，后者几乎实时地将模型应用于单个记录。可以将批处理方法设置为定期运行，例如每天运行，或者根据需要临时运行。

**SQL 查询** 我将用来执行批处理模型部署的第一种方法是最容易采用的方法之一，因为它直接使用 BigQuery，并且不需要增加额外的服务器。这种方法通过在查询中直接编码模型逻辑来应用模型。例如，我们可以如下应用 PMML 文件中指定的线性回归模型:

```
select weight_pounds as actual,  
  + 11.82825946749738
  + year * -0.0015478882184680862
  + plurality * -2.1703912756511254
  + apgar_5min * -7.204416271249425E-4
  + mother_age * 0.011490472355621577
  + father_age * -0.0024906543152388157
  + gestation_weeks * 0.010845982465606988
  + ever_born * 0.010980856659668442
  + case when mother_married then 1 else 0 end * 0.26494217739205
    as predicted
from records
```

结果是，数据集中的每条记录现在都有一个预测值，该值是根据模型规范计算的。对于这个例子，我手动将 PMML 文件转换为 SQL 查询，但是您可以构建一个工具来执行这个功能。因为所有的数据都已经在 BigQuery 中，所以这个操作运行起来相对较快，执行起来也不昂贵。还可以根据 SQL 中带有现有标签的记录来验证模型:

```
select sum(1) as records
  ,corr(actual, predicted) as Correlation
  ,avg(abs(actual - predicted)) as MAE
  ,avg(abs( (predicted - actual)/actual )) as Relative
from predictions
```

该查询的结果表明，我们的模型的平均绝对误差为 0.92 lbs，相关系数为 0.33，相对误差为 15.8%。使用 SQL 并不局限于线性回归模型，可以广泛应用于不同类型的模型，甚至[深网](/deep-neural-network-implemented-in-pure-sql-over-bigquery-f3ed245814d3)。下面是如何修改前面的查询来计算逻辑回归而不是线性回归:

```
1/(1 + exp(-1*(
    --regression calculation
)))as predicted
```

我过去也使用这种方法来部署增强模型，比如 AdaBoost。当模型的结构相对简单，并且您需要数据库中的模型结果时，这很有用。

**数据流—大查询**

![](img/5d71fc11d783ce600caf3d64be08fb94.png)

Components in the BigQuery Batch Model Deployment

如果您的模型更复杂，Dataflow 为部署模型提供了一个很好的解决方案。当使用 Dataflow Java SDK 时，您可以定义一个要在一组对象上执行的操作图，该服务将根据需要自动提供硬件进行扩展。在这种情况下，我们的图是一组三个操作:从 BigQuery 读取数据，计算每条记录的模型预测，并将结果写回 BigQuery。该流水线生成以下数据流 DAG:

![](img/ac9c4f44e56cac90f4c9b925b1ee7db0.png)

The Dataflow graph of operations used in this tutorial.

我使用 IntelliJ IDEA 来创作和部署数据流作业。虽然设置 Java 环境超出了本教程的范围，但是用于构建项目的 pom 文件可以在这里[获得](https://github.com/bgweber/WindfallData/blob/master/natality/pom.xml)。它包括对[数据流 sdk](https://github.com/GoogleCloudPlatform/DataflowJavaSDK) 和 [JPMML](https://github.com/jpmml) 库的以下依赖关系:

```
<dependency>            
  <groupId>com.google.cloud.dataflow</groupId>              
  <artifactId>google-cloud-dataflow-java-sdk-all</artifactId>   
  <version>2.2.0</version>        
</dependency><dependency>            
  <groupId>org.jpmml</groupId>            
  <artifactId>pmml-evaluator</artifactId>            
  <version>1.3.9</version>  
</dependency>
```

如上图所示，我们的数据流工作由三个步骤组成，我们将详细介绍这三个步骤。在讨论这些步骤之前，我们需要创建管道对象:

```
PmmlPipeline.Options options = PipelineOptionsFactory
  .fromArgs(args).withValidation().as(PmmlPipeline.Options.class); 
Pipeline pipeline = Pipeline.create(options);
```

我们创建一个管道对象，它定义了应用于对象集合的一组操作。在我们的例子中，管道正在对一组 [TableRow](https://developers.google.com/resources/api-libraries/documentation/bigquery/v2/java/latest/com/google/api/services/bigquery/model/TableRow.html) 对象进行操作。我们将一个选项类作为输入传递给 pipeline 类，该类为数据流作业定义了一组运行时参数，例如用于运行作业的 GCP 临时位置。

管道中的第一步是从公共 BigQuery 数据集中读取数据。从这一步返回的对象是 TableRow 对象的一个[p 集合](https://beam.apache.org/documentation/sdks/javadoc/0.5.0/org/apache/beam/sdk/values/PCollection.html)。特征查询字符串定义要运行的查询，并且我们指定在运行查询时要使用标准 SQL。

```
private static final String featureQuery =     
  "SELECT year, plurality, apgar_5min ... weight_pounds\n" +
  "FROM `bigquery-public-data.samples.natality`";pipeline.apply(BigQueryIO.read().fromQuery(featureQuery)           
       .usingStandardSql().withoutResultFlattening())
```

下一步是将模型预测应用于数据集中的每条记录。我们定义了一个 PTransform 来加载模型规范，然后应用一个 DoFn 在每个 TableRow 上执行模型计算。

```
.apply("PMML Application", new PTransform<PCollection<TableRow>,
  PCollection<TableRow>>() {model = new RegressionModelEvaluator(PMMLUtil.unmarshal(
  Resources.getResource("natality.pmml").openStream()));return input.apply("To Predictions", ParDo.of(
    new DoFn<TableRow, TableRow>() {           
     @ProcessElement          
     public void processElement(ProcessContext c) throws Exception {

     /* Apply Model */
}})))
```

应用模型代码段如下所示。它检索 TableRow 以创建估计值，为 pmml 对象创建输入字段的映射，使用模型来估计出生体重，创建新的 TableRow 以存储出生的实际和预测体重，然后将该对象添加到 DoFn 的输出中。总而言之，这个应用步骤加载模型，定义一个函数来转换输入集合中的每个记录，并创建预测对象的输出集合。

```
TableRow row = c.element();HashMap<FieldName, Double> inputs = new HashMap<>();            
for (String key : row.keySet()) {              
  if (!key.equals("weight_pounds")) {                   
    inputs.put(FieldName.create(key), Double
        .parseDouble(row.get(key).toString()));              
  }
}Double estimate =(Double)model.evaluate(inputs)
  .get(FieldName.create("weight_pounds"));TableRow prediction = new TableRow();            prediction.set("actual_weight", Double.parseDouble(
    row.get("weight_pounds").toString()));            prediction.set("predicted_weight", estimate);c.output(prediction);
```

最后一步是将结果写回 BigQuery。在本课的前面，我们定义了将记录写回 BigQuery 时要使用的模式。

```
List<TableFieldSchema> fields = new ArrayList<>();    
fields.add(new TableFieldSchema()
   .setName("actual_weight").setType("FLOAT64"));    
fields.add(new TableFieldSchema()
   .setName("predicted_weight").setType("FLOAT64"));    
TableSchema schema = new TableSchema().setFields(fields);.apply(BigQueryIO.writeTableRows()
  .to(String.format("%s:%s.%s",  PROJECT_ID, dataset, table))   
  .withCreateDisposition(BigQueryIO.Write.CreateDisposition     
  .CREATE_IF_NEEDED).withSchema(schema));pipeline.run();
```

我们现在已经定义了一个管道，可以运行它来为整个数据集创建预测。这个类的完整代码清单可以在[这里](https://github.com/bgweber/WindfallData/blob/master/natality/PmmlPipeline.java)找到。运行这个类将启动一个数据流作业，该作业将生成上面显示的 DAG，并将提供许多 GCE 实例来完成该作业。下面是一个用于运行此管道的自动缩放示例:

![](img/e3ed0b25e91423e9b91f32918d55dd76.png)

Autoscaling the Model Predicting Task

当作业完成时，输出是 BigQuery 项目中的一个新表，它存储了出生率数据集中所有记录的预测和实际权重。如果我们想要运行一个新的模型，我们只需要在数据流作业中指向一个新的 PMML 文件。运行离线分析和数据流项目所需的所有文件都可以在 [Github](https://github.com/bgweber/WindfallData/tree/master/natality) 上获得。

**数据流—数据存储**

![](img/67c9dd0fda58393c332429c3c2dae0c2.png)

Components in the Datastore Batch Model Deployment

通常，部署模型的目标是使结果对端点可用，以便应用程序可以使用它们。过去的两种方法将结果写入 BigQuery，这不是存储需要在事务中使用的数据的最佳位置。本节讨论的数据管道不是将结果写入 BigQuery，而是将结果写入 Datastore，web 服务或应用程序可以直接使用 Datastore。

执行此任务的管道重用前面管道的前两部分，从 BigQuery 读取记录并创建具有模型预测的 TableRow 对象。我在管道中添加了两个步骤，将 TableRow 对象转换为数据存储实体，并将结果写入数据存储:

```
.apply("To Entities", ParDo.of(new DoFn<TableRow, Entity>() { @ProcessElement
  public void processElement(ProcessContext c) throws Exception {
    TableRow row = c.element(); // Create a lookup key for the record
    String keyName = "Record" + row.get("recordID").toString();
    Key.Builder keyBuilder = Key.newBuilder();
    Key.PathElement.Builder path = Key.PathElement.
        newBuilder().setKind("Profile").setName(keyName);
    keyBuilder.addPath(path);
    Key key = keyBuilder.build(); // set the experiment group
    String expGroup = Double.parseDouble(row.get("predicted_weight")
        .toString()) >= 8 ? "Control" : "Treatment";
    Value value = Value.newBuilder().
        setStringValue(expGroup).build(); // create an entity to save to Datastore
    Entity.Builder builder = Entity.newBuilder().setKey(key);
    builder.putProperties("Experiment", value);
    c.output(builder.build());
  }
}))
.apply(DatastoreIO.v1().write().withProjectId(PROJECT_ID));
```

数据存储是一个 NoSQL 数据库，可由应用程序直接使用。要创建数据存储条目的实体，您需要创建一个键。在这个例子中，我使用了一个 recordID，它是使用 BigQuery 中的 *row_number()* 函数为出生记录生成的惟一标识符。该键用于存储关于配置文件的数据，可以使用该键作为查找来检索该数据。该操作的第二步是根据预测的出生体重，将记录分配给对照组或治疗组。这种方法对于手机游戏可能很有用，在手机游戏中，有很高购买可能性的用户可以被放入一个实验组，该实验组提供购买的提示。该代码片段的最后一部分构建实体对象，然后将结果传递给 Datastore。

存储在数据存储中的结果实体可以在客户端应用程序或 web 服务中使用。下面的 Java 代码片段展示了如何从数据存储中检索值。例如，基于预测模型的输出，可以使用 web 服务来检查登录时提供给用户的报价。

```
public static void main(String[] args) {
  Datastore datastore = 
      DatastoreOptions.getDefaultInstance().getService();
  KeyFactory keyFactory =
      datastore.newKeyFactory().setKind("Profile"); // get a user profile
  Entity profile = datastore.get(keyFactory.newKey("User101"));
  System.out.println(profile.getString("Experiment"));
}
```

## 实时部署

到目前为止，我们介绍的方法都有很大的延迟，并且对于创建实时预测没有用，比如根据用户当前的会话活动为他们推荐新内容。为了近乎实时地构建预测，我们需要使用不同类型的模型部署。

**Web 服务** 为模型提供实时预测的方法之一是建立一个计算输出的 Web 服务。因为我们已经在关于跟踪数据的帖子中讨论了 Jetty，所以我们将在这里重用它来完成这项任务。为了使这种方法有效，您需要将所有的模型输入指定为 web 请求的一部分。或者，您可以从像 Datastore 这样的系统中检索值。下面是一个使用 Jetty 和 JPMML 实现实时模型服务的示例:

```
public void handle(...) throws IOException, ServletException {
  // load the PMML model
  final ModelEvaluator<RegressionModel> evaluator;
  try {
    evaluator = new RegressionModelEvaluator(PMMLUtil.unmarshal(
        Resources.getResource("natality.pmml").openStream()));
  }
  catch (Exception e) {
    throw new RuntimeException(e);
  } // create a map of inputs for the pmml model
  HashMap<FieldName, Double> inputs = new HashMap<>();
  for (String attribute : modelFeatures) {
    String value = baseRequest.getParameter(attribute);
    inputs.put(FieldName.create(attribute), 
        Double.parseDouble(value));
  } // output the estimate
  Double estimate =(Double)evaluator.evaluate(inputs).
      get(FieldName.create("weight_pounds"));
  response.setStatus(HttpServletResponse.SC_OK);
  response.getWriter().println("Prediction: " + estimate);
  baseRequest.setHandled(true);
}
```

该代码处理一个消息请求，其中包含使用模型输入值的参数。该代码片段首先加载 PMML 规范，使用 web 请求参数创建要素的输入地图，应用赋值器获取预测值，并将结果作为输出写入。*模型特征*数组包含 PMML 文件中指定的特征列表。可以通过以下方式向服务提出请求:

```
[http://localhost:8080/&year=2000&plurality=1&apgar_5min=0&mother_age=30&father_age=28&gestation_weeks=40&ever_born=1&mother_married=1](http://localhost:8080/?year=2000&plurality=1&apgar_5min=0&mother_age=30&father_age=28&gestation_weeks=40&ever_born=1&mother_married=1)
```

在浏览器中输入这个 URL 的结果是一个网页，其中列出了 7.547 磅的预测体重。实际上，您可能希望使用消息编码，比如 JSON，而不是传递原始参数。这种方法很简单，延迟相对较低，但是需要管理服务。向上扩展很容易，因为每个模型应用都可以独立执行，并且在提供预测后不会更新任何状态。

**数据流—发布订阅**

![](img/18c216d3f43958142128972a40e2f706.png)

Components in the PubSub Live Model Deployment

也可以在流模式下使用数据流来提供实时预测。我们在数据管道上的 post 中使用了流式数据流，以便将事件流式传输到 BigQuery 和下游的 PubSub 主题。我们可以使用类似的方法进行模型预测，使用 PubSub 作为数据流作业的源和目的地。下面的 Java 代码展示了如何设置一个数据管道，该管道使用来自 PubSub 主题的消息，应用模型预测，并将结果作为消息传递给输出 PubSub 主题。该代码片段不包括加载 PMML 文件的过程，我们在本文前面已经介绍过了。

```
// Read messages from PubSub
PCollection<PubsubMessage> events = pipeline
   .apply(PubsubIO.readMessages().fromTopic(inboundTopic));// create a DoFn for applying the PMML model to instances
events.apply("To Predictions", ParDo.of(
    new DoFn<PubsubMessage, PubsubMessage>() {@ProcessElement
public void processElement(ProcessContext c) throws Exception {
  PubsubMessage row = c.element(); // create a map of inputs for the pmml model
  HashMap<FieldName, Double> inputs = new HashMap<>();
  for (String key : row.getAttributeMap().keySet()) {
    if (!key.equals("weight_pounds")) {
      inputs.put(FieldName.create(key),
          Double.parseDouble(row.getAttribute(key)));
    }
  } // get the estimate
  Double estimate = (Double)evaluator.evaluate(inputs).
      get(FieldName.create("weight_pounds")); // create a message with the prediction
  String message = "Prediction:" + estimate;
  PubsubMessage msg = new PubsubMessage(
      message.getBytes(), new HashMap());
  c.output(msg);
}}
.apply(PubsubIO.writeMessages().to(outboundTopic));
```

该代码从传入的主题中读取一条消息，然后从该消息中解析不同的属性，以用作模型的特征输入。结果保存在传递给出站主题的新消息中。由于数据流作业设置为在流模式下运行，管道将在收到消息时近乎实时地处理消息。这条管道的完整代码清单可以在 [Github](https://github.com/bgweber/StartupDataScience/tree/master/Productizing) 上找到。

在实践中，PubSub 可能有太多的延迟，这种方法对于在应用程序中直接处理 web 请求来说是无用的。当需要将预测传递给系统中的其他组件时，这种类型的方法非常有用。例如，它可以用来实现一个用户保留系统，向有高流失可能性的移动游戏用户发送有针对性的电子邮件。

**定制工程** 提供实时模型部署的其他可行方法有 Spark streaming、AWS Lambda 和 Kinesis analytics(针对简单模型)。

有时，数据科学团队不可能直接构建数据产品，因为需要应用模型的系统属于不同的工程团队。例如，电子艺界使用预测模型来改善[匹配平衡](https://wccftech.com/ea-matchmaking-algorithm/)，而构建该模型的团队可能无法直接访问执行该模型的游戏服务器。在这样的场景中，有必要有一个可以在两个团队之间传递的模型规范。虽然 PMML 是这里的一个选项，但定制模型规范和编码在行业中很常见。

过去，当数据科学团队需要与远程工程团队合作时，我也见过这种流程崩溃。如果一个模型需要被翻译，比如说从一个 Python 笔记本到 Go 代码，在翻译过程中可能会出现错误，这个过程可能会很慢，而且一旦部署了模型，就不可能对其进行修改。

## 结论

为了给初创公司提供价值，数据科学家应该能够构建支持新功能的数据产品。这可以通过与工程团队合作来完成，也可以由数据科学单独负责。我对初创公司的建议是，在构建数据产品时使用无服务器技术，以降低运营成本，并实现更快的产品迭代。

这篇文章介绍了将模型产品化的不同方法，从直接在 SQL 查询中编码逻辑到构建 web 服务，再到在数据流任务中使用不同的输出组件。将模型产品化的结果是，现在可以在产品中使用预测来构建新功能。

我还介绍了根据预测结果将用户分成不同实验组的想法。在下一篇文章中，我将讨论可以使用的不同实验方法，包括 A/B 测试和分阶段推出。