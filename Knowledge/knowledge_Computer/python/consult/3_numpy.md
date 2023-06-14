Numpy是python的一种开源的数值计算扩展；Numpy可用来存储和处理大型矩阵；Numpy支持大量的维度数组与矩阵运算。

# 数据类型

Numpy最基本最常用的数据类型是**ndarray（n维数组）**，其中的很多方法也是针对ndarray对象而开发的；

其与python自带数据类型list（列表）基本无差别；

因此对于list对象的操作都可以运用到ndarray对象上。

```python
import numpy as np
```

# 数据生成

np.array
`np.array([1,2,3,4,5])`

np.arange
`np.arange(1,10)`

np.linspace
`np.linspace(1,10,10)`

np.ones
`np.ones((2,2))`

np.ones_like
`np.ones_like([[1,2,3],[3,2,1]])`

np.zeros
`np.zeros((3,2))`

np.zeros_like
`np.zeros_like([[3,2,1],[1,2,3]])`

np.empty
`np.empty((3,4))`

np.empty_like
`np.empty_like([[1,2,3],[3,2,1]])`

## 最基本结构np.array

```python
#手动指定元素
np.array([1,2,3,4,5])
np.array([[1,2,3],[4,5,6]])
```

## 等差数列 np.arange/linspace

```python
#按范围指定自然数列
np.arange(1,10)

#生成等差数列 首项、尾项、项数
np.linspace(1,10,10)
```

## 返回全是1的矩阵 np.ones

```python
#返回一个全1的n维数组 设定维度
np.ones((2,3))

#array([[1., 1., 1.],
#       [1., 1., 1.]])
##第一个数字是行，第二个数字是列




#生成一个全是1的矩阵 维度类似于括号里的矩阵
np.ones_like([[1,2,3],[3,2,1]])
```

## 返回全是0的矩阵 np.zeros

```python
#返回一个全0的n维数组 定维度
np.zeros((3,2))

#生成一个全是0的矩阵 维度类似于括号里的矩阵
np.zeros_like([[3,2,1],[1,2,3]])
```

## 返回空值矩阵 np.empty

```python
#根据给定的维度和数值类型返回一个新的数组
np.empty((3,4))

#shape  整数或者整数组成的元组 空数组的维度，例如：(2, 3)或者2

#dtype  数值类型，可选参数   指定输出数组的数值类型，例如numpy.int8。默认为numpy.float64。

#

np.empty_like([[1,2,3],[3,2,1]])
```

## 返回对角矩阵 eye

创建一个N * N的单位矩阵（对角线为1，其余为0）:

```
numpy.eye(N, M=None, k=0, dtype=<class 'float'>, order='C')
```

## 生成随机数np.random

np.random模块可以用于生成呈各种分布的数据

```
np.random.seed(1234)#设置随机种子为1234
```

np.random.rand
`np.random.rand(2,3)`

np.random.randn
`np.random.randn(3,4)`

np.random.gamma
`np.random.gamma(3,10)`

np.random.normal
`np.random.normal(0,1)`

np.random.randint
`np.random.randint(0,10,10)`

```python
#均匀分布
np.random.rand(2,3)

#标准正态分布
np.random.randn(3,4)

np.random.gamma(3,10)

np.random.normal(0,1)

np.random.randint(0,10,10)
```

# 数据读取

```python
#将文件读入数组中

import numpy as npdata = np.loadtxt("data.txt")  
 #将文件中数据加载到data数组里
```

# 数据查看

## 切片

类似于列表

```python
# 一维数组
arr = np.array([1,2,3,4,5])
arr[3:]
arr[:-5]

# 多维数组
arr = np.ones((5,4))
arr[:3,:4]

```

## np.ndim 几维矩阵

```
np.ones((3,4)).ndim
```

## np.size 元素数

`np.size(np.ones((3,4)))`

## np.shape 宽高

`np.shape(np.ones((3,4)))`

## np.dtype 元素数据类型

```
np.ones((3,4)).dtype
```

# 数据变化

## 追加append

将值添加到数组末尾,输入数组的维度必须匹配否则将生成ValueError。

```python
numpy.append(arr, values, axis=None)


ndarray5 = np.append(ndarray2,[[0,0,0]],axis=0)

```

## 删除 delete

删掉某个轴的子数组，并返回删除后的新数组。

```python
Numpy.delete(arr, obj, axis) 


ndarray6 = np.delete(ndarray2,2,axis=0)


```

## 切分np.split

```python
np.split(np.ones((4,4)),2,axis=0)
#第二个参数是切分成几部分
#0横向切分

np.split(np.ones((4,4)),2,axis=1)
#1横纵向切分
```

## 维度重构.reshape

```python
# ndarray维度变化
np.ones((3,4)).reshape(2,6)
```

## 合并np.concatenate☆

```python
# ndarray合并
np.concatenate(np.ones((3,4)))
#array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])
#对一个矩阵用，会拉平

np.concatenate((a,b),axis=0)
#b在a下方
		#注意a和b要用括号括起来

np.concatenate(a,b,axis=1)
#b在a右方
		#可以任意多个项，这点远超R语言
```

## 转置transpose

```python
np.ones((3,4)).transpose( )
```

## 数组折叠为一维.flatten()

对数组进行降维，返回折叠后的一位数组

```python
np.arange(0,12).reshape(3,4).flatten( )
```

所以拉平就是把每一行接起来？？？

## 随机排列修改原数组 random.shuffle

shuffle(a) : 根据数组a的第一轴进行随机排列，改变数组a

```python
np.random.shuffle(a)
```

多维矩阵按照第一维打乱

permutation可以传入一个整数，它会返回一个洗牌后的arrange；而shuffle不能传入整数。

## 随机排列产生新数组 random.permutation☆

permutation(a) : 根据数组a的第一轴进行随机排列， 但是不改变原数组，将生成新数组

```python
np.random.permutation(a) 
```

## choice 概率抽取

从一维数组a中以概率p抽取元素， 形成size形状新数组，replace表示是否可以重用元素，默认为False。

```python
np.random.choice(a, size, replace, p) 
```

# 数值计算

np.sin
`np.sin(10)`

np.cos
`np.cos(60)`

np.exp
`np.exp(4)`

np.power
`np.power(2,3)`

```
np.sin(10)
np.cos(60)
np.exp(4)
np.power(2,3)
```

`add(x,y)`

数组的对应元素相加x+y

`subtract(x,y)`

数组的对应元素相减x-y

`multiply(x,y)`

数组的对应元素相乘x*y

`divide(x,y)`

数组的对应元素相除x/y

# 数据分析

np.abs
`np.abs(np.arange(-5,4))`

np.sum
`np.sum([1,2,3])`

np.var
`np.var([1,2,3])`

np.std
`np.std([1,2,3])`

np.mean
`np.mean([1,2,3])`

np.sqrt
`np.sqrt([4,9,16])`

np.floor
`np.floor([2.1,3.7,4.3])`

np.ceil
`np.ceil([2,1,3.7,4.3])`

np.median
`np.median([3,2,4])`

np.cumsum累加
`np.cumsum([[1,2,3],[3,2,1]])`

np.cumprod累乘
`np.cumprod([[1,2,3],[3,2,1]])`

```python
np.abs(np.arange(-5,4))

np.sum([1,2,3])

np.var([1,2,3])

np.std([1,2,3])

np.mean([1,2,3])

#开方
np.sqrt([4,9,16])

#向下取整
np.floor([2.1,3.7,4.3])

#向上取整
np.ceil([2.1,3.7,4.3])

#逐元素求和
np.cumsum([[1,2,3],[3,2,1]])
#array([ 1,  3,  6,  9, 11, 12])

#逐元素求乘
np.cumprod([[1,2,3],[3,2,1]])
#array([ 1,  2,  6, 18, 36, 36])
```

# 索引

```python
np.argmin([4,2,1,6,8])
np.argmax([4,2,1,6,8])
```
