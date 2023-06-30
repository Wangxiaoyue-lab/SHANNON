> **_参考资料：_**
>
> > [论文地址](https://www.nature.com/articles/s41592-021-01358-2)
>
> > [代码复现](https://github.com/theislab/squidpy_reproducibility)
>
> > [官网](https://squidpy.readthedocs.io/en/latest/)

---

Squidpy 是一个 Python 框架，它汇集了组学和图像分析工具，可以对空间分子数据(如转录组或多变量蛋白质)进行可扩展的描述。

Squidpy 提供了高效的基础设施和多种分析方法，可以有效地存储、操作和交互式可视化空间组学数据。Squidpy 是可扩展的，可以与各种已经存在的库进行接口，用于空间组学数据的可扩展分析。

基于解离的单细胞技术使细胞状态的深度表征和许多器官和物种的细胞图谱的创建成为可能。然而，细胞多样性如何构成组织和功能仍然是一个悬而未决的问题。空间分辨分子技术旨在通过在细胞和亚细胞分辨率下原位研究组织来弥合这一差距

与目前最先进的基于解离的方案相比，空间分子技术以多种形式获取数据，包括分辨率(每次观察到的细胞很少到亚细胞分辨率)、多路复用(全基因组表达谱的数十个特征)、模式(转录组学、蛋白质组学和代谢组学)，并且通常具有捕获组织的相关高含量图像。

现有的分析步骤结合仍然受到缺乏统一的数据表示和模块化应用程序编程接口的阻碍。

例如，加载来自 Starfish 的处理过的数据，将 stLearn 的组织图像综合分析与 Giotto 强大的空间统计、BayesSpace 空间聚类相结合，或利用最先进的基于深度学习的图像分割和可视化方法。

为此，作者开发了“Spatial Quantification of Molecular Data in Python”(Squidpy)，

Squidpy 旨在将空间数据的多样性带入通用数据表示中，并提供一套通用的分析和交互式可视化工具。

Squidpy 是建立在 Scanpy 和 Anndata 之上的，它依赖于 Python 中的几个科学计算库，如 Scikit-image、Napari 和 Dask。

Squidpy 和 Giotto，Seurat 分析功能对比
| | Squidpy | Giotto | Seurat (spatial) |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Package focus | Efficient unified data representations for spatial data<br />comprehensive spatial graph and image analysis tools<br />(interactive) visualisation in python | Spatial analysis and visualisation in R:<br /> a comprehensive package tool that <br />provides analysis and visualization tool <br />for spatial graph and image | Seurat extension to suport spatial visualization<br /> and spatially variable genes analysis |
| Infrastructure | | | |
| Store large source tissue image<br />(>500Mb) | **_Yes_** | **_Yes_** | No |
| Store small tissue image<br />(<10Mb) | **_Yes_** | **_Yes_** | **_Yes_** |
| Build spatial neighborhod graph | **_Yes_** | **_Yes_** | No |
| | | | |
| Spatial analysis | | | |
| Spatial statistics for cell types | **_Yes_** | **_Yes_** | No |
| Spatially variable genes | **_Yes_** | **_Yes_** | **_Yes_** |
| Ligand-receptor analysis | **_Yes_** | Partial (no database) | No |
| | | | |
| Image analysis | | | |
| Morphology features (Standard) | **_Yes_** | No | No |
| Morphology features (DNN based) | **_Yes_** | No | No |
| Segmentation | **_Yes_** | No | No |
| Registration | No | No | No |
| Interface with DL framework | **_Yes_** | No | No |
| | | | |
| Integration | | | |
| Image-gene expression integration | **_Yes_** (with external tool) | No | No |
| Mapping/Deconvolution | **_Yes_** (with external tool) | **_Yes_** (with external tool) | **_Yes_** |
| | | | |
| Visualization | | | |
| 2d static | **_Yes_** | **_Yes_** | **_Yes_** |
| 3d static | **_Yes_** | **_Yes_** | No |
| 2d interactive | **_Yes_** | **_Yes_** | No |
| Interactive cropping | **_Yes_** | **_Yes_** | No |
| | | | |
| Others | | | |
| Unit tests | **_Yes_** | No | **_Yes_** |
| Documentation | **_Yes_** | **_Yes_** | **_Yes_** |
| Framework | Python | R/Python/ImageMagick | R |

---

Squidpy 和 Giotto 详细功能对比
| | Analysis task | Squidpy | Giotto | Link Giotto | Link Squidpy | Comments |
| ---------------------- | ------------------------------------------ | ------------------------------------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| Spatial graph analysis | Distance distribution for each spatial | gr.co_occurrence | spatNetwDistributionsDistance | [link](https://rubd.github.io/Giotto_site/reference/spatNetwDistributions.html) | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.co_occurrence.html#squidpy.gr.co_occurrence) | |
| | Build spatial neighbors graph | gr.spatial_neighbors | createSpatialNetwork | [link](https://rubd.github.io/Giotto_site/reference/createSpatialNetwork.html) | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.spatial_neighbors.html) | |
| | Spatially variable genes | gr.spatial_autocorr, gr.sepal | trendsceek, spark, spatialDE | [link](https://rubd.github.io/Giotto_site/reference/trendSceek.html) | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.spatial_autocorr.html) | |
| | Create spatial pattern simulation | NONE | runPatternSimulation | [link](https://rubd.github.io/Giotto_site/reference/runPatternSimulation.html) | | |
| | Spatial co-expression correlations | NONE | detectSpatialCorGenes | [link](https://rubd.github.io/Giotto_site/reference/detectSpatialCorGenes.html) | | |
| | Spatial domains | NONE | HMRF | [link](https://rubd.github.io/Giotto_site/reference/doHMRF.html) | | |
| | Neighbor enrichment analysis | gr.nhood_enrichment | cellProximityEnrichment | [link](https://rubd.github.io/Giotto_site/reference/cellProximityEnrichment.html) | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.nhood_enrichment.html) | |
| | Spatially informed LR pairing | NONE | spatCellCellcom | [link](https://rubd.github.io/Giotto_site/reference/spatCellCellcom.html) | | |
| | LR interaction analysis | gr.ligrec | exprCellCellcom | [link](https://rubd.github.io/Giotto_site/reference/exprCellCellcom.html) | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.ligrec.html#squidpy.gr.ligrec) | Giotto has no database |
| | Spatial-mediated gene expression changes | NONE | findInteractionChangedGenes | [link](https://rubd.github.io/Giotto_site/reference/findInteractionChangedGenes.html) | | |
| | Network analysis for spatial graph | gr.interaction_matrix, gr.centrality_scores | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.interaction_matrix.html) | |
| | Spatial patterning of cell types | gr.ripley | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.gr.ripley_k.html) | |
| Image analysis | Infrastructure for large tissue images | ImageContainer | NONE | | [link](https://squidpy.readthedocs.io/en/latest/classes.html) | with memory-efficient handling with Dask |
| | Support for z-stack in large tissue images | ImageContainer | NONE | | LINK TO TUTORIAL | |
| | Image processing tools | im.process | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.im.process.html#squidpy.im.process) | |
| | Image segmentation tools | im.segment | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.im.segment.html#squidpy.im.segment) | |
| | Image feature extraction | im.calculate_image_features | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.im.calculate_image_features.html#squidpy.im.calculate_image_features) | |
| | Feature extraction at segmentation level | im.calculate_image_features | NONE | | [link](https://squidpy.readthedocs.io/en/latest/api/squidpy.im.calculate_image_features.html#squidpy.im.calculate_image_features) | |
| | Support for external segmentation tools | im.segment and tutorial | NONE | | LINK TO TUTORIAL | |
| | Support for ML/DL frameworks | tutorial with Tensorflow | NONE | | LINK TO TUTORIAL | |
| Others | Interactive visualization | img.interactive | giotto viewer | [link](http://spatialgiotto.rc.fas.harvard.edu/giotto.viewer.html) | [link](https://squidpy.readthedocs.io/en/latest/external_tutorials/tutorial_napari.html) | |
| | Visualization | pl.spatial | spatPlot | [link](https://rubd.github.io/Giotto_site/reference/spatPlot.html) | [link](https://scanpy.readthedocs.io/en/stable/api/scanpy.pl.spatial.html) | |
| | Slice and create cross sections | subset AnnData/ImageContainer | AnnData subseting | [link](https://rubd.github.io/Giotto_site/reference/createCrossSection.html) | TODO | |
| | Deconvolution | Tangram (external) | DWLS (internal) | [link](https://rubd.github.io/Giotto_site/reference/runSpatialDeconv.html "https://rubd.github.io/Giotto_site/reference/runSpatialDeconv.html") | [link](https://squidpy.readthedocs.io/en/latest/external_tutorials/tutorial_tangram.html) | |
