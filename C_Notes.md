C语言复习笔记
===========

格式化输入输出（IO）：
------------------
- `puts()`: 输出字符串，会自动换行
- `putchar()`: 输出单个字符
- `printf()`: 复杂的格式化输出
  - 格式化控制符完整格式：`%[flag][width][.precision]type`
  - type类型有：d(十进制)、u(无符号十进制)、x(十六进制)、o(八进制)、f(float)、lf(double)、p(指针)、c(字符)、s(字符串)
  - flag：-(左对齐)、+(输出正负号)、空格(正数输出前面加空格)、#(进制前缀或者小数点)
  - width：最小输出宽度
  - precision：小数时是小数点后的位数、整数时是长度不够会在左边补0

- `getchar()`：输入单个字符
- `getch()、getche()`：无缓冲区的输入函数，Windows特有的
- `scanf()`：复杂的格式化输入函数、和 printf 类似
- `fflush(stdin)`：清空输入缓冲区，也可以用  `while((c = getchar()) != '\n' && c != EOF)`

格式化读写文件：
------------
- `fscanf(FILE* p, char* format)`：从文件输入数据
- `fprintf(FILE* p, char* format)`：输出数据到文件
- `fgets(char* str, int n, FILE* p)`：从文件里面读取n-1个字符保存到str里面
- `fputs(char* str, FILE* p)`：将一行数据输出到文件里面

各种指针：
-------
- `int* p`: 普通指针
- `int* p[6]`: 指针数组
- `int (*p)[6]`: 二维数组指针
- `int (*p)()`: 函数指针
- 判断技巧
  - 第一步先找出里面的变量，从变量开始
  - 变量右边开始判断，`[]`的优先级比`*`高，先判断是什么
  - 然后再看变量左边的


字符串处理函数：
------------
- `strcpy(char *dest, char *src)`: 字符串复制
- `strcat(char *dest, char *src)`: 字符串拼接
- `strcmp(char *dest, char *src)`: 字符串比较


c++部分
======

继承和多态：
--------
- 多态实现的条件有三个
  - 有继承关系
  - 有 `override` 重写
  - 基类指针指向子类对象
- 实现时候的注意点
  - `override` 关键字是加在子类函数后面的
  - 记得加 `public` 修饰类中变量
  - 类、结构体、共用体定义的时候最后要加括号
  - 继承的时候也有 `public` 修饰

```c++
class Animal {
public:
    virtual void eat() {
        cout << "Animal eat" << endl;
    }
};

class Dog : public Animal{
public:
    void eat() override {
        cout << "Dog eat" << endl;
    }
};

class Cat : public Animal{
public:
    void eat() override {
        cout << "Cat eat" << endl;
    }
};

// 调用过程
Animal* a = new Dog();
a->eat();    // 输出 Dog eat
a = new Cat();
a->eat();   // 输出 Cat eat
```


