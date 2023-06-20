# rpy2安装

# rpy2配置

```python

import rpy2

import os

os.environ['R_HOME'] = '/public/software/apps/R-4.2.1-gcc910/lib64/R'

from rpy2.robjects import r

```

# 导入包

```python
from rpy2.robjects import r, pandas2ri
from rpy2.robjects.packages import importr
pandas2ri.activate()


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
# ipython或者jupyter使用
加载R扩展
```python
%load_ext rpy2.ipython
```

向R传入变量
```python
x = [1,2,3,4]
%R -i x
%R print(x)
#只要前面加%R 就是在R里运行0
```


从R提取变量
```python
%%R
...
...

#上面的R扩展里面计算
x = %R x

```