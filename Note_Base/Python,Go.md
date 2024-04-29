# 《Python》



## 动态语言与动态类型语言

**动态语言：**

动态语言或动态编程语言，Dynamic programming Language

动态语言是指程序在运行时可以改变其结构，新的函数可以被引进，已有的函数可以被删除等在结构上的变化。

<br/>

**动态类型语言：**

动态类型语言是指在运行期间才去做数据类型检查的语言，说的是数据类型，动态语言说的是运行是改变结构，说的是代码结构。





## 运算符重载

判断两个字典是否相同

一个一个key比较过去？

**可以直接用==进行判断**！！！

```python
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
print(a == b == c == d == e)
```

​    

**Python内部对==进行了重载，帮你实现了对key和value进行判断。**

怎样在两个字典中寻找相同点（比如相同的键、相同的值等）？

解决方案

考虑下面两个字典：

```python
a = {
  'x' : 1,
  'y' : 2,
  'z' : 3
}
b = {
  'w' : 10,
  'x' : 11,
  'y' : 2
}
```

寻找两个字典的相同点，可以在两字典的 keys()或者 items() 方法返回结果上执行集合操作。例如：

```python
# Find keys in common
a.keys() & b.keys() # Return { 'x', 'y' }
# Find keys in a that are not in b
a.keys() - b.keys() # Return { 'z' }
# Find (key,value) pairs in common
a.items() & b.items() # Return { ('y', 2) }
```

​    

**Python中的比较运算符重载：**

| 操作符         | 表达式   | 内部               |
| :------------- | :------- | :----------------- |
| 小于（<）      | p1 <p2   | p1 .__ lt __（p2） |
| 小于等于（<=） | p1 <= p2 | p1 .__ le __（p2） |
| 等于（==）     | p1 == p2 | p1 .__ eq __（p2） |
| 不等于（!=）   | p1！= p2 | p1 .__ ne __（p2） |
| 大于（>）      | p1> p2   | p1 .__ gt __（p2） |
| 大于等于（>=） | p1> = p2 | p1 .__ ge __（p2） |





## Cython 与 CPython

CPython是用C语言实现的Python解释器，也是官方的并且是最广泛使用的Python解释器。

**关于 Cython，我们必须要清楚两件事：**

1）Cython 是一门编程语言，它将 C 和 C++ 的静态类型系统融合在了 Python 身上。Cython 源文件的后缀是 .pyx，它是 Python 的一个超集，语法是 Python 语法和 C 语法的混血。当然我们说它是 Python 的一个超集，因此你写纯 Python 代码也是可以的。

2）当我们编写完 Cython 代码时，需要先将 Cython 代码翻译成高效的 C 代码，然后再将 C 代码编译成 Python 的扩展模块。

[Cython 是什么？为什么会有 Cython？_Python猫的博客-CSDN博客](https://blog.csdn.net/chinesehuazhou2/article/details/125252492)





## GIL

GIL并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。

[玩转python中的GIL前世今生与核心用法剖析](https://blog.51cto.com/u_15346267/3669137)





## 鸭子类型（duck typing）

在鸭子类型中，关注点在于对象的行为，能作什么；而不是关注对象所属的类型。

举个栗子：

```python
# 鸭子类
class Duck:

    def quack(self):
        print("这鸭子正在嘎嘎叫")

    def feathers(self):
        print("这鸭子拥有白色和灰色的羽毛")

# 人类
class Person:

    def quack(self):
        print("这人正在模仿鸭子")

    def feathers(self):
        print("这人在地上拿起1根羽毛然后给其他人看")


# 函数/接口
def in_the_forest(duck):
    duck.quack()
    duck.feathers()


if __name__ == '__main__':

    donald = Duck()  # 创建一个Duck类的实例
    john = Person()  # 创建一个Person类的实例

    in_the_forest(donald)  # 调用函数，传入Duck的实例
    in_the_forest(john)    # 调用函数，传入Person的实例
```

代码运行后输出：

```text
这鸭子正在嘎嘎叫
这鸭子拥有白色和灰色的羽毛
这人正在模仿鸭子
这人在地上拿起1根羽毛然后给其他人看
```

<br>

<br>



# 《Go》

Go语言的特色：

- 没有继承多态的面向对象
- 强一致类型
- interface不需要显式声明(Duck Typing)
- 没有异常处理(Error is value)
- 基于首字母的可访问特性
- 不用的import或者变量引起编译错误
- 完整而卓越的标准库包
- Go内置runtime（作用是性能监控、垃圾回收等）



## 协程

协程（Coroutines）是一种比线程更加轻量级的存在。协程完全由程序所控制（在用户态执行），带来的好处是性能大幅度的提升。

 一个操作系统中可以有多个进程；一个进程可以有多个线程；同理，一个线程可以有多个协程。

协程是一个特殊的函数，这个函数可以在某个地方挂起，并且可以重新在挂起处继续运行。

 **一个线程内的多个协程的运行是串行的，这点和多进程（多线程）在多核CPU上执行时是不同的。** 多进程（多线程）在多核CPU上是可以并行的。**当线程内的某一个协程运行时，其它协程必须挂起。**



### 协程切换

由于协程切换是在线程内完成的，涉及到的资源比较少。不像内核级线程（进程）切换那样，上下文的内容比较多，切换代价较大。协程本身是非常轻巧的，可以简单理解为只是切换了寄存器和协程栈的内容。这样代价就非常小。



## Go并发模型

Go实现了两种并发形式。第一种是大家普遍认知的：多线程共享内存。其实就是Java或者C++等语言中的多线程开发。另外一种是Go语言特有的，也是Go语言推荐的：CSP（communicating sequential processes）并发模型。

CSP讲究的是“以通信的方式来共享内存”。

Go的CSP并发模型，是通过`goroutine`和`channel`来实现的。



### 实现原理

[Go goroutine理解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/60613088)

[为什么要使用 Go 语言？Go 语言的优势在哪里？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/21409296/answer/1040884859)

​    

线程模型的实现，可以分为以下几种方式：

用户级线程模型（M：1）

内核级线程模型（1：1）

两级线程模型（M：N）

M个用户线程对应N个系统线程，缺点增加了调度器的实现难度。

Go语言的线程模型就是一种特殊的两级线程模型（GPM调度模型）。



### Go线程实现模型MPG

`M` 指的是 `Machine`，一个`M`直接关联了一个内核线程。由操作系统管理。

`P` 指的是 `processor`，代表了`M`所需的上下文环境，也是处理用户级代码逻辑的处理器。它负责衔接M和G的调度上下文，将等待执行的G与M对接。

`G` 指的是 `Goroutine`，其实本质上也是一种轻量级的线程。包括了调用栈，重要的调度信息，例如channel等。



### Goroutine优缺点

**优点：**

1、开销小

POSIX的thread API虽然能够提供丰富的API，例如配置自己的CPU亲和性，申请资源等等，线程在得到了很多与进程相同的控制权的同时，开销也非常的大，在Goroutine中则不需这些额外的开销，所以一个Golang的程序中可以支持10w级别的Goroutine。

每个 goroutine (协程) 默认占用内存远比 Java 、C 的线程少（*goroutine：*2KB ，线程：8MB）

2、调度性能好

在Golang的程序中，操作系统级别的线程调度，通常不会做出合适的调度决策。例如在GC时，内存必须要达到一个一致的状态。在Goroutine机制里，Golang可以控制Goroutine的调度，从而在一个合适的时间进行GC。

在应用层模拟的线程，它避免了上下文切换的额外耗费，兼顾了多线程的优点。简化了高并发程序的复杂度。

**缺点：**

协程调度机制无法实现公平调度。