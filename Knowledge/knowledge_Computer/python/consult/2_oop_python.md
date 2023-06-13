# 名词

- 类(Class): 用来描述具有相同属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。其中的对象被称作类的实例。
- 实例：也称对象。通过类定义的初始化方法，**赋予具体的值**，成为一个”有血有肉的实体”。
- 实例化：创建类的实例的过程或操作。
- 实例变量：定义在实例中的变量，只作用于当前实例。
- 类变量：类变量是所有实例公有的变量。类变量定义在类中，但在方法体之外。
- 数据成员：类变量、实例变量、方法、类方法、静态方法和属性等的统称。
- 方法：类中定义的函数。
- 静态方法：**不需要实例化**就可以由类执行的方法
- 类方法：类方法是将类本身作为对象进行操作的方法。
- 方法重写：如果从父类继承的方法不能满足子类的需求，可以对父类的方法进行改写，这个过程也称override。
- 封装：将内部实现包裹起来，对外透明，提供api接口进行调用的机制
- 继承：即一个派生类（derived class）继承父类（base class）的变量和方法。
- 多态：根据对象类型的不同以不同的方式进行处理。

# 内置方法

创建对象时候自动调用

```python
__new__
```

初始化对象时候自动调用

```python
__init__
```

销毁对象时候自动调用

```python
__del__
```

print函数时候自动调用

```python
__str__
```

# 创建类

```python
class example:
    """
    Description
    """
    def __init__(self):
        self.name=""
        pass
    def method1(self,para1):
        pass

```

# 实例化

```python
e=example()
e=example("a",6)
```

# 查看实例属性

```python
e.name
```

字典化展示属性

```python
e.__dict__
```

类的说明文档

```python
e.__doc__
```

类的模块

```python
e.__module__
```

类的父类

```python
e.__bases__
```

# hasattr判断是否包含属性

```python
hasattr(class1,"para1")
```

# 特性

## super  继承

子类继承父类的方法
子类括号里填写父类名字
继承多个父类时候，父类之间逗号分隔

```python
class father():
    def __init__(self,para1):
        pass
    def method1(self,para1):
        pass

class son(father):
    def __init__(self,para1):
        super().__init__(para1)
        #可以继承父类的所有方法
        super().method1(para1)
```

## 延申

继承属性后，自定义新增属性

```python
class father():
    def __init__(self,para1):
        pass

class son(father):
    def __init__(self,para1,para2):
        super().__init__(para1)
        self.para2=para2
```

## 重写

与父类方法同名 只使用子类里的同名方法

## 大类与小类

为了防止一个类太大
属性分别对应一个类

```python
class big_father():
    def __init__(self,para1):
        pass

class small_father():
    def __init__(self,para1):
        pass

class son(father):
    def __init__(self,para1):
        super().__init__(para1)
        self.para2 = small_father()
```

# 封装

外界只可以使用接口，不能直接访问属性
不需要知道内部细节

## 私有变量

禁止类的内部属性或者内部方法被外部访问修改
变量前加下划线

```python
class example:
    """
    Description
    """
    def __init__(self):
        self.__name=""
        pass
    def method1(self,para1):
        pass

```

如果需要读取或者修改私有变量，需要定义专门的方法

```python
    def method2(self,para1):
        return self.__name
```

单下划线开头的变量名_name :
这样的变量外部可以访问，但是预定俗成，视为私有变量

## __slots__

只准实例化修改元组内的属性

```python
class myclass():
    __slots__ = ("name","age")
    def __init__(self):
        self.name
        self.age
        self.level
```

```python
ex=myclass()
my.class.level #不允许修改
```

level不准实例修改

# 多态

它指的是不同类的对象可以使用相同的方法名，但具体执行的方法会根据对象所属的类而不同。

一个大父类 接着几个小类都定义同名方法
定义一个大函数，要求输入对象的父类
之后根据子类分别调用子类方法

```python
class Animal:
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        print('The dog barks')

class Cat(Animal):
    def make_sound(self):
        print('The cat meows')

def animal_sound(animal: Animal):
    animal.make_sound()

my_dog = Dog()
my_cat = Cat()

animal_sound(my_dog) # 输出: The dog barks
animal_sound(my_cat) # 输出: The cat meows
```

# 装饰器

使用@my_decorator装饰器来装饰my_function()函数。这相当于执行了如下代码:

```python
my_function = my_decorator(my_function)
```

## @staticmethod 静态方法  类和实例都不访问

处理与类相关但是与实例不直接相关的任务
不访问任何类或者实例的属性

```python
class ex():
    @staticmethod
    def method1(x,y):
        return x+y
```

外部调用的化如下写法

```python
ex.method1(x,y)
```

## @classmethod 类方法  访问类不访问实例

处理与类相关的任务
可以访问类的属性，不能访问实例的属性

类方法的第一个参数是cls，表示类本身

```python
class myclass():
    x = 1
    @classmethod
    def method1(cls)
        print(f'x = {cls.x}')

myclass.method1()
```

## @property  方法变成属性

把一个 Method 变成属性调用

这样可以限制访问 访问的属性只是运行结果，不是原属性本身

```python
class myclass():
    def __init__(self)：
        self.__name
        self.__age
    def method1(self):
        return self.__name
    @property
    def method1(self):
        return self.__name
```

被property装饰的方法不需要再加括号

```python
ex=myclass()
ex.method1()
ex.method2
```
