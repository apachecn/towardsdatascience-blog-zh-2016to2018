# PyTorch:第一个程序并遍历

> 原文：<https://towardsdatascience.com/pytorch-first-program-and-walk-through-ceb739134ab9?source=collection_archive---------2----------------------->

我看到 Fast.ai 正在 PyTorch 上转移，我看到 PyTorch 最适合研究原型。因此，我决定在 PyTorch 中实现一些研究论文。我已经在平行点研究过 C-DSSM 模型。但是我的实现是在喀拉斯。我将强调黑客的观点，将代码从 Keras 移植到 PyTorch，而不是博客中的研究观点。
我的实现在[nish Nik/Deep-Semantic-Similarity-Model-py torch](https://github.com/nishnik/Deep-Semantic-Similarity-Model-PyTorch)，我也记录了代码。
更多关于 C-DSSM 车型[在这里](http://research.microsoft.com/pubs/226585/cikm2014_cdssm_final.pdf)。

1.  C-DSSM 模型接受多个输入。一个是查询，正面单据和负面单据。在 Keras 中，您可以传递一个列表:

```
Model(inputs = [query, pos_doc] + neg_docs, outputs = prob)
# where query and pos_doc is the numpy array and neg_docs is a list of numpy array
```

在 PyTorch 中，您可以:

```
def forward(self, q, pos, negs):
# this is in the class definition itself, you can easily access negs[0] for the 0th element of list. I was surprised to find that it works with list
# Another way would have been:
# def forward(self, x):
#     q = x[0]
#     pos = x[1]
#     negs = x[2:]
```

2.C-DSSM 模型中的第一层是 Conv1d 层。所以，我对比了 Keras 和 PyTorch 的 conv1d 文档。在喀拉斯:

```
keras.layers.convolutional.Conv1D(filters, kernel_size, strides=1, padding='valid', dilation_rate=1, activation=**None**, use_bias=**True**, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=**None**, bias_regularizer=**None**, activity_regularizer=**None**, kernel_constraint=**None**, bias_constraint=**None**)
```

在 PyTorch:

```
*class* torch.nn.Conv1d(*in_channels*, *out_channels*, *kernel_size*, *stride=1*, *padding=0*, *dilation=1*, *groups=1*, *bias=True*)
```

我必须说明:

1.  *内核大小*:与 Keras *内核大小*相同
2.  *out_channels* :同 Keras *滤镜*
3.  *in_channels* :输入的通道数

> 我对 in_channels 感到困惑，我曾设想一个 5*90000 的输入，其中内核将排成一行并给出 5*300 的输出。但是我错了

PyTorch 拥有动态图形，这也是为什么它对程序员来说很棒的原因，你可以看到图形创建时发生了什么。

所以我输入了:

```
lq
( 0 ,.,.) = 
 1.8577e-01 8.3356e-01 8.5541e-01 … 1.1579e-01 6.4221e-01 6.4712e-01
 8.3658e-01 1.4647e-01 2.0220e-01 … 2.2165e-01 2.1841e-01 3.0833e-01
 7.1619e-01 1.8811e-01 1.6903e-01 … 9.2867e-01 5.6902e-01 9.1074e-01
 9.4578e-01 9.4889e-01 8.4584e-01 … 3.7839e-01 3.6997e-01 2.7487e-01
 6.4675e-01 2.5806e-01 3.1640e-01 … 9.8110e-01 8.6193e-01 1.0357e-02
[torch.FloatTensor of size 3x5x90000]
```

我对它进行了卷积核运算，权重为:

```
( 0 ,.,.) = 
3.0515e-01
-3.3296e-01
-4.1675e-01
1.3134e-01
4.1769e-01
```

和偏差:

```
0.1725
```

得到的结果是:

```
qc
 4.5800e-02 5.3139e-01 5.3828e-01 … 2.0578e-01 4.6649e-01 -7.2540e-02
 -7.0118e-02 -5.8567e-01 -5.5335e-01 … 3.0976e-01 -3.3483e-02 -1.3638e-01
 -8.8448e-02 -4.1210e-02 -8.5710e-02 … -6.0755e-01 -5.5320e-01 -4.0962e-01
 … ⋱ … 
 -2.8095e-01 1.1081e-01 7.9184e-02 … -3.7869e-01 -1.7801e-01 -4.1227e-01
 -8.7498e-01 -8.1998e-01 -8.2176e-01 … -7.7750e-01 -8.6171e-01 -5.1366e-01
 -5.7234e-01 -2.5415e-01 -2.5126e-01 … -5.8857e-01 -4.8405e-01 -2.4303e-01
[torch.FloatTensor of size 3x300x90000]
```

让我们手动计算:(内核权重*输入)+偏差

```
0.30515 * 0.18577 + -0.33296 * 0.83658 + -0.41675 * 0.71619 + 0.13134 * 0.94578 + 0.41769 * 0.645 + 0.1725
```

结果是:

```
0.045796651399999944
```

哪个接近 qc[0][0][0]
注意:我刚刚展示了第一维度的第 0 个元素。(仅与数学相关的内容)
因此，这个动态图让我直观地了解了 Conv1d 在 PyTorch 中是如何按列操作的。
上面的代码很简单:

```
query_len = 5
lq = np.random.rand(3, query_len, WORD_DEPTH)
lq = Variable(torch.from_numpy(lq).float())
query_conv = nn.Conv1d(WORD_DEPTH, K, FILTER_LENGTH)
# print(lq)
# print (query_conv.weight)
# print (query_conv.bias)
qc = query_conv(lq)
# print (qc)
```

3.在获得查询、pos 和否定文档的所有向量之后。我们必须计算余弦相似度，在 Keras 中你必须为它创建一个新层。在 PyTorch:

```
dots = [q_s.dot(pos_s)]
dots = dots + [q_s.dot(neg_s) for neg_s in neg_ss]
```

变得如此简单:)

4.现在`dots`是一个列表，要把它转换成 PyTorch 变量，我们只需做:

```
dots = torch.stack(dots)
```

PS:这是我在 PyTorch 中的第一个实现，如果代码中有任何问题或者你不明白的地方，请联系我。
我的 twitter 句柄是[nishan tiam](https://twitter.com/nishantiam)github 句柄是 [nishnik](https://github.com/nishnik) 。