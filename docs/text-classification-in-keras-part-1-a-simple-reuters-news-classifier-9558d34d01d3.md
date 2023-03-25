# Keras 中的文本分类(第一部分)——一个简单的路透社新闻分类器

> 原文：<https://towardsdatascience.com/text-classification-in-keras-part-1-a-simple-reuters-news-classifier-9558d34d01d3?source=collection_archive---------8----------------------->

## Keras 中教授 NLP 和文本分类系列的第 1 部分

![](img/933fd967e4e56f22b67fc3adceca9a8e.png)

# 教程视频

如果你喜欢这个视频或者觉得它有任何帮助，如果你给我一两美元来资助我的机器学习教育和研究，我会永远爱你！每一美元都让我离成功更近一步，我永远心存感激。

# 代码

```
import keras
from keras.datasets import reutersUsing TensorFlow backend.(x_train, y_train), (x_test, y_test) = reuters.load_data(num_words=None, test_split=0.2)
word_index = reuters.get_word_index(path="reuters_word_index.json")

print('# of Training Samples: {}'.format(len(x_train)))
print('# of Test Samples: {}'.format(len(x_test)))

num_classes = max(y_train) + 1
print('# of Classes: {}'.format(num_classes))# of Training Samples: 8982
# of Test Samples: 2246
# of Classes: 46index_to_word = {}
for key, value in word_index.items():
    index_to_word[value] = keyprint(' '.join([index_to_word[x] for x in x_train[0]]))
print(y_train[0])the wattie nondiscriminatory mln loss for plc said at only ended said commonwealth could 1 traders now april 0 a after said from 1985 and from foreign 000 april 0 prices its account year a but in this mln home an states earlier and rise and revs vs 000 its 16 vs 000 a but 3 psbr oils several and shareholders and dividend vs 000 its all 4 vs 000 1 mln agreed largely april 0 are 2 states will billion total and against 000 pct dlrs
3from keras.preprocessing.text import Tokenizer

max_words = 10000

tokenizer = Tokenizer(num_words=max_words)
x_train = tokenizer.sequences_to_matrix(x_train, mode='binary')
x_test = tokenizer.sequences_to_matrix(x_test, mode='binary')

y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)print(x_train[0])
print(len(x_train[0]))

print(y_train[0])
print(len(y_train[0]))[0\. 1\. 0\. ... 0\. 0\. 0.]
10000
[0\. 0\. 0\. 1\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0.
 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0.]
46from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation

model = Sequential()
model.add(Dense(512, input_shape=(max_words,)))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
print(model.metrics_names)['loss', 'acc']batch_size = 32
epochs = 3

history = model.fit(x_train, y_train, batch_size=batch_size, epochs=epochs, verbose=1, validation_split=0.1)
score = model.evaluate(x_test, y_test, batch_size=batch_size, verbose=1)
print('Test loss:', score[0])
print('Test accuracy:', score[1])Train on 8083 samples, validate on 899 samples
Epoch 1/3
8083/8083 [==============================] - 13s 2ms/step - loss: 1.3051 - acc: 0.7192 - val_loss: 0.9643 - val_acc: 0.7931
Epoch 2/3
8083/8083 [==============================] - 12s 2ms/step - loss: 0.5136 - acc: 0.8841 - val_loss: 0.8800 - val_acc: 0.8165
Epoch 3/3
8083/8083 [==============================] - 13s 2ms/step - loss: 0.2873 - acc: 0.9344 - val_loss: 0.9045 - val_acc: 0.8065
2246/2246 [==============================] - 0s 175us/step
Test loss: 0.8878143835789586
Test accuracy: 0.7983081033478224
```

原贴于 hunterheidenreich.com 的[。](http://hunterheidenreich.com/blog/keras-text-classification-part-1/)