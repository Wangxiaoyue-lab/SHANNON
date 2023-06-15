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

```python
from rpy2.robjects import globalenv
rdf = pandas2ri.py2rpy(df)
globalenv['rdf'] = rdf
```



# 运行R脚本

```python

rscript = """
x <- iNEXT(rdf)
data <- x$iNextEst
"""
r(rscript)


```
