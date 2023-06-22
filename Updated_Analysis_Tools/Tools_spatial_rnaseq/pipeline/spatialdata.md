> **_参考资料：_**
>
> > [论文地址](https://www.biorxiv.org/content/10.1101/2023.05.05.539647v1.full)
> > [代码复现](https://github.com/scverse/spatialdata-notebooks/tree/main/notebooks/paper_reproducibility)
> > [SpatialData 官网](https://spatialdata.scverse.org/en/latest/)

---

# 简介

由于**数据量大、数据类型异构以及缺乏统一的空间感知数据结构**，处理单模态和多模态空间组学数据集仍然是一个挑战。
SpatialData 是一个框架，它建立了统一的、可扩展的多平台文件格式、大于内存的数据的延迟表示、转换以及与公共坐标系的对齐。SpatialData 促进了空间注释和跨模态聚合和分析，其实用性通过多个梗概来说明，包括对多模态 Xenium 和 Visium 乳腺癌研究的综合分析。

虽然在分析单个空间组学数据集方面取得了进展，但整合多模态空间组学数据仍然是一个实际挑战。

# 组织架构的整体视图--空间多组学数据处理中的挑战

目前的空间组学技术在读取(如转录组、蛋白质组、形态学)、分辨率和全面性方面存在差异。
例如，Visium 分析以超细胞分辨率为代价给出了全转录组读数:每个 Visium 循环捕获位置描述了多达数十个细胞的聚集表达。
另一方面，单分子杂交和原位测序技术能够以有限的亚细胞分辨率解析数百万个转录本，但代价是较少的靶标组(通常最多几百个)。
越来越多地，多种互补的技术和分析方法被应用于相同的样本。然而，由于缺乏整合和联合分析不同模式的方法，这些努力受到阻碍。

我们确定了 SpatialData 专门解决的 **4 个关键突出挑战**:

## _1. 大图像数据_

图像为分子谱提供了重要的补充信息。

例如，H&E 图像可以为分子测量提供关键的组织学和解剖学背景。
多维图像通常存储为密集数组。由于它们的尺寸，图像需要特别考虑性能处理和与分子剖面的整合。
例如，延迟加载和多尺度表示只加载给定处理操作所需的图像的所需部分和细节级别，因此应**允许处理比可用内存大**的图像。

## _2. 多模态空间组学数据的空间对齐_

每种空间组学模式都测量分子结构的特定特征(例如，RNA 表达、形态、染色质可及性)。
为了建立对组织结构的整体理解，能够结合互补模式并在它们之间传递量化和注释是有益的。

例如，如果通过 H&E 图像中的组织学特征确定了两个感兴趣的区域(roi)，我们可能想要聚集并比较这些 roi 中蛋白质丰度和 RNA 表达的分子谱，以研究产生不同组织学特征的信号。为了实现这样的整合，需要将不同的数据集**对齐到一个公共坐标系统**中。

## _3. 交叉模态整合_

空间注释可以有效地使用每个模态提供的互补见解。在上面的例子中，将来自 H&E 图像的 roi 的空间注释转移到分子剖面中，可以对不同生物区室中的信号进行分层和比较。
如果没有将 H&E 图像的注释转移到分子剖面上，这样的比较是不可能的。虽然在概念上很简单，但在实践中，**跨模式传输空间注释**是很困难的，因为相应的聚合操作必须应用于不同的数据类型(例如，多维数组、多边形、点)。

## _4. 交互式分析_

解释空间组学数据集需要领域专家的输入。
例如，通常需要根据组织学或解剖学特征注释感兴趣的区域。这样的注释对于比较不同的解剖区室和为训练和验证分析算法提供基础事实是必要的。

## 基本数据类型

- **光栅图像**:多路显微镜图像
- **栅格标签**:分割掩码
- **多尺度光栅**:大显微镜图像或标签的金字塔状形式
- **多边形**:表示感兴趣区域的多边形列表(例如病理注释，组织区域)
- **规则形状**:类似于多边形，用于表示基于阵列的技术的捕获位置(例如，圆形用于 Visium，方形用于 DBiT-seq 等)
- **点**:无量纲点的列表，用于表示例如转录本的位置。
- **特征矩阵**:基因或蛋白质表达矩阵。
- **标注矩阵**:实验元数据、聚类标注等
- **图形**:区域(细胞，斑点，形状)或特征(基因，蛋白质)之间的相邻图形。

## 需要的分析操作

- **点聚合**:计算点和区域(标签，多边形或形状)之间的汇总统计数据，例如计算跨分段细胞的转录本数量。
- **几何相交**:设置多边形，标签或形状之间的操作。
- **变换**:在坐标系之间变换元素(图像，区域和点)
- **坐标系统**:支持为每个元素指定不同的坐标系统(例如基于像素的坐标系统与全局物理坐标系统)。

> **_总结：_** **空间组学分析工具图景：**

> - 首先，**以一致的方式将数据集加载到分析管道中时**，受到数据类型(例如，用于测序的表格数据和用于图像的 10s-100s GB 密集数组)和文件格式(例如，特定于技术的供应商格式)的多样性的阻碍。
> - 此外，个体空间组学模式通常具有**不同的空间分辨率**，并且可以从同一组织的不同区域获取数据。因此，为了整合这些数据，必须对它们进行**适当的转换**，使它们与公共坐标系统(common coordinate system，CCS)对齐。将数据集与 CCS 对齐是建立全球通用坐标框架(CCF，common coordinate framework)的基石。
> - 最后，解开多模态空间组学数据集的复杂性需要**专家领域知识**，激励能够进行大规模交互式数据探索和注释的方法。
> - 因此，为了释放新兴空间多组学研究的全部潜力，需要使用**统一的可编程接口**来**存储、探索、分析和注释**不同的空间组学数据的计算基础设施。

**SpatialData 框架实现了多模态空间组学数据的整合。
独立于语言的存储格式增加了数据源的互操作性，而 Python 库则标准化了对不同数据类型的访问和操作。
SpatialData 格式支持所有主要的空间组学技术。**

# SpatialData 的概念和实现

空间组学数据集包含不同的数据类型，因此加载和整合具有挑战性。

SpatialData 存储格式和内存表示支持 5 种基本的 spatialelement 来表示不同的数据集和数据类型:图像、标签、点、形状和表(下面将详细解释)。

这些 SpatialElements 允许表示来自广泛的空间组学分析的原始和派生数据，并且我们为大多数常见的空间组学数据格式提供了方便的阅读器功能。

多个 spatialelement 可以在一个 SpatialData 对象中组合在一起。SpatialData 对象可以表示单个数据集或多个数据集。SpatialData 对象以 SpatialData 格式存储在磁盘上，该格式建立在 OME-NGFF 规范的 Zarr 实现之上。

OME-NGFF 是一个社区驱动的数据标准，使用 Python、Java 和 JavaScript 进行读取，增强了 SpatialData 存储格式的互操作性。
使用来自 OME-NGFF 的标准化元数据还可以提高 SpatialData 的可访问性和可再现性。

- **图像**:图像是存储高分辨率显微镜图像的光栅数据。它们被存储为 Zarr 数组，并在内存中表示为(多尺度)SpatialImage 类。SpatialImage 继承了 xarray 和 xarray-datatree，用于表示和操作带有命名坐标的高维数组。
- **标签**:标签是栅格数据，包含感兴趣的区域，如分割掩码。与图像类似，它们以 Zarr 数组的形式存储在磁盘上，并在内存中表示为(多尺度)SpatialImage。
- **形状**:形状是多边形数据，包含感兴趣的区域，如细胞分割，基于阵列的空间转录组学数据或其他类型的 roi 的捕获位置。它们被存储为一系列数组，包含多边形的坐标和偏移量，作为磁盘上的 Zarr 数组，并在内存中表示为 GeoPandas 数据框中的 Shapely 对象。
- **点**:点包含大量的坐标和注释(通常是数百万或数十亿)，如文本位置及其相关的元数据。它们作为 parquet 文件存储在磁盘上，并在内存中表示为带有 DaskDataFrame 的惰性对象。
- **表**:表存储分子谱信息(基因表达，蛋白质表达等)和相关的元数据。它还存储空间图的邻接矩阵以及任何相关的附加元数据。它存储在磁盘上，在内存中表示为 AnnData。

> **\*总结**：\*
>
> - _空间数据集使用五种基本元素来表示:图像(光栅图像)、标签(例如光栅分割掩码)、点(例如分子探针)、形状(例如感兴趣的多边形区域、阵列捕获位置等)和表(例如分子量化和注释)。_
> - _该文件格式还跟踪应用于单个数据集的任何坐标转换或对齐步骤。_
> - _来自多个分析的数据集集合可以存储在单个 SpatialData 文件中，由于它们之间的空间关系是通过坐标转换存储的，因此可以一起分析它们_
> - _SpatialData 格式建立在 OME-NGFF 规范之上，并利用了 Zarr 文件格式，后者为传统的基于文件系统的存储和基于云的存储提供了高性能、可互操作的访问_

## 支持的技术

**SpatialData Python 库将这种格式表示为内存中的 SpatialData 对象，它支持延迟加载大于内存的数据。
SpatialData Python 库为当前几乎所有的空间组学技术提供了 reader 函数。**

## 基本操作

该库为操作和访问 SpatialData 对象提供了多种功能。一个核心特征是生物系统的**公共坐标系统(CCSs)**的有效定义。

- 简而言之，来自各个数据模式的数据集与坐标转换相关联。SpatialData 实现仿射坐标转换，可以组合([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/transformations.html))。
- 一旦对齐，数据集就可以使用模式内部和模式之间的空间注释进行
- 查询和聚合允许从大型数据中创建按生物信息因素分组的新数据集，利于探索和数据访问

  - **查询**([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/spatial_query.html))

    > - 在处理大数据时，能够将数据集细分为多个区域是很有帮助的。
    > - 如提取特定的解剖区域或划分数据集进行并行处理。通过空间查询接口，用户可以请求特定坐标系下边界框中包含的数据。子集操作的结果作为一个 SpatialData 对象返回。
    > - 在未来的工作中，我们将扩展坐标系统以进行更高级的查询，例如空间坐标和特征值的复合查询(例如，区域 X 内的所有单元格，属于集群 Y)。

  - **聚合**([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/aggregation.html))

    > - 聚合操作构成了在多模态分析中跨模态传递量化和注释的基础。
    > - SpatialData 框架支持将存储在任何 SpatialElement 中的数据聚合(也称为图像处理中的积累)到任何一组目标几何或掩码中。
    > - 例如，可以在代表细胞的多边形几何中计算特定基因的单分子数量。同样地，这些相同的分子可以在代表细胞细胞质的图像掩码中计数。
    > - 另一个例子是在给定的解剖区域内平均细胞基因表达。SpatialData 提供了将标准聚合操作符应用于任何 SpatialElement 的数据(count、sum、mean、standard deviation)的灵活性。
    > - 此外，SpatialData 还提供了一个接口，用于应用用户定义的聚合操作符。
    > - 利用 SpatialData 公共坐标系统，可以在具有不同空间尺度和/或不完全重叠的两个 spatialelement 之间执行聚合。

  - **交互操作**

    > - napari-SpatialData 插件允许分析人员进行空间注释，例如绘制感兴趣的区域([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/napari_rois.html))或用于指导多数据集注册的地标([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/alignment_using_landmarks.html))。
    > - 静态图形和图形可以使用 spatialdata-plot 库创建([教程链接](https://spatialdata.scverse.org/projects/plot/en/latest/index.html))

  - **深度学习**

    > - 实现 PyTorch Dataset 类，可以直接从 SpatialData 对象中训练深度学习模型
    > - 利用空间查询 API, SpatialData 附带 PyTorch Dataset class
    > - Dataset 实现使用空间查询功能从 SpatialData 对象生成 tile。该实现使用户能够将 SpatialData 数据集与丰富的 Python 深度学习生态系统(包括来自 MONAI 的模型和基础设施)整合。([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/densenet.html))
    > - 其中我们使用 SpatialData PyTorch 数据集接口来训练 MONAI DenseNet 编码器，演示了如何通过使用 SpatialData PyTorch Dataset 类查询每个 Xenium 单元周围的图像块，从空间上与两个 Xenium 数据集之一对齐的 H&E 图像生成图像块。
    > - 我们使用这些数据来训练 DenseNet 来预测每个图像块的细胞类型。

- 此外，在[虚拟生态系统中的包](https://github.com/scverse/ecosystem-packages) 可以用来分析 SpatialData 对象,如 squidpy 的整合([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/squidpy_integration.html))

# SpatialData 在多模态、多技术乳腺癌实验中的应用

为了说明 SpatialData 在多模式整合和分析方面的适用性，我们将该框架应用于一项乳腺癌研究，该研究结合了 H&E 图像、10x Genomics Visium 和 Xenium 分析([文章地址](https://www.biorxiv.org/content/10.1101/2022.10.06.510405v2?ijkey=16f0f8af2c83c6a44f652928adea4a4a815b5c4f&keytype2=tf_ipsecsha))。
该研究包括来自乳腺癌肿瘤连续切片的两个原位测序(Xenium)和一个空间转录组学数据集(10 X Visium CytAssist)。

- **对齐**

  - 首先，我们使用 napari-spatialdata 来定义所有数据集中存在的地标点。
  - 然后，我们使用仿射变换对齐所有三个数据集，从而为该数据集定义一个 CCS。
  - 作为对齐的结果，SpatialData 使我们能够识别公共空间区域，可以使用跨所有三个数据集的 SpatialData 查询访问该区域。

- **注释/聚合**

  - 使用来自所有三个数据集的集合信息来创建一组共享的空间注释。
  - 使用 napari-spatialdata 根据 H&E 图像中的组织学特征选择了四个 roi
  - 在 Visium 中使用全基因组转录组信息来推断拷贝数变化(CopyKat)并选择主要的遗传亚克隆
  - 使用标签转移方法(scanpy)对 Xenium 进行注释，使用独立的乳腺癌 scRNA-seq atlas 的细胞类型标记
    - Xenium 重复之间的高度一致性(Visium 位置的 Pearson 中位数 R=0.75)
    - 并且 Xenium 和基于反卷积的估计之间的总体一致性很好(Pearson 中位数 R=0.60)

- **第二个聚合例子**

  - 比较了来自 Xenium 复制的转录-环聚集与原始 Visium 转录计数
  - Xenium 重复的总计数高度一致(Pearson 中位数 R=0.62)
  - Xenium 和 Visium 计数之间的一致性较小(Pearson 中位数 R=0.48)
  - 总体转录本丰度与不同组织切片和技术之间的一致性之间存在直接关系

> **_总结_**
>
> - 说明了聚合功能的灵活性
> - 可以在不同类型的 spatialelement(点、圆形捕获位置、细胞、更大的解剖 roi)之间应用
> - 传输不同类型的空间注释(细胞表达、细胞类型分数)

# 进一步应用和其他用例的说明

> - 作为将 SpatialData 与不同技术结合使用的起点，我们还提供了来自[总共 8 种技术的 40 多个数据集的预格式化的 SpatialData 对象](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/datasets/README.html)
> - 可以对单模态和多模态数据集执行交互式注释([教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/napari_rois.html), [教程链接](https://spatialdata.scverse.org/en/latest/tutorials/notebooks/notebooks/examples/alignment_using_landmarks.html))
> - 通过将 12 张 Visium 幻灯片映射到一个大的前列腺切片，演示如何使用 SpatialData 将多个视图字段对齐到一个全局参考坐标系(原文 Supplementary Figure 9)

# 总结

> - **SpatialData 是一个灵活的、基于社区标准的框架，用于存储、处理和注释迄今为止几乎所有可用的空间组学技术的数据**

> - **轻松创建通用坐标系统来对齐数据集的能力是建立通用坐标框架(common coordinate frameworks，CCFs)的关键基石**

> - **CCFs 将开启新的分析方法，促进跨研究样本的可靠比较和重用**

> - **SpatialData 框架提供的灵活性和易于访问的解决方案使新的分析成为可能，并增强了综合空间分析的可重复性。**

> - **展望**
>
> > - [ ] 未来目标是将 SpatialData 互操作性扩展到 R/Bioconductor
> > - [ ] 提供对多尺度点和多边形表示的支持
> > - [ ] 并通过编程和可视化工具 Vitessce 支持基于云的数据访问
