# 机器学习算法可以帮助我们评估金融决策的风险

> 原文：<https://towardsdatascience.com/financial-data-analysis-80ba39149126?source=collection_archive---------12----------------------->

## 贷款资格预测:数据处理-1

![](img/6999a37544690c11871e3bdf888f9dd0.png)

# 介绍

金融机构/公司已经使用预测分析很长时间了。最近，由于计算资源的可用性和机器学习方面的大量研究，使得更好的数据分析从而更好的预测成为可能。在这一系列文章中，我将解释如何创建一个预测贷款模型来识别更有可能被取消的不良申请人。在一步一步的过程中，我展示了如何处理原始数据，清除不必要的部分，选择相关的特性，执行探索性的数据分析，最后建立一个模型。

作为一个例子，我使用 Lending club loan [数据集](https://drive.google.com/drive/folders/1SrC90L7273QT2r3d6UBFmifwxo5qVqlY?usp=sharing)。Lending Club 是世界上最大的连接借款人和投资者的在线市场。贷款的一个不可避免的结果是借款人违约。本教程的想法是创建一个预测模型，确定哪些申请人的贷款风险相对较高。为此，我将整个系列分为四个部分，如下所示:

*   [**数据处理-1**](https://medium.com/@sabber/financial-data-analysis-80ba39149126) :在这第一部分，我展示了如何清理和移除不必要的特征。数据处理非常耗时，但是更好的数据会产生更好的模型。因此，为了准备更好的数据，需要进行仔细和非常详细的检查。我将展示如何识别常量要素、重复要素、重复行和具有大量缺失值的要素。
*   [**数据处理-2**](https://medium.com/@sabber/financial-data-analysis-bf4b5e78c45c) :在这一部分，我手动浏览从第 1 部分选择的每个特征。这是最耗时的部分，但为了更好的模型，这是值得的。
*   [**EDA**](https://medium.com/@sabber/financial-data-analysis-2f86b1341e6e) :在这一部分中，我对第 1 部分和第 2 部分中选择的特性做了一些探索性的数据分析(EDA)。一个好的 EDA 需要更好的领域知识。我们需要花一些宝贵的时间来找出这些特征之间的关系。
*   [**创建模型**](https://medium.com/@sabber/financial-data-analysis-51e7275d0ae) :最后，在这最后但不是最后的部分，我创建模型。创建模型也不是一件容易的事情。这也是一个迭代的过程。我将展示如何从一个简单的模型开始，然后慢慢增加复杂性以获得更好的性能。

好了，让我们开始第 1 部分:数据处理、清理和特征选择。在此之前，你需要[从这个链接](https://drive.google.com/drive/folders/1SrC90L7273QT2r3d6UBFmifwxo5qVqlY?usp=sharing)下载数据集来和我一起编码。

**数据处理-1**

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings("ignore")
```

在这个项目中，我使用了三年的数据集(2014 年、2015 年和 2017 年(第一季度))并存储在五个独立的 CSV 文件中。让我们先读文件:

```
df1 = pd.read_csv(‘./data/2017Q1.csv’, skiprows=[0])
df2 = pd.read_csv(‘./data/2017Q2.csv’, skiprows=[0])
df3 = pd.read_csv(‘./data/2017Q3.csv’, skiprows=[0])
df4 = pd.read_csv(‘./data/2014.csv’, skiprows=[0])
df5 = pd.read_csv(‘./data/2015.csv’, skiprows=[0])
```

由于数据存储在不同的文件中，我们必须确保每个文件中有相同数量的特征。我们可以使用下面的代码片段进行检查:

```
columns = np.dstack((list(df1.columns), list(df2.columns), list(df3.columns), list(df4.columns), list(df5.columns)))
coldf = pd.DataFrame(columns[0])
```

上面的代码是不言自明的，我们首先提取列名，并使用 Numpy 'dstack '对象将它们堆叠在一起。如果你看看 Github 上的 Jupyter-notebook，你会发现它们是一样的。这对我们有好处。我们可以进入下一步了。是时候检查数据的形状了:

```
df = pd.concat([df1, df2, df3, df4, df5])
df.shape(981665, 151)
```

我们看到大约有一百万个例子，每个例子有 151 个特征，包括目标变量。让我们看一下特性名称来熟悉数据。必须了解领域，尤其是特性与目标变量之间关系的细节。一夜之间学会并不容易，这就是为什么需要花几天或一周的时间来熟悉数据，然后再进行进一步的详细分析。让我们看看功能名称:

```
**print(list(df.columns))**['id', 'member_id', 'loan_amnt', 'funded_amnt', 'funded_amnt_inv', 'term', 'int_rate', 'installment', 'grade', 'sub_grade', 'emp_title', 'emp_length', 'home_ownership', 'annual_inc', 'verification_status', 'issue_d', 'loan_status', 'pymnt_plan', 'url', 'desc', 'purpose', 'title', 'zip_code', 'addr_state', 'dti', 'delinq_2yrs', 'earliest_cr_line', 'fico_range_low', 'fico_range_high', 'inq_last_6mths', 'mths_since_last_delinq', 'mths_since_last_record', 'open_acc', 'pub_rec', 'revol_bal', 'revol_util', 'total_acc', 'initial_list_status', 'out_prncp', 'out_prncp_inv', 'total_pymnt', 'total_pymnt_inv', 'total_rec_prncp', 'total_rec_int', 'total_rec_late_fee', 'recoveries', 'collection_recovery_fee', 'last_pymnt_d', 'last_pymnt_amnt', 'next_pymnt_d', 'last_credit_pull_d', 'last_fico_range_high', 'last_fico_range_low', 'collections_12_mths_ex_med', 'mths_since_last_major_derog', 'policy_code', 'application_type', 'annual_inc_joint', 'dti_joint', 'verification_status_joint', 'acc_now_delinq', 'tot_coll_amt', 'tot_cur_bal', 'open_acc_6m', 'open_act_il', 'open_il_12m', 'open_il_24m', 'mths_since_rcnt_il', 'total_bal_il', 'il_util', 'open_rv_12m', 'open_rv_24m', 'max_bal_bc', 'all_util', 'total_rev_hi_lim', 'inq_fi', 'total_cu_tl', 'inq_last_12m', 'acc_open_past_24mths', 'avg_cur_bal', 'bc_open_to_buy', 'bc_util', 'chargeoff_within_12_mths', 'delinq_amnt', 'mo_sin_old_il_acct', 'mo_sin_old_rev_tl_op', 'mo_sin_rcnt_rev_tl_op', 'mo_sin_rcnt_tl', 'mort_acc', 'mths_since_recent_bc', 'mths_since_recent_bc_dlq', 'mths_since_recent_inq', 'mths_since_recent_revol_delinq', 'num_accts_ever_120_pd', 'num_actv_bc_tl', 'num_actv_rev_tl', 'num_bc_sats', 'num_bc_tl', 'num_il_tl', 'num_op_rev_tl', 'num_rev_accts', 'num_rev_tl_bal_gt_0', 'num_sats', 'num_tl_120dpd_2m', 'num_tl_30dpd', 'num_tl_90g_dpd_24m', 'num_tl_op_past_12m', 'pct_tl_nvr_dlq', 'percent_bc_gt_75', 'pub_rec_bankruptcies', 'tax_liens', 'tot_hi_cred_lim', 'total_bal_ex_mort', 'total_bc_limit', 'total_il_high_credit_limit', 'revol_bal_joint', 'sec_app_fico_range_low', 'sec_app_fico_range_high', 'sec_app_earliest_cr_line', 'sec_app_inq_last_6mths', 'sec_app_mort_acc', 'sec_app_open_acc', 'sec_app_revol_util', 'sec_app_open_act_il', 'sec_app_num_rev_accts', 'sec_app_chargeoff_within_12_mths', 'sec_app_collections_12_mths_ex_med', 'sec_app_mths_since_last_major_derog', 'hardship_flag', 'hardship_type', 'hardship_reason', 'hardship_status', 'deferral_term', 'hardship_amount', 'hardship_start_date', 'hardship_end_date', 'payment_plan_start_date', 'hardship_length', 'hardship_dpd', 'hardship_loan_status', 'orig_projected_additional_accrued_interest', 'hardship_payoff_balance_amount', 'hardship_last_payment_amount', 'disbursement_method', 'debt_settlement_flag', 'debt_settlement_flag_date', 'settlement_status', 'settlement_date', 'settlement_amount', 'settlement_percentage', 'settlement_term']
```

看上面的特征，可能先显得吓人。但是我们将浏览每个功能，然后选择相关的功能。让我们从目标特性“loan_status”开始

```
df.loan_status.value_counts()Current               500937
Fully Paid            358629
Charged Off            99099
Late (31-120 days)     13203
In Grace Period         6337
Late (16-30 days)       3414
Default                   36
Name: loan_status, dtype: int64
```

我们看到有七种类型的贷款状态。然而，在本教程中，我们感兴趣的是两类人:1)全额支付的人:支付贷款利息的人；2)冲销的人:无法支付并最终冲销的人。因此，我们为这两个类选择数据集:

```
df = df.loc[(df['loan_status'].isin(['Fully Paid', 'Charged Off']))]df.shape
(457728, 151)
```

观察形状，我们会发现现在的数据点是原始数据的一半，但特征数量相同。在手动处理和清理之前，让我们先做一些常规的数据处理步骤:

*   移除与 85%以上缺失值相关的要素
*   移除恒定特征
*   移除重复要素
*   删除重复的行
*   移除高度共线的特征(在第 3 部分 EDA 中)

好了，让我们从典型的数据处理开始:

1.  **删除与 90%缺失值相关的特性:**在下面的代码中，我首先使用 pandas 的内置方法‘is null()’来查找与缺失值相关的行。然后我将它们相加，得到每个特征的数量。最后，我根据缺失值的数量对要素进行排序，并创建一个数据框以供进一步分析。

在上面的结果中，我们看到有 53 个特征有 400000 个缺失值。我用熊猫滴法去掉这 53 个特征。请注意，在此函数中，我将“inplace”选项设置为 True，“这将从原始数据帧 **df** 中移除这些要素，而不返回任何内容。

**2。移除恒定特征:**在这一步，我们移除具有单一唯一值的特征。与一个唯一值相关联的要素无法帮助模型很好地进行概化，因为其方差为零。基于树的模型不能利用这些类型的特征，因为模型不能分割这些特征。识别具有单一唯一值的要素相对简单:

在上面的代码中，我创建了一个函数“find_constant_features”来标识常量特性。该函数遍历每个要素，并查看其唯一值是否少于两个。如果是，这些特征被添加到恒定特征列表中。我们也可以通过方差或标准差来找出常量特征。如果该特征的方差或标准差为零，我们就可以确定该特征只有一个唯一值。print 语句显示五个特性有一个唯一的值。所以我们使用“inplace”选项 true 来删除它们。

**3。去除重复特征:**重复特征是在多个同名/异名特征中具有相同值的特征。为了找出重复的特性，我从这个[堆栈溢出链接](https://stackoverflow.com/questions/14984119/python-pandas-remove-duplicate-columns)中借用了以下代码:

我们只看到一个似乎是复制的特征。我不打算删除该功能，而是等到我们在下一部分做 EDA。

**4。删除重复的行:**在这一步中，我们删除所有重复的行。我使用 pandas 内置的“drop_duplicates(inplace= True)”方法来执行这个操作:

```
df.drop_duplicates(inplace= True)
```

以上四个处理是基本的，任何数据科学项目我们都需要做。让我们看看所有这些步骤之后的数据形状:

```
df.shape
(457728, 93)
```

我们看到，执行上述步骤后，我们有 93 个特征。

在本教程的[下一部分](https://medium.com/@sabber/financial-data-analysis-bf4b5e78c45c)中，我将浏览每个特性，然后进行清理，并在必要时将其移除。同时，如果你对这部分有任何问题，请在下面写下你的意见。你可以联系我:

```
Email: sabbers@gmail.com
LinkedIn: [https://www.linkedin.com/in/sabber-ahamed/](https://www.linkedin.com/in/sabber-ahamed/)
Github: [https://github.com/msahamed](https://github.com/msahamed)
Medium: [https://medium.com/@sabber/](https://medium.com/@sabber/)
```