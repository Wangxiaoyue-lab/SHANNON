# AnnData

## 介绍

`AnnData`专为类似矩阵的数据而设计

例如，在scRNA-seq数据中，每行对应于具有条形码的细胞，每列对应于具有基因ID的基因。此外，对于每个细胞和每个基因，我们可能有额外的元数据，例如（1）每个细胞的供体信息，或（2）每个基因的替代基因符号。最后，我们可能还有其他非结构化元数据，如调色板用于绘图。在不涉及每个基于Python的花哨数据结构的情况下，我们认为今天仍然没有其他替代方案真正存在：

## 标准构成

scanpy和seurat的矩阵方向不一样

scanpy是 行为细胞 列为特征

### X|表达矩阵  行细胞 列特征

```python
adata[:3, 'A'].X

adata[:13, :6].X
```

但是其实按上面那样是看不到的，因为是稀疏矩阵

```
print(adata[:3, 'A'].X)
```

### layers| 处理后表达矩阵

```python
adata.layers['log_transformed']=np.log1p(adata.X)
```

最后，原始核心数据可能有不同的形式，可能是规范化的，也可能不是。这些数据可以存储在AnnData的不同层中。例如，让我们对原始数据进行日志转换，并将其存储在一个层中:

### obs| 观察/细胞注释

 一维观测的注释，是数据框类型的。通过.运算符访问

```python
cho_pri.obs
```

细胞元数据

```python
cho_pri.obs.columns
```

元数据名

```
cho_pri.obs['NK']
```

 访问其中的一个属性，就通过普通的dataframe来访问：

### var| 特征注释

 一维的针对特征的注释，返回数据框类型。

```python
cho_pri.var
```

### uns|数据集参数标注  任何非结构化元数据

无结构的标注，有序字典。

```python
cho_pri.uns
cho_pri.uns['louvain']
```

这个uns的部分不是针对行/列的，而是针对行和列标注的参数的（暂时这么理解），在上述中pbmc的obs中有louvain观测，那么在uns中就是运行louvain算法的参数。是以哈希形式存储的。

它允许任何非结构化元数据。这可以是任何东西，例如包含一些对我们的数据分析有用的一般信息的列表或字典。尝试仅将此插槽用于无法有效存储在其他插槽中的数据。

### obsm|细胞的多维注释/降维

 对观测的多维的注释，它是可变的ndarray，长度为n_obs，维度为2至多维。这里的m指的就是multi-dim多个维度的，和obs一个维度可以都放在一个数据框下不同。

```python
cho_pri.obsm
```

```python
cho_pri.obsm['X_pca']
```

### varm|特征的多维注释

 描述特征的，与obsm相对。

```python
cho_pri.varm
```

```python
cho_pri.varm['PCs']
```

### obsp|观察的配对注释

 针对观测的配对的注释，前两维都是n_obs，
  比如点与点之间的距离和连通性。

```python
cho_pri.obsp
```

```python
cho_pri.obsp['distances']
```

```python
cho_pri.obsp['connectivities']
```

## 创建对象

### 导入包和函数

```python
import numpy as np 
import pandas as pd 
import anndata as ad 
import scanpy as sc
```

### 拓展工具

```python
import scanpy.external as sce
```

[External API — Scanpy 1.9.1 documentation](https://scanpy.readthedocs.io/en/stable/external.html#module-scanpy.external)

### 产生虚拟表达矩阵

#### 设置obs

```python
# 设置观测样本的数量
n_obs=1000
# obs用于保存观测量的信息
obs=pd.DataFrame()

obs['time']=np.random.choice(['day1','day2','day4','day8'],size=n_obs)

```

#### 设置feature

```python
from string import ascii_uppercase


# 设置特征名var_names
print(ascii_uppercase) # ABCDEFGHIJKLMNOPQRSTUVWXYZ
var_names=[i*letter for i in range(1,10) for letter in ascii_uppercase]
print(var_names)
# ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', ......, 'X', 'Y', 'Z',
# ......
# 'AAAAAAAAA', 'BBBBBBBBB', 'CCCCCCCCC', ......, 'YYYYYYYYY', 'ZZZZZZZZZ']

# 特征数量
n_vars=len(var_names) # 234

# 将特征定义到 Dataframe
var=pd.DataFrame(index=var_names)
print(var.head()) # 现在var没有columns(列索引), 只有index(行索引)

```

#### 创建矩阵

```python
# 创建数据矩阵 adata.X
X=np.arange(n_obs*n_vars).reshape(n_obs,n_vars)
```

### 创建anndata对象

```python
adata=ad.AnnData(X,obs=obs,var=var,dtype='int32')
```

指定表达矩阵   obs注释和var注释

### 添加注释

```python
adata_subset.obs['foo'] = range(5)
```

### 稀疏矩阵和array的转化

numpy矩阵变成稀疏矩阵

```python
from scipy.sparse import csr_matrix
sparse_A = csr_matrix(A)
```

稀疏矩阵变成numpy

```python
A = sparse_A.toarray()
```

# scanpy

## 命名

pp是preprocessing
pl是plotting
tl是tools

log1p = log（x+1）

## 1. 基本设置

```python
sc.settings.verbosity = 3             # verbosity: errors (0), warnings (1), info (2), hints (3)
sc.logging.print_header()
sc.settings.set_figure_params(dpi=80, facecolor='white')
```

```python
import os 
os.getcwd()  ##查看当前路径
os.chdir('./Integrate/ingest') ##修改路径

```

## 2. 读取

h5ad

```python
filePath
cho_pri=sc.read_h5ad(filePath)


Myadata=sc.read('./test.h5ad')

```

10Xgenomics的.h5

```python
filePath
sc.read_10x_h5(filePath)
```

sc.read_10x_mtx文件夹读取

```python
adata = sc.read_10x_mtx(
    'data/filtered_gene_bc_matrices/hg19/',  # the directory with the `.mtx` file
    var_names='gene_symbols',                # use gene symbols for the variable names (variables-axis index)
    cache=True)                              # write a cache file for faster subsequent reading

```

空间转录组

```python
read_visium(path[, genome, count_file, ...])

#Read 10x-Genomics-formatted visum dataset.
```

```
read_umi_tools(filename[, dtype])

Read a gzipped condensed count matrix from umi_tools.

```

## 3. 存储

### 示例数据集 pbmc3k

在线下载

```
pbmc = sc.datasets.pbmc3k()
adata = sc.dataset.paul15()
adata = sc.datasets.pbmc3k_processed()
adata = sc.datasets.pbmc68k_reduced()

```

```
datasets.blobs([n_variables, n_centers, ...])
```

Gaussian Blobs.

```
datasets.ebi_expression_atlas(accession, *)
```

Load a dataset from the EBI Single Cell Expression Atlas

```
datasets.krumsiek11()
```

Simulated myeloid progenitors [Krumsiek11].模拟骨髓祖细胞

```
datasets.moignard15()
```

Hematopoiesis in early mouse embryos [Moignard15].小鼠早期胚胎的造血

```
datasets.pbmc3k()
```

3k PBMCs from 10x Genomics.

```
datasets.pbmc3k_processed()
```

Processed 3k PBMCs from 10x Genomics.

```
datasets.pbmc68k_reduced()
```

Subsampled and processed 68k PBMCs.

```
datasets.paul15()
```

Development of Myeloid Progenitors [Paul15].

```
datasets.toggleswitch()
```

Simulated toggleswitch.

```
datasets.visium_sge([sample_id, ...])
```

Processed Visium Spatial Gene Expression data from 10x Genomics.

### 暂存为h5ad

```
adata.write("pca_results.h5ad")
```

## 4. 查看

### 4.1 metadata注释

见上面

### 4.2 查询基因的biomart注释

基因注释

```
queries.biomart_annotations(org, attrs, *[, ...])
```

Retrieve gene annotations from ensembl biomart.

查询基因坐标

```
queries.gene_coordinates(org, gene_name, *)
```

Retrieve gene coordinates for specific organism through BioMart.

```
queries.mitochondrial_genes(org, *[, ...])
```

Mitochondrial gene symbols for specific organism through BioMart.

```
queries.enrich(container, *[, org, ...])
```

Get enrichment for DE results.

## 5. 量度

Collections of useful measurements for evaluating results.

### metadata混淆矩阵

```python
import scanpy as sc; 
import seaborn as sns
pbmc = sc.datasets.pbmc68k_reduced()
cmtx = sc.metrics.confusion_matrix("bulk_labels", "louvain", pbmc.obs)
sns.heatmap(cmtx)
```

[`metrics.confusion_matrix`](https://scanpy.readthedocs.io/en/stable/generated/scanpy.metrics.confusion_matrix.html#scanpy.metrics.confusion_matrix "scanpy.metrics.confusion_matrix")(orig, new[, data, ...])

Given an original and new set of labels, create a labelled confusion matrix.

### 细胞的距离度量/构建knn图

Represent data as a neighborhood structure, usually a knn graph.

```python
sc.pp.eighbors(adata[, n_dcs, neighbors_key])
```

Data represented as graph of nearest neighbors.

- distances表示细胞间距离 取决于metric参数

'cityblock', 'cosine', 'euclidean', 'l1', 'l2', 'manhattan'

'braycurtis', 'canberra', 'chebyshev', 'correlation', 'dice', 'hamming', 'jaccard', 'kulsinski', 'mahalanobis', 'minkowski', 'rogerstanimoto', 'russellrao', 'seuclidean', 'sokalmichener', 'sokalsneath', 'sqeuclidean', 'yule'

默认是欧几里得

- connectivitives表示细胞间连通性  取决于method参数

'umap', 'gauss', 'rapids'

默认是umap

- knn参数

```
knn=True
```

可以关闭knn

- key_added= 'my'

给输出矩阵添加前缀

```
key_added
```

### 空间自相关量度Geary s C

```python
import scanpy as sc, numpy as np
pbmc = sc.datasets.pbmc68k_processed()
pc_c = sc.metrics.gearys_c(pbmc, obsm="X_pca")

```

[`metrics.gearys_c`](https://scanpy.readthedocs.io/en/stable/generated/scanpy.metrics.gearys_c.html#scanpy.metrics.gearys_c "scanpy.metrics.gearys_c")()

Calculate [Geary&#39;s C](https://en.wikipedia.org/wiki/Geary's_C), as used by [VISION](https://doi.org/10.1038/s41467-019-12235-0).
Geary s C是图上某个测度的自相关测度。这可以是相邻细胞之间的度量是否相关。数值越低表示相关性越大。

### 空间自相关量度Moran's I

```python
import scanpy as sc, numpy as np

pbmc = sc.datasets.pbmc68k_processed()
pc_c = sc.metrics.morans_i(pbmc, obsm="X_pca")

```

[`metrics.morans_i`](https://scanpy.readthedocs.io/en/stable/generated/scanpy.metrics.morans_i.html#scanpy.metrics.morans_i "scanpy.metrics.morans_i")()

Calculate Moran’s I Global Autocorrelation Statistic.
Moran 's I是一个全局自相关统计量，用于图上的某种度量。在空间数据分析中，它通常用于评估二维网格上的自相关性。它与Geary的C语言密切相关，但并不完全相同。更多信息可以在这里找到。

## 6. 提取

### 提取某一layer变成dataframe

```python
sc_obj.to_df[layer="counts"]

```

### 提取obs注释

Return values for observations in adata.

```
get.obs_df(adata[, keys, obsm_keys, layer, ...])
```

### 提取var注释

Return values for observations in adata.

```
get.var_df(adata[, keys, varm_keys, layer])
```

### rank_genes_groups()

scanpy.tl.rank_genes_groups() results in the form of a DataFrame.

```
get.rank_genes_groups_df(adata, group, *[, ...])
```

### 取细胞子集

Subsample to a fraction of the number of observations.

```
pp.subsample(data[, fraction, n_obs, ...])
```

### 取count矩阵子集

Downsample counts from count matrix.

```
pp.downsample_counts(adata[, ...])
```

这个应该是人为构建稀疏性？？

## 7. 合并

```python
#合并两个数据集
adata_concat = adata_ref.concatenate(adata, batch_categories=['ref', 'new']) 


#合并多个数据集
batch_categories = ['A', 'B', 'C'] 
# 指定每个输入对象所属的批次
adata_merged = adata_list[0].concatenate(*adata_list[1:], batch_categories=batch_categories)
#创建一个新的AnnData对象adata_merged，
#它包含了来自列表中所有输入对象的数据。
#此外，它还会在观察值注释（即adata_merged.obs）中添加一个名为batch的列，
#用于存储每个观察值所属的批次。
```

## 8. 质控

### 保证基因名/列名不重复

```python
cho_pri.var_names_make_unique()
```

### 画出表达最多的基因

```python
sc.pl.highest_expr_genes(adata, n_top=20 )
```

### 基本过滤(最小基因数/细胞数)

```python
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)
```

### 计算指标

#### n_counts、n_features

```R
adata.obs['n_counts'] = adata.X.sum(axis=1).A1

#adata.obs['n_features'] = adata.X[adata.X>0].count(axis=1).A1
???


```

>>> type(adata.X.sum(axis=1))
>>> <class 'numpy.matrix'>
>>> type(adata.X.sum(axis=1).A1)
>>> <class 'numpy.ndarray'>
>>>
>>

#### 线粒体、核糖体

```python
mito_genes = adata.var_names.str.startswith('MT-')
##输出的是bool值，即基因是否是线粒体基因

adata.obs['percent_mito'] = np.sum(adata[:, mito_genes].X, axis=1).A1 / np.sum(adata.X, axis=1).A1

```

#### 计算qc量度

```
pp.calculate_qc_metrics(adata, *[, ...])


```

Calculate quality control metrics.

```python
import scanpy as sc
import seaborn as sns

pbmc = sc.datasets.pbmc3k()
pbmc.var["mito"] = pbmc.var_names.str.startswith("MT-")
#获得每个基因是否是线粒体基因



###计算各种qc指标
sc.pp.calculate_qc_metrics(pbmc, qc_vars=["mito"], inplace=True)

```


```
sns.jointplot(
    data=pbmc.obs,
    x="log1p_total_counts",
    y="log1p_n_genes_by_counts",
    kind="hex",
)

```


```
sns.histplot(pbmc.obs["pct_counts_mito"])
```

 
pct好像是百分比?

#### 细胞周期

```
tl.score_genes_cell_cycle(adata, s_genes, ...)
```

Score cell cycle genes

#### 基因集打分

```
tl.score_genes(adata, gene_list[, ...])
```

Score a set of genes

The score is the `<mark class="hltr-red">` average expression `</mark>` of a set of genes subtracted with the average expression of a reference set of genes. The reference set is randomly sampled from the gene_pool for each binned expression value.

This reproduces the approach in Seurat [Satija15] and has been implemented for Scanpy by Davide Cittaro.

#### 基因对评估

```
tl.sandbag(adata[, annotation, fraction, ...])

#Calculate marker pairs of genes.
```

根据基因计数矩阵和已知阶段的注释，计算作为每个阶段标记对的基因对。这再现了[Fechtner18]实现中[Scialdone15]的方法。

[类似SCRAN的细胞周期打分，具体没看]

```
tl.cyclone(adata[, marker_pairs, ...])
```

Assigns scores and predicted class to observations [Scialdone15] [Fechtner18].

### 绘制质控图

```python
sc.pl.violin(adata, ['n_genes', 'n_counts', 'percent_mito'], jitter=0.4, multi_panel=True)
```

![[附件/Pasted image 20221014235604.png]]

### 绘制相关关系图

```
sc.pl.scatter(adata, x='n_counts', y='percent_mito')
sc.pl.scatter(adata, x='n_counts', y='n_genes')
```

![[附件/Pasted image 20221014232436.png]]

### 真正质控

```python
adata = adata[adata.obs['n_genes'] < 4000, :]
adata = adata[adata.obs['percent_mito'] < 0.3, :]
```

## 9 解复用与双细胞

Sample demultiplexing, Doublet detection

### Scrublet 预测双细胞

```
pp.scrublet(adata[, adata_sim, batch_key, ...])
```

Predict doublets using Scrublet [Wolock19].

### 模拟双细胞

```
pp.scrublet_simulate_doublets(adata[, ...])
```

Simulate doublets by adding the counts of random observed transcriptome pairs.

### 直方图展示

```
pl.scrublet_score_distribution(adata[, ...])
```

Plot histogram of doublet scores for observed transcriptomes and simulated doublets.
观察到的转录组和模拟的 doublet 的 doublet 得分的直方图。

### HashSolo样本解复用

```
pp.hashsolo(adata, cell_hashing_columns[, ...])
```

Probabilistic demultiplexing of cell hashing data using HashSolo [Bernstein20].
使用HashSolo对细胞 hash数据进行概率解复用

## 10 插补

### DCA

```
pp.dca(adata[, mode, ae_type, ...])

#Deep count autoencoder [Eraslan18].

```

### MAGIC

```

pp.magic(adata[, name_list, knn, decay, ...])

#Markov Affinity-based Graph Imputation of Cells (MAGIC) API [vanDijk18].



```

## 11 预处理

### 标准化

```python
sc.pp.normalize_per_cell(adata, counts_per_cell_after=1e4)


sc.pp.normalize_total(adata, target_sum=1e4, inplace=True)
```

### 对数化

```
sc.pp.log1p(adata)
```

log1p = log（x+1）

### 计算高变基因

```
sc.pp.highly_variable_genes(data, 
							n_top_genes=None, 
							min_disp=0.5, 
							max_disp=inf, 
							min_mean=0.0125, 
							max_mean=3)

```

该函数用于确定高变基因；
常用参数说明：

- `data`：AnnData Matrix，行对应细胞列对应基因
- `n_top_genes`：要保留的高变基因的数量

`<mark class="hltr-red">`高变异基因存在var里 `</mark>`  应该是bool格式

可以将保守的基因去除，留下差异表达的基因用于后续分析

```
adata = adata[:, adata.var['highly_variable']]

```

#### 绘制基因标准差-平均值图

```
pl.highly_variable_genes(adata_or_result[, ...])
```

Plot dispersions or normalized variance versus means for genes.

#### 绘制变异度-均值图

```
pl.filter_genes_dispersion(result[, log, ...])
```

Plot dispersions versus means for genes.

高变异基因就是highly variable features（HVGs），就是在细胞与细胞间进行比较，选择表达量差别最大的基因，Seurat使用FindVariableFeatures函数鉴定高变基因，这些基因在不同细胞之间的表达量差异很大（在一些细胞中高表达，在另一些细胞中低表达）。默认情况下，会返回2,000个高变基因用于下游的分析。

利用FindVariableFeatures函数，会计算一个mean-variance结果，也就是给出表达量均值和方差的关系并且得到top variable features，这一步的目的是鉴定出细胞与细胞之间表达量相差很大的基因，用于后续鉴定细胞类型。

### 回归协变量

```
sc.pp.regress_out(adata, ['n_counts', 'percent_mito'])
```

### scale

```
sc.pp.scale(adata, max_value=10)
```

## 12 降维

### PCA

#### pca分析

```R
sc.tl.pca(adata, svd_solver='arpack') # PCA分析

```

```
adata.obsm['X_pca'] 
adata.varm['PCs']
```

#### elbow图

作碎石图，观测主成分的质量。这个图用于选择后续应该使用多少个PC，用于计算细胞间的相邻距离。

```R
sc.pl.pca_variance_ratio(adata, log=True)
```

![[附件/Pasted image 20221014233300.png]]

#### pca loading图

```
pl.pca_loadings(adata[, components, ...])
```

Rank genes according to contributions to PCs.

#### pca总观图

```
pl.pca_overview(adata, **params)
```

Plot PCA results.

#### 某obs注释的pca点图

```
sc.pl.pca(adata, color='CST3') #绘图
```

### TSNE

### UMAP

#### 进行umap

需要先sc.pp.neighbors获得knn图

```python
sc.pp.neighbors
```

```python
sc.tl.umap(adata)
```

#### 某obs的umap点图

```python
## 表达量未标准化
sc.pl.umap(adata, color=['CST3', 'NKG7', 'PPBP'])

## 表达量标准化
sc.pl.umap(adata, color=['CST3', 'NKG7', 'PPBP'], use_raw=False)
```

- layer指定哪一层
- 离散变量更换颜色

```python
platte=['#11111','#11111','#11111','#11111']
```

- 连续变量更换颜色

```python
color_map="viridis","plasma","inferno","magma","cividis"
```

### phate

```python
sce.tl.phate(adata[, n_components, k, a, ...])

PHATE [Moon17].
```

```
sce.pl.phate(adata, *[, color, gene_symbols, ...])

#Scatter plot in PHATE basis.

```

###  diffusion maps

扩散图是一种非线性数据汇总技术。由于扩散分量强调数据中的跃迁，因此它们 `<mark class="hltr-red">`主要用于感兴趣的连续过程 `</mark>` （如分化）时。通常，每个扩散分量（即扩散图维数）突出显示不同细胞群的异质性。
[比如所有细胞有共同谱系]

```
tl.diffmap(adata[, n_comps, neighbors_key, ...])
```

```
pl.diffmap(adata, *[, color, gene_symbols, ...])
```

Scatter plot in Diffusion Map basis.

### palantir --Diffusion map

```
tl.palantir(adata[, n_components, knn, ...])
```

Run Diffusion maps using the adaptive anisotropic kernel

### Force-directed graph drawing力导向布局

```python
tl.draw_graph(adata[, layout, init_pos, ...])
```

```python
pl.draw_graph(adata, *[, color, ...])

#Scatter plot in graph-drawing basis.
```

### TriMap

```

tl.trimap(adata[, n_components, n_inliers, ...])
```

TriMap: Large-scale Dimensionality Reduction Using Triplets [Amid19].

```
pl.trimap(adata, *[, color, gene_symbols, ...])

#Scatter plot in TriMap basis.

```

### Self-Assembling Manifolds

```
tl.sam(adata[, max_iter, num_norm_avg, k, ...])
```

Self-Assembling Manifolds single-cell RNA sequencing analysis tool [Tarashansky19].

```
pl.sam(adata[, projection, c, cmap, ...])

#Scatter plot using the SAM projection or another input projection.

```

### 嵌入细胞密度

```python
tl.embedding_density(adata[, basis, ...])
 

```

Calculate the density of cells in an embedding (per condition).

## 13 聚类

### louvain法

 计算细胞间的距离：
这里的参数就先按照默认值设定：

```PYTHON
sc.pp.neighbors(adata, n_neighbors=10, n_pcs=40)

sc.tl.louvain(adata)

```

参数说明：
n_neighbors指的是每个点的邻近点的数量，`<mark class="hltr-red">`neighbors的个数越多，聚类数会越少 `</mark>` 。
pc的数量依赖于上面所做的碎石图，一般是选在拐点处的的主成分，只需要一个粗略值，不同的pc数量所产生的聚类形状也不同。

### leiden法 更推荐

```python
sc.tl.leiden(adata)
```

Louvain 算法有一个主要的缺陷：可能会产生任意的连接性不好的社区(甚至不连通)。为了解决这个问题，作者引入了Leiden算法。证明了该算法产生的社区保证是连通的。此外证明了当Leiden算法迭代应用时，它收敛于一个划分，其中所有社区的所有子集都是局部最优分配的。并且算法速度比Louvain算法更快。

和Seurat等人一样，scanpy推荐Traag *等人(2018)提出的Leiden graph-clustering方法(基于优化模块化的社区检测)。注意，Leiden集群直接对cell的邻域图进行聚类，我们在sc.pp.neighbors已经计算过了。

scanpy 中的默认分辨率参数是 1.0。然而，在许多情况下，分析人员可能希望尝试不同的分辨率参数来控制聚类的粗糙度。因此，我们建议将聚类结果保存在指示所选分辨率的指定键下。

```python
sc.tl.leiden(adata, key_added="leiden_res0_25", resolution=0.25)
sc.tl.leiden(adata, key_added="leiden_res0_5", resolution=0.5)
sc.tl.leiden(adata, key_added="leiden_res1", resolution=1.0)
```

### dendrogram层次聚类

```
tl.dendrogram(adata, groupby[, n_pcs, ...])
```

Computes a hierarchical clustering for the given groupby categories.

### PhenoGraph聚类

tl.phenograph(adata[, clustering_algo, k, ...])

```


```

PhenoGraph clustering [Levine15].

### 绘制聚类点图

```
sc.pl.umap(adata, color=['louvain'])
```

![[附件/Pasted image 20221014235504.png]]

## 14 差异分析

### 指定分组 ☆

Rank genes for characterizing groups.

```python
tl.rank_genes_groups(adata, groupby[, ...])
```

### 过滤基因

Filters out genes based on log fold change and fraction of genes expressing the gene within and outside the groupby categories.

```python
tl.filter_rank_genes_groups(adata[, key, ...])
```

### 数据marker与先验重叠性

Calculate an overlap score between data-deriven marker genes and provided markers

```python
tl.marker_gene_overlap(adata, ...[, key, ...])
```

### 可视化

```python
pl.rank_genes_groups(adata[, groups, ...])
#Plot ranking of genes.


pl.rank_genes_groups_violin(adata[, groups, ...])
#Plot ranking of genes for all tested comparisons.


pl.rank_genes_groups_stacked_violin(adata[, ...])
#Plot ranking of genes using stacked_violin plot (see stacked_violin())


pl.rank_genes_groups_heatmap(adata[, ...])
#Plot ranking of genes using heatmap plot (see heatmap())


pl.rank_genes_groups_dotplot(adata[, ...])
#Plot ranking of genes using dotplot plot (see dotplot())


pl.rank_genes_groups_matrixplot(adata[, ...])
#Plot ranking of genes using matrixplot plot (see matrixplot())


pl.rank_genes_groups_tracksplot(adata[, ...])
#Plot ranking of genes using heatmap plot (see heatmap())

```

## 15 整合与批次效应

### 简单批次效应

ComBat function for batch effect correction

```
pp.combat(adata[, key, covariates, inplace])


```

### 复杂整合

#### BBKNN

[单细胞转录组之Scanpy - 样本整合分析 - 简书 (jianshu.com)](https://www.jianshu.com/p/beef8a8be360)

```python
import bbknn     
sc.tl.pca(adata_concat)
sc.external.pp.bbknn(adata_concat, batch_key='batch')  
sc.pl.umap(adata_concat, color=['batch', 'louvain'],legend_fontsize='xx-small')
pt.savefig('pbmc_batch_umap_bbknn.pdf')


```

#### harmonypy

```
pp.harmony_integrate(adata, key[, basis, ...])
```

Use harmonypy [Korunsky19] to integrate different experiments.

#### mnn

```
pp.mnn_correct(*datas[, var_index, ...])
```

Correct batch effects by matching mutual nearest neighbors [Haghverdi18] [Kang18].

#### Scanorama

```
pp.scanorama_integrate(adata, key[, basis, ...])
```

Use Scanorama [Hie19] to integrate different experiments.

### 转移label

Map labels and embeddings from reference data to new data.

```python
tl.ingest(adata, adata_ref[, obs, ...])

```

## 16 轨迹分析

### dpt

Infer progression of cells through geodesic distance along the graph [[Haghverdi16]](https://scanpy.readthedocs.io/en/stable/references.html#haghverdi16) [[Wolf19]](https://scanpy.readthedocs.io/en/stable/references.html#wolf19).

```python
sc.tl.dpt(adata[, n_dcs, n_branchings, ...])
```

```python
pl.dpt_groups_pseudotime(adata[, color_map, ...])

#Plot groups and pseudotime.

pl.dpt_timeseries(adata[, color_map, show, ...])

#Heatmap of pseudotime series.

```

### paga

[PAGA：图抽象通过拓扑保留单个单元的映射来协调聚类与轨迹推断 - PMC (nih.gov)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6425583/)

本质也是一种伪时序
paga适合复杂轨迹，slingshot适合简单轨迹

```python
sc.tl.paga(adata)
```

paga单细胞嵌入图

paga路径图

paga的基因变化图

```python
pl.paga(adata[, threshold, color, layout, ...])
#Plot the PAGA graph through thresholding low-connectivity edges.


pl.paga_path(adata, nodes, keys[, use_raw, ...])
#Gene expression and annotation changes along paths in the abstracted graph.


pl.paga_compare(adata[, basis, edges, ...])
#Scatter and PAGA graph side-by-side.
```

### wishbone 分叉发育轨迹

```
tl.wishbone(adata, start_cell[, branch, k, ...])
```

Wishbone identifies bifurcating developmental trajectories from single-cell data [Setty16].

## 模拟

Simulate dynamic gene expression data

```
scanpy.tl.sim()
```

样本来自文献策划的布尔基因调控网络构建的随机微分方程模型，如[Wittmann09]所建议。

```python
pl.sim(adata[, tmax_realization, ...])

#Plot results of simulation.
```

## 可视化

### 散点图Scatter plot

Scatter plot along observations or variables axes.

```
pl.scatter(adata[, x, y, color, use_raw, ...])
```

### 热图heatmap

#### heatmap

Heatmap of the expression values of genes.

```
pl.heatmap(adata, var_names, groupby[, ...])
```

```python
markers = {'T-cell': 'CD3D', 'B-cell': 'CD79A', 'myeloid': 'CST3'}
sc.pl.heatmap(adata, markers, groupby='bulk_labels', dendrogram=True)
```

#### matrixplot

Creates a heatmap of the mean expression values per group of each var_names.

```
pl.matrixplot(adata, var_names, groupby[, ...])
```

#### clustermap

Hierarchically-clustered heatmap.

```
pl.clustermap(adata[, obs_keys, use_raw, ...])
```

### 大点图dotplot

Makes a dot plot of the expression values of var_names.

```
pl.dotplot(adata, var_names, groupby[, ...])
```

### tracksplot

In this type of plot each var_name is plotted as a filled line plot where the y values correspond to the var_name values and x is each of the cells.

```
pl.tracksplot(adata, var_names, groupby[, ...])
```

```python
import scanpy as sc
adata = sc.datasets.pbmc68k_reduced()
markers = ['C1QA', 'PSAP', 'CD79A', 'CD79B', 'CST3', 'LYZ']
sc.pl.tracksplot(adata, markers, 'bulk_labels', dendrogram=True)
```

### 小提琴图Violin plot.

```
pl.violin(adata, keys[, groupby, log, ...])
```

### 堆叠小提琴图 Stacked violin plots.

```
pl.stacked_violin(adata, var_names, groupby)
```

### 排序图 ranking

Plot rankings.

```
pl.ranking(adata, attr, keys[, dictionary, ...])
```

### 系统树图dendrogram

Plots a dendrogram of the categories defined in groupby.

```
pl.dendrogram(adata, groupby, *[, ...])
```

### 空间转录组

```
sc.pl.spatial(adata, img_key="hires", color=["total_counts", "clusters"])
```
