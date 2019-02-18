# Python笔记五 （面向对象编程）

## 1. 类的定义

`class` 后面紧接着是类名，类名通常是大写开头的单词，紧接着是 `(object)` ，即继承的类

```py
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score
```

`__init__()` 方法类似于构造函数，传入构造对象时候的参数，用来初始化，python第一个参数是 `self` ，传入时不需要传入

python属于动态语言，可以在运行阶段给对象实例加上变量和方法

python的类内部方法的第一个参数都是 `self`

可以在变量前面添加两个 `__` 来限制外部的调用，实际上这个变量的名称在外部被改成 `_Objectname__name` ，外部仍然可以修改和调用，但是不建议这么使用

## 2. 多态

python属于动态语言，多态性和静态语言有点不同。静态语言的多态是需要继承，重写，调用时父类引用指向子类对象。而动态语言的python只需要类里面实现了要调用的方法就可以了

## 3. 获取对象属性 (包括成员变量和成员函数)

```py
type()                      # 返回对象的类型

isinstance(object, Type)    # 判断是不是Type类型

isinstance(object, (Type1, Type2))  # 判断是不是属于后面的tuple里面的类型之一

dir()                       # 获取对象的所有属性，通过list返回出来

getattr()                  # 获取相应的属性

setattr()                  # 设置相应的属性

hasattr()                  # 判断是否具有相应的属性
```

```py
>>> hasattr(obj, 'x')      # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y')      # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19)  # 设置一个属性'y'
>>> hasattr(obj, 'y')      # 有属性'y'吗？
True
>>> getattr(obj, 'y')      # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```

## 4. 实例属性

可以使用 `del` 关键字来删除对象里面的变量，但是当生成对象的类具有这个属性的时候，调用该属性时会调到类的属性值

## 5. 使用 `@property` 修饰器

可以通过 `@property` 修饰器来将一个方法变成属性，好处是可以检查参数，同时又可以像设置一个属性那样方便地设置

```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2019 - self._birth
```

注意要变成属性的方法的名字都是一样的，但是要和原本的属性名称区分开，要是设置一样就会报错

只是设置一个 `@property` 相当于只读属性

## 6. 使用 `__slots__`

动态语言中，可以直接给一个实例添加一个属性，也可以使用 `MethodType()` 方法来动态给类或者对象新增一个方法

```python
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```

如果要限制类和对象的成员变量和成员函数，使用 `__slots__` 来限制

```py
class Student(object):
    __slots__ = ('name', 'age')  # 用tuple定义允许绑定的属性名称
```

要注意一点就是， `__slots__` 定义的属性仅对当前类实例起作用，对继承的子类是不起作用的

## 7. 多重继承

```python
class Dog(Mammal, Runnable):
    pass
```

一般有一个主要的继承父类，然后其他的当成增加的功能，可以使用 `MixIn`

```python
class MyTCPServer(TCPServer, ForkingMixIn):
    pass
```

## 8. 枚举类型

```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

>>> print(Month['Feb'])
Month.Feb

>>> print(Month.Feb.value)
2

>>> print(Month(3))
Month.Mar
```

## 9. 定制类

给类加上一些方法，使得这个类兼容一些常用的操作，甚至可以像操作 `List` ， `dict` 那样

```python
def __str__(self):    # 类似于Java的 toString 方法
    pass

def __len__(sef):     # 重写这个可以被 len() 函数调用
    pass

def __repr__(self):  # 类似于__str__(self)，在调试期间显示
    pass

def __call__(self):   # 使得对象像函数那样可以被调用
    pass

def __iter__(self):   # 使得可以被 for...in 调用，返回一个迭代对象
    pass

def __next__(self):   # 迭代过程拿到下一个值，要设置 StopItera 错误
    pass

def __getitem__(self): # 使得对象可以像 list 那样进行按下标调用
    pass

def __getattr__(self, attr): # 在没有找到属性的情况下，才调用__getattr__，可以根据 attr 参数进行判断
    pass
```
