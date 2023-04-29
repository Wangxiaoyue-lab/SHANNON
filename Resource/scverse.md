# **[scverse](https://scverse.org/)**
Foundational tools for single-cell omics data analysis
<https://github.com/scverse>

## CORE PACKAGES
<https://scverse.org/packages/#core-packages>

**anndata**
<https://anndata.readthedocs.io/en/latest/>
Anndata is a Python package for handling annotated data matrices in memory and on disk, positioned between pandas and xarray. anndata offers a broad range of computationally efficient features including, among others, sparse data support, lazy operations, and a PyTorch interface.

**mudata**
<https://mudata.readthedocs.io/en/latest/>
MuData is a format for annotated multimodal datasets where each modality is represented by an AnnData object. MuData's reference implementation is in Python, and the cross-language functionality is achieved via HDF5-based .h5mu files with libraries in R and Julia.


**scanpy**
<https://scanpy.readthedocs.io/en/latest/>
Scanpy is a scalable toolkit for analyzing single-cell gene expression data built jointly with anndata. It includes preprocessing, visualization, clustering, trajectory inference and differential expression testing. The Python-based implementation efficiently deals with datasets of more than one million cells.

**muon**
<https://muon.readthedocs.io/en/latest/>
muon is a Python framework for multimodal omics analysis. While there are many features that muon brings to the table, there are three key areas that its functionality is focused on.

**scvi-tools**
<https://docs.scvi-tools.org/en/stable/>
scvi-tools is a library for developing and deploying machine learning models based on PyTorch and AnnData. With an emphasis on probablistic models, scvi-tools steamlines the development process via training, data management, and user interface abstractions. scvi-tools also contains easy-to-use implementations of more than 14 state-of-the-art probabilistic models in the field.

**scirpy**
<https://icbi-lab.github.io/scirpy/latest/>
Scirpy is a scalable toolkit to analyse T-cell receptor or B-cell receptor repertoires from single-cell RNA sequencing data. It seamlessly integrates with scanpy and provides various modules for data import, analysis and visualization.

**squidpy**
<https://squidpy.readthedocs.io/>
Squidpy is a tool for the analysis and visualization of spatial molecular data. It builds on top of scanpy and anndata, from which it inherits modularity and scalability. It provides analysis tools that leverages the spatial coordinates of the data, as well as tissue images if available.

## ECOSYSTEM
<https://scverse.org/packages/#ecosystem>
A broader ecosystem of packages builds on the scverse core packages. These tools implement models and analytical approaches to tackle challenges in spatial omics, regulatory genomics, trajectory inference, visualization, and more.

<https://github.com/morris-lab/CellOracle>


## MISSION
<https://scverse.org/blog/hello-world/>
<https://scverse.org/about/mission/>
scverse is a consortium of foundational tools (mostly in Python) for omics data in life sciences. It has been founded to ensure the long-term maintenance of these core tools.


## TEAM
<https://scverse.org/join/>
<https://scverse.org/people/>
