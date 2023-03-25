# 50 tensor flow . js API 5 分钟讲解| TensorFlow.js Cheetsheet

> 原文：<https://towardsdatascience.com/50-tensorflow-js-api-explained-in-5-minutes-tensorflow-js-cheetsheet-4f8c7f9cc8b2?source=collection_archive---------4----------------------->

## TensorFlow API Cheetsheet

![](img/357c3770e5a5e6d7792c106b39073603.png)

在这篇文章中，我真的想看看张量流**。js**API，从整体上理解这个库，并理解它为机器学习社区提供了哪些令人惊叹的东西。

我知道这篇文章应该有 5 分钟长，但不要担心，理解这些显而易见的 API 不会超过 5 分钟，即使它们中的许多从名字上看非常明显，对我来说保持初学者友好是很重要的。

我在文章的其余部分尽可能用例子来说明问题。但是如果你们有任何疑问，让我们在评论中讨论吧。

![](img/4a11c87fa04e0be9f81cacbf4499cf80.png)

它将帮助你为任何新的未来项目编写更好的通用机器学习代码。

# **创造🚀**

API 有助于创建张量、克隆体等东西。

> **1** - **tf.tensor ( *数值，*** *形状？* ***，*** *dtype？* **)**

> **创建具有指定数据类型形状的张量。**

```
*// Pass an array of values to create a vector.* 
tf.tensor([1, 2, 3, 4]).print();**------RESULT------**
**Tensor     [1, 2, 3, 4]***// Pass a flat array and specify a shape yourself.* 
tf.tensor([1, 2, 3, 4], [2, 2]).print();**------RESULT------**
**Tensor     [[1, 2],      
            [3, 4]]**
```

Returns → **tf。张量**

> **注**:他们也有形状指定的 API，比如 *tf.tensor1d，tf.tensor2d，*TF . tensor 3d 和 *tf.tensor4d* 。

> **2**-[-**TF . buffer**](https://js.tensorflow.org/api/0.11.2/#buffer)**(shape，dtype？，价值观？)**

> **创建一个缓冲张量。**

```
*// Create a buffer and set values at particular indices.* 
**const** buffer = tf.buffer([2, 2]); 
buffer.set(3, 0, 0); 
buffer.set(5, 1, 0);  *// Convert the buffer back to a tensor.* buffer.toTensor().print();**-------RESULT------**
**Tensor     [[3, 0],
            [5, 0]]**
```

返回→ **tf。张量缓冲器**

> **3**-[**TF . from pixels**](https://js.tensorflow.org/api/0.11.2/#fromPixels)**(****num channels？* **)***

> *从图像中创建一个张量。*

```
***const** image = new ImageData(1, 1); 
image.data[0] = 100; 
image.data[1] = 150; 
image.data[2] = 200; 
image.data[3] = 255;  
tf.fromPixels(image).print();**------RESULT------**
**Tensor      [ [[100, 150, 200],]]***
```

*返回→ **tf。Tensor3D***

> ***4**-[-**TF . linspace**](https://js.tensorflow.org/api/0.11.2/#linspace)**(*start，stop，num* )***

> ***创建一个具有均匀间隔数字的张量。***

```
*tf.linspace(0, 9, 10).print();**------RESULT------**
**Tensor     [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]***
```

*返回→ **tf。张量 1D***

> ***5**-[**TF . onehot**](https://js.tensorflow.org/api/0.11.2/#oneHot)**(*indexes，depth，*** *onValue？* ***，*** *offValue？* **)***

> ***将一组稀疏的标签转换成密集的独热表示。***

```
*tf.oneHot**(**tf.tensor1d([0, 1], 'int32'), 3**)**.print();**------RESULT------
Tensor     [[1, 0, 0],
            [0, 1, 0]]***
```

*返回→ **tf。张量 2D***

> ***6**-[**TF . print**](https://js.tensorflow.org/api/0.11.2/#print)**(****啰嗦？* **)****

> ****打印信息。关于任何张量。****

```
****const** verbose = true; 
tf.tensor2d([1, 2, 3, 4], [2, 2]).print(verbose);**------RESULT------
Tensor   dtype: float32  
         rank: 2  
         shape: [2,2]  
         values:     [[1, 2], 
                      [3, 4]]****
```

**退货→ **作废****

> ****7**-[**TF . variable**](https://js.tensorflow.org/api/0.11.2/#variable)**(*initial value，*** *可训练？，名字？，dtype？* **)****

> ****用提供的初始值创建一个张量变量。****

```
****const** x = tf.variable(tf.tensor([1, 2, 3]));
x.assign(tf.tensor([4, 5, 6]));
x.print();**------RESULT------** **Tensor     [4, 5, 6]****
```

**Returns → **tf。变量****

# **tf 的方法。张量**

> **8 - [**展平**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.flatten) ()**

> ****将张量展平为 1D 数组****

**返回→ **tf。张量 1D****

> **9-[](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.asScalar)**()****

> ******转换一个 size-1 的 tf。张量到一个 tf.Scalar.******

****返回→ **tf。标量******

> ****10.1-[**as1D**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.as1D)**()******

> ******转换一个 tf。张量到一个 tf.Tensor1D.******

****返回→ **tf。张量 1D******

> ****10.2-[**as2D**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.as2D)**(行、列)******

> ******转换一个 tf。张量到一个 tf.Tensor2D.******

****返回→ **tf。张量 2D******

> ****10.3-[**as3D**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.as3D)(**行、列、深度**)****

> ******转换一个 tf。张量到一个 tf.Tensor3D.******

****Returns→ **tf。张量 3D******

> ****10.4 - [**as4D**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.as4D) ( **行，列，深度，depth2** )****

> ******转换一个 tf。张量到一个 tf.Tensor4D.******

****Returns→ **tf。张量 4D******

> ****11-[**as type**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.asType)**【dtype】******

> ******施放一个 tf。指定数据类型的张量。******

****返回→ **本******

> ****12 - [**缓冲**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.buffer) **( )******

> ****返回一个 tf。保存底层数据的 TensorBuffer。****

****返回→ **tf。张量缓冲器******

> ****13 - [**数据**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.data) ()****

> ****tf.data API 有助于构建灵活高效的输入管道。****

****退货→ **承诺******

> ****14 - [**处分**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.dispose) **( )******

> ******处置 tf。来自记忆的张量。******

****退货→ **作废******

> ****15.1 - [**浮动**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.toFloat) **( )******

> ******将数组转换为 float32 类型。******

****返回→ **这个******

> ****15.2-[**toInt**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.toInt)**()******

> ****将数组转换为 int32 类型。****

****返回→ **这个******

> ****15.3 - [**托布尔**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.toBool) **( )******

> ****将数组转换为 bool 类型。****

****返回→ **这个******

> ****16-[](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.reshapeAs)****(x)********

> ********将张量整形为所提供的张量的形状。********

******返回→ **tf。张量********

> ******17 - [**toString**](https://js.tensorflow.org/api/0.11.2/#tf.Tensor.toString) **(啰嗦？)********

> ********返回张量的可读描述。对日志记录有用。********

******返回→ **字符串********

# ******tf 的方法。变量扩展到 tf。张量******

> ******18 - [**赋值**](https://js.tensorflow.org/api/0.11.2/#tf.Variable.assign) **(新值)********

> ******分配一个新的任务组。这个变量的张量。******

******退货→ **作废********

# ********TF 的方法。张量缓冲********

********tf。TensorBuffer 允许在声明一个 tf 之前设置一个值。不可改变的张量。********

> ******[19-**设置**](https://js.tensorflow.org/api/0.11.2/#tf.TensorBuffer.set) **( value，locs )********

> ********在缓冲区的给定位置设置一个值。********

******退货→ **作废********

> ******20 - [**获取**](https://js.tensorflow.org/api/0.11.2/#tf.TensorBuffer.get) **( locs )********

> ********返回缓冲区中指定位置的值。********

******退货单→ **编号********

> ******21-[](https://js.tensorflow.org/api/0.11.2/#tf.TensorBuffer.toTensor)****()**********

> ******创建一个不可变的 tf。缓冲区中的张量对象。******

******返回→ **tf。张量********

# ******变形******

> ******22-[**TF . expanddims**](https://js.tensorflow.org/api/0.11.2/#expandDims)**(x，轴？)********

> ********返回一个 tf。通过在张量的形状中插入一个维度来扩展秩的张量。********

```
********const** x = tf.tensor1d([1, 2, 3, 4]); 
**const** axis = 1; 
x.expandDims(axis).print();**------RESULT------
Tensor     [[1],      [2],      [3],      [4]]********
```

******返回→ **tf。张量********

> ******23-[**TF . cast**](https://js.tensorflow.org/api/0.11.2/#cast)**(x，dtype )********

> ********施放一个 tf。张量到一种新的数据类型。********

```
********const** x = tf.tensor1d([1.5, 2.5, 3]);
tf.cast(x, 'int32').print();**------RESULT------
Tensor     [1, 2, 3]********
```

******返回→ **tf。张量********

> ******24-[**TF . pad**](https://js.tensorflow.org/api/0.11.2/#pad)**(x，paddings，constantValue？)********

> ********垫一个 tf。具有给定值和填充的张量。********

```
******const x = tf.tensor1d([1, 2, 3, 4]);
x.pad([[1, 2]]).print();**------RESULT------
Tensor     [0, 1, 2, 3, 4, 0, 0]********
```

******返回→ **tf。张量********

> ******25-[**TF . shape**](https://js.tensorflow.org/api/0.11.2/#reshape)**(x，shape)********

> ********重塑一个 tf。给定形状的张量。********

```
********const** x = tf.tensor1d([1, 2, 3, 4]); 
x.reshape([2, 2]).print();**------RESULT------
Tensor     [[1, 2],      
            [3, 4]]********
```

******Returns→ **tf。张量********

> ******26 - tf.squeeze ( x，轴？)******

> ********从 tf 的形状中删除尺寸为 1 的尺寸。张量********

```
******const x = tf.tensor([1, 2, 3, 4], [1, 1, 4]);
x.squeeze().print();**------RESULT------
Tensor     [1, 2, 3, 4]********
```

******Returns→ **tf。张量********

******![](img/31e201a70203836cb98df7e8408f9617.png)******

# ******模型******

******模型是层的集合，有关层如何连接的详细信息，请参见模型创建。******

> ******27 - [tf。](https://js.tensorflow.org/api/0.11.2/#sequential) [型号](https://js.tensorflow.org/api/0.11.2/#model)()延伸至集装箱******

> ********模型是由层组成的数据结构，其节点被定义为输入和输出。********

******用 tf.model 制作的模型比 **tf.sequential** 模型更普通。******

> ******下面的代码片段定义了一个由**两个密集层(全连接)**组成的模型， **10 和 4 个单元。********

```
*******//Define input, which has a size of 5(not including batch dimension)*
**const input = tf.input({shape: [5]});** 
*// First dense layer uses relu activation.* 
**const denseLayer1 = tf.layers.dense({units: 10, activation: 'relu'});** 
*// Second dense layer uses softmax activation.* 
**const denseLayer2 = tf.layers.dense({units: 2, activation: 'softmax'})**;  
*// Obtain the output symbolic tensor by applying the layers on the input.* 
**const output = denseLayer2.apply(denseLayer1.apply(input));** *// Create the model based on the inputs.* 
**const model = tf.model({inputs: input, outputs: output});** *// The model can be used for training, evaluation and prediction.* 
*// For example, the following line runs prediction with the model on* *// some fake data.* 
**model.predict(tf.ones([2, 5])).print();****------RESULT------
Tensor     [[0.3465916, 0.6534085],      [0.3465916, 0.6534085]]********
```

> ******27.1 - [**编译**](https://js.tensorflow.org/api/0.11.2/#tf.Model.compile) **( config )********

> ********为训练和评估配置和准备模型。编译用配置(优化器、损失和/或度量)装备模型。在未编译的** **模型上调用 fit 或 evaluate 会抛出错误。********

******退货→ **作废********

> ******27.2 - [**评估**](https://js.tensorflow.org/api/0.11.2/#tf.Model.evaluate) **( x，y，config？)********

> ******返回测试模式下模型的损失值和度量值，这些值是在 compile()期间指定的，需要在调用 evaluate()之前发生。******

```
********const** model = tf.sequential({    
layers: [tf.layers.dense({units: 1, inputShape: [10]})] }); model.compile({optimizer: 'sgd', loss: 'meanSquaredError'}); 
**const** result = **model.evaluate(tf.ones([8, 10]), tf.ones([8, 1]),               { batchSize: 4})**; 
result.print();
**------RESULT------
Tensor     0.00010169133020099252********
```

******返回→ **tf。标量**或 **tf。标量[]********

> ******27.3 - [**预测**](https://js.tensorflow.org/api/0.11.2/#tf.Model.predict) **( x，config？)********

> ********为输入样本生成输出预测，计算分批完成。********

```
********const** model = tf.sequential({
   layers: [tf.layers.dense({units: 1, inputShape: [10]})]
});
model.predict(tf.ones([8, 10]), {batchSize: 4}).print();
**------RESULT------
Tensor    
[[1.8470926],      
[1.8470926],      
[1.8470926],      
[1.8470926],      
[1.8470926],      
[1.8470926],      
[1.8470926],      
[1.8470926]]********
```

******返回→ **tf。张量**或 **tf。张量[]********

> ******27.4 - [**预测批次**](https://js.tensorflow.org/api/0.11.2/#tf.Model.predictOnBatch) **( x )********

> ********返回一批样本的预测值，其中 x 是张量。********

```
********const** model = tf.sequential({
   layers: [tf.layers.dense({units: 1, inputShape: [10]})]
});
model.predictOnBatch(tf.ones([8, 10])).print();
**------RESULT------** **Tensor     
[[-1.1825931],      
[-1.1825931],      
[-1.1825931],      
[-1.1825931],      
[-1.1825931],      
[-1.1825931],      
[-1.1825931],      
[-1.1825931]]********
```

******返回→ **tf。张量**或 **tf。张量[ ]********

> ******27.5 - [**拟合**](https://js.tensorflow.org/api/0.11.2/#tf.Model.fit) **( x，y，config？)********

> ********为固定数量的历元(数据集上的迭代)训练模型。********

```
********const** model = tf.sequential({      
layers: [tf.layers.dense({units: 1, inputShape: [10]})] }); model.compile({optimizer: 'sgd', loss: 'meanSquaredError'}); 
for (let i = 1; i < 5 ; ++i) {    
**const** h = await model.fit(tf.ones([8, 10]), tf.ones([8, 1]), 
                          { batchSize: 4, epochs: 3    });    
console.log("Loss after Epoch " + i + " : " + h.history.loss[0]); 
}
**------RESULT------** **Loss after Epoch 1 : 0.32676371932029724 
Loss after Epoch 2 : 0.016571789979934692 
Loss after Epoch 3 : 0.0008404387044720352 
Loss after Epoch 4 : 0.00004262066795490682********
```

******退货→ **承诺********

> ******28 - [**tf。**](https://js.tensorflow.org/api/0.11.2/#model)**()延伸到 tf。型号********

> ********一个有一堆层的模型，从一层到下一层线性进给。********

```
*******// Define a model for linear regression.*   
**const** model = tf.sequential();   
model.add(tf.layers.dense({units: 1, inputShape: [1]}));    
*//Prepare model for training: Specify the loss and the optimizer.*   
model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});    
*// Generate some synthetic data for training.*   
**const** xs = tf.tensor2d([1, 2, 3, 4], [4, 1]);   
**const** ys = tf.tensor2d([1, 3, 5, 7], [4, 1]);    
*// Train the model using the data then do inference on a data point //the* *model hasn't seen:*   
await model.fit(xs, ys);   model.predict(tf.tensor2d([5], [1, 1])).print();**------RESULT------** **Tensor      [[3.1389987],]********
```

> ******28.1 - [**添加**](https://js.tensorflow.org/api/0.11.2/#tf.Sequential.add) **(图层)********

> ********在层堆栈上添加一个层实例。********

```
******const model = tf.sequential();
model.add(tf.layers.dense({units: 8, inputShape: [1]})); model.add(tf.layers.dense({units: 4, activation: 'relu6'})); model.add(tf.layers.dense({units: 1, activation: 'relu6'}));

*// Note that the untrained model is random at this point.*
 model.predict(tf.randomNormal([10, 1])).print();**------RESULT------** **Tensor     [[0.0679427],      
[0.3440365],      
[0.4146437],      
[0        ],      
[0.0855753],      
[0.0688378],      
[0.1540508],      
[0        ],      
[0        ],      
[0        ]]********
```

******退货→ **作废********

> ******28.2 - [**评估**](https://js.tensorflow.org/api/0.11.2/#tf.Sequential.evaluate) **( x，y，config？)********

> ********返回测试模式下模型的损失值&度量值，计算是成批完成的。********

```
********const** model = tf.sequential({
   layers: [tf.layers.dense({units: 1, inputShape: [10]})]
});
model.compile({optimizer: 'sgd', loss: 'meanSquaredError'});
**const** result = model.evaluate(tf.ones([8, 10]), tf.ones([8, 1]), {
   batchSize: 4,
});
result.print();**------RESULT------** **Tensor     5.569275379180908********
```

******返回→ **tf。标量**或 **tf。标量[]********

> ******28.3 - [**预测**](https://js.tensorflow.org/api/0.11.2/#tf.Sequential.predict) **( x，config？)********

> ********为输入样本生成输出预测，计算分批完成。********

```
********const** model = tf.sequential({
   layers: [tf.layers.dense({units: 1, inputShape: [10]})]
});
model.predict(tf.ones([2, 10])).print();**------RESULT------** **Tensor     [[-0.6576568],      [-0.6576568]]********
```

******返回→ **tf。张量**或 **tf。张量[]********

> ******28.4 - [**拟合**](https://js.tensorflow.org/api/0.11.2/#tf.Sequential.fit) **( x，y，config？)********

> ********为固定数量的历元(数据集上的迭代)训练模型。********

```
********const** model = tf.sequential({
   layers: [tf.layers.dense({units: 1, inputShape: [10]})]
});
model.compile({optimizer: 'sgd', loss: 'meanSquaredError'});
**const** history = await model.fit(tf.ones([8, 10]), tf.ones([8, 1]), {
   batchSize: 4,
   epochs: 3
});
console.log(history.history.loss[0]);**------RESULT------** **0.486328125********
```

******退货→ **承诺********

> ******29-[**TF . load model**](https://js.tensorflow.org/api/0.11.2/#loadModel)**(path 或 IOHandler )********

> ********加载一个模型，包括它的拓扑和可选的权重。********

********例 1** :将 tf.model()的拓扑和权重保存到浏览器本地存储；然后装回去。******

```
********const** model = tf.sequential(
     {layers: [tf.layers.dense({units: 1, inputShape: [3]})]});
console.log('Prediction from original model:');
model.predict(tf.ones([1, 3])).print();

**const** saveResults = await model.save('localstorage://my-model-1');

**const** loadedModel = await tf.loadModel('localstorage://my-model-1');
console.log('Prediction from loaded model:');
loadedModel.predict(tf.ones([1, 3])).print();**------RESULT------
Prediction from original model: Tensor      [[0.7820088],] Prediction from loaded model: Tensor      [[0.7820088],]********
```

********示例 2:** 从 HTML 文件输入元素中从用户选择的文件中加载模型。******

```
*******// Note: this code snippet will not work without the HTML elements in the page*
**const** jsonUpload = document.getElementById('json-upload');
**const** weightsUpload = document.getElementById('weights-upload');**const** model = await tf.loadModel**(**
tf.io.browserFiles([jsonUpload.files[0],     weightsUpload.files[0]])**)**;******
```

********示例 3:** 从 HTTP 服务器加载一个模型。******

```
********const** model = await
     tf.loadModel('https://storage.googleapis.com/tfjs-models/tfjs/iris_v1/model.json')******
```

# ******模型管理******

> ******30.1-[**TF . copy model**](https://js.tensorflow.org/api/0.11.2/#copyModel)**(sourceURL，destURL )********

> ********将模型从一个 URL 复制到另一个 URL。********

```
*******// First create and save a model.* 
**const** model = tf.sequential(); 
model.add(tf.layers.dense**(**      {units: 1, inputShape: [10], activation: 'sigmoid'})**)**; 
await model.save('localstorage://demo/management/model1'); 

*// Then list existing models.* 
console.log(await tf.io.listModels()); *// Copy the model, from Local Storage to IndexedDB.* 
await tf.io.copyModel('localstorage://demo/management/model1',      'indexeddb://demo/management/model1'); *// List models again.* 
console.log(await tf.io.listModels()); 

*// Remove both models.* 
await tf.io.removeModel('localstorage://demo/management/model1'); await tf.io.removeModel('indexeddb://demo/management/model1');**------RESULT------
[object Object] [object Object]********
```

******退货→ **承诺********

> ******30.2-[**TF . list models**](https://js.tensorflow.org/api/0.11.2/#listModels)**()********

> ********列出存储在注册存储介质中的所有型号。********

```
*******// First create and save a model.* 
const model = tf.sequential(); 
model.add**(**tf.layers.dense(  {units: 1, inputShape: [10], activation:   'sigmoid'})**)**; 
await model.save('localstorage://demo/management/model1');

*// Then list existing models.* 
console.log(await tf.io.listModels()); *// Delete the model.* 
await tf.io.removeModel('localstorage://demo/management/model1'); 

*// List models again.* 
console.log(await tf.io.listModels());**------RESULT------
[object Object] [object Object]********
```

******退货:→ **承诺********

> ******30.3-[**TF . move model**](https://js.tensorflow.org/api/0.11.2/#moveModel)**(sourceURL，destURL )********

> ********将模型从一个 URL 移动到另一个 URL。********

```
*******// First create and save a model.* 
**const** model = tf.sequential(); 
model.add(tf.layers.dense({units: 1, inputShape: [10], activation: 'sigmoid'})); 
await model.save('localstorage://demo/management/model1'); *// Then list existing models.* 
console.log(await tf.io.listModels()); *// Move the model, from Local Storage to IndexedDB.* 
await tf.io.moveModel('localstorage://demo/management/model1',      'indexeddb://demo/management/model1'); *// List models again.* 
console.log(await tf.io.listModels()); *// Remove the moved model.* 
await tf.io.removeModel('indexeddb://demo/management/model1');**------RESULT------
[object Object] [object Object]********
```

******退货→ **承诺********

> ******30.4-[**TF . remove model**](https://js.tensorflow.org/api/0.11.2/#removeModel)**(网址)********

> ********从注册的存储介质中删除由 URL 指定的型号。********

```
*******// First create and save a model.* 
**const** model = tf.sequential(); 
model.add(tf.layers.dense(      {units: 1, inputShape: [10], activation: 'sigmoid'})); 
await model.save('localstorage://demo/management/model1'); *// Then list existing models.
* console.log(await tf.io.listModels()); *// Delete the model.* 
await tf.io.removeModel('localstorage://demo/management/model1'); *// List models again.* 
console.log(await tf.io.listModels());
**------RESULT------
[object Object] [object Object]********
```

******退货→ **承诺********

# ******层(高级激活)******

> ******31.1-[**TF . layers . leaky relu**](https://js.tensorflow.org/api/0.11.2/#layers.leakyReLU)**(config？)********

> ********输入形状:任意。将该层用作模型中的第一层时，使用配置** `**inputShape**` **。********
> 
> ******`(x) = alpha * x for x < 0.` `f(x) = x for x >= 0.`******
> 
> ******输出形状→与输入形状相同。******

******returns→**TF . layers . layer********

> ******31.2-[**TF . layers . soft max**](https://js.tensorflow.org/api/0.11.2/#layers.softmax)**(config？)********

> ********输入形状** →任意。将该层用作模型中的第一层时，使用配置`inputShape`。******
> 
> ********输出形状** →与输入形状相同。******

******returns→**TF . layers . layer********

# ******层(基本功能)******

> ******32.1-[](https://js.tensorflow.org/api/0.11.2/#layers.dense)****(单位，激活好玩。，配置？)**********

> ********创建一个密集的(完全连接的)层。********
> 
> ********输出=激活(点(输入，内核(又名权重))+偏差)********

********单位**(数字)正整数，输出空间的维数。******

******returns→**TF . layers . layer********

> ******32.2-[**TF . layers . dropout**](https://js.tensorflow.org/api/0.11.2/#layers.dropout)**(rate，config？)********

> ********将漏失应用于输入。********

********速率**(数字)在 0 和 1 之间浮动。要丢弃的输入单位的分数。******

******returns→**TF . layers . layer********

> ******32.3-[**TF . layers . flatten**](https://js.tensorflow.org/api/0.11.2/#layers.flatten)**(config？)********

> ********拉平输入，不影响批量大小。********
> 
> ********它将输入到 1D 的每一批变平(使输出为 2D)。********

```
********const** input = tf.input({shape: [4, 3]});
**const** flattenLayer = tf.layers.flatten();*// Inspect the inferred output shape of flatten layer, which*
*// equals `[null, 12]`. The 2nd dimension is 4 * 3, i.e., the result // of the flattening.(The 1st dimension is undermined batch size.)*console.log(JSON.stringify(flattenLayer.apply(input).shape));**------RESULT------
[null,12]********
```

******returns→**TF . layers . layer********

# ******层(卷积)******

> ******33-[**TF . layers . conv2d**](https://js.tensorflow.org/api/0.11.2/#layers.conv2d)**(滤镜)********

> ********该层创建一个卷积核，该卷积核与层输入进行卷积，以产生输出张量。********

********滤波器**(数字)输出空间的维数(即卷积中滤波器的数量)。******

******returns→**TF . layers . layer********

> ******34-[**TF . layers . cropping 2d**](https://js.tensorflow.org/api/0.11.2/#layers.cropping2D)**(裁剪，配置？)********

> ********该层可以在图像张量的顶部、底部、左侧和右侧裁剪输入。********

********裁剪** (number|[number，number]|[[number，number]，[number，number]])裁剪沿宽度和高度的尺寸。******

```
********const** model = tf.sequential(); model.add(tf.layers.cropping2D({cropping:[[2, 2], [2, 2]],                                 inputShape: [128, 128, 3]})); *//now output shape is [batch, 124, 124, 3]*******
```

******returns→**TF . layers . layer********

# ******图层(标准化)******

> ******35-[**TF . layers . batch 规格化**](https://js.tensorflow.org/api/0.11.2/#layers.batchNormalization) **( axis，config？)********

> ********标准化每批前一层的激活，即应用一个保持平均激活接近 0 和激活标准偏差接近 1 的变换。********

********轴**(数字)应该归一化的整数轴(通常是特征轴)。默认值为-1。******

******返回→ **tf.layers.Layer********

# ******层(池化)******

> ******36-[**TF . layers . averagepool2d**](https://js.tensorflow.org/api/0.11.2/#layers.averagePooling1d)**(poolSize，config )********

> ********空间数据的平均池化操作。********

********poolSize** (number 或[number，number])在每个维度[垂直，水平]中缩减的因子。需要一个整数或 2 个整数的数组。******

******returns→**TF . layers . layer********

> ******注:他们还有**平均池 1d** 和**全局平均池**等。******

> ******37-[**TF . layers . maxpool2d**](https://js.tensorflow.org/api/0.11.2/#layers.maxPooling2d)**(**poolSize， **config )********

> ********空间数据的最大池化操作。********

********pool size**(number |[number，number])在每个维度[垂直，水平]中缩减的因子。需要一个整数或 2 个整数的数组。******

******returns→**TF . layers . layer********

> ******注:它们还有 **maxPooling1d** 和 **globalMaxPooling** 等。******

# ******层(包装)******

> ******38-[**TF . layers . bidirectional**](https://js.tensorflow.org/api/0.11.2/#layers.bidirectional)**(layer，config )********

> ********它包括复制网络中的第一个循环层，从而现在有两个并排的层，然后将输入序列原样作为输入提供给第一层，并将输入序列的反向副本提供给第二层。这项技术有助于 LSTMs。********

********图层** (RNN)一个要被包裹的 RNN 图层的实例。******

******退货→ **包装********

> ******39-[**TF . layers . time distributed**](https://js.tensorflow.org/api/0.11.2/#layers.timeDistributed)**(图层)********

> ********输入应该至少是 3D 的，索引 1 的维度将被认为是时间维度。********

******returns→**TF . layers . layer********

# ******图层(当前)******

> ******40-[**TF . layers . rnn**](https://js.tensorflow.org/api/0.11.2/#layers.rnn)**(cell**(TF。RNNCell 或 tf。RNNCell[ ] **) )********

> ********输入形状:带形状的 3D 张量。********

******returns→**TF . layers . layer********

> ******41 - [**tf。**](https://js.tensorflow.org/api/0.11.2/#layers.simpleRNN)[**multi rnncell**](https://js.tensorflow.org/api/0.11.2/#multiRNNCell)**(lstmCells，data，c，h)********

> ********计算 LSTMCells 堆栈的下一个状态和输出。********
> 
> ********每个单元格的输出用作下一个单元格的输入********

********lstmCells**LSTMCell 函数的数组。******

********数据** *输入到单元格中。*******

********c**前一个单元格状态的数组。******

********h** *前一个单元输出的数组。*******

******返回→**【TF。Tensor2D[]，tf。Tensor2D[]]********

> ******注意:它们还有[**TF . layers . simplernncell**](https://js.tensorflow.org/api/0.11.2/#layers.simpleRNNCell)**，********
> 
> ******[**TF . layers . stackednncells**](https://js.tensorflow.org/api/0.11.2/#layers.stackedRNNCells)**(配置)，********
> 
> ******[**layers . simplernn**](https://js.tensorflow.org/api/0.11.2/#layers.simpleRNN)**(配置)********

> ******42-[**TF . layers . lstm**](https://js.tensorflow.org/api/0.11.2/#layers.lstm)**【配置】********

> ********长短期记忆层********

```
********const** lstm = tf.layers.lstm({units: 8, returnSequences: true}); *// Create an input with 10 time steps.* 
**const** input = tf.input({shape: [10, 20]}); 
**const** output = lstm.apply(input); console.log(JSON.stringify(output.shape)); *// [null, 10, 8]: 1st dimension is unknown batch size; 
// 2nd dimension is the same as the sequence length of [tf.input()]//(#input), due to `returnSequences`: `true`;* 
*// 3rd dimension is the `LSTMCell`'s number of units.***------RESULT------
[null,10,8]********
```

******returns→**TF . layers . layer********

> ******43-[**TF . layers . lstmcell**](https://js.tensorflow.org/api/0.11.2/#layers.lstmCell)**(配置)********

> ********LSTM 单元与 RNN 子类 LSTM 的不同之处在于，其应用方法仅获取单个时间步长的输入数据，并在该时间步长返回单元的输出，而 LSTM 获取多个时间步长的输入数据。********

******例如:******

```
******const cell = tf.layers.lstmCell({units: 2}); 
const input = tf.input({shape: [10]}); 
const output = cell.apply(input);  console.log(JSON.stringify(output.shape)); 
*// [null, 10]: This is cell's output at a single time step. The 1st* *// dimension is the unknown batch size.***------RESULT------
[null,10]********
```

******LSTMCell 工作流最典型的用途是将多个像元组合成一个堆叠的 RNN 像元。例如******

```
********const** cells = [tf.layers.lstmCell({units: 4}),          tf.layers.lstmCell({units: 8}), ]; 
const rnn = tf.layers.rnn({cell: cells, returnSequences: true}); *//Create input with 10 time steps and length-20 vector at each step.* 
const input = tf.input({shape: [10, 20]}); 
const output = rnn.apply(input);console.log(JSON.stringify(output.shape)); *// [null, 10, 8]: 1st dimension is unknown batch size; 2nd dimension //is the* *same as the sequence length of [tf.input()](#input), due to //`returnSequences`: `true`;* *3rd dimension is the last `lstmCell`'s //number of units.***------RESULT------
[null,10,8]********
```

******返回→ **tf。RNNCell********

> ******44-[**TF . basiclstmcell**](https://js.tensorflow.org/api/0.11.2/#basicLSTMCell)**(健忘偏差，lstmKernel，lstmBias，数据，c，h )********

> ********计算 BasicLSTMCell 的下一个状态和输出。********

********c** 前一个单元格状态。******

********h** 前一个单元输出。******

******returns→**【TF。Tensor2D，tf。Tensor2D]********

# ******训练(梯度)******

> ******45-[**TF . grad**](https://js.tensorflow.org/api/0.11.2/#grad)**(f)********

> ********f** 代表数学函数。******

```
*******// f(x) = x ^ 2* 
**const** f = x => x.square(); *// f'(x) = 2x* 
**const** g = tf.grad(f); 

**const** x = tf.tensor1d([2, 3]); 
g(x).print();**------RESULT------
Tensor     [4, 6]********
```

******另一个例子******

```
*******// f(x) = x ^ 3* 
**const** f = x => x.pow(tf.scalar(3, 'int32')); *// f'(x) = 3x ^ 2* 
**const** g = tf.grad(f); *// f''(x) = 6x* 
**const** gg = tf.grad(g);  
**const** x = tf.tensor1d([2, 3]); 
gg(x).print();**------RESULT------
Tensor     [12, 18.0000038]********
```

******returns→**(x:**[**TF。张量**](https://js.tensorflow.org/api/0.11.2/#class:Tensor) **，dy？:**T30**TF。张量****)=>**[**TF。张量**](https://js.tensorflow.org/api/0.11.2/#class:Tensor)******

> ******46-[**TF . custom grad**](https://js.tensorflow.org/api/0.11.2/#customGrad)**【f】********

```
*******// Override gradient of our custom x ^ 2 op to be dy * abs(x);*
**const** customOp = tf.customGrad**(**x => {    
    return {value: x.square(), gradFunc: dy => [dy.mul(x.abs())]}; 
}**)**;**const** x = tf.tensor1d([-1, -2, 3]); 
**const** dx = tf.grad(x => customOp(x));  
console.log(`f(x):`); 
customOp(x).print(); 
console.log(`f'(x):`); 
dx(x).print();**------RESULT------
f(x): Tensor     [1, 4, 9] f'(x): Tensor     [1, 2, 3]********
```

******returns→**(…args:**[**TF。张量**](https://js.tensorflow.org/api/0.11.2/#class:Tensor)**[])=>**[**TF。张量**](https://js.tensorflow.org/api/0.11.2/#class:Tensor)******

# ******培训(优化人员)******

> ******47-[**TF . train . Adam**](https://js.tensorflow.org/api/0.11.2/#train.adam)**(learning rate？，beta1？，beta2？，ε？)********

> ********构建一个使用 Adam 算法的 AdamOptimizer。*见***[*https://arxiv.org/abs/1412.6980*](https://arxiv.org/abs/1412.6980)******

## ******参数:******

********learningRate** (数字)用于 Adam 梯度下降算法。可选择的******

******(数字)一阶矩估计值的指数衰减率。可选择的******

******(数字)二阶矩估计值的指数衰减率。可选择的******

********ε**(数字)用于数值稳定的小常数。可选择的******

******Returns→ **AdamOptimizer********

> ******48-[TF . train . rms prop](https://js.tensorflow.org/api/0.11.2/#train.rmsprop)**(learning rate，decay？，气势？，ε？，居中？)********

> ********构造一个 tf。使用 RMSProp 梯度下降的 RMSPropOptimizer。********

## ******参数:******

********学习率**(数字)用于 RMSProp 梯度下降算法的学习率。******

********衰减**(数字)历史/未来梯度的贴现因子。可选择的******

********动量**(数字)用于 RMSProp 梯度下降算法的动量。可选择的******

********ε**(数字)小值避免分母为零。可选择的******

********居中**(布尔型)如果为真，则通过梯度的估计方差对梯度进行归一化。可选择的******

******Returns→ **tf。RMSPropOptimizer********

# ******培训(损失)******

> ******49-[**TF . losses . meansquadererror**](https://js.tensorflow.org/api/0.11.2/#losses.meanSquaredError)**(标签，预测，权重？，还原？)********

> ********计算两个张量之间的均方误差。********

******返回→ **tf。张量********

> ******50-[**TF . losses . softmaxcrossentropy**](https://js.tensorflow.org/api/0.11.2/#losses.softmaxCrossEntropy)**(labels，logits，dim？)********

> ********计算逻辑和标签之间的 softmax 交叉熵。它测量类别互斥的离散分类任务中的概率误差。********

******例如，每个 CIFAR-10 图像都有且只有一个标签:图像可以是狗或卡车，但不能同时是狗和卡车。******

******返回→ **tf。张量********

# ******自我提醒:不要列很长的清单******

# ******— — — — — —奖金— — — — —******

# ******表演******

> ******[**TF . tidy**](https://js.tensorflow.org/api/0.11.2/#tidy)**(nameOrFn，Fn？，gradMode？)********

> ********执行 f 提供的函数，执行后清除 f 分配的所有中间张量，f 返回的除外********

******使用这种方法有助于避免内存泄漏。一般来说，将对操作的调用包装在 [tf.tidy()](https://js.tensorflow.org/api/0.11.2/#tidy) 中进行自动内存清理。******

```
*******// y = 2 ^ 2 + 1* 
**const** y = tf.tidy(() => {    */
/ a, b, and one will be cleaned up when the tidy ends.*    
**const** one = tf.scalar(1);    
**const** a = tf.scalar(2);    
**const** b = a.square();     
console.log('numTensors (in tidy): ' + tf.memory().numTensors);     *// The value returned inside the tidy function will return*    
*// through the tidy, in this case to the variable y.*    
return b.add(one); });  
console.log('numTensors (outside tidy): ' + tf.memory().numTensors); **y.print();****------RESULT------
numTensors (in tidy): 16 
numTensors (outside tidy): 14 
Tensor     5********
```

******returns→**void | number | string | TF。张量|tf。张量[]|{[key: string]:tf。张量|数字|字符串}********

> ******[](https://js.tensorflow.org/api/0.11.2/#keep)****(结果)**********

> ********保持一个 tf。tf.tidy()内部生成的张量不会被自动释放。********

```
********let** b; 
**const** y = tf.tidy**(**() => **{**    
      const one = tf.scalar(1);    
      const a = tf.scalar(2); *// b will not be cleaned up by the tidy. a and one will be cleaned //up when the tidy ends.*    
b = tf.keep(a.square());     
console.log('numTensors (in tidy): ' + tf.memory().numTensors); *// The value returned inside the tidy function will return*    
*// through the tidy, in this case to the variable y.*    
return b.add(one); 
**})**; console.log('numTensors (outside tidy): ' + tf.memory().numTensors); console.log('y:'); y.print(); 
console.log('b:'); 
b.print();**------RESULT------
numTensors (in tidy): 16 
numTensors (outside tidy): 15 
y: Tensor     5 
b: Tensor     4********
```

******Returns→ **tf。张量********

> ******[](https://js.tensorflow.org/api/0.11.2/#memory)****()**********

> ********返回程序当前时间的内存信息。********

******Returns→ **MemoryInfo********

> ******[**TF . time**](https://js.tensorflow.org/api/0.11.2/#time)**(f)********

> ********执行** f( ) **并返回一个带有计时信息的承诺。********

```
********const** x = tf.randomNormal([20, 20]); 
**const** time = await tf.time(() => x.matMul(x)); console.log**(**`kernelMs: ${time.kernelMs},                       wallTimeMs:   ${time.wallMs}`**)**;**------RESULT------
kernelMs: 0.10000000149011612, wallTimeMs: 33.40000000037253********
```

******退货→ **承诺********

> ******[**TF . set back end**](https://js.tensorflow.org/api/0.11.2/#setBackend)**(backen dtype，safeMode？)********

> ********设置后端(cpu、webgl 等)负责创建张量并在这些张量上执行操作。********

******退货→ **作废********

> ******[**TF . topixels**](https://js.tensorflow.org/api/0.11.2/#toPixels)**(img，canvas？)********

> ********画出一个** [**tf。像素值的张量**](https://js.tensorflow.org/api/0.11.2/#class:Tensor) **到一个字节数组或可选的画布。********

******此外，当输入的 dtype 为“float32”时，我们假定值在范围[0–1]内。否则，当输入为“int32”时，我们假定值在范围[0–255]内。******

******要绘制到的画布。可选择的******

********img** (tf。Tensor2D 或 tf。Tensor3D)秩 2 或秩 3 张量。如果等级为-2，则绘制灰度。如果秩为 3，则深度必须为 1、3 或 4。当深度为 1 时，绘制灰度。******

******退货→ **承诺********

******![](img/d5dcb638e4c91a2393a9b70e627de1db.png)******

******现在…我很确定我已经错过了你最喜欢的 API，所以不要忘记在评论或任何问题中提到它。******

> ******感谢您阅读帖子。******
> 
> ******我希望这篇文章对你有所帮助。******

******关注我 [**中**](https://medium.com/@sagarsharma4244) 获取更多惊艳教程。******

********鼓掌吧！分享一下！跟我来。********

******乐意帮忙。荣誉……..******

# ******你会喜欢的以前的故事:******

******[](/tensorflow-on-mobile-tensorflow-lite-a5303eef77eb) [## 手机上的 tensor flow:tensor flow Lite

### 我们得到了什么？

towardsdatascience.com](/tensorflow-on-mobile-tensorflow-lite-a5303eef77eb) [](/epoch-vs-iterations-vs-batch-size-4dfb9c7ce9c9) [## 纪元与批量大小与迭代次数

### 了解您的代码…

towardsdatascience.com](/epoch-vs-iterations-vs-batch-size-4dfb9c7ce9c9) [](/tensorflow-on-mobile-tutorial-1-744703297267) [## 手机上的 TensorFlow:教程

### 在 Android 和 iOS 上

towardsdatascience.com](/tensorflow-on-mobile-tutorial-1-744703297267) [](/activation-functions-neural-networks-1cbd9f8d91d6) [## 激活函数:神经网络

### Sigmoid，tanh，Softmax，ReLU，Leaky ReLU 解释！！！

towardsdatascience.com](/activation-functions-neural-networks-1cbd9f8d91d6)******