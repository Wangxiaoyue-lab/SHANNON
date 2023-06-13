 SciPy 是一款开源、方便、专为科学和工程设计的 Python 工具包.它包括统计,优化,整合,线性代数模块,傅里叶变换,信号和图像处理,常微分方程求解器等等。

SciPy 是建立在 Python 的 NumPy 扩展之上的数学算法和便利函数的集合。

# scipy.stats

```python
from scipy import stats
```

## 描述性统计

均值、方差、标准差、偏度、峰度等基本统计量：stats.describe()

频数分布和直方图：stats.relfreq() stats.cumfreq() stats.histogram()

分位数和百分位数：stats.scoreatpercentile() stats.percentileofscore()

相关系数和协方差矩阵：stats.pearsonr() stats.spearmanr() stats.kendalltau() stats.cov()

https://blog.csdn.net/qq_39377957/article/details/129390620

## 概率分布

常见的连续型概率分布：正态分布(stats.norm) 指数分布(stats.expon) 卡方分布(stats.chi2) t分布(stats.t) F分布(stats.f) 等

常见的离散型概率分布：伯努利分布(stats.bernoulli) 泊松分布(stats.poisson) 几何分布(stats.geom) 超几何分布(stats.hypergeom) 等

概率密度函数、累积密度函数、生存函数等概率函数：pdf() cdf() sf()

pdf和cdf

```
stats**.norm(0,1).cdf(1.96)**
```

## 参数估计和假设检验

点估计和区间估计：stats.ttest_1samp() stats.ttest_ind() stats.ttest_rel() stats.chisquare()stats.f_oneway() stats.wilcoxon() 等

stats.ttest_1samp()是一个用于进行单样本t检验的函数，它可以检验一个样本的均值是否等于某个指定的值1。
stats.ttest_ind()是一个用于进行独立样本t检验的函数，它可以检验两个独立样本的均值是否相等2。
stats.ttest_rel()是一个用于进行相关样本t检验的函数，它可以检验两个相关样本的均值是否相等3。
stats.chisquare()是一个用于进行卡方检验的函数，它可以检验一个或多个观察频数是否符合期望频数或者两组观察频数是否有显著差异。
stats.f_oneway()是一个用于进行单因素方差分析（ANOVA）的函数，它可以检验多个（至少两个）组别之间的均值是否有显著差异。
stats.wilcoxon()是一个用于进行Wilcoxon符号秩检验或Wilcoxon秩和检验的函数，它可以在不满足正态分布假设时，对两个相关样本或独立样本进行非参数检验

原文链接：https://blog.csdn.net/qq_39377957/article/details/129390620

# linalg 线性代数

# spatial 空间计数

# sparse 稀疏

# cluster 聚类
