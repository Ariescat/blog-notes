# 《C++》



## C++基础

### 基本类型

- 浮点数比较

  fabs(f1-f2) < 预先指定的精度

  而abs()函数是针对整数的



### 字符串、向量和数组

- string.h

  - memcpy

  - memset

  [C 标准库 –  | 菜鸟教程 (runoob.com)](https://www.runoob.com/cprogramming/c-standard-library-string-h.html)

- [动态申请二维数组](https://blog.csdn.net/qq_41822235/article/details/81142107)

  - 利用一个**二级指针**来实现

    ```c
    //5 行 2 列的数组
    int **p = (int **)malloc(sizeof(int *) * 5);
    for (int i = 0; i < 5; ++i) {
        p[i] = (int *)malloc(sizeof(int) * 2);
    }
    //输出数组每个元素地址
    printf("%p\n", &p[i][j]);
    ```

  - 利用**数组指针**来实现

    ```c
    //申请一个 5 行 2 列的整型数组
    int(*p)[2] = (int(*)[2])malloc(sizeof(int) * 5 * 2);
    //输出数组每个元素地址
    printf("%p\n", &p[i][j]);
    ```

  - 利用**一维数组**来模拟二维数组

    ```c
    int *p = (int *)malloc(sizeof(int) * 5 * 2);
    //输出数组每个元素地址
    printf("%p\n", &p[i*2+j]);
    ```

    > `malloc`返回的其实是`void *`，所以其需要强转，`void *`的用处还有`memcpy`，`memset`等

- "\ddd" 和 "\xhh" 分别是什么意思?

  "\ddd" 表示1~3位八进制数ddd对应的字符，例如 '\141' 代表字符常量 'a'

  "\xhh" 表示1~2位十六进制数hh对应的字符，例如 '\x41' 代表字符常量 'A'

  如：`putchar('\101')`输出字符A；`putchar('\015')`输出回车，不换行，使输出的当前位置移到本行开头





## C++标准库

C++:STL（Standard Template Library，标准模板库）

STL的代码从广义上讲分为三类：algorithm（算法）、container（容器）和iterator（迭代器），几乎所有的代码都采用了模板类和模版函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。在C++标准中，STL被组织为下面的13个头文件：

`<algorithm>`、`<deque>`、`<functional>`、`<iterator>`、`<vector>`、`<list>`、`<map>`、`<memory>`、`<numeric>`、`<queue>`、`<set>`、`<stack>`、`<utility>`。 

`#include<bits/stdc++.h>`包含C++的全部头文件

附：STL标准入门汇总 http://www.cnblogs.com/shiyangxt/archive/2008/09/11/1289493.html





### IO库

- 输入输出重定向

  在默认情况下，cin 只能接收从键盘输入的数据，cout 也只能将数据输出到屏幕上。但通过重定向，cin 可以将指定文件作为输入源，同样 cout 可以将原本要输出到屏幕上的数据转而写到指定文件中。

  实现：

  freopen()函数

  rdbuf()函数

  在控制台中使用 > 或者 < 实现重定向的方式，如 `C:\Users\mengma>D:\demo.exe <in.txt >out.txt`

- 打印格式

  %3d		 可以指定宽度，不足的左边补空格

  %-3d		左对齐

  %03d		一种左边补0 的等宽格式,比如数字12,%03d出来就是: 012





## 高级

- 结构体占用内存大小
- 内存对齐 是什么？





## 第三方库

- skynet
- libevent