参考资料：
[Python3 模块 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-module.html)

# package

在了解包之前，先了解一下模块吧。

## module

也许你一开始用 python 解释器来编程，如果你从 Python 解释器退出再进入，那么你定义的所有的方法和变量就都消失了。为此 Python 提供了一个办法，把这些定义存放在文件中，为一些脚本或者交互式的解释器实例使用，这个文件被称为模块。

**模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。
模块可以被别的程序引入，以使用该模块中的函数等功能。这也是使用 python 标准库的方法。**

## import

当解释器遇到 import 语句，如果模块在当前的搜索路径就会被导入。
**一个模块只会被导入一次**，不管你执行了多少次 import。这样可以防止导入模块被一遍又一遍地执行。

当我们使用 import 语句的时候，Python 解释器是怎样找到对应的文件的呢？

这就涉及到 Python 的搜索路径，搜索路径是由一系列目录名组成的，Python 解释器就依次从这些目录中去寻找所引入的模块。
这看起来很像**环境变量**，事实上，也可以通过定义环境变量的方式来确定搜索路径。

导入后，可以使用模块名称来访问函数。如 `module.function()`

**_from … import_**

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中
如只导入某个模块的部分函数

**\*from … import \*\***
把一个模块的所有内容全都导入到当前的命名空间
导入后，不需要使用模块名称来访问函数。如 `function()`即可

这将把所有的名字都导入进来，但是那些由单一下划线（\_）开头的名字不在此例。

## \_\_name\_\_

一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用\_\_name\_\_属性来使该程序块仅在该模块自身运行时执行。

```python
# Filename: using_name.py

if name == 'main':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```

运行输出如下：

```shell
$ python using_name.py
程序自身在运行
$ python
import using_name
我来自另一模块
```

**\*dir() 函数** \*内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回:

## 导入包

包是一种管理 Python 模块命名空间的形式，采用"点模块名称"。

比如一个模块的名称是 A.B， 那么他表示一个包 A 中的子模块 B 。

就好像使用模块的时候，你不用担心不同模块之间的全局变量相互影响一样，采用点模块名称这种形式也不用担心不同库之间的模块重名的情况。

在导入一个包的时候，Python 会根据 sys.path 中的目录来寻找这个包中包含的子目录。
目录只有包含一个叫做 \_\_init\_\_.py 的文件才会被认作是一个包。

最简单的情况，放一个空的 :file:\_\_init\_\_.py 就可以了。当然这个文件中也可以包含一些初始化代码或者为（将在后面介绍的） \_\_all\_\_变量赋值。

用户可以每次只导入一个包里面的特定模块，比如:
`import sound.effects.echo `

这将会导入子模块:sound.effects.echo。 他必须使用全名去访问:
`sound.effects.echo.echofilter(input, output, delay=0.7, atten=4) `

还有一种导入子模块的方法是:
`from sound.effects import echo `

这同样会导入子模块: echo，并且他不需要那些冗长的前缀，所以他可以这样使用:
`echo.echofilter(input, output, delay=0.7, atten=4) `

还有一种变化就是直接导入一个函数或者变量:
`from sound.effects.echo import echofilter `

同样的，这种方法会导入子模块: echo，并且可以直接使用他的 echofilter() 函数:
`echofilter(input, output, delay=0.7, atten=4)`

注意当使用 from package import item 这种形式的时候，对应的 item 既可以是包里面的子模块（子包），或者包里面定义的其他名称，比如函数，类或者变量。

import 语法会首先把 item 当作一个包定义的名称，如果没找到，再试图按照一个模块去导入。如果还没找到，抛出一个 :exc:ImportError 异常。

反之，如果使用形如 import item.subitem.subsubitem 这种导入形式，除了最后一项，都必须是包，而最后一项则可以是模块或者是包，但是不可以是类，函数或者变量的名字。

- 当使用 import 时，只能访问包中定义的顶层变量和函数。要访问包中子目录中的模块，需要使用点号 . 来指定模块的完整路径。
- 内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回:

## **从一个包中导入\***

如果我们使用 from sound.effects import \* 会发生什么呢？

导入语句遵循如下规则：如果包定义文件 \_\_init\_\_.py 存在一个叫做 \_\_all\_\_ 的列表变量，那么在使用 from package import \* 的时候就把这个列表中的所有名字作为包内容导入。

作为包的作者，可别忘了在更新包之后保证 \_\_all\_\_ 也更新了啊。

以下实例在 file:sounds/effects/\_\_init\_\_.py 中包含如下代码:

\_\_all\_\_ = ["echo", "surround", "reverse"]
这表示当你使用 from sound.effects import \*这种用法时，你只会导入包里面这三个子模块。

如果 \_\_all\_\_ 真的没有定义，那么使用 from sound.effects import \*这种语法的时候，就不会导入包 sound.effects 里的任何子模块。他只是把包 sound.effects 和它里面定义的所有内容导入进来（可能运行\_\_init\_\_.py 里定义的初始化代码）。

这会把 \_\_init\_\_.py 里面定义的所有名字导入进来。并且他不会破坏掉我们在这句话之前导入的所有明确指定的模块。看下这部分代码:

```python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```

这个例子中，在执行 from...import 前，包 sound.effects 中的 echo 和 surround 模块都被导入到当前的命名空间中了。（当然如果定义了 \_\_all\_\_ 就更没问题了）

通常我们并不主张使用 \* 这种方法来导入模块，因为这种方法经常会导致代码的可读性降低。不过这样倒的确是可以省去不少敲键的功夫，而且一些模块都设计成了只能通过特定的方法导入。

记住，使用 from Package import specific_submodule 这种方法永远不会有错。事实上，这也是推荐的方法。除非是你要导入的子模块有可能和其他包的子模块重名。

如果在结构中包是一个子包（比如这个例子中对于包 sound 来说），而你又想导入兄弟包（同级别的包）你就得使用导入绝对的路径来导入。比如，如果模块 sound.filters.vocoder 要使用包 sound.effects 中的模块 echo，你就要写成 from sound.effects import echo。

```python
from . import echo
from .. import formats
from ..filters import equalizer
```

无论是隐式的还是显式的相对导入都是从当前模块开始的。主模块的名字永远是"\_\_main\_\_"，一个 Python 应用程序的主模块，应当总是使用绝对路径引用。

包还提供一个额外的属性\_\_path\_\_。这是一个目录列表，里面每一个包含的目录都有为这个包服务的\_\_init\_\_.py，你得在其他\_\_init\_\_.py 被执行前定义。可以修改这个变量，用来影响包含在包里面的模块和子包。这个功能并不常用，一般用来扩展包里面的模块。

# 开发 python 包

## 创建

要在一个目录下创建 Python 包，您需要执行以下步骤：

1. 在目录中创建一个包含您的 Python 代码的子目录。这个子目录的名称应该与您的包名称相同。
2. 在子目录中创建一个名为 `\_\_init\_\_.py` 的文件，这个目录中的 `\_\_init\_\_.py`里需要一些字符串。`\_\_init\_\_.py`表示该目录是一个 Python 包。每个子文件夹下都需要，可以是空的。

   - 包目录 `\_\_init\_\_.py` 文件是每次导入包时都会执行的文件。它用于初始化包并执行任何必要的设置。一般需要有一段字符串，以及包的版本定义。如：

   ```python
   '''description'''
   __version__ = '0.0.1'
   ```

3. 在目录中创建一个名为 setup.py 的文件。这个文件定义了包的元数据和依赖关系。
   在 setup.py 文件中，使用 setuptools.setup() 函数来定义包的元数据和依赖关系。如：

   ```python
   from setuptools import setup, find_packages

   setup(
       name='your_package_name',
       version='0.1',
       packages=find_packages(),
       install_requires=[
           # List your package dependencies here
       ],
   )
   ```

4. 运行 `python setup.py sdist` 来创建源代码分发包。
5. 完成上述步骤后，您就可以使用 `pip install -e .` 来在开发过程中安装并使用您的包了

   - `pip install -e` 是 `pip install --editable` 的缩写，它用于在可编辑模式下安装包。这意味着您可以在安装包的同时对其进行开发和修改。
   - 如果您想要在开发过程中安装并使用您的包，您可以在包含 setup.py 文件的目录中运行 `pip install -e .`。这将安装您的包并使其可供使用，同时允许您对其进行更改。

## 重新加载

如果您在运行 Python 命令行时更改了包中的文件，您需要重新加载包以使更改生效。您可以使用 importlib.reload() 函数来重新加载包。例如，如果您的包名称为 your_package，则可以使用以下命令重新加载它：

```python
import importlib
import your_package
importlib.reload(your_package)
```

请注意，reload() 函数只会重新加载指定的模块，而不会重新加载该模块导入的任何其他模块。
