# Python笔记六 （错误处理、调试）

## 1. 错误处理机制

Python内置 `try...except...finally...` 错误处理机制，和 `java` 类似

```python
try:
    print('try...')
    r = 10 / 0
    print('result:', r)
except ZeroDivisionError as e:
    print('except:', e)
finally:
    print('finally...')
print('END')
```

当捕获错误后会停止后面代码的执行，跳到错误处理的语句里面，然后不管怎么样都会执行 `finally` 里面的语句

如果没有错误发生，可以在 `except` 语句块后面加一个 `else` ，当没有错误发生时，会自动执行 `else` 语句

Python的错误其实也是 `class` ，所有的错误类型都继承自 `BaseException` ，当捕获了父类的错误，子类错误是永远不会被捕获的

Python内置的 `logging` 模块可以非常容易地记录错误信息

```python
import logging

def main():
    try:
        pass
    except Exception as e:
        logging.exception(e)
```

抛出错误可以使用 `raise` 关键字，可以直接 `raise` ，将捕获到的错误抛给上一级处理，也可以 `raise` 一个自定义的错误，但是抛出的错误要合理

## 2. 调试

* 使用 `print()` 打印一些变量的信息进行调试

* 使用断言 `assert` ，断言后面的语句是 `True` ，如果是 `False` ，则会打印相应的信息

```python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')

Traceback (most recent call last):
  ...
AssertionError: n is zero!
```

* 使用 `logging` 来进行调试

有 `debug` ， `info` ， `warning` ， `error` 等几个级别

```py
import logging
logging.basicConfig(level=logging.INFO)  # 设置logging显示的级别

s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)

```

## 3. 测试

包括单元测试和文档测试，单元测试用来测试一个函数的功能，文档测试将测试代码写在注释里面，python会自动地测试
