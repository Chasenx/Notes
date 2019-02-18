# Python笔记三 （高级特性）

## 1. 切片(Slice)

Slice是取 `list` 或者 `tuple` 里面特定的元素的一种操作  

```py
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']

>>> L[:3]                        #第一个索引是0可以省略
['Michael', 'Sarah', 'Tracy']


>>> L[-2:]                       #可以使用负数来表示倒数的
['Bob', 'Jack']

L[::2]                           #使用变步长
['Michael', 'Tracy', 'Jack']
```

## 2. 迭代

* 只要是可迭代的就可以使用 `for...in` 来进行迭代 

* dict迭代的是key。如果要迭代 `value` ，可以用 `for value in d.values()` ，如果要同时迭代 `key` 和 `value` ，可以用 `for k, v in d.items()`

* 字符串也可以进行迭代

* 判断是否可以进行迭代使用以下的函数

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable)     # str是否可迭代
True
>>> isinstance([1,2,3], Iterable)   # list是否可迭代
True
>>> isinstance(123, Iterable)       # 整数是否可迭代
False
```

* Python内置的 `enumerate` 函数可以把一个 list 变成索引-元素对

```py
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```

## 3. 列表生成器

使用for循环快速生成一个list

```py
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]

>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

## 4. 生成器 (generator)

* 第一种直接使用把一个列表生成式的 `[]` 改成 `()` ，就创建了一个 `generator` 

```py
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

通过 `next()` 函数获得 `generator` 的下一个返回值

```py
>>> next(g)
0
>>> next(g)
1
```

* 第二种方法使用关键字 `yield` 来通过函数生成 `generator`，可以在这个函数里面设置一个死循环，然后实现一个无限长的数列。每次调用 `next()` 的时候执行，遇到 `yield` 语句返回，再次执行时从上次返回的 `yield` 语句处继续执行

```py
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)

>>> o = odd()
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
```

如果想要拿到 `generator` 的返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中

## 5. 迭代器

* 凡是可作用于 `for` 循环的对象都是 `Iterable` 类型；

* 凡是可作用于 `next()` 函数的对象都是 `Iterator` 类型，它们表示一个惰性计算的序列；

* 集合数据类型如 `list` 、 `dict` 、`str` 等是 `Iterable` 但不是 `Iterator` ，不过可以通过 `iter()` 函数获得一个 `Iterator` 对象。

```py
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
t = iter(L)
print(next(t))
print(next(t))
print(next(t))

Michael
Sarah
Tracy
```
