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

如果不slots限制的话，可以随意添加新属性

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

## 鸭子类型
鸭子类型是一种编程概念，它通过关注对象的行为而不是类型来提高类型检查的灵活性。
在鸭子类型中，对象是否适用于特定操作是由它是否具有必要的方法和属性来决定的，而不是由它的类型来决定
“当你看到一只鸟走起来像鸭子，游泳起来鸭子，叫起来也像鸭子，那么这只鸟就被称为鸭子类型”
这种形式中，不管对象属于哪个类，也不管声明的具体接口是什么，只要对象实现了相应的方法，函数就可以在对象上执行操作

```python
class Animal():
    def who(self):
        print("I am an Animal")
class Duck(Animal):
    def who(self):
        print("I am a duck")

class Dog(Animal):
    def who(self):
        print("I am a dog")

def func(obj):
    obj.who()

if __name__ == "__main__":
    duck=Duck()
    dog=Dog()
    cat=Cat()
    func(duck)
    func(dog)
    func(cat)
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

# 抽象类

在面向对象的思想中类是客观事物的抽象，而抽象类又是类的进一步抽象。

抽象类是一种不能被实例化且必须由其他类子类化的类。抽象类可以包含抽象方法和具体方法。抽象方法是声明但未实现的方法，而具体方法是具有定义实现的方法。抽象类的目的是为其子类提供一个共同的接口，定义所有子类必须实现的行为。这允许更好地抽象和代码重用，因为可以在抽象类中定义公共行为并由其子类继承。抽象类与普通类不同，它们不能被实例化，必须被子类化才能使用。

抽象类是对类本质的抽象，表达的是 is a 的关系，比如：奔驰是车。抽象类包含并实现子类的通用特性，将子类存在差异化的特性进行抽象，交由子类去实现。

# 面向接口编程

面向接口编程就是一堆接口，通过接口规约对象的属性和方法，是面向对象一部分。 

面向接口编程就是编码时使用接口而不是具体的对象。为什么要面向接口编程呢？其实主要是为了解耦。我定义一个接口，我甚至不知道别人会怎样实现它，我的系统用谁的具体实现都能正常工作，甚至没有具体实现也能正常工作，别人一点都不会影响我，这就是解耦。

并不是比面向对象编程更先进的一种独立的编程思想，可以说属于面向对象思想体系的一部分或者说它是面向对象编程体系中的思想精髓之一

就像电脑上的USB插口，如果你觉的有线鼠标用着不过瘾，完全可以购买一个无线鼠标插入原来有线鼠标的插口即可实现无缝切换，而不需要修改计算机中的任何地方，无论你的无线鼠标是否和有线鼠标是同种类型，因为这个插口就是一种标准，只要你购买的鼠标实现了这个标准，它就可以通过这个插口使用对应的鼠标。

在系统分析和架构中，分清层次和依赖关系，每个层次不是直接向其上层提供服务（即不是直接实例化在上层中），而是通过定义一组接口，仅向上层暴露其接口功能，上层对于下层仅仅是接口依赖，而不依赖具体类。

## 与抽象类差别

接口与抽象类不同。接口是一组抽象方法的集合，它为多个类定义了一组共同的行为。而抽象类则是一个不能被实例化且可能包含抽象方法和具体方法的类。接口和抽象类之间的主要区别在于，接口只包含抽象方法，而抽象类可以包含抽象方法和具体方法。此外，一个类可以实现多个接口，但只能继承一个抽象类。

## abc库

abc模块放置的是python中的抽象基类

abc模块有以下两个主要功能：

* 某种情况下，判定某个对象的类型，如：isinstance(a, Sized)
* 强制子类必须实现某些方法，即ABC类的派生类


下面是一个在Python中演示面向接口编程的例子，它展示了系统不同层之间关注点分离的情况：

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius

    def perimeter(self):
        return 2 * 3.14 * self.radius

shapes = [Rectangle(2, 3), Circle(5)]
for shape in shapes:
    print(f"Area: {shape.area()}, Perimeter: {shape.perimeter()}")
```

在上面的示例中，我们定义了一个名为Shape的抽象基类，它具有两个抽象方法：area和perimeter。

然后，我们定义了两个类：Rectangle和Circle，它们都实现了Shape接口，并提供了这两个方法的具体实现。
这种设计允许我们在系统的不同层之间进行分离。例如，我们可以在上层代码中使用Shape接口来处理不同类型的形状，而无需关心它们是如何实现的。这样，如果我们想要添加新的形状类型，只需创建一个新类并实现Shape接口即可，而无需更改上层代码。


当一个类（如Circle）子类化Shape抽象基类时，它必须为基类中定义的所有抽象方法提供实现。如果子类没有为所有抽象方法提供实现，它也将被视为抽象类，不能被实例化。


与鸭子类型不同，因为示例使用显式类型定义和继承来定义对象的行为，而不是依赖它们的方法和属性。