# R 里面调用 python: reticulate

```R
library(reticulate)
Sys.setenv(RETICULATE_PYTHON="/public/home/luoliheng//miniconda3/envs/py1/bin/python")
```

在 Python 中， _和\*\*是用于解包参数的操作符。_ 用于解包可迭代对象，**用于解包字典对象。在函数定义中，\*args 和**kwargs 通常用于接收任意数量的位置参数和关键字参数。 _args 表示接收任意数量的位置参数，\*\*kwargs 表示接收任意数量的关键字参数。在函数调用中，_ 和\*\*用于将可迭代对象和字典对象解包为位置参数和关键字参数。

MuDataSeurat

[PMBio/MuDataSeurat: .h5mu files interface for Seurat (github.com)](https://github.com/PMBio/MuDataSeurat)

adata[["connectivities"]] <-NULL

adata[["distances"]] <-NULL

在 Python 中，`...`是一个特殊的值，也称为 `Ellipsis`。它主要用于 NumPy 和 Pandas 等库中，用于表示多维数组切片时省略的维度。例如，在 NumPy 中，如果您有一个三维数组 `arr`，您可以使用以下代码来获取第一维的第一个元素：

```python
arr[0, ...]
```

这等同于：

```python
arr[0, :, :]
```

除此之外，`...`也可以用作函数定义中的占位符，表示函数体尚未实现。例如：

```python
def my_function():
    ...
```
