# Python笔记二  （函数）

## 1. 函数的定义，返回值

* 使用 `def` 开头，后面加上函数名，圆括号以及冒号  
* 返回值可以直接写 `return` ，相应地返回 `None`  
* 多个返回值的时候，实际上是返回一个 `tuple`

## 2. 函数的参数

* 位置参数，就是正常情况下的参数，必须按照顺序来赋值 
  
* 默认参数必须指向不可变对象（字符串、整数、不包含可变对象的tuple），默认参数可以不按顺序，但是必须指定对应的值。

```python
def enroll(name, gender, age=6, city='Shenzhen'):
    print(name)
    ...

enroll('Adam', 'M', city='Shantou')  #城市手动设置，年龄默认
```


* 可变参数：可变参数和定义一个 `list` 或 `tuple` 参数相比，仅仅在参数前面加了一个 \* 号。在函数内部，对应参数接收到的是一个tuple。若已有现成的list或者tuple，可以直接在前面加 \* 转化成可变参数

```py
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

* 关键字参数：允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

```py
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

* 命名关键字参数：可以限制关键字参数的名字，命名关键字参数需要一个特殊分隔符 `*` ,后面的参数就是命名关键字参数。命名关键字参数可以有缺省值。当前面有一个可变参数的时候，可以省略 `*`
  
```py
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)

>>> person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```

## 3. 参数组合

参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。  
对于任意函数，都可以通过类似 `func(*args, **kw)` 的形式调用它

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```