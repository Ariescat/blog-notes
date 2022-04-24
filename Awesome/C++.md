# 《C++》



## C++基础

### 基本类型

- 浮点数比较

  fabs(f1-f2) < 预先指定的精度

  而abs()函数是针对整数的



### 字符串

C语言没有原生的字符串类型！

C风格字符串就是最后一位为'\0'的**字符数组**！C语言通过字符指针来管理字符串！

在C++语言中，除了继承了C语言中的这种字符串表达形式外，还新添了string类用来表达字符串。为了区分C++中这两种不同的字符串，使用“C风格字符串”来特指来源于C语言的字符串存储方式。

- string.h

  注意：

  `#include <string.h>` 和 `#include <cstring>` 是相同作用的，而`#include <string>`是C++字符串变量 string 必须要的头文件（包含一些操作符的重载等）。

  一些重要的方法：

  - memcpy

  - memset

  [C 标准库 –  | 菜鸟教程 (runoob.com)](https://www.runoob.com/cprogramming/c-standard-library-string-h.html)



### 向量和数组

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

- 内存对齐

  为什么要内存对齐：[C++：内存对齐_六月的翅膀的博客-CSDN博客](https://blog.csdn.net/cd_yourheart/article/details/109341988)

  结构体占用内存大小

- .hpp与.h区别

  .hpp，本质就是将.cpp的实现代码混入.h头文件当中，定义与实现都包含在同一文件，则该类的调用者只需要include该.hpp文件即可，无需再将cpp加入到project中进行编译。而实现代码将直接编译到调用者的obj文件中，不再生成单独的obj，采用hpp将大幅度减少调用project中的cpp文件数与编译次数，也不用再发布lib与dll文件，因此非常适合用来编写公用的开源库。

  原文链接：https://blog.csdn.net/f_zyj/article/details/51735416

- gcc与g++的区别

  编译的四个阶段

  1. 预处理：编译处理宏定义等宏命令（eg:#define）——生成后缀为“.i”的文件
  2. 编译：将预处理后的文件转换成汇编语言——生成后缀为“.s”的文件
  3. 汇编：由汇编生成的文件翻译为二进制目标文件——生成后缀为“.o”的文件
  4. 连接：多个目标文件（二进制）结合库函数等综合成的能直接独立执行的执行文件——生成后缀为“.out”的文件

  在我们理解了上述四个流程后，我们在关注[gcc](https://so.csdn.net/so/search?q=gcc&spm=1001.2101.3001.7020)和g++在流程上的区别。

  gcc无法进行库文件的连接，即无法编译完成步骤4；而g++则能完整编译出可执行文件。（实质上，g++从步骤1-步骤3均是调用gcc完成，步骤4连接则由自己完成）

- std::move

  [什么是move？理解C++ Value categories，move， move in Rust](https://zhuanlan.zhihu.com/p/374392832)





## 调用其他语言

- Python
  - C++调用Python
  - Python调用C++





## 第三方库

- skynet

  一个基于C跟lua的开源服务端并发框架，这个框架是单进程多线程模型

- boost

  Boost 库通过加入一些在实践中非常有用的函数对 C++ 标准进行了补充。

  [Boost C++ 库-在线教程](https://www.mianquan.net/tutorial/boost/)

- 网络库
  - libevent

  - boost 的 asio

    一个很强大的实现socket通讯方式的跨平台（windows、linux、solaris、mac os x）解决方案，能同时支持数千个并发的连接。

    [Boost.Asio的使用技巧 | blog | 逍遥郡 (jqian.net)](http://blog.jqian.net/post/boost-asio.html)

  - zeromq

    [云风的 BLOG: ZeroMQ 的模式 (codingnow.com)](https://blog.codingnow.com/2011/02/zeromq_message_patterns.html)

    引用：

    > 基于定义好的模型，我们可以看到，api 可以实现的非常简单易用。我们不再需要 bind/listen/accept 来架设服务器，因为这个模型天然是 1:N 而不是 1:1 的，**不需要为每个通道保留一个句柄**。我们也不必在意 server 是否先启动（bind），而后才能让 client 工作起来（connect）。

- 服务发现
  - etcd