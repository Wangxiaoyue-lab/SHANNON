# Methods

| value | tool | time | platform | link | description |  |
| ----- | ---- | ---- | -------- | ---- | ----------- | - |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |
|       |      |      |          |      |             |  |

# Benchmark

# Details

## limma


出现批次效应可以转化为连续数据，sva去除后，应用线性回归函数进行差异分析



## DEseq2

**removeBatchEffect**这个函数用于进行聚类或无监督分析之前，移除与杂交时间或其他技术变异相关的批次效应。 它是针对芯片设计的，因此不要直接使用read counts，数据需要经过一定的标准化操作，如log转化。 removeBatchEffect只用于衔接聚类、PCA等可视化展示，不要在线性建模之前使用。

DEseq2想要差异分析时候考虑批次效应，可以是作为协变量加入回归方程。也可以是combat-seq处理后输入


# edgeR


# Comments
