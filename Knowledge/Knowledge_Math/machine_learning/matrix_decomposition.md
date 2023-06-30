# NMF

**定义：**

任意给定的一个非负矩阵V，NMF算法能够找到一个非负矩阵W和一个非负矩阵H，使得非负矩阵V≈W*H 成立 ，从而将一个非负的矩阵分解为左右两个非负矩阵的乘积。

**应用**
**[非负矩阵分解在单细胞数据分析中的主要应用：](https://www.jianshu.com/p/1cf65bd9aae7)**

1. 非负矩阵分解在做亚群细分和提取feature的时候是一个非常有效的工具。
2. 多样本整合，LIGER中用的是非负矩阵分解
3. CellChat 做细胞通讯也用到了非负矩阵
4. 空间转录组去卷积工具SPOTlight也用到了NMF


## cNMF



## **Joint non-negative matrix factorization**
[Biomarker discovery by integrated joint non-negative matrix factorization and pathway signature analyses](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6021419/)
多组学数据中挖掘具有生物学意义的生物标志物

JNMF可用于从癌症患者的mRNA表达，microRNA表达和DNA甲基化数据中检测常见的簇（辅模块）