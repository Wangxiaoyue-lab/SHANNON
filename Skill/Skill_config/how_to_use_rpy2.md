参考资料：
[在 python 中使用 R ｜ rpy2 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/422482178)
[rpy2 库 | 在 jupyter 中调用 R 语言代码\_邓旭东 HIT 的博客-CSDN 博客](https://blog.csdn.net/weixin_38008864/article/details/108090921) # 比较全面的 rpy2 使用，包括 jupyter 示例
[【Jupyter 教程 1】如何优雅地在 Jupyter Notebook 中同时运行 R 和 IPython - 知乎](https://zhuanlan.zhihu.com/p/139032905)

# rpy2 安装

# rpy2 配置

```python
import rpy2
import os

os.environ['R_HOME'] = '/public/software/apps/R-4.2.1-gcc910/lib64/R'

from rpy2.robjects import r, pandas2ri # r可以看成调用r的通道，括号内部的对象会当做r代码执行
from rpy2.robjects.packages import importr # importr可以将r包当成对象来使用，其中的函数成为方法
pandas2ri.activate() # pandas2ri 使得pandas和data.frame自动转换

```

# 导入包

```python
ggplot2 = importr("ggplot2")
```

# 数据转换

## pandas转data.frame

```python
rdf = pandas2ri.py2rpy(df)

```

# 传递变量R
方式1
```python
r.assign('rdata',pydata)
```

方式2
```python
import rpy2.robjects as ro

ro.globalenv['rdf'] = rdf
```
anndata可以用方式2传递

# 运行R脚本
```python
# 运行R当行命令
r("seurat_obj <- CreateSeuratObject(mat)")
# R结果传递给python变量
var = r("seurat_obj@assays$SCT@var.features")
```

```python
rscript = """
x <- iNEXT(rdf)
data <- x$iNextEst
"""
r(rscript)
```

# jupyter

## 安装需要的 R 包

在 R Studio console 中运行（或者 R 命令行中运行也可以）

```
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest','IRkernel'))
# 或者对于IRkernel
install.packages('devtools')
devtools::install_github('IRkernel/IRkernel')

# 将jupyter和R进行关联
# 只在当前用户下安装
IRkernel::installspec()
# 或者是在系统下安装
IRkernel::installspec(user = FALSE)
```

## 使用

先输入 `%load_ext rpy2.ipython`，使用`%%R` 符号

将整个 cell 设为 R 代码：` %%R`

将一行设为 R 代码： `%R`

从 Python 中传入数据到 R： `%R -i <Python 中的变量 >`

从 R 中传入数据到 Python： `%R -o <Python 中的变量 >`
