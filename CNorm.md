# C一些代码规范

## 1. 缩进

- 缩进建议使用 `Tab` 键（`CapsLock`键上面那个）
- 也可以使用四个空格，一些编辑器会自动把 `Tab` 转换成四个空格
- 无论选择哪一种，切记不要混用

下面是一些常用代码片段的例子

```c
#include<stdio.h>
int main() {
    // write your code
    return 0;
}
```

- 条件语句

```c
if (condition) {  // 圆括号里没有空格.
	...  // 一个缩进.
} else {
	... // 一个缩进.
}
// important --> 条件语句按上面这个结构，就算只有一条语句，也建议把 { } 加上，方便后面修改代码
```

- 循环语句

```c
while (condition) {
  // 循环的内容
}
```

```c
for(i = 1; i < 10; i++) {
    printf("%d", a[i]);
}
// important --> for循环按上面这个结构，就算只有一条语句，也建议把 { } 加上，方便后面修改代码
```

## 2. 加点空格

```c
void example() {
    int a = 3 * 4 + 5;  // 运算符之间加空格，更容易区分每一部分
    if (a == 1) {       // == 两边有两个空格
        printf("a is 1");
    }
}
```

## 3. 加点空行

```c
void example() {
    int a = 3 * 4 + 5;       // 运算符之间加空格，更容易区分每一部分
    int i;
    int b[10] = {1, 2, 3};
    
    if (a == 1) {             // 实现不同功能的部分可以加点空行，
        printf("a is 1");
    }
    
    for(i = 0; i < 10; i++) {  // 这上面也可以加一行
        printf("%d\n", b[i]);
    }
}
```

以上格式参考了 [Google 代码规范](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents/)，可以自己看看





# 作业格式规范

## 1. 题号标注

- 代码以外的文字都写在注释里面
- 题号加个 `{{  }}` ，这个主要是方便批改
- 下面给一个模板，写作业可以按照这个格式来写

```C
// {{P215,T2}}  -- 这个的意思是P215第二题
// 随便写点什么，或者有什么话想对助教说的，都放在注释里面
#include<stdio.h>
int main() {
    printf("hi t2");
    return 0;
}

// {{P215,T8}}
// 想写什么都可以
#include<stdio.h>
void p() {
    printf("hi t8");
}

int main() {
    p();
    return 0;
}

// {{P215,T18}}
#include<stdio.h>
int main() {
    printf("hi t18");
    return 0;
}
```

- 将这个放到一个 `txt` 文本文档里面就是一份作业了
- 推荐使用 `txt` 文本文档，但有时候需要带上运行截图的话，就需要用 `Word`了

## 2. scanf 输入

- `scanf` 输入各式各样，这里统一一下，方便作业批改

```c
int example() {
    int a, b;
    scanf("%d%d", &a, &b);
    // 两个%d中间没有东西
}
```

- 连续输入多个值的时候，统一中间不加空格或者逗号

## 3. system("pause")

- 一些同学的程序运行后不会自动暂停，加入了 `system("pause")`

- 无论如何，用了这句话都把 `#include<stdlib.h>` 加在开头
