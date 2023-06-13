# trick技巧

如果function有多个输出，只想要某一个
利用下划线选定

```python
_,a = function()
```

# base

## 调试

对象长度

```python
len(obj)
```

对象类型

```python
type(obj)
```

对象结构

```python
dir(obj)
```

对象属性

```python
vars(obj)
```

## 迭代器

可迭代对象 iterable 包括字符串、列表、元组、字典和集合
迭代器 iterator

检查是否可迭代,是的话变成迭代器
python 使用for循环时候会自动对可迭代对象使用iter()函数

```python
iter(list)
```

提取迭代器的下一个元素

```python
next(iter)
```

等差数列

```python
range(10)
range(1,10)# 包含1不包含10
range(1,10,2)
```

## 常用处理

排序 sorted

```python
b = sorted(a,reverse=True)
```

压缩对象 zip

```python
x = [3,2,1]
y = [4,5,6]
list(zip(x,y))
```

匿名函数 lambda

```python
lambda x: x**x
```

过滤 filter

```python
filter(function,iterable)

filter(lambda x: x > 5, [1,3,5,7,9])
```

批处理

```python
map(function,iterable)

map(square, [1,4,5,6,7])
```

## 基本类型

### list

切片

```python
mylist[1:2]

mylist[::-1]#可省略参数
```

修改

```python
mylist[0]="a"
```

插入指定位置

```python
mylist.insert(3,"a")
```

插入尾部

```python
mylist.append("a")
```

合并

区分os.path.join()

```python
'_'.join(mylist)
```

删除

```python
#删除具体值
mylist.remove("a")

#删除坐标
del mylist[2]
```

### dict

双list生成

```python
mydict=dict(zip(x,y))
```

提取值 不存在就创建

```python
mydict.get("a")

#不存在就给予0值，存在就提取本来的值
mydict.get("a",0)
```

获得所有key

```python
mydict.keys()
```

获得所有value

```python
mydict.values()
```

获得所有键值对

```python
mydict.items()
```

新增

```python
score["a"]=1
```

合并

```python
dict3=dict1.dict2
```

### set

### tuple

创建

```python
t=("a",)

t=("a","b","c","d")
```

获取

```python
t[0]
```

## 生成式

列表生成式

```python
my_list = [x**2 for x in range(11) if x % 3 == 1]
```

字典生成式

```python
my_dir = [x: x**2 for x in range(11) if x % 3 == 1]
```

集合生成式

```python
my_set = {x**2 for x in range(11) if x % 3 == 1}
```

元组没有生成式

## 编译

eval 转化简单表达式为代码

```python
eval("a+b+c")
```

exec 转化复杂语句为代码

```python
exec(code)
```

compile

```python
compile(s,"<string>","exec")
```

## 缺失值与空值

空值

```python
a = None
```

缺失值

```python
numpy.nan
pandas.NaT
```

## 库、函数

函数参数传递

*args和**kwargs是Python中的两种特殊语法，用来传递可变数量的参数给函数。
*args用来传递非关键字参数。它会将传递给函数的所有非关键字参数打包成一个元组，然后传递给函数。

在函数内部，可以使用args变量来访问这些参数。
**kwargs用来传递关键字参数。它会将传递给函数的所有关键字参数打包成一个字典，然后传递给函数。

在函数内部，可以使用kwargs变量来访问这些参数。

**kwargs用来传递关键字参数只会包括没有被指定的参数

```python
def my_function(x, y, *args, **kwargs):
    print(f'x = {x}')
    print(f'y = {y}')
    print(f'args = {args}')
    print(f'kwargs = {kwargs}')

my_function(1, 2, 3, 4, a=5, b=6)
# 输出:
# x = 1
# y = 2
# args = (3, 4)
# kwargs = {'a': 5, 'b': 6}
```

装饰器函数可以接受另外一个函数作为输入。

被装饰器装饰之后直接用原函数相当于又使用了一次装饰函数

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print('Before function call')
        result = func(*args, **kwargs)
        print('After function call')
        return result
    return wrapper

@my_decorator
def my_function(x, y):
    print(f'x + y = {x + y}')

my_function(1, 2)
# 输出:
# Before function call
# x + y = 3
# After function call
```

查看帮助

```python
#查看module
help(math)
#查看方法
help(math.sqrt)
#查看类
help(list)
```

版本

```python
import session_info
session_info.show()
```

## 时间

当前时间

```python
from datetime import datatime
now=datatime.now()
```

当前日期

```python
from datetime import datatime
now=datatime.today()
```

运行时间

```python
import time
stat_time = time.time()
...
end_time = time.time()
elapsed_time = end_time - stat_time
```

# I/O

## 读取文件

```python
with open('out.log', 'r') as f_handler:
```

r是只读

缓慢地逐行读取

```python
with open('out.log', 'r') as f_handler:
    per = f_handler.readline()
```

整体读取，逐行拆分

```python
with open('out.log', 'r') as f_handler:
    all = f_handler.readlines()
```

## 写入文件

```python
with open('out.log', 'w') as f_handler:
```

x是新建，w是从头编辑，a是追加

二进制文件是b

## pickle

```python
import pickle
with open("data.pickle","w") as f:
	pickle.dump(data,f)

```


## 格式化输出

规定显示的格式

```python
print("%3.1f%s%s" % (num, unit, suffix))
#s是string
#f是浮点，小数点前数字是长度，小数点后数字是小数位

```

打印时引入变量{}

```python
print(f'{var1:.2f},and,{var2:s}')
```

# os 访问文件系统

## 判断

判断是否是文件

```python
os.path.isfile
```

判断是否是文件夹

```python
os.path.isdir
```

判断是否是链接

```python
os.path.islink
```

查询路径path是否存在

```python
os.path.exists(path)
```

## 获得

生成当前目录

```python
os.curdir()
```

返回当前工作目录

```python
os.getcwd()
```

生成绝对路径

```python
os.path.abspath()

os.path.abspath(os.curdir())

```

返回path路径最后一个\\后的内容，可以为空

```python
os.path.basename(path)
```

返回path路径最后一个\\之前的内容

```python
os.path.dirname(path)
```

生成合并路径

```python
os.path.join(A, 'b')
```

列举指定目录的文件名

```python
os.listdir(path)
```

递归穿透

```python
os.walk(path)

for root, dirs, files in os.walk(dir_path):
    print(f"{root}")
    print(f"{dirs}")
    print(f"{files}")
```

root是你所要遍历的目录的地址，
dirs是一个列表，内容是该文件夹中所有的目录的名字(不包括子目录)，
files同样是一个列表，内容是该文件夹中所有的文件(不包括子目录)。
会依次每个目录列出地址、下属子目录和下属文件

获得文件的统计信息

```python
os.stat(path)
```

## 操作

### 改变目录

改变当前工作目录，path必须为字符串形式的目录

```python
os.chdir(path)
```

### 创建

创建path指定的文件夹,只能创建一个单层文件，而不能嵌套创建，若文件夹存在则会抛出异常

```python
os.mkdir(path)
```

创建多层目录 ，可以嵌套创建

```python
os.makedirs(path)
```

创建超链接

```python
os.symlink('/old/path','/new/path')
```

读取超链接

```python
os.readlink('/new/path')
```

### 重命名

文件old重命名为new

```python
os.rename(old,new)
```

### 执行

```python
os.system()
```

### 删除

删除指定文件

```python
os.move(file_name)
```

删除单层目录，遇见目录非空时则会抛出异常

```python
os.rmdir(path)
```

逐层删除多层目录

```python
os.removedirs(path)
```

# sys 访问python相关

## 环境变量

获取python的环境变量，常用来添加想要搜索文件的路径。

```python
sys.path()
sys.path.append()
```

## 输入输出打印

读取输入信息，实现人机交互

```python
sys.stdin.read()
```

向屏幕打印字符串

```python
sys.stdout.write(s)
```

控制台内容写入文件

```python
#打开一个文本文件
with open('out.log', 'w') as f_handler:
    #如果控制台内容只是暂时写入，那么控制台先暂存
    __console__ = sys.stdout
    #让打开的文件于控制台链接
    sys.stdout = f_handler 
    #打印内容
    print('hello')
    #打印还给控制台
    sys.stdout = __console__
```

## 其他

程序中间的退出，arg=0为正常退出

```python
sys.exit([arg])
```

获取系统当前编码，默认为utf-8

```python
sys.getdefaultencoding()
```

设置系统的默认编码

```python
sys.setdefaultencoding()
```

返回对象的大小

```python
sys.getsizeof(object[, default])
```

# re 正则表达式

禁止自动转义

```python
print(r"\n")
```

## 匹配

完全匹配才返回

```python
re.fullmatch(pattern,string,flags=0)
#flags=1 无视大小写
```

起始开始匹配，一旦出现不匹配就停止返回匹配部分
否则返回None

```python
re.match(pattern,string,flags=0)
#flags=1 无视大小写
```

匹配后用group函数拆分

```
import re
a = "123abc456ww"
pattern = "(?:[0-9]*)([a-z]*)([0-9]*)"
print(re.search(pattern,a).group(0,1,2))
```

单一搜寻

```python
re.search(pattern,string,flags=0)
#flags=1 无视大小写
```

多搜寻

```python
re.findall(pattern,string,flags=0)
#flags=1 无视大小写
```

## 替换

```python
re.sub(pattern, repl, string, count=0, flags=0)
```

count 可选参数，count 是要替换的最大次数，必须是非负整数。如果省略这个参数或设为 0，所有的匹配都会被替换
flag 可选参数，标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。

## 分割

```python
re.split(pattern, string, maxsplit=0, flags=0)
```

 maxsplit 分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数

# string 字符

```python
print("asdf4".isdigit())
```

## 判断

是否是纯字母

```python
str.isalpha()
```

是否为纯数字

```python
str.isdigit()
```

是否是小写

```python
str.islower()
```

## 查询

字符第一次出现的位置

```python
"qwertasdqwezxcqwe".find("qwe")
```

计数出现次数

```python
import string 
# 全部字符串内 搜索qwe 出现的次数
print("qwertasdqwezxcqwe".count("qwe"))
# 由于字符串从0开始计数，1为字符串第二个，相当于从w开始
print("qwertasdqwezxcqwe".count("qwe",1))
# 从字符串第 2个开始到第15个截止，共出现qwe的次数
print("qwertasdqwezxcqwe".count("qwe",1,14))
```

## 转化

```python
#首字母大写
str.captitalize()
#首字母大写其他小写
str.title()
str.lower()
str.upper()
#翻转大小写
str.swapcase()
```

## 生成

连接

```python
str.join(",")
```

分割

```python
str.split(",")
```
