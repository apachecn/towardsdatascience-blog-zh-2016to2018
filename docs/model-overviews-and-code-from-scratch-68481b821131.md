# 从头开始创建模型概述和代码

> 原文：<https://towardsdatascience.com/model-overviews-and-code-from-scratch-68481b821131?source=collection_archive---------1----------------------->

我最近在一次聚会上与一位数据科学家交谈，他提到他发现从头构建模型在智力上是有益的。注意到这让他对模型的行为有了很好的直觉，也是一个很好的 OOP 项目。我发现，通过在这个项目上花几天时间，我了解了每个模型的新特征，在堆叠、追加和整形 numpy 数组方面变得更加流畅，并且提高了我对优化算法以及使用 numpy 与标准 python 操作的效果的认识。

此外，我最近在深度学习中阅读和学习了大量新概念，我想后退一步，刷新我对常见分类算法的理解。

我决定采用 3 种难度递增的分类模型和一种额外的聚类算法:

*   KNN — [参见完整代码](https://github.com/xbno/Projects/blob/master/Models_Scratch/KNN%20from%20scratch.ipynb)
*   决策树— [见完整代码](https://github.com/xbno/Projects/blob/master/Models_Scratch/Decision%20Trees%20from%20scratch.ipynb)
*   随机福里斯特— [见完整代码](https://github.com/xbno/Projects/blob/master/Models_Scratch/Random%20Forest%20from%20scratch.ipynb)
*   Kmeans — [参见完整代码](https://github.com/xbno/Projects/blob/master/Models_Scratch/Kmeans%20from%20scratch.ipynb)

# **K-最近邻**

kNN 是机器学习中使用最广泛的分类模型之一，也是最容易理解的分类模型之一，因为它的决策过程是透明的。它被认为是一个懒惰的学习者，因为它根本不建立模型——记忆的训练数据集就是模型！为了进行样本外预测，kNN 找到 k 个最相似的训练样本，并对与它们相关联的结果类进行计数。并且通过多数投票或其他投票方案，算法预测未知样本的类别。

正如我提到的，kNN 是一个懒惰的学习者，因为它直到最后一秒——在预测时间——才做任何事情。这有几个影响。首先，这意味着 kNN 需要在预测时间内计算所有训练样本与每个未知点之间的相似度。这通常会导致更大的内存需求，并且可能比构建表示的其他模型更慢，计算量也更大。然而，如果结构适当，这可能是有利的。例如，如果新的训练样本不断生成，旧的样本被删除，那么 kNN 的预测将来自手头“最新”或“最相关”的数据。一个重要的注意事项是，kNN *假设特征被缩放*。这是确保在确定相似性时所有特征被平均加权的要求。

超参数是 k 的值、相似性度量和加权度量。k 的值是预测时用于投票的邻居数量。它介于 1 和 n 之间，其中 n 是训练样本的总数。

```
 **def** find_neighbors(self, new_sample):
        *'''List the k neighbors closest to the new sample.*

 *'''*
        distances = []      
        **for** i **in** range(len(self.X)):
            **if** self.metric == 'euclidean':  
                distance = self.euclidean(self.X[i], new_sample)
            **if** self.metric == 'manhattan':
                distance = self.manhattan(self.X[i], new_sample)
            distances.append((self.y[i],distance))
        distances = sorted(distances,key=operator.itemgetter(1))

        neighbors = []
        **for** i **in** range(self.k):
            neighbors.append(distances[i][0])
        **return** neighbors
```

相似性度量是根据距离量化未知样本和训练样本之间的关系的数学函数。您可以想象，相似点的排序以及反过来的分类严重依赖于所选择的度量。有许多相似性度量，每一个都有自己的用例。通常，决策基于数据集中的数据类型和一些经验法则。

默认的数字度量是欧几里德距离。它是两点之间的“直线距离”,由平方差之和定义。另一种方法是曼哈顿距离，这是从西 22 街和第三大道走到西 27 街和第五大道所需的距离，它受轴的限制，在数学上由绝对差之和定义。此外，分类数据和二进制数据通常使用汉明距离进行评估，这基本上类似于逻辑 and。它计算点之间相同属性的数量。每个度量标准都会产生不同的点排序，基于这些点它认为与未知点最相似。

```
def **euclidean**(self, a, b):
    return np.sqrt(((a-b)**2).sum(axis=0))
```

恰好有一个距离度量来统治它们。它叫做闵可夫斯基距离。我的意思是，当 p = 2 时，它有能力表示欧几里得，当 p = 1 时，它有能力表示曼哈顿，当 p 接近 0 时，它有能力表示近汉明尔。因此，这是您在现成的 kNNs 中看到的最常见的距离度量。

```
def **minkowski**(self, a, b, p):
    return ((np.abs(a-b)**p).sum(axis=0))**(1/p)
```

加权超参数旨在基于 k 个邻居在相似性线中的位置来修改对每个 k 个邻居投票的影响。它通常设置为多数投票，这是我在实现中硬编码的，但倒数或等级距离也是其他典型的选项。

```
 **def** majority_vote(self, neighbors):
        *'''Determine majority class from the set of neighbors.*

 *'''*
        class_votes = {}
        **for** i **in** range(len(neighbors)):
            sample_class = neighbors[i]
            **if** sample_class **in** class_votes:
                class_votes[sample_class] += 1
            **else**:
                class_votes[sample_class] = 1
        sorted_votes = sorted(class_votes.items())
        **if** self.ties:
            sorted_votes = self.tie(sorted_votes)
        **return** sorted_votes[0][0]
```

## kNN 培训:

1.记忆数据

## kNN 预测:

1.  接受新样本
2.  使用指定的度量计算所有训练点和新样本之间的距离
3.  基于相似性对所有训练点进行排序。距离越小，相似度越高
4.  将 k 个最近点定义为排序列表中的前 k 个点
5.  基于 k 个最近点类的加权集合计算预测值

```
 **def** predict(self, X_test):
        *'''Predict class for each value in array of new samples.*

 *'''*
        self.tie_count = 0
        y_pred = []
        **for** i **in** range(len(X_test)):
            neighbors = self.find_neighbors(X_test[i])
            pred_class = self.majority_vote(neighbors)
            y_pred.append(pred_class)
        **if** self.ties:
            print('**{}** ties'.format(self.tie_count))
        **return** y_pred
```

虽然很少，但我也添加了一个警告:当多数投票产生平局时，平局将出现在训练数据中出现频率最高的类中。由于 Iris 数据集很小，这种情况经常发生，但在较大的数据集中就不太常见了。

```
 **def** tie(self,sorted_votes):
        *'''Determine when ties occur in the the neighbors. Of the tied classes,*
 *choose the class most frequent in the training data.*

 *Print out number of ties.*
 *'''*
        tie = {}
        **for** pair **in** sorted_votes:
            count = pair[1]
            **if** count **in** tie:
                self.tie_count += 1
                *#print('tie')*
                tie[count].append(pair[0])
            **else**:
                tie[count] = [pair[0]]
            *#print(tie)*
        tie_class_frequency = {}
        **if** len(tie[count]) > 1:
            *#print('tie')*
            **for** tie_class **in** tie[count]:
                tie_class_frequency[tie_class] = np.count_nonzero(self.y == tie_class)
            max_class = max(tie_class_frequency, key=tie_class_frequency.get)
            *#print(max_class)*
            sorted_votes = [(max_class,1)]
        **return** sorted_votes
```

这个练习对我来说是一个很好的方式，可以深入到 kNN 的各个层次。这是从零开始构建更多模型的一个简单的开始，给了我一个解决问题的良好基础。了解从相似性度量到加权选项的众多选项有助于我构建类，以便在我决定做更多事情时允许增量添加。

# 决策树

决策树是一种流行的机器学习模型，因为它们易于解释并且具有预测能力。然而，它们本身并不常用。相反，它们被用作被认为是最先进模型的集成方法的基础。决策树是通过递归地将数据集一分为二来构建的，直到分支是纯的或者满足停止标准。最后的节点称为叶节点，对应于一个输出类。其余节点表示任意输入要素和分割数据集的要素的值。为了预测新样本的类别，样本简单地沿着分裂逻辑一个节点一个节点地向下遍历树，直到到达叶节点。

```
 **def** split(self, feat, val, Xy):
        Xi_left = np.array([]).reshape(0,self.Xy.shape[1])
        Xi_right = np.array([]).reshape(0,self.Xy.shape[1])
        **for** i **in** Xy:
            *#print(i.shape)*
            **if** i[feat] <= val:
                Xi_left = np.vstack((Xi_left,i))
            **if** i[feat] > val:
                Xi_right = np.vstack((Xi_right,i))
        **return** Xi_left, Xi_right
```

树是贪婪的算法，这意味着在每次分裂时，做出最佳决策，希望得到的树将导致全局最大值。然而，这很少偶然发生，通常他们需要许多正则化超参数来减少过拟合。上面提到的停止标准就是这样的例子。最大深度是控制树可以向下生长的总步数的参数，最小样本数是控制发生分裂所需的样本数的参数。通过减少最大深度或增加最小样本数，可以控制树的大小。

任何给定特征的最佳分离由定义纯度的成本函数确定。最常见的是用于连续特征的基尼指数和用于分类特征的信息增益或熵。基尼指数通过所得两组的阶级构成来量化分裂，其中低分意味着接近完美的分裂，高分意味着两组的构成几乎相同。

```
**def** gini_score(groups,classes):
    n_samples = sum([len(group) **for** group **in** groups])
    gini = 0
    **for** group **in** groups:
        size = float(len(group))
        **if** size == 0:
            **continue**
        score = 0.0
        *#print(size)*
        **for** class_val **in** classes:
            *#print(group.shape)*
            p = (group[:,-1] == class_val).sum() / size
            score += p * p
        gini += (1.0 - score) * (size / n_samples)
        *#print(gini)*
    **return** gini
```

为了计算最佳分割，模型必须以各种可能的方式计算每个特征分割的基尼系数。例如，以各种可能的方式分割特征[x]上的样本[a，b，c，d，e]将导致[a]和[b，c，d，e]，[a，b]和[c，d，e]，[a，b，c]和[d，e]以及[a，b，c，d]和[e]。将对每个分割进行评分，并保存最低的基尼系数。那么对特征[y]和[z]也是如此。然后，所有特征中最低的基尼系数将被选为最佳分割，并创建一个节点。对每个节点的所得两组中的每一组重复该过程，直到遇到点点。

```
 **def** best_split(self, Xy):
        classes = np.unique(Xy[:,-1])
        best_feat = 999
        best_val = 999
        best_score = 999
        best_groups = **None**
        **for** feat **in** range(Xy.shape[1]-1):
            **for** i **in** Xy:
                groups = self.split(feat, i[feat], Xy)
                gini = self.gini_score(groups, classes)
                *#print('feat {}, valued < {}, scored {}'.format(feat,i[feat], gini))*
                **if** gini < best_score:
                    best_feat = feat
                    best_val = i[feat]
                    best_score = gini
                    best_groups = groups
        output = {}
        output['feat'] = best_feat
        output['val'] = best_val
        output['groups'] = best_groups
        **return** output
```

这个结构是通过使用递归函数快速创建的。只需评估基尼系数，在最佳分割点创建节点，保存结果数据集，然后重复。

```
 **def** terminal_node(self, group):
        classes, counts = np.unique(group[:,-1],return_counts=**True**)
        **return** classes[np.argmax(counts)]

    **def** split_branch(self, node, depth):
        left_node, right_node = node['groups']
        **del**(node['groups'])
        **if** **not** isinstance(left_node,np.ndarray) **or** **not** isinstance(right_node,np.ndarray):
            node['left'] = node['right'] = self.terminal_node(left_node + right_node)
            **return**
        **if** depth >= self.max_depth:
            node['left'] = self.terminal_node(left_node)
            node['right'] = self.terminal_node(right_node)
            **return**
        **if** len(left_node) <= self.min_num_sample:
            node['left'] = self.terminal_node(left_node)
        **else**:
            node['left'] = self.best_split(left_node)
            self.split_branch(node['left'], depth+1)
        **if** len(right_node) <= self.min_num_sample:
            node['right'] = self.terminal_node(right_node)
        **else**:
            node['right'] = self.best_split(right_node)
            self.split_branch(node['right'], depth+1)
```

## 决策树训练:

1.  基于所有要素上所有可能分割点的最低基尼系数分割输入数据
2.  基于上述分割创建了两个数据集
3.  同样，每个新数据集都是根据所有可能的最低基尼系数进行分割的
4.  在所有特征上分割点
5.  同样，从每个节点创建两个数据集
6.  依此类推，直到根据最大深度或最小样本数标准创建叶节点

## 决策树预测:

1.  接受新样本
2.  基于根节点的拆分标准评估样本
3.  前进到下一个节点
4.  基于该节点的拆分标准评估样本
5.  前进到下一个节点
6.  依此类推，直到到达叶节点
7.  基于叶节点的类确定预测

决策树要求每个新样本遍历训练数据的学习表示。然后将 n 个样本中的每一个附加到输出中。

```
 **def** predict_sample(self, node, sample):
        *#print(node)*
        **if** sample[node['feat']] < node['val']:
            **if** isinstance(node['left'],dict):
                **return** self.predict_sample(node['left'],sample)
            **else**:
                **return** node['left']
        **else**:
            **if** isinstance(node['right'],dict):
                **return** self.predict_sample(node['right'],sample)
            **else**:
                **return** node['right'] **def** predict(self, X_test):
        self.y_pred = np.array([])
        **for** i **in** X_test:
            *#print(i)*
            self.y_pred = np.append(self.y_pred,self.predict_sample(self.root,i))
        **return** self.y_pred
```

从头开始创建决策树算法教会了我很多东西。首先，我从来没有意识到分割搜索是多么详尽，直到我被迫编程。此外，分裂纯洁的概念现在对我来说也更清楚了。显而易见，当你构建决策树时，很容易过度拟合。只有通过创建像最大深度和最小样本数这样的超参数，树才有机会泛化。

在未来，我会跟进一个随机森林和 Kmeans 的解释，敬请期待！