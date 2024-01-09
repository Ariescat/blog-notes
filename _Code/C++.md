# 《C++》



## C++基础

### 基本类型

**浮点数比较**

fabs(f1-f2) < 预先指定的精度

而abs()函数是针对整数的



### 字符串string

**C语言没有原生的字符串类型！**

C风格字符串就是最后一位为'\0'的**字符数组**！C语言通过字符指针来管理字符串！

在C++语言中，除了继承了C语言中的这种字符串表达形式外，还新添了string类用来表达字符串。为了区分C++中这两种不同的字符串，使用”C风格字符串”来特指来源于C语言的字符串存储方式。

<br/>

**string.h**

注意：

`#include <string.h>` 和 `#include <cstring>` 是相同作用的，而`#include <string>`是C++字符串变量 string 必须要的头文件（包含一些操作符的重载等）。

一些重要的方法：

- memcpy

- memset

[C 标准库 –  | 菜鸟教程 (runoob.com)](https://www.runoob.com/cprogramming/c-standard-library-string-h.html)

<br/>

**"\ddd" 和 "\xhh" 分别是什么意思?**

"\ddd" 表示1~3位八进制数ddd对应的字符，例如 '\141' 代表字符常量 'a'

"\xhh" 表示1~2位十六进制数hh对应的字符，例如 '\x41' 代表字符常量 'A'

如：`putchar('\101')`输出字符A；`putchar('\015')`输出回车，不换行，使输出的当前位置移到本行开头



### 数组vector

**动态申请二维数组**

[动态申请二维数组（C语言版）_楚楚可薇的博客-CSDN博客](https://blog.csdn.net/qq_41822235/article/details/81142107)

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

<br/>

**指针数组&数组指针**

指针数组，首先它是一个数组，数组里面的每个元素都是一个指针，例如比如`int *p[4]` 就是一个指针数组，因为运算符`[]`的优先级比运算符`*`的优先级高，所以`p`优先和`[]`组成数组，然后`*`和类型int组合成数组元素的类型。

数组指针，首先它是一个指针，这个指针所指向的对象是数组，比如这个指针是`p`，那么通过解引用`*p`获得内容就是一个数组，例如`int (*p)[4]`，主意带上括号， **通常数组指针也作为一个二维数组来使用**。

```c++
int a[2][3]={ {1,2,3}, {4,5,6} };
int (*p)[3] = a;
```

**等价关系：**

> `a+i == p+i`
> `a[i] == p[i] == *(a+i) == *(p+i)`
> `a[i][j] == p[i][j] == *(a[i]+j) == *(p[i]+j) == *(*(a+i)+j) == *(*(p+i)+j)`







## C++标准库



### 介绍

C++:STL（Standard Template Library，标准模板库）

STL的代码从广义上讲分为三类：algorithm（算法）、container（容器）和iterator（迭代器），几乎所有的代码都采用了模板类和模版函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。在C++标准中，STL被组织为下面的13个头文件：

`<algorithm>`、`<deque>`、`<functional>`、`<iterator>`、`<vector>`、`<list>`、`<map>`、`<memory>`、`<numeric>`、`<queue>`、`<set>`、`<stack>`、`<utility>`。 

`#include<bits/stdc++.h>`包含C++的全部头文件

附：STL标准入门汇总 http://www.cnblogs.com/shiyangxt/archive/2008/09/11/1289493.html



### IO库

**输入输出重定向**

在默认情况下，cin 只能接收从键盘输入的数据，cout 也只能将数据输出到屏幕上。但通过重定向，cin 可以将指定文件作为输入源，同样 cout 可以将原本要输出到屏幕上的数据转而写到指定文件中。

实现：

freopen()函数

rdbuf()函数

在控制台中使用 > 或者 < 实现重定向的方式，如 `C:\Users\mengma>D:\demo.exe <in.txt >out.txt`

<br/>

**打印格式**

%3d		 可以指定宽度，不足的左边补空格

%-3d		左对齐

%03d		一种左边补0 的等宽格式,比如数字12,%03d出来就是: 012







## 高级



### 指针

**指针为什么有类型**

为了指针运算和取值。

<br/>

**数组指针**

看上面「数组」部分内容。

<br/>

**二级指针**

指向指针的指针，道理是这么个道理，但有什么用？

二级指针在C++中可能用的不多，但是在C中是经常使用的一把利器，它**通常作为一个函数的参数，起到在函数内部对一个指针进行初始化的作用**， 比如经典的音视频处理工具`FFmpeg`中就大量使用了二级指针。以下例子展示如何通过二级指针对指针形式赋值：

```c++
void initP(int **p){
    *p = new int(10);
}
int main() {
    int *p = nullptr; // 一个空的指针
    initP(&p); // 通过二级指针初始化指针p
    std::cout << "*p的值：" << *p << endl;
    delete p;
    return 0;
}
```

难道不能通过给函数传递一级指针给指针初始化吗？这是不行的，这是因为值传递的缘故。具体可看：[二级指针作用_XZshijian的博客-CSDN博客](https://blog.csdn.net/XZshijian/article/details/122703565)

<br/>

**指针与多态绑定**

在C++语言中，当我们使用基类的引用（或指针）调用一个虚函数时将发生动态绑定。也就是说使用通过**父类的指针或引用**就能按照实参的**实际类型**是父类还是子类**调用不同的虚函数**。

```c++
class Base{
public:
    virtual void print() const{
        std::cout << "base print" << endl;
    }
    virtual ~Base(){
    }
};

class Child:public Base{
public:
    void print() const override{
        std::cout << "Child print" << endl;
    }
};

void testPrint(const Base &base){
    base.print();
}

int main() {
    Base a = Child();
    testPrint(a);// 打印Base print
    Child b = Child(); // 注意，不能写成Base b = Child()，否则打印的是Base的print
    testPrint(b); // 打印Child print
    Base *c = new Child(); // 指针，动态类型与静态类型不一致
    testPrint(*c); // 打印Child print

    Base &&r = Child(); // 表达式是右值引用，动态类型与静态类型不一致
    testPrint(r); // 打印Child print
    return 0;
}
```

为什么在上面的程序中变量`a`的实际类型是Child，但是函数`testPrint`内部调用的却是父类的打印方法呢？不是说引用会触发多态吗？函数`testPrint`也是通过引用传递的呀， 真是百思不得其jie呀。

要解开这个疑惑就得了解下静态类型和动态类型的知识了。静态类型在编译时总是已知的，首先静态类型是**变量声明时的类型或表达式生成的类型**；动态类型则是变量或表达式表示的**内存中的对象的类型**，动态类型直到运行时才可知。如果变量在定义时表达式**既不是引用也不是指针**，则它的动态类型永远与静态类型一致的，也就是声明时所指的类型，否则的话静态类型可能与动态类型不一致。

<br/>

**函数指针**

`返回值类型 (*函数名) (参数)` 

函数指针的一个重要用途就是作为函数的参数，用于在函数内部进行指针函数的调用，一般用作回调函数

<br/>

**类成员指针**

`<类型说明符><类名>::*<指针名>`

类成员指针可以指向类的非静态成员。一般情况下，一个指针指向一个对象，但是成员指针指示的是类的成员，而非类的对象。指向类的静态成员的指针和普通指针没有什么区别。

类成员函数指针指向类的成员函数的指针。





### 引用

**指针和引用的区别**

1. 指针可以为空，**引用必须初始化**，引用不能为空

2. **指针可以被重新赋值，但是引用不行**。也就是指针可以重新指向另外一个对象，而引用却不行，引用一直指向的都是最初的那个对象。

3. 指针可以有多级，而引用只能是一级，例如我们平时说的指向指针的指针，也就是二级指针，但是就没有所谓的指向引用的引用。

4. 有了指针为什么还需要引用？我们都知道指针在使用过程中需要特别地小心，很容易就出现空指针、野指针等令人诟病的问题。但是引用因为它自始至终都是指向一个单一的对象，所以引用比指针更具安全性， 而且使用引用在处理C++的某些问题更加的得心应手，例如运算符的重载等。

   > **C语言是没有引用的，引用是在C++里面才存在的神级操作。**

5. 有了引用为什么还需要指针？在C++中既然有了引用，设置引用的性能比指针更高点，那么为什么还需要指针呢？为了兼容C语言。

6. 返回引用还是返回指针？这个要看具体的使用场景，如果作为函数的返回值，C++明确表明是不可以返回局部对象的引用的，因为局部对象在函数返回后就会被析构掉，所以返回它的引用也就没有了意义。但返回非局部对象的引用是允许的，例如STL中vector中按下标取值就可以返回一个引用。

   如果确实需要返回一个局部对象的话，可以返回一个在堆中的对象指针。暗示返回一个局部对象的指针的话会引发另外一个问题， 那就是这个指针什么时候释放呢？由谁来释放呢？一旦管理不好，内存泄漏是分分钟的事情。因此如果是非局部对象的话可以返回对象的引用，否则可以按值返回，**按值返回特别是在C++11之后已经自带了RVO优化**， 可以放心使用，更多RVO相关只是可以看[《C++之RVO返回值优化》](https://mp.weixin.qq.com/s?__biz=MzI2MDkzMTExMQ==&mid=2247484218&idx=1&sn=80a1ca7de7a4202abbf44fdaa46ec2b6&scene=21#wechat_redirect) 。

7. 指向指针的引用

   首先要说明的是指向指针的引用，它是一个引用，而不是指针，指针本质上来说是一个内存地址，但是引用并不是一个对象，引用不会开辟新的内存空间，所以不存在着指向引用的指针这么一说。

   例如下面的示例代码：

   ```c++
   int i = 100;
   int *p;
   int *&r = p; // r是一个引用，引用类型是指针
   r = &i; //因为r是一个引用，所以给r赋值，就是将p指向i
   *r = 0; // 解引用r，也就是将指针p指向的值改为0，也就是将i改为0
   ```

   从右向左阅读，r的定义离变量名最近的符号（此例中是&r的符号&）对变量的类型有最直接的影响。因此r是一个引用，它引用的对象是一个int类型的指针。





### 多态与重载

多态性分两个部分：

1. 静态多态性：通过重载实现，同名不同参。
2. 动态多态性：多态，子类覆盖父类方法，运行时根据指针的类型来决定最终执行的方法。即：虚函数。





### delete和delete[]

如果调用delete，只会调用一次析构函数，如果调用delete[]会多次调用析构函数。当类的析构函数中需要去释放堆内存的时候，本该用delete[]多次调用析构函数，结果用delete，只调用了一次析构函数，那么就会造成内存泄漏。





### 内存管理

**C 的内存管理**

- malloc 和 free
  - 基于系统调用 sbrk 或 mmap 实现
  - 分配的内存位于 [heap] 或者 匿名 mmap







## 编译



### 内存对齐

为什么要内存对齐：[C++：内存对齐_六月的翅膀的博客-CSDN博客](https://blog.csdn.net/cd_yourheart/article/details/109341988)

结构体占用内存大小



### .hpp与.h区别

.hpp，本质就是将.cpp的实现代码混入.h头文件当中，定义与实现都包含在同一文件，则该类的调用者只需要include该.hpp文件即可，无需再将cpp加入到project中进行编译。而实现代码将直接编译到调用者的obj文件中，不再生成单独的obj，采用hpp将大幅度减少调用project中的cpp文件数与编译次数，也不用再发布lib与dll文件，因此非常适合用来编写公用的开源库。

原文链接：https://blog.csdn.net/f_zyj/article/details/51735416



### gcc与g++的区别

编译的四个阶段

1. 预处理：编译处理宏定义等宏命令（eg:#define）——生成后缀为“.i”的文件
2. 编译：将预处理后的文件转换成汇编语言——生成后缀为“.s”的文件
3. 汇编：由汇编生成的文件翻译为二进制目标文件——生成后缀为“.o”的文件
4. 连接：多个目标文件（二进制）结合库函数等综合成的能直接独立执行的执行文件——生成后缀为“.out”的文件

在我们理解了上述四个流程后，我们在关注[gcc](https://so.csdn.net/so/search?q=gcc&spm=1001.2101.3001.7020)和g++在流程上的区别。

gcc无法进行库文件的连接，即无法编译完成步骤4；而g++则能完整编译出可执行文件。（实质上，g++从步骤1-步骤3均是调用gcc完成，步骤4连接则由自己完成）







## C++11

### 右值引用

**意义**

[C++之右值引用 - 简书 (jianshu.com)](https://www.jianshu.com/p/de859b5cd42d)

右值引用是 C++11 引入的与 Lambda 表达式齐名的重要特性之一。它的引入解决了 C++ 中大量的历史遗留问题，消除了诸如 std::vector、std::string 之类的额外开销。

左值就有内存地址的，存活的生命周期较长的，而右值一般是无法获取到内存地址的（比如整形字面量）。

右值引用的特点之一是可以延长右值的生命周期；**但，延长临时对象生命周期并不是这里右值引用的最终目标，其真实目标应该是减少对象复制，提升程序性能。**



**将亡值**

[【乔红】裤衩 C++ 之 右值引用（一）为什么会有右值引用_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Vq4y1K7ut?spm_id_from=333.1007.partition_recommend.content.click)





### 移动构造函数

**先说拷贝构造函数，默认拷贝构造函数：**

c++类的中有两个特殊的构造函数，(1)无参构造函数，(2)拷贝构造函数。它们的特殊之处在于：

(1)当类中没有定义任何构造函数时，编译器会默认提供一个无参构造函数且其函数体为空；

(2)当类中没有定义拷贝构造函数时，编译器会默认提供一个拷贝构造函数，进行成员变量之间的拷贝。(这个拷贝操作是浅拷贝)

<br/>

**了解深拷贝和浅拷贝：**

拷贝者和被拷贝者若是同一个地址，则为浅拷贝，反之为深拷贝。 

类的默认拷贝构造函数只会用**被拷贝类的成员的值**为拷贝类简单初始化，也就是说二者的p指针指向的内存空间是一致的。

<br/>

**默认拷贝构造函数的弊端：**

[c++ 拷贝构造函数(重点在内含指针的浅拷贝和深拷贝) - 知行者的博客 - 博客园 (cnblogs.com)](https://www.cnblogs.com/r-yan/p/11727889.html)

```c++
class TestCls{
public:
    int a;
    int *p;
public:
    TestCls(){
        p = new int;
    }
    ~TestCls(){
        delete p;
    }
};
int main(void){
    TestCls t1;
    TestCls t2 = t1; //效果等同于TestCls t2(t1); 
    return 0;
}
```

编译器为我们默认定义的拷贝构造函数为：

```c++
TestCls(const TestCls &testCls)
{
    a = testCls.a;
    p = testCls.p; //两个类的p指针指向的地址一致。
};
```

main函数将要退出时，拷贝类t2的析构函数先得到执行，它把自身p指向的堆空间释放了；接下来，t1的析构函数得到调用，被拷贝类t1的析构函数得到调用，它同样要去析构自身的p指向指向的堆空间，但是该空间和t2类中p指向的空间一样，造成重复释放，程序运行崩溃。（当然，如果只有基本类型数据是没有问题的）

解决办法就是自定义拷贝构造函数：

```c++
class TestCls{
public:
    int a;
    int *p;
public:
    TestCls(){
        p = new int;
    }
    TestCls(const TestCls &testCls){
        a = testCls.a;
        // p = testCls.p;
        p = new int;
        *p = *(testCls.p); //为拷贝类的p指针分配空间，实现深度拷贝
    }
    ~TestCls(){
        delete p;
    }
};
```

所以，当类中拥有指针类型的成员变量时，拷贝构造函数中需要以深拷贝（而非浅拷贝）的方式复制该指针成员。

<br/>

**C++11移动构造函数的功能和用法：**

直接看这篇吧：

[C++11移动构造函数的功能和用法_Hardy20200507的博客-CSDN博客](https://blog.csdn.net/Hardy20200507/article/details/123315890)

```c++
TestCls(TestCls &&t) : p(t.p)
{
    t.p = NULL;
    std::cout << "move construct!" << std::endl;
}
```

也就是，在之前 TestCls 类的基础上，我们手动为其添加了一个构造函数。和其它构造函数不同，此构造函数使用右值引用形式的参数，又称为移动构造函数。**并且在此构造函数中，num 指针变量采用的是浅拷贝的复制方式，同时在函数内部重置了 d.num，有效避免了“同一块对空间被释放多次”情况的发生。**

在实际开发中，**通常在类中自定义移动构造函数的同时，会再为其自定义一个适当的拷贝构造函数，由此当用户利用右值初始化类对象时，会调用移动构造函数；使用左值（非右值）初始化类对象时，会调用拷贝构造函数。**





### std::move

[什么是move？理解C++ Value categories，move， move in Rust](https://zhuanlan.zhihu.com/p/374392832)

[std::move和右值引用_液压姬的博客](https://blog.csdn.net/crazty/article/details/113092170)



**在使用智能指针时，通常需要使用 std::move 来进行对象的转移。**

对于 std::unique_ptr，由于它不能被复制，因此只能使用 std::move 来将其转移给另一个 std::unique_ptr。对于 std::shared_ptr，由于它可以被多个指针共享，因此需要使用 std::move 来将其转移给另一个 std::shared_ptr，或者使用 std::make_shared 来创建一个新的 std::shared_ptr。



**std::move 对于 std::shared_ptr 的行为与其他类型的对象略有不同。**

std::shared_ptr 是一种智能指针，它会自动管理动态分配的内存，并在不再需要时自动释放。std::move 可以将 std::shared_ptr 的所有权转移给另一个 std::shared_ptr，但是不会影响内存的引用计数。

具体来说，当使用 std::move 将一个 std::shared_ptr 转移给另一个 std::shared_ptr 时，会将源 std::shared_ptr 中的指针和引用计数移动到目标 std::shared_ptr 中，同时将源 std::shared_ptr 置为空指针。这样做可以避免不必要的内存复制和引用计数的增加，从而提高性能。

下面是一个示例代码，演示了如何使用 std::move 将一个 std::shared_ptr 转移给另一个 std::shared_ptr：

```c++
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> p1 = std::make_shared<int>(42);
    std::shared_ptr<int> p2 = std::move(p1); // 将 p1 的所有权转移给 p2
    std::cout << *p2 << std::endl; // 输出 42
    std::cout << (p1 == nullptr) << std::endl; // 输出 1，即 p1 为空指针
    return 0;
}
```

需要注意的是，使用 std::move 转移 std::shared_ptr 时，需要确保源 std::shared_ptr 不再需要使用，否则可能会导致内存泄漏或者程序崩溃。此外，还需要注意避免出现循环引用的情况，否则可能会导致内存泄漏。





### std::forward

回顾上面的「C++之右值引用 - 简书 (jianshu.com)」链接：

**先了解万能引用：**

所谓的万能引用就是既可以引用左值，也可以引用右值的引用。

```c++
void test(int &t){
    // 左值引用
}
void test(int &&t){
    // 右值引用，有明确的类型
}
template<typename T>
void test(T &&){
    // 万能引用，因为模板需要类型推导
}
int getNum(){
    return 20;
}
int main() {
    int &&num1 = getNum(); // 右值引用
    auto &&num2 = getNum(); // 万能引用，类型推导
    return 0;
}
```

在上面的注释中我们发现只要发生了类型推导就会是万能引用，在`T&&`和`auto&&`的初始化过程中都会发生类型的推导所以它们是万能引用。在这个推导过程中，初始化的源对象如果是一个左值，则目标对象会推导出左值引用；反之如果源对象是一个右值，则会推导出右值引用。

**完美转发：**

万能引用，它的一个重要用途就是进行完美转发，所谓完美转发指的是**函数模板可以将自己的参数“完美”地转发给内部调用的其它函数**，不仅能准确地转发参数的值，还能保证被转发参数的左、右值属性不变。在C++11使用标准库中的`std::forward`函数就可以试下完美转发：

```c++
void test(int &t){
    // 左值引用
    cout << "左值" << endl;
}

void test(int &&t){
    // 右引用
    cout << "右值" << endl;
}

template<typename T>
void funcForward(T &&t){
    // 进行了转发，根据传递进来的值类型而调用不同test
    test(std::forward<T>(t));
}

template<typename T>
void funcNormal(T &&t){
    // 没有进行转发，始终调用的都是左值的test
    test(t);
}

int main() {
    int a = 20;
    funcNormal(1); // 右值，但是调用的是左值的test
    funcNormal(a); // 左值
    cout << "----------------------" << endl;
    funcForward(1); // 右值
    funcForward(a); // 左值
    return 0;
}
```





### RVO返回值优化

RVO的全称是Return Value Optimization。RVO是一种编译器优化技术，可以把通过函数返回创建的临时对象给”去掉”，然后可以达到少调用拷贝构造的操作目的， 它是C++11标准的一部分。

上面「引用」小节有一篇文章链接「《C++之RVO返回值优化》」，可以看看。





### 智能指针

C++11 新标准增添了 unique_ptr、shared_ptr 以及 weak_ptr 这 3 个智能指针来实现堆内存的自动回收。

[C++之智能指针 (qq.com)](https://mp.weixin.qq.com/s/mJCQkl_ombv89FxhiQ7haA)

头文件`<memory>`

智能指针不是一个指针，它其实是一个对象。它是通过C++的RAII机制实现的。主要是利用C++中对象在释放的时候，会自动调用析构函数这一特性。



[c++智能指针 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/336293980)

**注意：不要使用裸指针进行初始化**

因为使用裸指针初始化智能指针，容易导致多次使用同一个裸指针对多个智能对象进行初始化。这样就会导致两个智能指针在销毁的时候会去释放同一片内存空间。会造成程序异常崩溃。 如：

```c++
Test* pTest = new Test();
shared_ptr<Test> t(pTest);
//t1释放的时候会导致程序异常
shared_ptr<Test> t1(pTest);
```

**注意：循环引用**

shared_ptr的两个对象中各有一个智能指针类型的成员，而且这两个智能指针指向的都是对方的内存空间。





### auto和decltype

auto、decltype是C++11新增特性，主要是用来做类型推导。这个特性是C++11新增特性，但是这个功能，C++编译器之前就具备，只是未对开发者开放使用。

auto会忽略变量顶层的const、&属性，也就是说，一个变量如果是const int类型的，那么，如果用auto推导之后获取的变量类型则会是int。同样，&（引用）属性也会被忽略，如，int &，用auto推导之后就会变成int类型，但decltype不会这样：

```c++
const int ci = 42, &cj = ci;
decltype(ci) x = 0; //变量x的类型是const int
auto z = ci;      //变量z的类型是int
decltype (cj) y = x; //变量y的类型是const int&
auto w = cj;     //变量w的类型是int
```







## 其他



### 调用其他语言

- Python
  - C++调用Python
  - Python调用C++





### 包管理工具

- vcpkg

  vcpkg是Microsoft的跨平台开源软件包管理器，极大地简化了 Windows、Linux 和 macOS 上第三方库的购置与安装。如果项目要使用第三方库，建议通过 vcpkg 来安装它们。





### 第三方库

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

    关于 Boost.Asio 的多线程模型：

    [浅谈 Boost.Asio 的多线程模型 - Boblim - 博客园 (cnblogs.com)](https://www.cnblogs.com/fnlingnzb-learner/p/10402276.html)

  - zeromq

    七大消息模式：
  
    [重头戏！带你全览ZeroMQ的七大消息模式_董哥的黑板报的博客-CSDN博客](https://blog.csdn.net/qq_41453285/article/details/106865539)
  
    主要API接口：
    
    [消息队列库——ZeroMQ - 如果的事 - 博客园 (cnblogs.com)](https://www.cnblogs.com/chenny7/p/6245236.html)
    
    指南：
    
    [介绍 | ZMQ 指南 (gitbooks.io)](https://wizardforcel.gitbooks.io/zmq-guide/content/)
    
    [云风的 BLOG: ZeroMQ 的模式 (codingnow.com)](https://blog.codingnow.com/2011/02/zeromq_message_patterns.html)
    
    引用：
    
    > 基于定义好的模型，我们可以看到，api 可以实现的非常简单易用。我们不再需要 bind/listen/accept 来架设服务器，因为这个模型天然是 1:N 而不是 1:1 的，**不需要为每个通道保留一个句柄**。我们也不必在意 server 是否先启动（bind），而后才能让 client 工作起来（connect）。
    
    [全网仅此一篇！万字详解ZeroMQ的zmq_msg_t消息处理、多部分消息、及消息接口_董哥的黑板报的博客-CSDN博客](https://blog.csdn.net/qq_41453285/article/details/106794294)
  
- 服务发现
  
  - etcd