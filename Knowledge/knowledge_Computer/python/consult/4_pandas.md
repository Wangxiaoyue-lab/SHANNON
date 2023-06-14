Series就是数据框的一列，有行名有列名

`Pandas` 是基于 `NumPy` 开发的，可以与其它第三方科学计算库完美集成。

# 数据结构

`Pandas` 主要的数据结构是

`Series`（一维）与 `DataFrame`（二维），使用这两种数据结构就足以应对金融、统计、社会科学、工程等领域里的大多数数据了。

相较于 `R` 语言的 `data.frame`，`DataFrame` 提供了更加丰富的功能。

# 安装

```
pip install pandas
```

# 加载

在Python中要使用数据框的类型，需要加载pandas模块。

```python
#加载包
import numpy as np
import pandas as pd
```

# 关键属性

- axis参数  0是行 1是列
  axis=0和axis=1参数到底怎么用经常混淆，到底是对列还是对行操作。其实这么理解比较好，
  axis=0是默认的，按行进行操作，其实就是对列进行一系列操作;
  axis=1需要自己设置，按列进行操作，其实就是对行进行一系列操作。
  [1是竖过来的]
- columns是列名 index是行名

分别返回列名和行名

```
提取某列时候方括号里写列名
cho_pri.obs['source']
```

- `Series`：只有 `index`
- `DataFrame`：`index` 和 `columns`
- 名称属性name

```python
s = pd.Series(np.random.randn(5), name='something')

 s
Out[34]:
0    1.926419
1    1.175251
2   -0.568534
3   -0.014069
4    1.401082
Name: something, dtype: float64

In [35]: s.name
Out[35]: 'something'
```

如果 Series 用于生成 DataFrame，则 Series 的名称将成为其索引或列名称。每当使用解释器显示系列时，也会使用它。

- inplace 修改覆盖

# 1 读存数据

读取表格

```python
pd.read_table('./1.txt',sep='\t',header=None)

#sep是间隔
#header是 是否有标题

```

读取csv

```python
import pandas as pd

effect=pd.read_csv("/home/caoj/data/newnet/DM+EM_scenic/effect_chu.csv")
```

保存csv

```python
pd.to_csv( sep=',', columns=None, header=True, index=True)
```

其实重要的就是指定路径和文件名，指定是否要第一行，是否要索引和列名，指定分隔符，其他都不是特别重要。

读取excel

```python
import pandas as pd

#读取工作簿和工作簿中的工作表
data_frame=pd.read_excel('E:\\研究生学习\\python数据\\实验数据\\Excel文件实验数据\\sales_2017.xlsx',sheet_name='january_2013')

```

新建一个工作簿

```python
#新建一个工作簿
writer=pd.ExcelWriter('E:\\研究生学习\\python数据\\实验数据\\Excel文件实验数据\\sale_january_2017_in_pandas.xlsx')

```

保存excel

```python
#使用to_excel将之前读取的工作簿中工作表的数据写入到新建的工作簿的工作表中
data_frame.to_excel(writer,sheet_name='jan_2017_output_sheet',index=False)
#保存并且关闭工作簿
writer.save()
```

读取pickle存储对象
pickle提供了一个简单的持久化功能。可以将对象以文件的形式存放在磁盘上。

pickle模块只能在python中使用，python中几乎所有的数据类型（列表，字典，集合，类等）都可以用pickle来序列化，

```python
pd.read_pickle(target_fname, compression='xz')

```

# 2 构建数据

## DataFrame数据框

 列名都要单独指定

### 字典变数据框

先生成字典，再变成数据框

```python
>>> df = pd.DataFrame({
    'brand': ['YumYum','YumYum', 'YumYum', 'Indomie', 'Indomie', 'Indomie'],
    'style': ['cup','cup', 'cup', 'cup', 'pack', 'pack'],
    'rating': [4, 4, 4, 3.5, 15, 5]})
>>> df
     brand style  rating
0   YumYum   cup     4.0
1   YumYum   cup     4.0
2   YumYum   cup     4.0
3  Indomie   cup     3.5
4  Indomie  pack    15.0
5  Indomie  pack     5.0
```

```python
data = {'year': [2010, 2011, 2012, 2010, 2011, 2012, 2010, 2011, 2012],
        'team': ['FCBarcelona', 'FCBarcelona', 'FCBarcelona', 'RMadrid', 'RMadrid', 'RMadrid', 'ValenciaCF',
                 'ValenciaCF', 'ValenciaCF'],
        'wins':   [30, 28, 32, 29, 32, 26, 21, 17, 19],
        'draws':  [6, 7, 4, 5, 4, 7, 8, 10, 8],
        'losses': [2, 3, 2, 4, 2, 5, 9, 11, 11]
        }
football = pd.DataFrame(
    data, columns=['year', 'team', 'wins', 'draws', 'losses'])
football
```

### 用numpy的array

```python
 df = pd.DataFrame(np.random.randn(8, 3), index=index, columns=["A", "B", "C"])
```

## ~~Series  不重要~~

Series 是带标签的一维数组，可以*存储任意数据类型*，
如整数、浮点数、字符串、Python 对象等类型的数据。

轴标签称为索引（index），可以使用 pd.Series 函数来创建

```
 s = pd.Series(data, index=index)
```

其中，`data` 可以是

- `python` 字典
- 多维数组ndarray
- 标量值（如 `5`） `index` 是对应的标签列表。

> index是行名

根据不同的数据类型，分为以下几种情况：

**ndarray+index**
当 `data` 是多维数组时，`index` 长度必须与 `data` 长度一致。
如果没有指定 `index` 参数，会自动创建递增的数值型索引，即 `[0, ..., len(data) - 1]`

```
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
 
a    0.469112
b   -0.282863
c   -1.509059
d   -1.135632
e    1.212112
dtype: float64
```

`<mark class="hltr-red">`提取行名.index `</mark>`

```python
s.index
#Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
```

_注意_：`Pandas` 的索引值是可以重复，如果使用不支持重复索引的操作会触发异常。

`<mark class="hltr-purple">`提取列名.columns `</mark>`

```python
df = pd.DataFrame(np.random.randn(5, 3), index=['a', 'b', 'c', 'd', 'e'], columns=["A", "B", "C"])

df.columns
#Index(['A', 'B', 'C'], dtype='object')
```

**字典直接转换**
可以使用字典实例化 `Series`：

```
d = {'b': 1, 'a': 0, 'c': 2}

pd.Series(d)
Out[9]:
b    1
a    0
c    2
dtype: int64
```

_注意_：当 `data` 为字典，且未设置 `index` 参数时，如果 `Python` 的版本 >= 3.6 且 `Pandas` 的版本 `>= 0.23`，`Series` 会按字典的*插入顺序*对索引排序。
在其他版本中 `Series` 会按字典的键（`key`）的*字母顺序*排序列表。
也就是说，如果我们的 `Python < 3.6` 或 `Pandas < 0.23`，上面的结果将是

```text
a    0
b    1
c    2
dtype: int64
```

设置index参数
会将 `data` 的数据根据 `index` 的顺序排序显示，不存在的值会复制为 `NaN`

```
d = {'a': 0., 'b': 1., 'c': 2.}
pd.Series(d, index=['b', 'c', 'd', 'a'])
 
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64
```

_注意_：在 `pandas` 中使用 `NaN(Not a Number)`表示缺失值

**标量值**
如果 `data` 是标量值，那么必须设置索引值。`Series` 会按索引的长度重复该标量值。

```PYTHON
In [12]: pd.Series(5., index=['a', 'b', 'c', 'd', 'e'])
Out[12]:
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64
```

# 3 转换数据

## ~~series~~

`<mark class="hltr-orange">`Series 转换 numpy 数组array `</mark>`

```
s.array
   
   
<PandasArray>
[-1.9744628992708957,  1.9260314025924432,  0.6598612596804069,
 -1.3863546449807986,  1.0776610911873974]
Length: 5, dtype: float64
```

```
s.to_numpy()

array([-1.9744629 ,  1.9260314 ,  0.65986126, -1.38635464,  1.07766109])
```

## pandas的df转numpy

```
df.to_numpy()
```

## df转字典

```
df.to_dict()
```

## df转列表

```
df.tolist()
```

## get_dummies|转换为onehot编码

```python
#提取某列
pd.get_dummies(data_b.C)
-- 输出
   1  2
0  0  1
1  0  1
2  1  0
3  0  1

```

onehot编码又叫独热编码，其为一位有效编码，主要是采用N位状态寄存器来对N个状态进行编码，每个状态都由他独立的寄存器位，并且在任意时候只有一位有效。
Onehot编码是分类变量作为二进制向量的表示。这首先要求将分类值映射到整数值。然后，每个整数值被表示为二进制向量，除了整数的索引之外，它都是零值，它被标记为1。
使用onehot有什么好处？
one hot编码是将类别变量转换为机器学习算法易于利用的一种形式的过程。
这样做的好处主要有：
1.解决了分类器不好处理属性数据的问题
2.在一定程度上也起到了扩充特征的作用

我们通常使用onehot编码来处理离散型的数据。
原因如下：
在回归，分类，聚类等机器学习算法中，特征之间距离的计算或相似度的计算是非常重要的，常用的距离或相似度的计算都是在欧式空间的相似度计算，计算余弦相似性，基于的就是欧式空间。
使用one-hot编码，将离散特征的取值扩展到了欧式空间，离散特征的某个取值就对应欧式空间的某个点。
将离散型特征使用one-hot编码，会让特征之间的距离计算更加合理。

> 比如，一个离散型特征，代表工作类型，共有三个取值，不使用one-hot编码，其表示分别是:x_1 = (1), x_2 = (2), x_3 = (3)。
> 两个工作之间的距离:d(x_1, x_2) = 1, d(x_2, x_3) = 1, d(x_1, x_3) = 2。
> 那么x_1和x_3工作之间就越不相似吗？
> 显然这样的表示，计算出来的特征的距离是不合理。
> 如果使用one-hot编码，则得到x_1 = (1, 0, 0), x_2 = (0, 1, 0), x_3 = (0, 0, 1)，**那么两个工作之间的距离就都是sqrt(2).即每两个工作之间的距离是一样的，显得更合理。**

补充一下：**不需要使用one-hot编码来处理的情况**
将离散型特征进行one-hot编码的作用，**是为了让距离计算更合理，但如果特征是离散的，并且不用one-hot编码就可以很合理的计算出距离，那么就没必要进行one-hot编码。**
比如，该离散特征共有1000个取值，我们分成两组，分别是400和600,两个小组之间的距离有合适的定义，组内的距离也有合适的定义，那就没必要用one-hot 编码。
离散特征进行one-hot编码后，编码后的特征，其实每一维度的特征都可以看做是连续的特征。

除此，**one-hot编码要求每个类别之间相互独立，如果之间存在某种连续型的关系，或许使用distributed respresentation（分布式）更加合适。**

## 长宽数据转化

### melt 宽转长

```python
df.melt(id_vars=['A','B'])

```

```python
DataFrame.melt(id_vars=None, value_vars=None, var_name=None, value_name=‘value’, col_level=None, ignore_index=True)

```

`melt`方法叫做数据融合，是dataFrame拥有的方法，使用较为频繁

- id_vars:[tuple, list, ndarray]，列中识别符变量， 不参与融合   。
- value_vars:[tuple, list, ndarray]，列中融合变量，默认全部融合。
- var_name:[scalar]，融合后变量名字  ，默认variable。
- value_name:[scalar]，融合后值名字，默认value。
- col_level:[int, str]，多重列索引时选择列。
- ignore_index:[bool]，融合后索引是否重新排序，默认True。

### wide_to_long 宽转长 涉及新变量

```

pandas.wide_to_long(df, stubnames, i, j, sep=’’, suffix=’\d+’)
```

df:[pd.dataframe]，宽型数据框
stubnames:[str,list-like]，列名中的存根名字
i:[str,list-like]，列中的索引变量
j:[str]，后缀的重命名
sep:[str,default “”]，存根名与后缀之间的分隔符
suffix:[str,default “\d+”]，后缀

### pivot 长转宽|转换为透视表

```python
data = {'date': ['2018-08-01', '2018-08-02', '2018-08-03', '2018-08-01', '2018-08-03', '2018-08-03',
                 '2018-08-01', '2018-08-02'],
        'variable': ['A','A','A','B','B','C','C','C'],
        'value': [3.0 ,4.0 ,6.0 ,2.0 ,8.0 ,4.0 ,10.0 ,1.0 ]}
df = pd.DataFrame(data=data, columns=['date', 'variable', 'value'])
df1 = df.pivot(index='date', columns='variable', values='value')
```

## 因子化
factorize 得到因子与坐标
```python
import pandas as pd

# 创建一个包含类别数据的序列
data = pd.Series(['apple', 'banana', 'apple', 'cherry', 'banana'])

# 使用 factorize 函数对数据进行编码
codes, uniques = pd.factorize(data)

# 输出结果
print('Codes:', codes)
print('Uniques:', uniques)

#Codes: [0 1 0 2 1]
#Uniques: ['apple' 'banana' 'cherry']
```
用于对类别数据进行编码。它接受一个序列作为输入，并返回两个数组：一个整数编码数组和一个唯一值数组。
、、、


Categorical 分类变量
节省内存，提高性能
```python
#创建分类变量列
df["A"] = pd.Categorical(['a','c','a','f'])

#修改为分类变量
df['A'] = df['A'].astype('category')

```


# 4 查询数据

## .values 返回numpy值

可以返回对应的 `Numpy` 值数组。

## head 和 tail

```text
long_series.head()
 
long_series.tail(3)
 
```

## shape 对象的轴维度

```
.shape
```

## describe 描述性统计| 类似于R的summary

```python
df.describe()

```

，没有任何参数，直接对着你的数据框用就可以了，输出的是对于你数据框整体数据的描述。

## info函数|查看基本信息 包括尺寸

 这个函数用来查看数据的类型，有没有缺失值，在进行特征工程的时候比较好用。

```python
 data_a.info()
--输出
Data columns (total 4 columns):
1    3 non-null int64
2    3 non-null int64
3    3 non-null int64
4    3 non-null int64
dtypes: int64(4)
memory usage: 176.0 bytes
```

## duplicated检查重复值返回bool

`<mark class="hltr-red">`看是否有行完全重复 `</mark>`

**keep参数是描述要保留的行**

```
df.duplicated()
df.duplicated(keep='first')

```

默认情况下，对于每一组重复的值，第一次出现的值设置为False，其他所有值设置为True

```
df.duplicated(keep='last')
```

通过使用' last '，每组重复值的最后一次出现被设置为False，而其他所有重复值被设置为True。

```
df.duplicated(keep=False)
```

通过将keep设置为False，所有重复项都为True。

`<mark class="hltr-blue">`特定列 `</mark>`

```
df.duplicated(subset=['brand'])
```

## isX()判断列类型一致性

```
isalpha ( ) 判断是否全是字母
isdigit ( ) 判断是否全是数字
isalnum ( ) 是否全是数字或字母
isupper ( ) ... 大写字母
islower ( ) ... 小写字母
istitle ( ) ... 首字母大写
isspace() ... 空白字符

```

# 5 修改数据

## rename修改指定行名列名

其实可以直接指定，但是得全长

```
df1.columns=['a','B','c']
```

这个函数是对列名进行更改，通常在读数据的时候可以设置header=True读取第一行作为列名，或者人为给定columns参数，但是很多时候我们需要对列名进行更改，此时用rename函数，基本用法为

```python

df.rename(index=str, columns={"A": "a", "B": "c"},inplace=True)

’inplace’参数不用的话会修改失败
```

 rename方法对 Series 重命名

```
In [36]: s2 = s.rename("different")

In [37]: s2.name
Out[37]: 'different'
```

## drop_duplicates去除重复值

```python
data.drop_duplicates(subset=['A','B'],keep='first',inplace=True)
```

subset指明了要去除重复值的列，keep指明要保留的行
inplace=True是让修改结果覆盖原来结果

## drop去除某行某列

```python
data_b.drop(columns=['A'])
data_b.drop('A',axis=1)

```

默认删除的是行，如果想删除列的话，得指定axis=1。也可以直接指定index或columns确定要删除的行或列。

### 删除行.drop

```python
df=df.drop(df[df.score<50].index)

df21=df1.drop(labels=[1,3],axis=0)   
# axis=0 表示按行删除，删除第1行和第3行

#要删除连续的多行可以用range(),删除连续的多列不能用此方法
df22=df1.drop(labels=range(1,4),axis=0)   
# axis=0 表示按行删除，删除索引值是第1行至第3行的正行数据


#这样才行！！！
coef_mat_raw=coef_mat_raw.drop(coef_mat_raw.index[range(0,4)],axis=0)

```

### 删除列.drop

```
df.drop("",axis=1)#不改变原始数据
df.drop("",axis=1,inplace="True")#改变原始数据
```

## replace指定坐标数据替换

```python
data_b.replace({'A':1,'B':3},10)

data_b.replace(to_replace={'A':1,'B':3},value=10)
```

其中to_replace的字段可以用字典表示

### loc坐标式替换

```
# 直接等于要的值
df.loc['a':'c',['A','C']]=x
```

## 修改数据类型

### 整体

```
pd.to_numeric(s)
pd.to_string(s)
```

### 某列

☆dtype 属性获取数据的类型

```python
df.dtypes
```

☆astype() 设置数据的类型

```python
df[['two', 'three']] = df[['two', 'three']].astype(float)
```

```python

df[['col2','col3']] = df[['col2','col3']].apply(pd.to_numeric)
```

## 缺失值处理

### isnull/notnull判断

```
data_b.isnull()
```

这两个函数是对元素级别的操作，即对每一个位置判断是否是空值，isnull的功能是如果是空值就返回Ture，否则返回False，notnull和isnull的作用相反。由于是元素级的操作，因此返回的是和原dataframe行列一样的包含逻辑True和False的dataframe，用于布尔型的逻辑判断。

### dropna丢失含缺失值的行列

```
data_b.dropna()

```

dropna这个函数就是移除缺失值，但是针对series和dataframe它的操作逻辑是不一样的。对于series只是把NA值删除，而对于dataframe是把包括NA值的行或列都删除，默认axis=0。更多用法参考请参考官方文档。dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)，介绍一个thresh参数如果设置了这个值，那么只有含大于这个值的非空行/列才会保留，还有subset参数指定删除哪一行或者哪一列包含空值的数据。

### fillna填充缺失值

```
data_c.fillna(10)
```

对缺失值进行填充处理，fillna(value=None, method=None, axis=0) ，method包括'backfill', 'bfill', 'pad', 'ffill', None这几个用法各不相同。首先fillna只有一个value，就是全部空值填充。当然value也可以用字典填充，指定特定的列填充不同的值{'A':10,'B':20}。说完这个接着说method，pad和ffill类似，都是前项填充，backfill和bfill都是后项填充，可以用limit限制最多向前寻找几个值，用axis设置填充方向。

## 小数位 round

```
df['A'].round()
```

## shift函数  | df整体移动

```
data_b.shift()
-- 输出
   A    B    C    D
0  NaN  NaN  NaN  NaN
1  1.0  1.0  2.0  2.0
2  2.0  3.0  2.0  2.0
3  2.0  3.0  1.0  4.0
```

如果不理解上面两个函数的操作，那么来讲一下shift操作，其实就是数据的移动，是上面两个函数计算的中间步骤。基本用法为

```
shift(periods=1, freq=None, axis=0)
```

，这个和diff函数一样一样的，这个freq我稍微解释一下就是按什么技术，默认就是int，还有datetime的例子，因为这个一般用于时间序列的处理。
      

# 6 提取数据

## ~~series~~

 series和numpy的ndarray
`Series` 的行为与 `ndarray` 类似，支持大部分的 `NumPy` 函数，同时还支持索引切片。

 **索引**
`Series` 类似字典，可以用索引标签提取值或设置对应的值

```python
In [22]: s['b']
Out[22]: 1.9260314025924432

In [23]: s['e'] = 12.

In [24]: s
Out[24]:
a    -1.974463
b     1.926031
c     0.600000
d    -1.386355
e    12.000000
dtype: float64

In [25]: 'e' in s
Out[25]: True

In [26]: 'f' in s
Out[26]: False
```

 **切片**

```python
In [13]: s[0]
Out[13]: -1.9744628992708957

In [14]: s[:3]
Out[14]:
a   -1.974463
b    1.926031
c    0.659861
dtype: float64

In [15]: s[s > s.median()]
Out[15]:
b    1.926031
e    1.077661
dtype: float64

#切片 切第5/4/2个
In [16]: s[[4, 3, 1]]
Out[16]:
e    1.077661
d   -1.386355
b    1.926031
dtype: float64

 
>>> s[[4, 3, 1, 1]]
e    1.077661
d   -1.386355
b    1.926031
b    1.926031
dtype: float64



In [17]: np.exp(s)
Out[17]:
a    0.138836
b    6.862223
c    1.934524
d    0.249985
e    2.937800
dtype: float64
```

## dataframe

### 提取index？

```python
 
s.index.array
```

### df['A'] | 取指定列

```
df['A']
# A是column中的一个
```

### 二元切片 行是字典，列是list

#### loc | 按名字切

```python


[单独只写一个应该就是按行切]
#按index切  loc 

#取第'a'行，'A'列
df.loc['a','A']

#取第'a'行到第'c'行，'A'列及'C'列
df.loc['a':'c',['A','C']]
#如何只取列，行也需要：作为占位符

```

#### iloc | 按坐标数字切

```python
#按数字切   iloc

#取第1行，2列
df.iloc[1,2]

#取第1行到第3行，2列及3列
df.iloc[1:3,[0,2]]

#第3列 
df.iloc[:, 2]  

#从开头到第二行，第2列 到最后
df.iloc[:2,1:]


df.iloc[:, :-1]

df.iloc[:, -1:]
```

### 按条件

```python
#选取等于某些值的行记录 用 ==

df.loc[df['A']==some_value]
#即某列里等于某数的行
 
df.loc[df['A']>1]
#     A        B         C
#a  1.1604  0.7533  1.2389
#b  1.1449 -0.2637 -1.5948
#e  3.0491  0.5017  2.1185
#注意筛出来的是行而不仅仅是A列里的元素

#选取某列是否是某一类型的数值 用 isin
df.loc[df['column_name'].isin(some_values)]

 

#多种条件的选取 用 &

df.loc[(df[‘column’] == some_value) & df[‘other_column’].isin(some_values)]

 

#选取不等于某些值的行记录 用 ！=
df.loc[df[‘column_name’] != some_value]

 

#isin返回一系列的数值,如果要选择不符合这个条件的数值使用~
df.loc[~df[‘column_name’].isin(some_values)]
```

## 抽样 sample

```
df['A'].sample()
```

# 7 数据合并

## merge | 只能合并两个

```python
data_a.merge(data_b,left_index=True,right_index=True,how='outer')
```

这个函数是将两个dataframe进行合并，需要指明用于合并的列，即on参数，可以理解为sql的join操作。

基本用法为

```
merge(df1,df2, how=‘inner’, on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=(’_x’, ‘_y’))
```

，要解释的参数是how，选择连接的方式，有outer、inner、left、right四个参数可供选择，跟sql一样，不多说了。
left_on和right_on指定左右连接的主键，如果两个主键名字一样，用on就行了。
left_index和right_index指定用左右的索引作为连接键。suffixes列名重复了怎么办，用下划线区分。
最后注意一点，一次只能merge两个dataframe。

## concat函数 | 合并多个函数☆

```python
pd.concat([data_b,data_b], axis=0)
```

  数据拼接函数，可以拼接series，dataframe等，与merge不同，concat可以连接很多个dataframe，基本用法为

```
pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,
keys=None, levels=None, names=None, verify_integrity=False)
```

，默认是按行的方向堆叠。其他参数都不是特别重要，一般也不用设置，当然有时候根据使用场景还是要调整一下。这个要注意objs参数是dataframe或者series构成的列表。不能写一个dataframe，否则会报错。

# 8 批处理

## groupby分组处理

 groupby有两种典型用法：

```
grouped=df['列a'].groupby([df['列b'],df['列c'])
```

```
grouped=df.groupby(['列b','列c'])['列a']
```

  这两种写法是一样的意思，用哪个都可以，但是应用的时候要注意特殊的写法。

[单纯groupby是没用的，要配合agg函数]

```
data_b.groupby(['B','C']).agg([sum,np.mean,max])
data_b.groupby(['B']).agg({'A':sum,'B':np.mean})
```

## applymap 元素批处理

```
data_b.applymap(lambda x: x*x)
```

 map函数是元素级别的操作，这个要注意什么呢，你不能对着一个dataframe进行map操作，要用map首先得是一个series，然后是对里边的元素进行操作。如果你想对数据框进行操作怎么办，有一个applymap函数，是对数据框里的所有元素进行操作。
 

## apply 行列批处理

```
df[['c3','c5']] = df[['c3','c5']].apply(pd.to_numeric)
```

```python
df.apply(func, *args, **kwds)
```

 `apply()` 使用时，通常放入一个 `lambda` 函数[表达式](https://so.csdn.net/so/search?q=%E8%A1%A8%E8%BE%BE%E5%BC%8F&spm=1001.2101.3001.7020)、或一个函数作为操作运算

- `func`：函数或 [lambda](https://so.csdn.net/so/search?q=lambda&spm=1001.2101.3001.7020) 表达式,应用于每行或者每列

```python
lambda x: x % 3 == 0

lambda x : True if x % 3 == 0 else False
```

- `axis`：{0 or ‘index’, 1 or ‘columns’}, 默认为0
  - 0 or ‘index’: 表示函数处理的是每一列
  - 1 or ‘columns’: 表示函数处理的是每一行
- raw：bool 类型，默认为 False;
  - False ，表示把每一行或列作为 Series 传入函数中；
  - True，表示接受的是 ndarray 数据类型；

## agg 多函数批处理

```python
 agg函数也有两种写法：
 
df.groupby(['列b']).agg([sum,mean,max])

df.groupby(['列b']).agg({'列a':sum,'列b':np.mean})

```

agg函数和apply函数其实是一样的，只不过它可以指定不同的列用不同的函数，或者指定所有列用不同的函数，可供选择的范围更广。

# 数据统计

注意类似于mean sum 要给方向
axis=1 本来是列，但是对列统计所以是横过来的
axis=0本来是行，但是对行统计所以是竖过来的

## 均值 np.mean

```
不要直接用mean
用np.mean
```

## std和var函数

返回标准差和方法，计算方法是除以样本数-1(n-1)。

## corr函数| 列相关性

这个函数返回的是各列的相关系数，[-1,1]之间，1代表严格正相关，-1代表严格负相关。
基本用法为：

```
corr(method='pearson', min_periods=1)
```

，其中method可选{‘pearson’, ‘kendall’, ‘spearman’}，默认用pearson，min_periods样本最少的数据量，默认是1。

```
data_b.corr()
--输出
          A         B         C         D
A  1.000000  1.000000 -0.333333  0.333333
B  1.000000  1.000000 -0.333333  0.333333
C -0.333333 -0.333333  1.000000 -1.000000
D  0.333333  0.333333 -1.000000  1.000000
```

## 非NA计数 count

```
data_b.count()
```

计数函数，对dataframe里的数据进行非NA值的统计。这个是对所有元素进行的无差别操作，就是有一个非NA值就加1。

## 某列元素统计 value_counts

但是有的时候我们需要对于某一列中有哪些不同元素进行计数统计，这个时候就要用value_counts函数，由于这是元素级别的操作，所以这个只能对dataframe的某一列就是series使用。

```
data_b['A'].value_counts()
```

## 不重复值 unique

```
data_b['A'].unique()
```

## 累加累乘  cumsum函数和cumprod函数

这个是对行列操作的函数，返回的是与原dataframe大小完全一样的dataframe，不过是对结果进行累加和累乘操作了。

```
data_b.cumsum()
data_b.cumprod()
```

## 计算协方差 cov

## 计算分位数 quantile

# 矢量操作

## series

```
In [30]: s + s
Out[30]:
a    -3.948926
b     3.852063
c     1.200000
d    -2.772709
e    24.000000
dtype: float64

In [31]: s * 2
Out[31]:
a    -3.948926
b     3.852063
c     1.200000
d    -2.772709
e    24.000000
dtype: float64
```

Series 之间的操作会自动对齐标签数据，因此，不用顾及执行计算操作的 Series 是否有相同的标签。

```
In [32]: s[1:] + s[:-1]
Out[32]:
a         NaN
b    3.852063
c    1.200000
d   -2.772709
e         NaN
dtype: float64
```
