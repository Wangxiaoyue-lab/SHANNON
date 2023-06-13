# 读取

## 三个文件(barcode、gene、matrix)文件夹

```R
 #如果是gz打包

gunzip("/home/caoj/mydata/gse_scrna1/data/urd/GSM3084402/barcodes.tsv.gz", remove = TRUE)

#dir填写三文件地址

pbmc.counts <- Read10X(data.dir = "~/Downloads/pbmc3k/filtered_gene_bc_matrices/hg19/")

pbmc <- CreateSeuratObject(counts = pbmc.counts)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/7066abee-c96f-498f-912b-86bcb26281b6.jpg)

## 读取h5

```R
 E18_S3 <- Seurat::Read10X_h5(filename = "/home/caoj/mydata/gse_scrna1/data/neuron1/GSM4635079_E18_S3_filtered_gene_bc_matrices_h5.h5", use.names = T)
```

## 创建对象

```
sdata.cov15 <- CreateSeuratObject(cov.15, project = "covid_15")
```

project参数指定了orig.ident的值

## 添加元数据

 AddMetaData

```
pbmc <- AddMetaData(pbmc, CellsMetaTrim)
pbmc2 <- AddMetaData(object = pbmc2, metadata = predicted.labels)
```

## 添加assay

```R
object[[assay_name]] <- CreateAssayObject(data =  XXX )
#或者嵌入别的slot中
```

```R
slot_name = paste0(assay_name, '_coefs')

Misc(object = object, slot = slot_name) <- ko_inf$coefficients

```

# 基本信息提取

## seurat类

 Seurat类

```
utils::methods(class = 'Seurat')

 [1] [                             [[ 

 [3] [[<-                          $ 

 [5] $<-                           AddMetaData 

 [7] as.CellDataSet                as.SingleCellExperiment 

 [9] colMeans                      colSums 

[11] Command                       DefaultAssay 

[13] DefaultAssay<-                dim 

[15] dimnames                      droplevels 

[17] Embeddings                    FindClusters 

[19] FindMarkers                   FindNeighbors 

[21] FindSpatiallyVariableFeatures FindVariableFeatures 

[23] FoldChange                    GetAssay 

[25] GetAssayData                  GetImage 

[27] GetTissueCoordinates          head 

[29] HVFInfo                       Idents 

[31] Idents<-                      Key 

[33] levels                        levels<- 

[35] Loadings                      merge 

[37] Misc                          Misc<- 

[39] names                         NormalizeData 

[41] Project                       Project<- 

[43] ProjectUMAP                   RenameCells 

[45] RenameIdents                  ReorderIdent 

[47] rowMeans                      rowSums 

[49] RunCCA                        RunICA 

[51] RunLDA                        RunPCA 

[53] RunSPCA                       RunTSNE 

[55] RunUMAP                       ScaleData 

[57] ScoreJackStraw                SCTResults 

[59] SetAssayData                  SetIdent 

[61] show                          SpatiallyVariableFeatures 

[63] StashIdent                    Stdev 

[65] subset                        SVFInfo 

[67] tail                          Tool 

[69] Tool<-                        VariableFeatures 

[71] VariableFeatures<-            Version 

[73] WhichCells 
```

## 总体  str

```
 pmbc  ##直接看

str(pmbc)
```

## 细胞编号

```
 colnames(x = pbmc)  # 各个细胞的编号

Cells(pbmc)   # 和上面的一样，各个细胞的编号
```

## 基因编号

```
 rownames(x = pbmc)   # 基因名
```

## 细胞默认分群

```R
 Idents(object = pbmc)

levels(pbmc)

table(Idents(pbmc))  # 获取每个细胞类型的细胞数目表格

#把老分群保存为新列

pbmc[["old.ident"]] <- Idents(object = pbmc)

pbmc <- StashIdent(object = pbmc, save.name = "old.ident")
```

# 数据提取

## 特征基因

```
 VariableFeatures(alldata)
```

## 特征基因提取特征均值和离散度

```
 HVFInfo(object = alldata)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/d08f3b68-82a1-4166-980d-c6a89143077b.png)

## 使用GetAssayData函数获取'counts', 'data'和'scale.data'信息

```r
 GetAssayData(object = alldata,assay="RNA", slot = "counts")
```

## pca信息

```R
 Embeddings(object = pbmc, reduction = "pca") #检索每个细胞的PC矩阵

alldata@reductions$pca@cell.embeddings

Loadings(object = pbmc, reduction = "pca") #检索每个基因的PC矩阵

alldata@reductions$pca@feature.loadings 

Loadings(object = pbmc, reduction = "pca", projected = TRUE)

#tsne降维

Embeddings(object = alldata, reduction = "tsne")

alldata@reductions$tsne@cell.embeddings

#umap降维

Embeddings(object = alldata, reduction = "umap")

alldata@reductions$umap@cell.embeddings
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/4e3806d0-b71c-4056-85f5-e839b8e815c4.png)

## FetchData函数取任何值

FetchData函数可以从**expression matrices**, **cell embeddings**或**metadata**中取出任何值

```R
 #提取metadata列数据

FetchData(object = pbmc, vars = c("ident", "orig.ident"))

#提取几个slot里的数据

FetchData(object = pbmc, vars = c("PC_1", "percent.mt", "MS4A1"))
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/a8823a99-5619-47b8-a9a6-ac052d59f702.png)![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/395ee834-d22c-4077-8449-1e1eca932b5e.png)

# 转化

## 老seurat对象转换新版本对象

```
 #Seurat old object 转成 Seurat3

PRO1 <- UpdateSeuratObject(object = alldata)
```

## Seurat对象输出10x数据

```
 ## Seurat对象输出10x数据

library(DropletUtils)

write10xCounts(x = PRO@assays$RNA@counts, path = '10x', version="3")

write10xCounts(x = alldata@assays$RNA@counts, path = '10x', version="4")
```

## Seurat对象输出h5ad格式

```
library(Seurat)

library(SeuratDisk)

PRO = UpdateSeuratObject(object = PRO)

SaveH5Seurat(PRO, filename = "c1613.neg.h5Seurat")

Convert("c1613.neg.h5Seurat", dest = "h5ad")
```

## 默认assay

```
 DefaultAssay(alldata)

DefaultAssay(alldata) <- "RNA"
```

## 切换分群

```
Idents(object = pbmc) <- "orig.ident"
```

## 分群重命名

```
 pbmc <- RenameIdents(object = pbmc, `CD4 T cells` = "T Helper cells")
```

## 随机给予抽样化的分组

```
 random_group_labels <- sample(x = c("g1", "g2"), size = ncol(x = pbmc), replace = TRUE)

pbmc$groups <- random_group_labels
```

## 合并

```
 # Merge两个Seurat对象

merge(x = pbmc1, y = pbmc2)

# Merge多个Seurat对象

merge(x = pbmc1, y = list(pbmc2, pbmc3))
```

## 将外来数据矩阵插入插槽slot

```
 new.seurat.object <- SetAssayData(object = pbmc, slot = "counts", new.data = count.data, assay = "RNA"）

#⚠️ 可以使用SetAssayData将数据添加到counts，data或scale.data插槽中。

#新数据必须具有与当前数据相同顺序的相同细胞。

#添加到counts'或data`中的数据必须具有与当前数据相同的features。
```

## 批量修改细胞名

```
 # 替换cell ID名称，-1改成_1

new_obj <- RenameCells(obj, new.names=gsub("-1", "_1", colnames(obj)))
```

# 计算

## 计算选定分群各ident的平均表达量

```R
 # 获取平均表达量

Idents(scRNA_data) <- "seurat_clusters"   # 这一步可以指定要计算哪一个分组的平均表达量，可以选择细胞类型（"CellType"）cluster（"seurat_clusters"）或者是样本类型（"orig.ident")，要注意这里的变量名称不一定正确，要根据数据中的具体变量来指定

AverageExp <- AverageExpression(scRNA_data)

expr <- AverageExp$RNA

```

## FoldChange 提取集群FC

相比findmarker而言，无条件阈值

```R
FoldChange(datadeg2,ident.1="Acinar cell",ident.2="Ductal cell2")


```

直接用的话会和FindMarkers结果不一样
需要指定mean.fxn函数

```r
fc=FoldChange(data.DM,ident.1="Spib",ident.2="Non-Targeting",mean.fxn = function(x) {
	return(log(x = rowMeans(x = expm1(x = x)) + 1, base =2))
})
```

## 增加分组前缀

```
# 增加分组前缀，这里增加的是"Cluster"

for(i in 1:ncol(expr)){colnames(expr)[i] = paste("Cluster", colnames(expr)[i],sep = "")}
```

# 取子集

## WhichCells 依据元数据的条件筛选

```r
 selected_c <- WhichCells(alldata, expression = nFeature_RNA > 200)
```

## 拆分成列表

```R
#按orig.ident拆分样本来源
alldata.list <- SplitObject(alldata, split.by = "orig.ident")
```

## 按分群筛选

```R
 data.filt <- subset(x = pbmc, idents = "B cells")

subset(x = pbmc, idents = c("CD4 T cells", "CD8 T cells"), invert = TRUE)
```

## 根据表达量的值来进行筛选

```r
 subset(x = pbmc, subset = MS4A1 > 3 & PC1 > 5)

subset(x = pbmc, subset = MS4A1 > 3, idents = "B cells")
```

## 根据metadata里某一项进行筛选

```r
 subset(x = pbmc, subset = orig.ident == "Replicate1")
```

## 随机downsample

```r
 subset(x = pbmc, downsample = 100)

#这是按ident进行分别抽样的
```

## 筛选特殊基因

```R
 subset(x = pbmc_small, features = VariableFeatures(object = pbmc_small))
```

## 也可以使用数组的形式提取

```R
 pbmc_small_sub = pbmc_small[,pbmc_small@meta.data$seurat_clusters %in% c(0,2)]

pbmc_small_sub = pbmc_small[, Idents(pbmc_small) %in% c( "T cell" ,  "B cell" )]  # 需要此时的pbmc_small数据Idents(pbmc_small)为细胞类型
```

# 基本流程

## 标准化

```R
 pbmc <- NormalizeData(object = pbmc)
```

## 寻找特征基因

```R
 pbmc <- FindVariableFeatures(object = pbmc)
```

## 归一化

```R
 pbmc <- ScaleData(object = pbmc)
```

## 降维

```R
 pbmc <- RunPCA(object = pbmc)

pbmc <- RunTSNE(object = pbmc)
```

## 聚类

```R
 pbmc <- FindNeighbors(object = pbmc)

pbmc <- FindClusters(object = pbmc)
```


# 可视化

## 合并

```R
library(cowplot) 
 plot_grid(ncol = 3,)
```

## 个性化

| 主题          | 功能                                              |
| ------------- | ------------------------------------------------- |
| DarkTheme     | Set a black background with white text            |
| FontSize      | Set font sizes for various elements of a plot     |
| NoAxes        | Remove axes and axis text删除所有轴和轴文本       |
| NoLegend      | Remove all legend elements删除所有图例元素        |
| RestoreLegend | Restores a legend after removal移除后恢复一个图例 |
| RotatedAxis   | Rotates x-axis labels                             |

来源： [http://events.jianshu.io/p/104f9290af8b](http://events.jianshu.io/p/104f9290af8b)

去掉图例

```R
 plot <- DimPlot(object = pbmc) + NoLegend()
```

增加标签

```R
 # Label points on a ggplot object

LabelPoints(plot = plot, points = TopCells(object = pbmc[["pca"]]), repel = TRUE)
```

 交互化信息？

```R
# HoverLocator replaces the former `do.hover` argument It can also show extra data throught the `information` argument,

# designed to work smoothly with FetchData

HoverLocator(plot = plot, information = FetchData(object = pbmc, vars = c("ident", "PC_1", "nFeature_RNA")))

# FeatureLocator replaces the former `do.identify`

select.cells <- FeatureLocator(plot = plot)

CellSelector(plot = FeaturePlot(object = alldata, features = "MS4A1"))
```

更换sci配色

```
 DimPlot(alldata, label = T) + ggsci::scale_color_igv()
```

## 聚类展示顺序

```R
levels(pbmc) <- c("CD4 Naive","CD4 Memory","CD8 Naive","CD8 Effector","DN T","NK CD56bright","NK CD56Dim","pre-B",'pro-B',"pDC","DC","CD14 Mono",'CD16 Mono')
```

## 维度图

```R
 # Dimensional reduction plot for PCA or tSNE

DimPlot(object = pbmc, reduction = "tsne")

DimPlot(object = pbmc, reduction = "pca")

DimPlot(alldata, reduction = "pca", group.by = "orig.ident",dims = 1:2)

##split.by 参数可以把维度图拆分成几个来看

##group.by 参数可以对维度图进行细胞注释，前提是注释本身在metadata里

 #label = TRUE,repel = TRUE 两个参数用来添加文本标签

DimPlot(object = pbmc) + DarkTheme()

DimPlot(object = pbmc) + labs(title = "2,700 PBMCs clustered using Seurat and viewed\non a two-dimensional tSNE")

label = TRUE #显示标签

repel = TRUE    #标签不重叠
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/103d17fc-551c-4a68-9773-b117d676641e.jpg)![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/0bdf9e31-9976-43fb-86f9-3bbd940d60d4.jpg)

### PCA模式图

```R
 VizDimLoadings(alldata, dims = 1:5, reduction = "pca", ncol = 5, balanced = T)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/2c3999ec-0a03-4858-b638-cca7df1f91c4.jpg)

### 碎石图

```R
 ElbowPlot(alldata, reduction = "pca", ndims = 50)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/d3ef6db2-b2d4-4ce0-a4a8-68bee2732961.png)

### 主成分热图

```R
 DimHeatmap(object = pbmc, reduction = "pca", cells = 200)

DimHeatmap(alldata, dims = 1:5, cells = 500, balanced = TRUE)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/12947487-3024-46df-b5a2-be1732bf7238.png)

### JackStrawPlot函数

```R
  seurat_data <- JackStraw(object = seurat_data, dims = 50)  
  
seurat_data  <- ScoreJackStraw(seurat_data, dims = 1:50) 

JackStrawPlot(object = seurat_data, dims = 1:50)
```


### 精确阈值法

- 主成分累积贡献大于90%
- PC本身对方差贡献小于5%
- 两个连续PCs之间差异小于0.1%

```R
# Determine percent of variation associated with each PC
pct<-alldata[["pca"]]@stdev/sum(alldata[["pca"]]@stdev)*100

# Calculate cumulative percents for each PC
cumu <- cumsum(pct)

# Determine which PC exhibits cumulative percent greater than 90% and % variation associated with the PC as less than 5
co1 <- which(cumu > 90 & pct < 5)[1]
co1

# Determine the difference between variation of PC and subsequent PC
co2 <- sort(which((pct[1:length(pct)-1]-pct[2:length(pct)])>0.1),decreasing=T)[1]+1

# last point where change of % of variation is more than 0.1%.
co2

# Minimum of the two calculation
pcs <- min(co1,co2)
pcs

# Create a dataframe with values
plot_df <- data.frame(pct = pct,cumu = cumu,rank = 1:length(pct))

# Elbow plot to visualize 
ggplot(plot_df, aes(cumu, pct, label = rank, color = rank > pcs)) + 
geom_text() + 
geom_vline(xintercept = 90, color = "grey") + 
geom_hline(yintercept = min(pct[pct > 5]), color = "grey") +
theme_bw()
```

## 基因分布图

```R
 # Dimensional reduction plot, with cells colored by a quantitative feature

#维度图基础的基因分布图

FeaturePlot(object = pbmc, features = "MS4A1")

FeaturePlot(alldata, reduction = "umap", dims = 1:2, features = myfeatures, ncol = 3, order = T)

#基因混合图

FeaturePlot(object = pbmc, features = c("MS4A1", "CD79A"), blend = TRUE)



order=T  将低表达量的点置于上层
pt.size 设置点的大小

```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/bdb4a418-b1cd-4410-a4fa-53baa2a7a0f4.jpg)

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/debf60de-36e2-4902-bcbd-f859693bf763.png)

也可以看其他特征的分布

```R
 FeaturePlot(alldata, features = paste0("PC_", 1:12), ncol = 4)
```

## 基因/成分关联图

```R
 # Scatter plot across single cells, replaces GenePlot

FeatureScatter(object = pbmc, feature1 = "MS4A1", feature2 = "PC_1")

FeatureScatter(object = pbmc, feature1 = "MS4A1", feature2 = "CD3D")
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/bd5ea96d-3955-4801-a0c0-20406fbe88a1.png)![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/601afbe8-9251-400b-99c4-5292c5484457.png)
 

## 细胞关联图

```R
 # Scatter plot across individual features, repleaces CellPlot

CellScatter(object = pbmc, cell1 = "AGTCTACTAGGGTG", cell2 = "CACAGATGGTTTCT")

CellScatter(object = alldata, cell1 = "covid_15_CTCCATGTCAACGTGT-15", cell2 = "covid_15_TTCACCGTCAGGAAGC-15")
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/a4b74839-358f-4122-8200-827a680be6cc.png)

## 小提琴图

```R
 VlnPlot(object = pbmc, features = c("LYZ", "CCL5", "IL32"))

VlnPlot(object = pbmc, features = "MS4A1", split.by = "groups")
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/05dee60d-fd91-420d-b9d6-ea9f51b6d9db.jpg)

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/376bf7cd-7d53-46c2-9e9a-9c125beea4ba.png)

## 特征基因图

```
 VariableFeaturePlot(object = pbmc)

LabelPoints(plot = VariableFeaturePlot(alldata), points = top20, repel = TRUE)
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/2bcccc6e-4db7-45fd-996e-2dd721a20c9e.jpg)

## 分群叠嶂图

```R
 RidgePlot(object = pbmc, feature = c("LYZ", "CCL5", "IL32"))

#不止是基因，主成分也行
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/10e2f2f4-76de-4db9-82bd-9b9227f664f0.jpg)

## 差异基因热图

```R
 # Heatmaps

DoHeatmap(object = pbmc, features = heatmap_markers)

DoHeatmap(alldata, features = as.character(unique(top5$gene)), group.by = sel.clust, assay = "RNA")
```

![](file:///E:/software/mygongxuo/为知笔记/subdictionary/temp/8f4d2c89-d22f-4c27-9571-87079feb781b/128/index_files/7b13951a-3dda-43a5-8977-f04f7f4e5cf3.jpg)

## 点图

是用data这个slot画的
所哟normalization前后存在差别

```R
 DotPlot(object = pbmc, features = c("LYZ", "CCL5", "IL32"), group.by = "groups")
```
