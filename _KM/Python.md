# 《Python》



## 动态语言与动态类型语言

**动态语言：**

动态语言或动态编程语言，Dynamic programming Language

动态语言是指程序在运行时可以改变其结构，新的函数可以被引进，已有的函数可以被删除等在结构上的变化。

<br/>

**动态类型语言：**

动态类型语言是指在运行期间才去做数据类型检查的语言，说的是数据类型，动态语言说的是运行是改变结构，说的是代码结构。





## Cython 与 CPython

CPython是用C语言实现的Python解释器，也是官方的并且是最广泛使用的Python解释器。

**关于 Cython，我们必须要清楚两件事：**

1）Cython 是一门编程语言，它将 C 和 C++ 的静态类型系统融合在了 Python 身上。Cython 源文件的后缀是 .pyx，它是 Python 的一个超集，语法是 Python 语法和 C 语法的混血。当然我们说它是 Python 的一个超集，因此你写纯 Python 代码也是可以的。

2）当我们编写完 Cython 代码时，需要先将 Cython 代码翻译成高效的 C 代码，然后再将 C 代码编译成 Python 的扩展模块。

[Cython 是什么？为什么会有 Cython？_Python猫的博客-CSDN博客](https://blog.csdn.net/chinesehuazhou2/article/details/125252492)





## GIL

GIL并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。

[玩转python中的GIL前世今生与核心用法剖析](https://blog.51cto.com/u_15346267/3669137)