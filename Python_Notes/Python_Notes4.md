# Python笔记四 (函数式编程)

## 1. `map()` 函数

`map()` 函数接收两个参数，一个是函数，一个是 `Iterable` ， `map` 将传入的函数依次作用到序列的每个元素，并把结果作为新的 `Iterator` 返回，如果要把结果显示出来，可以多加一个 `list()` 函数

```py
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

## 2. `reduce()` 函数

 `reduce()` 函数接收两个参数，一个是要实现操作的函数，另外一个是 `Iterable` 。操作的过程是将迭代过程的结果与下一个元素进行操作。这种操作可以用来实现字符串转数字

```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def char2num(s):
    return DIGITS[s]

def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```

## 3. `filter()` 函数

 `filter()` 也接收一个函数和一个序列， `filter()` 把传入的函数依次作用于每个元素，然后根据返回值是 `True` 还是 `False` 决定保留还是丢弃该元素

```python
# 利用filter()函数求素数，使用埃氏筛法

def _odd_iter():   # 从3开始的奇数序列
    n = 1
    while True:
        n = n+2
        yield n

def _not_divisible(n):
    return lambda x: x % n > 0

def primes():
    yield 2
    it = _odd_iter()
    while True:
        n = next(it)
        yield n
        it = filter(_not_divisible(n), it)

for i in primes():
    if i < 1000:
        print(i)
    else:
        break
```

## 4. `sorted()` 函数

 `sorted()` 函数可以对 `list` 进行从小到大排序，也可以输入 `key` 参数进行操作。 `sorted()` 函数将对每个数字进行 `key` 操作然后排序。

 传入参数 `reverse=True` 可以实现倒序排序

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

## 5. 匿名函数

关键字 `lambda` 表示匿名函数，冒号前面的 `x` 表示函数参数

```python
lambda x: x * x  # 和下面等价

def f(x):
    return x * x
```

## 6. 偏函数

通过设定参数的默认值，可以降低函数调用的难度，固定函数里面某一些参数，然后返回一个新的函数

```python
import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
```

使用 `partial()` 函数，第一个参数输入接收函数的对象，第二个参数输入要预设的参数，可以是关键字参数

## 7. 闭包 （函数作为返回值，装饰器就是闭包的一种高级应用）

+ 闭包是将函数里面的子函数作为返回值返回出来，其他的程序可以调用这个子函数。神奇的地方在于，就算原来的父函数被销毁了，子函数里面引用父函数的变量仍然存在，可以继续使用。因为子函数在被返回出来的时候，和那些变量一起被打成一个所谓的闭包。

+ 闭包有一点就是不能改变里面引用父函数的变量，引用的话会报错

```python
def create_count():     
    f = [0]
    def count():
        f[0] += 1
        print('Called:')
        return f[0]
    return count

counterA = create_count()

for i in range(2):
    print(counterA())


Called:
1
Called:
2
```

+ 修改的是 `list` 第一个元素的指向，对 `list` 整体没有改变。若是用普通整数变量，则需要加上 `python3` 的新语法 `nonlocal`

## 8. 装饰器

装饰器的作用是在不改变原函数的情况下给函数增加一些功能，是在运行期间动态增加的功能。常用的作用有打印日志，记录程序运行时间等

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

@log
def now():
    print('2019-02-18')

call now():
2019-02-18
```

