# 《Python》



## Cython 与 CPython

CPython是用C语言实现的Python解释器，也是官方的并且是最广泛使用的Python解释器。

**关于 Cython，我们必须要清楚两件事：**

1）Cython 是一门编程语言，它将 C 和 C++ 的静态类型系统融合在了 Python 身上。Cython 源文件的后缀是 .pyx，它是 Python 的一个超集，语法是 Python 语法和 C 语法的混血。当然我们说它是 Python 的一个超集，因此你写纯 Python 代码也是可以的。

2）当我们编写完 Cython 代码时，需要先将 Cython 代码翻译成高效的 C 代码，然后再将 C 代码编译成 Python 的扩展模块。

[Cython 是什么？为什么会有 Cython？_Python猫的博客-CSDN博客](https://blog.csdn.net/chinesehuazhou2/article/details/125252492)



## GIL

GIL并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。

[玩转python中的GIL前世今生与核心用法剖析](https://blog.51cto.com/u_15346267/3669137)