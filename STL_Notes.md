# STL (标准模板库)

string (字符串对象)
=================

构造函数：
-------
- `string()`: 初始化空字符串
- `string("str_data")`: 使用char* 字符串初始化 🍖
- `string( size_type length, char ch )`: 使用 n 个字符来初始化
- `string( string &str, size_type index, size_type length )`: 用另一个字符串中间几个来初始化
- `string(const string& str)`: 拷贝构造函数

属性函数：
-------
- `capacity()`: 输出容量，15 + 16 * n
- `reverse()`: 修改容量，只能改大不能改小
- `length()`: 长度
- `size()`: 长度
- `resize()`: 重置字符个数

输入函数：
-------
- 使用 `cin` 
- 使用 `getline(cin, string)`

输出函数：
-------
- `cout`: 直接输出
- `c_str()`: 输出普通字符串
- `at()`: 以及 [] 下标访问: 输出单个字符，注意不能越界
- `back()`: 输出最后一个元素

修改函数：
-------
- `at()` 以及 [] 下标修改
- 中间插入
    - `insert(size_type index, const basic_string &str )`: 中间位置插入一个string对象
    - `insert(size_type index, const char *str )`: 中间位置插入一个字符数组
    - `insert(size_type index1, const basic_string &str, size_type index2, size_type num)`: 插入另一个对象的某一段
    - `insert(size_type index, const char *str, size_type num )`: 插入前几个
    - `insert(size_type index, size_type num, char ch )`: 插入 num 个 ch
- 尾部插入
    - += 拼接字符串
    - `append(const basic_string &str)`: 尾巴通过一个对象拼接
    - `append(const char *str )`: 尾巴通过一个字符串拼接
    - `append(const basic_string &str, size_type index, size_type len)`: 拼接一个字对象的某一段
    - `append(const char *str, size_type num )`: 拼接字符串前几个
    - `append(size_type num, char ch)`: 拼接 num 个字符ch
- 赋值函数: 
    - `assign()`

- 删除操作：
    - `erase(size_type index = 0, size_type num = npos)`: 删除指定位置开始指定个数

- 排序操作：
    - `sort(First, Last, greater<>())`: 参数三 可以指定从大到小

vector (动态数组)
===============

构造函数：
-------
- `vector<> vec()`: 无参构造函数
- `vector<> vec(n)`: 指定容器的大小
- `vector(size_type num, const TYPE &val )`: 用 num 个 val 来初始化容器
- `vector(const vector &from)`: 拷贝构造
- `vector(input_iterator start, input_iterator end)`: 迭代器初始化

属性函数：
-------
- `capacity()`: 容量函数，容量不够时增加已有容量的一半
- `size()`: 大小
- `resize()`: 修大小
- `empty()`: 判断是否为空

操作函数：
--------
- `at()` 以及 [] 下标访问
- `back()` 返回最后的一个元素
- 增加操作 (迭代器操作)：
    - `push_back()`: 尾添加
    - `void insert(iterator loc, size_type num, const TYPE &val)`: 在某个迭代器后加 num 个值为 value 的元素
    - `void insert(iterator loc, input_iterator start, input_iterator end)`:在某个迭代器后加入另一个向量的中间一段
- 删除操作:
    - `pop_back()`: 尾删除
    - `erase(iterator loc)`: 删除一个
    - `erase(iterator start, iterator end)`: 删除一段
    - `clear()`: 删除所有
    - `swap(vector &from)`: 交换两个容器

随机数生成：
=========
- `#include <stdlib.h>`
- `#include <time.h> `
- 初始化随机数种子，放在开头 `srand((unsigned)time(NULL));`
- `rand()` 生成 `0 ~ RAND_MAX` 之间任意的随机数

map(底层红黑树)、unorderd_map(底层哈希表)：
=====================================
- 定义 `map<int, string> dict`
- 大小 `dict.size()`
- 直接使用 `[]` 来访问和修改字典值 `dict[3] = "hello"`
- 查找是否存在:
  - `map<int, string>::iterator iter`
  - `iter = dict.find(3)`
  - 不存在时，`iter == dict.end()` 根据这个来判断是否存在
- 遍历
```c++
    for (auto iter = dict.begin(); iter != dict.end(); iter++) {
        cout << iter->first << ": " << iter->second << endl;
    }
```

stack(栈)：
=========
- 定义 `stack<int> s`
- 返回栈顶元素 `s.top()`
- 出栈 `i = s.pop()`
- 入栈 `s.push(i)`

queue(队列)：
==========
- 定义 `queue<int> q`
- 返回队前元素 `q.from()`
- 返回队尾元素 `q.back()`
- 入队 `q.push(i)`
- 删除队首元素 `q.pop()`


