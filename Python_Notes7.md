# Python笔记七 （IO编程）

## 1. 文件读写

### 读文件

Python内置的 `open()` 函数，如果文件不存在，函数就会抛出一个IOError的错误

```py
>>> f = open('/Users/michael/test.txt', 'r')
```

调用 `read()` 方法可以一次读取文件的全部内容，Python将内容读取到内存，然后用一个 `str` 对象表示。使用完记得使用 `close()` 方法将文件关掉

```py
>>> f.read()
'Hello, world!'
>>> f.close()
```

Python引入了with语句来自动帮我们调用 `close()` 方法

```py
with open('/path/to/file', 'r') as f:
    print(f.read())
```

调用 `read()` 方法会一次性将文件内容全部读取到内存，为防止爆内存，使用 `read(size)` 方法，一次读取 size 个字节的内容。

`readline()` 一次读取一行的内容，调用 `readlines()` 一次读取所有内容并按行返回 `list`。 相当于一个 `list` 里面的元素是 `str`。 

```python
for line in f.readlines():
    print(line.strip())     # 把末尾的'\n'删掉
```

### 读二进制文件

在参数里面加一个 `'b'`

```python
>>> f = open('/Users/michael/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...'   # 十六进制表示的字节
```

### 字符编码

在参数里面加一个关键字参数 `encoding = 'gbk'` ，遇到不规范的编码可以再加一个 `errors` 参数

```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'

>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

### 写文件

写文件传入标识符 `'w'` 或者 `'wb'` 表示写文本文件或写二进制文件

```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```

以 `'w'` 模式写入文件时，如果文件已存在，会直接覆盖（相当于删掉后新写入一个文件）。如果我们希望追加到文件末尾怎么办？可以传入 `'a'` 以追加（append）模式写入

## 2. StringIO 和 BytesIO

`StringIO` 和 `BytesIO` 是内存中读写 `str` ，可以做缓存。

要把 `str` 写入 `StringIO` ，我们需要先创建一个 `StringIO` ，然后，像文件一样写入即可。使用 `getvalue()` 方法用于获得写入后的str

```python
>>> from io import StringIO
>>> f = StringIO()
>>> f.write('hello')
5
>>> f.write(' ')
1
>>> f.write('world!')
6
>>> print(f.getvalue())
hello world!
```

要读取 `StringIO` ，可以用一个 `str` 初始化 `StringIO` ，然后，像读文件一样读取

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!')
>>> while True:
...     s = f.readline()
...     if s == '':
...         break
...     print(s.strip())
...
Hello!
Hi!
Goodbye!
```

`StringIO` 操作的只能是 `str` ，如果要操作二进制数据，就需要使用 `BytesIO`

```py
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

可以用一个 `bytes` 初始化 `BytesIO`，然后，像读文件一样读取

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```

## 3. 操作文件和目录

Python的 `os` 模块封装了操作系统的目录和文件操作，要注意这些函数有的在 `os` 模块中，有的在 `os.path` 模块中

```python
>>> import os
>>> os.name    # 操作系统类型
'nt'

>>> os.environ  # 系统环境变量，得到一个 dict
environ({'ALLUSERSPROFILE': 'C:\\ProgramData', 'ANDROID_SDK_HOME': 'C:\\Android', 'APPDATA'})
```

要获取某个环境变量的值，可以调用 `os.environ.get('key')`

```py
>>> os.environ.get('PATH')
'/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin'
>>> os.environ.get('x', 'default')   # 若没有对应的变量，返回None
'default'
```

操作文件和目录

```py
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```

```py
# 拆分路径，使用 `os.path.split()` 函数
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')

# 拆分文件拓展名
>>> os.path.splitext('/path/to/file.txt') 
('/path/to/file', '.txt')

# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
```

可以利用Python的一些高级特性来进行一些骚操作

```python
# 列出当前目录下的所有目录
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]

# 要列出所有的.py文件
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']
```

## 4. 序列化

序列化是把内存中变成可存储或传输的过程

### Python内置的 `pickle` 模块

`pickle.dumps()` 方法把任意对象序列化成一个bytes，使用 `pickle.dump()` 直接把对象序列化后写入一个 `file-like Object`

```python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```

```py
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```

从硬盘读取内容到内存，并且转化为对象的时候，使用 `pickle.loads()` 方法可以将读取到的 `bytes` 转化为对象。也可以使用 `pickle.load()` 方法从一个 `file-like Object` 中直接反序列化出对象

```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

### 使用JSON序列化

Python内置的 `json` 模块提供了Python对象到 `JSON` 格式的转换，具体的使用方法和 `pickle` 很类似

使用 `dumps()` 方法返回一个 `str` ，`dump()` 方法可以直接把 `JSON` 写入一个 `file-like Object` 。默认情况下都是将 `dict` 转化的

```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

若想将普通对象转换为 `json` 格式，需要设置相应的参数。可选参数 `default` 就是把任意一个对象变成一个可序列为 `JSON` 的对象。需要为对象专门写一个转换函数。

```python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }

>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```

大多数情况下，每个实例对象都具有一个 `__dict__` 属性，用来存储示例变量，可以使用这个属性将对象转换为 `dict` 。当要反序列化的时候，可以先使用 `loads()` 方法

```python
print(json.dumps(s, default=lambda obj: obj.__dict__))
```

要将 `json` 转换为一个对象的时候，需要先用 `loads()` 方法将字符串转化为 `dict` ，然后用 `object_hook` 函数把 `dict` 转换为实例对象，同时也需要一个专门的转换函数

```py
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```

dict里面含有中文的时候，可以使用 `json.dumps()` 提供
的 `ensure_ascii` 参数

```python
obj = dict(name='小明', age=20)
s = json.dumps(obj, ensure_ascii=True)

{"name": "\u5c0f\u660e", "age": 20}