# Python笔记一 (List、Tuple、dict、set、循环)

## 1. List类创建与使用（可变的有序表）

Python内置的一种数据类型是列表：list，list是一种有序的集合，可以随时添加和删除其中的元素

使用中括号进行创建，可以进行嵌套

```Python
classmates = ['Michael', 'Bob', 'Tracy']
```

调用的时候也是使用中括号的，索引是从0开始

```Python
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy
```

也可以使用负数来进行调用，索引 `-1` 表示最后一个， `-2` 表示倒数第二个

```Python
>>> classmates[-1]
'Tracy
```

## 2. List类长度

```Python
>>> len(classmates)
3
```

## 3. List类元素的添加与删除

使用 `append()` 方法在末尾添加元素

```Python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

使用 `insert()` 方法把元素插入到指定的位置

```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

使用 `pop()` 方法删除末尾的元素

```Python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

删除指定位置的元素，用pop(i)方法

```python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

也可以直接像Matlab的矩阵一样，使用+进行相加

## 4. tuple类的创建和使用（不可变有序元组）

使用圆括号进行创建

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')
```

定义一个空的tuple，可以写成()：

```python
>>> t = ()
>>> t
()
```

只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：

```python
>>> t = (1,)
>>> t
(1,)
```

## 4. dict

Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用 键-值（key-value）存储，具有极快的查找速度。内部使用哈希函数来实现的，直接根据关键字计算出对应信息的地址

使用大括号进行创建，中间逗号隔开。key必须是不可变对象，所以是整数或者字符串作为key

```python
d = {1: 100, 2: 80, 3: 60, 'four': 40}
```

调用的时候一样是使用中括号

```python
d[1]
```

添加内容或者更改直接操作即可

```python
d['five'] = 50
```

删除使用 `pop()` 方法

```python
d.pop(2)
```

判断 key 是否在里面

```python
'four' in d
True

d.get('four', message) #若存在则返回value，不存在返回message
```

## 5. set

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

要创建一个set，需要提供一个list作为输入集合：

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

添加元素使用 `add()` 方法

```py
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

删除元素使用 `remove()` 方法

```py
>>> s.remove(4)
>>> s
{1, 2, 3}
```

## 6. 循环

### 第一种使用 `for...in` 语法来实现循环，一次把list或者tuple的元素迭代出来

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

当要实现整数迭代的时候，使用python内置的 `range()` 方法，配合 `list()` 方法可以转化成 `list` 

```py
>>> list(range(5))  #range(n)可以得到 0~n-1 的n个数字
[0, 1, 2, 3, 4]
```

### 第二种是使用 `while` 语法实现，和c语言差不多，也有 `break` 和 `continue` 语法