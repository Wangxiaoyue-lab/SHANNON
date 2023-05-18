# Methods

| value | tool        | time | platform | link                                       | description          |  |
| ----- | ----------- | ---- | -------- | ------------------------------------------ | -------------------- | - |
| ****  | infercnv    |      | R        | https://github.com/broadinstitute/infercnv | 基因窗口的平滑平均量 |  |
| ***   | copykat     | 2021 | R        |                                            | 贝叶斯分段方法       |  |
|       | CaSpER      | 2020 | R        | https://github.com/akdess/CaSpER           | 多标度信号处理       |  |
|       | HoneyBADGER | 2018 | R        |                                            |                      |  |
|       |             |      |          |                                            |                      |  |
|       |             |      |          |                                            |                      |  |
|       |             |      |          |                                            |                      |  |

# Benchmark

Inferring copy number variation from gene expression data: methods, comparisons, and applications to oncology

https://www.biorxiv.org/content/10.1101/2021.10.18.463991v1


copykat的正常细胞不能太少。而且copykat假设肿瘤细胞的非整倍性比较强，如果是二倍体肿瘤细胞可能无法识别

casper需要bam文件，输出的cnv是离散的，而infercnv和copycat输出连续的cnv值


copykat归一化到-1到1之间，正常细胞是0

infercnv归一化到0到2之间，正常细胞是1  


infercnv和copykat最好都提供正常样本作为参考，否则性能下降。


患者样本的cnv变异性比方法推断变异性要高很多。 

# Details

## infercnv

- 
- input

  - 表达矩阵
    - 列为细胞，行为基因
  - 细胞注释文件
    - 第一列是细胞名，第二列是细胞类型名
    - 无列名和行名
  - 基因坐标文件
    - symbol、chr、start、end
    - 必须排序
- script
- output

  - picture

    - infercnv.preliminary.png : 初步的inferCNV展示结果（未经去噪或HMM预测）
    - infercnv.png : 最终inferCNV产生的去噪后的热图.
      - 其中染色体区域扩增显示为红色块，而染色体区域缺失显示为蓝色块
  - infercnv.references.txt : 正常细胞矩阵.
  - infercnv.observations.txt : 肿瘤细胞矩阵.
  - infercnv.observation_groupings.txt : 肿瘤细胞聚类后的分组关系.
  - infercnv.observations_dendrogram.txt : 与热图匹配的肿瘤细胞的newick格式的树状图
- details

  - infercnv 去噪和cnv预测是独立的
  - 去噪就是去掉reference细胞的信号，让背景干净。是论文里的热图
  - cnv预测是输出具体的cnv位置列表
    - 需要考虑是模式选择samples还是subcluster
    - samples就是按sample进行划分，subcluster会按亚克隆进行划分
      - subcluster的cnv模式会更细节复杂，考虑样本内部的亚克隆
    - cnv预测的热图只标记大块颜色区域，意义不大

# Comments

- 不是所有的肿瘤都存在CNV。儿童肿瘤和血液肿瘤中基本没有copy number event，所以是不适合用这些方法（copyKAT或inferCNV）来寻找肿瘤细胞的
- cnv有三种

  * Copy gain
  * Copy loss
  * Loss of heterozygosity

* infercnv和copykat表现比较好
  * https://statbiomed.github.io/SingleCell-Workshop-2021/CNV-analysis.html#application-on-tnbc1
  * CaSpER和HoneyBADGER会使用bam文件call出maf

- infercnv参考

  - 源码与原理解析https://www.jianshu.com/p/70f7a168fe62
- HoneyBADGER参考

  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6071640/
  - https://jef.works/HoneyBADGER/
- CaSpER

  - 需要bam生成maf
- copykat

  - https://www.nature.com/articles/s41587-020-00795-2
  -
