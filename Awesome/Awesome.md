# 《技术总结》

### Web

#### 分布式 session 一致性

1. session复制，对web服务器(例如Tomcat)进行搭建集群
2. session绑定，使用nginx `ip-hash策略`，无论客户端发送多少次请求都被同一个服务器处理
3. 基于redis存储，spring为我们封装好了spring-session，直接引入依赖即可



### 必会框架

#### 工具库

##### Apache commons

- Commons IO
  - `FileAlterationMonitor`和`FileAlterationObserver`（Alyx曾发现这里每隔10秒会涨10M内存，待研究）
- Commons Lang3等



##### Google Guava 

Google Guava 是 Google 公司内部 Java 开发工具库的开源版本。Google 内部的很多 Java 项目都在使用它。它提供了一些 JDK 没有提供的功能，以及对 JDK 已有功能的增强功能。

- 主要包括了：
  1. 集合（Collections）
  2. 缓存（Caching）
  3. **原生类型支持**（Primitives Support）
  4. **并发库**（Concurrency Libraries）
  5. 通用注解（Common Annotation）
  6. 字符串处理（Strings Processing）
  7. 数学计算（Math）
  8. I/O事件
  9. **总线（EventBus）**
- 一些有用的小工具：
  1. `BloomFilter`布隆过滤器的实现

- 源码分析：[https://ifeve.com/google-guava](https://ifeve.com/google-guava/)



##### Json

- [关于Gson的几个坑](https://ariescat.top/2020/03/12/%E5%85%B3%E4%BA%8EGson%E7%9A%84%E5%87%A0%E4%B8%AA%E5%9D%91/)



##### 懒人工具

- [Java代码生成利器之rapid-generate应用](http://blog.csdn.net/likeaboy_fire/article/details/44024987)



#### 缓存

- Guava的缓存

  Guava Cache说简单点就是一个支持**LRU**的ConcurrentHashMap

  1. [简析guava cache线程安全设计哲学 - 简书 (jianshu.com)](https://www.jianshu.com/p/699869cb5421)

- **Caffeine** 来自未来的缓存

  Caffeine是基于JAVA 1.8 Version的高性能缓存库。Caffeine提供的内存缓存使用参考Google guava的API。Caffeine是基于Google Guava Cache设计经验上改进的成果。



#### 时间库

- **joda** 对时间的操作
- Quartz 定时任务



#### 日志

- 区分`commons-logging`，`slf4j`，`log4j`，`logback`
  1. 了解`jcl-over-slf4j`，`jul-to-slf4j`这些jar的作用
  2. 了解`log4j`和`log4j2`的区别，**`lmax disruptor`**应用场景
- **`Flume`** 日志采集系统，一般用于日志聚合



#### ASM神器

spring-core自带有asm，org.ow2.asm也是一个轻量级的jar

还有byte buddy库，javassist库



#### Spring

> 最好能抽空看看源码，最起码bean的生命周期，如何解决循环依赖，父子容器，还有boot的启动流程，事务实现原理，动态代理原理等，你知道越多越好。

- [Spring源码浅析](https://blog.csdn.net/linuu/column/info/lovespring)

- Spring AOP

  - AOP原理，ProxyFactory

  - AOP中Pointcut，Advice 和 Advisor 三个概念 还有Advised

    > Advised->在Spring中创建了AOP代理之后，就能够使用org.springframework.aop.framework.Advised接口对它们进行管理。 任何AOP代理都能够被转型为这个接口，不论它实现了哪些其它接口
    >
    > Advisor->类似使用Aspect的@Aspect注解的类
    >
    > Advice->@Before、@After、@AfterReturning、@AfterThrowing、@Around
    >
    > Pointcut->@Pointcut

- Spring tx

  `<tx:annotation-driven/>`的理解

- [Spring Cache 介绍](https://www.cnblogs.com/rollenholt/p/4202631.html)

- [Spring Webflux](https://www.jianshu.com/p/c029de45d23b) （reactive web框架，与前端Flux架构名字相同）

  命令式编程 VS **响应式编程**

- Spring Data

  - [Spring Data JPA 简单查询--接口方法 - 如莲家园 - 博客园](https://www.cnblogs.com/rulian/p/6557471.html)

- 与其他构架的整合

  - [企业大型互联网分布式架构{Java分布式架构 dubbo+springmvc+mybatis+ehcach+redis }-IT未来-ITPUB博客](http://m.blog.itpub.net/31452580/viewspace-2148363/)
  - [手把手教你从最基本的Java工程搭建SpringMVC+SpringDataJPA+Hibernate(含源码下载) - anxpp的博客 - CSDN博客](http://blog.csdn.net/anxpp/article/details/51415366)



#### SpringBoot



#### SpringCloud



#### Web

- JAX-RS

  全称：Java API for RESTful Web Services，是一套用java实现REST服务的规范，提供了一些标注将一个资源类，一个POJOJava类，封装为Web资源。

  包括：

  - @Path，标注资源类或方法的相对路径
  - @GET，@PUT，@POST，@DELETE，标注方法是用的HTTP请求的类型
  - @Produces，标注返回的MIME媒体类型
  - @Consumes，标注可接受请求的[MIME](http://liugang594.iteye.com/wiki/MIME)媒体类型
  - @PathParam，@QueryParam，@HeaderParam，@CookieParam，@MatrixParam，@FormParam，分别标注方法的参数来自于HTTP请求的不同位置，例如@PathParam来自于URL的路径，@QueryParam来自于URL的查询参数，@HeaderParam来自于HTTP请求的头信息，@CookieParam来自于HTTP请求的Cookie

  `Eureka`的`ApplicationResource`有用到



#### ORM库

- hibernate

  查询：HQL查询，QBC查询，SQL查询

  级联查询：一对一，一对多（多对一），多对多；懒加载，1+n问题

  其他：

  1. session.get(): 非懒加载方法

     session.load(): 默认就是是懒加载

  2. 抓取策略（fetch）和 懒加载（lazy）



#### Netty

- [概述](https://www.jianshu.com/p/1a6d1a25e6cc)

- Netty的线程模型

  通过**Reactor模型**基于**多路复用器**接收并处理用户请求，内部实现了两个线程池，boss线程池和work线程池，其中boss线程池的线程负责处理请求的accept事件，当接收到accept事件的请求时，把对应的socket封装到一个NioSocketChannel中，并交给work线程池，其中work线程池负责请求的read和write事件

- NioEventLoop设计原理

- 定时任务的原理

- **netty对象池使用与回收**

- 时间轮算法

  [HashedWheelTimer](http://novoland.github.io/%E5%B9%B6%E5%8F%91/2014/07/26/%E5%AE%9A%E6%97%B6%E5%99%A8%EF%BC%88Timer%EF%BC%89%E7%9A%84%E5%AE%9E%E7%8E%B0.html)

  > hashWheel定时器和Quartz的区别：  
  > 1）Quartz将定时任务分为任务和触发器，而hashWheel只有任务的概念
  >
  > 2）Quartz通过一个TreeSet对所有的触发器进行管理，而hashWheel通过一个hash轮来对所有的任务进行管理
  >
  > 3）Quartz能够非常方便的删除定时任务，而netty的hashWheel暂时没有删除任务的接口（除非自己实现一个hashWheel定时器）
  >
  > 4）Quartz有一个专门的调度线程对任务进行管理，任务执行有另外专门的线程池，而hashWheel用一个线程实现对任务的管理和任务的执行。
  >
  > 5）Quartz能够通过序列化，将定时任务保存在数据库，而hashWheel不能
  >
  > 总的来说，Quartz的功能相对强大，而hashWheel相对要轻量级一点。

- 附：

  个人认为netty对用户来说是异步，但是实际底层IO是IO多路复用模型，本质上还是一种同步非阻塞（是的，个人认为IO多路复用模型还是**同步**非阻塞，并且[**真正的IO操作**都将**阻塞**应用线程](https://weread.qq.com/web/reader/1e732510718f63a11e7dee2k98f3284021498f137082c2e)），他只是多了一个Selector（需要底层操作系统支持），如此一个线程就可以控制大量的通信（相比传统IO，不管他是不是非阻塞）。

  另看 [IO#IO概念](#io)，这里也收录了一些理解



#### Disruptor

- 背景

  1. [锁的缺点 - Disruptor 入门](http://wiki.jikexueyuan.com/project/disruptor-getting-started/lock-weak.html)

  2. 并发中的伪共享问题

  3. 代码的并发执行大约是两件事：互斥和变化的可见性。

     互斥是关于管理某些资源的竞争更新。

     变化的可见性是关于控制何时使这些更改对其他线程可见。

- 设计上的优势

  1. 内部数据存储使用环形缓冲（Ring Buffer），这样分配支持了**CPU缓存位置预测**，**GC的压力更小**
  2. **尽量使用无锁设计，合理使用CAS**
  3. 优化数据结构（填充缓存行），**解决伪共享问题**
  4. 合理位运算（如2次方幂求模），**合理使用Unsafe**

- 策略

  `WaitStrategy`可以选择`YieldingWaitStrategy`（无锁）

- 参考博客

  1. [解读Disruptor系列](https://www.jianshu.com/u/4c940e688e05)，这个系列挺好的，他每篇文章后面都有份参考资料，也可以认真看看

- 扩展

  1. AtomicXXX.lazySet 这个方法的作用（Sequence#set相当于AtomicLong#lazySet）
  2. Unsafe类的作用？为什么要用这个类？除了JDK，在Netty、Spring、Kafka、Storm等非常多的流行开源项目中都使用了Unsafe



#### 中间件

##### ActiveMQ

##### Elasticsearch

- [Elasticsearch基础教程 - CSDN博客](http://blog.csdn.net/cnweike/article/details/33736429)
- [玩转单元测试之DBUnit - WadeXu - 博客园](https://www.cnblogs.com/wade-xu/p/4547381.html)
- [基于注解的配置 - Spring-Data-Elasticsearch](https://es.yemengying.com/5/5.1/5.1.2.html)



#### 原子类型集合库

避免开销很大的装箱/拆箱操作，节省了原始类型装箱消耗的内存

- **Koloboke**

  [生成高性能的 JAVA 基本类型 map/set](https://blog.csdn.net/qinyongye/article/details/81282961)

- Eclipse Collections



#### Akka

- **Actor模型**
- [akka设计模式系列-基础模式](https://yq.aliyun.com/articles/616951?spm=a2c4e.11153940.blogcont616952.14.28751adcybgYqt)



#### RxJava 

[➮详细](/2019/01/29/事件驱动编程RxJava/)

" a library for composing asynchronous and event-based programs using observable sequences for the Java VM "  （一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库）



### 新型编程思想

#### Reative（响应式）编程

Reactive响应式(反应式)编程 是一种新的编程风格，其特点是异步或并发、事件驱动、推送PUSH机制以及观察者模式的衍生。

JVM应用：RxJava、Akka、Actors模型、Vert.x、Webflux

#### 领域驱动设计

> 他是综合软件系统分析和设计的面向对象**建模方法**，如今已经发展为一种针对大型复杂系统的领域建模与分析方法。
>
> 将要解决的**业务概念和业务规则**转换为**软件系统中的类型及类型的属性与行为**，**通过合理运用面向对象的封装、继承、多态等设计要素**，降低或隐藏整个系统的业务复杂性，并使得系统具有更好的扩展性，应对纷繁多变的现实业务问题。
>
> ——抄录于《高可用可伸缩微服务架构：基于Dubbo、Spring Cloud和Service Mesh》2.1节

- [领域驱动设计在互联网业务开发中的实践](https://tech.meituan.com/2017/12/22/ddd-in-practice.html)
- [美团DDD实践 示例项目](https://github.com/1987539447/draw-lottery)



### 分布式基石

- 理论基石CAP原理

  **C** - **C**onsistent ，一致性

  **A** - **A**vailability ，可用性

  **P** - **P**artition tolerance ，分区容忍性

  一句话概括 CAP 原理就是——**网络分区发生时，一致性和可用性两难全**

- 一致性Hash

- RPC

  RPC涉及：通讯，序列化，超时，重发（重复），消息顺序，负载 等等。（个人理解）

  - 协议：thrift 等等
  - JavaRMI
    - [深究Java中的RMI底层原理](https://blog.csdn.net/sinat_34596644/article/details/52599688)
  - HSF
    阿里巴巴集团内部使用的分布式服务框架 High Speed Framework

- 分布式锁

  分布式锁一般有三种实现方式：1. 数据库乐观锁；2. 基于Redis的分布式锁；3. 基于ZooKeeper的分布式锁

- 分布式事务

  - [分布式事务与一致性算法Paxos & raft & zab](https://blog.csdn.net/followmyinclinations/article/details/52870418)
  - atomikos:[4.0 atomikos JTA/XA全局事务](http://www.tianshouzhi.com/api/tutorials/distributed_transaction/386)
  - xaresource
  - [分布式事务](https://javatar.iteye.com/blog/981787)
  - [分布式事务系列（2.1）分布式事务的概念](https://yq.aliyun.com/articles/39047)



#### Zookeeper

- [Zookeeper的功能以及工作原理](https://www.cnblogs.com/felixzh/p/5869212.html)
- [Leader选举-选举过程介绍比较清晰](https://blog.csdn.net/gaoshan12345678910/article/details/67638657)
- [ZAB协议理解](https://blog.csdn.net/junchenbb0430/article/details/77583955)



### 高可用技术

- 服务器端如何处理超大量合法请求？

  服务器架构层面，做负载均衡，将请求分发给其它服务器处理。

  软件服务架构层面，做请求队列，将1w个请求放入队列，业务处理完的请求再返回。

  代码层面，优化业务处理，把单机请求做到支持1w并发。



### 前沿技术

#### Docker

#### 微服务化

##### ServiceMesh(服务网格)

##### 中台

##### 大数据

人工智能、区块链等



### 编程工具

- 构建工具
  - Maven
  - Gradle
    - [十分钟理解Gradle - Bonker - 博客园](https://www.cnblogs.com/Bonker/p/5619458.html)
    - 慕课实战：Gradle3.0自动化项目构建技术精讲+实战
- 版本管理工具
  - Git
- Jenkins



### C/C++

> 在学习skynet源码的时候，需要看C和lua，因此这里记一下C相关的用法

- [动态申请二维数组](https://blog.csdn.net/qq_41822235/article/details/81142107)

  - 利用一个**二级指针**来实现

    ```c
    //5行2列的数组
    int **p = (int **)malloc(sizeof(int *) * 5);
    for (int i = 0; i < 5; ++i) {
        p[i] = (int *)malloc(sizeof(int) * 2);
    }
    //输出数组每个元素地址
    printf("%p\n", &p[i][j]);
    ```

  - 利用**数组指针**来实现

    ```c
    //申请一个5行2列的整型数组
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

- 内存对齐 是什么？



### 脚本语言

- 动态语言与动态类型语言

  动态语言：(Dynamic programming Language -动态语言或动态编程语言)，动态语言是指程序在运行时可以改变其结构，新的函数可以被引进，已有的函数可以被删除等在结构上的变化。

  动态类型语言：动态类型语言是指在运行期间才去做数据类型检查的语言，说的是数据类型，动态语言说的是运行是改变结构，说的是代码结构。

#### Groovy

- [30分钟groovy快速入门并掌握](https://www.cnblogs.com/amosli/p/3970810.html)
- [Groovy 语言快速入门](https://www.jianshu.com/p/e8dec95c4326)
- Groovy 与 **Java**
  - [实战 Groovy，在 Java 应用程序中加一些 Groovy 进来](https://www.ibm.com/developerworks/cn/java/j-pg05245/)
  - [利用SPRING管理热加载的GROOVY对象](https://palexu.github.io/posts/spring-dynamic-load-groovy-bean?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
  - [spring + groovy 很强大](https://blog.csdn.net/qq362228416/article/details/8811136)
  - [Spring动态部署Bean/Controller/Groovy Controller](https://jinnianshilongnian.iteye.com/blog/1999284)
- Groovy as DSL 与 **Gradle**
  - [Gradle：新一代自动化构建工具](http://hao.jobbole.com/gradle/)
  - [Groovy DSL 百度搜索](https://www.baidu.com/s?ie=utf8&oe=utf8&wd=Groovy DSL&tn=98010089_dg&ch=4)
  - [gradle dsl](https://www.baidu.com/s?ie=utf8&oe=utf8&wd=gradle dsl&tn=98010089_dg&ch=5)





### 字符编解码

- 字符集

  1. ASCII

  2. Unicode

     目前Unicode字符分为17组编排，0x0000至0x10FFFF,每组称为平面（Plane）,每个面拥有65536个码位，共1114112个。

- 字符编码

  UTF-32、UTF-16和 UTF-8 是 Unicode 标准的编码字符集的字符编码方案

  - 附：

    1. Java的`char`内部编码为`UTF-16`，而与`Charset.defaultCharset()`无关。

       看 [Unicode 编码理解](https://blog.csdn.net/wdeng2011/article/details/80155795) 可知`UTF-16`编码完全可以满足Unicode 的17组编排（平面），因为有平面0的0xD800-0xDFFF代理区。

       [关于java中char占几个字节，汉字占几个字节](https://www.cnblogs.com/nevermorewang/p/7808092.html)，这里指出Java中的`char`是占用两个字节，只不过有些字符需要两个char来表示，同时这篇博客也给了一个官方Oracle链接里面明确的说明了*值在16位范围之外且在0x10000到0x10FFFF范围内的字符称为补充字符，并定义为**一对char值***。

       测试代码：

       ```java
       public static void main(String[] args) {
       
           char[] c = new char[]{'一'};
           System.err.println(Integer.toHexString(c[0]));
           String s = new String(c);
           // String#length事实上调用了char[].length
           System.err.println(s + " " + s.length());
       
           String str = "一";
           System.err.println(str + " " + str.length());
       
           // Unicode编码 汉字扩展B '𠀀' 字
           c = new char[]{'\uD840', '\uDC00'};
           s = new String(c);
           System.err.println(s + " " + s.length());
       
           str = "\uD840\uDC00";
           System.err.println(str + " " + str.length());
       
           // 输出：由输出可见这个字用了两个char来存
           // 一 1
           // 一 1
           // 𠀀 2
           // 𠀀 2
       }
       ```

    2. [UniCode编码表](https://www.cnblogs.com/csguo/p/7401874.html)

    3. [汉字unicode编码范围](https://blog.csdn.net/gywtzh0889/article/details/71083459/)

  - 参考博客：

    1. 吴秦（Tyler）[字符集和字符编码（Charset & Encoding）](https://www.cnblogs.com/skynet/archive/2011/05/03/2035105.html)

    2. 廖雪峰 [字符串和编码](https://www.liaoxuefeng.com/wiki/1016959663602400/1017075323632896)

       该文有简单有效的解释了：

       在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。
       用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：

       ![字符编码·图1](/img/awesome/Unicode1.png)

       浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器：

       ![字符编码·图2](/img/awesome/Unicode2.png)

       所以你看到很多网页的源码上会有类似`<meta charset="UTF-8" />`的信息，表示该网页正是用的UTF-8编码。

- Base64编码：

  Base64编码本质上是一种将二进制数据转成文本数据的方案。对于非二进制数据，是先将其转换成二进制形式，然后每连续6比特（2的6次方=64）计算其十进制值，根据该值在上面的索引表中找到对应的字符，最终得到一个文本字符串。

- 常见问题处理之Emoji

  所谓Emoji就是一种在Unicode位于\u1F601–\u1F64F区段的字符。这个显然超过了目前常用的UTF-8字符集的编码范围\u0000–\uFFFF。Emoji表情随着IOS的普及和微信的支持越来越常见。

  ![字符编码·图3](https://img-blog.csdnimg.cn/20181119221259676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3podXNvbmd6aXll,size_16,color_FFFFFF,t_70)

  那么Emoji字符表情会对我们平时的开发运维带来什么影响呢？最常见的问题就在于将他存入MySQL数据库的时候。一般来说MySQL数据库的默认字符集都会配置成UTF-8，mysql支持的 utf8 编码最大字符长度为 **3 字节**，而utf8mb4在5.5以后才被支持，也很少会有DBA主动将系统默认字符集改成utf8mb4。那么问题就来了，当我们把一个需要4字节UTF-8编码才能表示的字符存入数据库的时候就会报错：ERROR 1366: Incorrect string value: '\xF0\x9D\x8C\x86' for column 。 如果认真阅读了上面的解释，那么这个报错也就不难看懂了。我们试图将一串Bytes插入到一列中，而这串Bytes的第一个字节是\xF0意味着这是一个四字节的UTF-8编码。但是当MySQL表和列字符集配置为UTF-8的时候是无法存储这样的字符的，所以报了错。

  那么遇到这种情况我们如何解决呢？有两种方式：升级MySQL到5.6或更高版本，并且将表字符集切换至utf8mb4。第二种方法就是在把内容存入到数据库之前做一次过滤，将Emoji字符替换成一段特殊的文字编码，然后再存入数据库中。之后从数据库获取或者前端展示时再将这段特殊文字编码转换成Emoji显示。第二种方法我们假设用-*-1F601-*-来替代4字节的Emoji，那么具体实现python代码可以参见[Stackoverflow上的回答](http://stackoverflow.com/questions/3220031/how-to-filter-or-replace-unicode-characters-that-would-take-more-than-3-bytes)

- 补码

  补码(为什么按位取反再加一)：告诉你一个其实很简单的问题 [原文](https://blog.csdn.net/wenxinwukui234/article/details/42119265)

  其核心思想就是：**一个正数对应的负数（也就是俩相反数），这两个数的二进制编码加起来必须等于0才对**







### Github干货

- [521xueweihan / HelloGitHub](https://github.com/521xueweihan/HelloGitHub)  分享 GitHub 上有趣、入门级的开源项目 

- awesome

  - [hadyang / interview](https://github.com/hadyang/interview)

    Java 笔试、面试 知识整理

  - [Snailclimb / JavaGuide](https://github.com/Snailclimb/JavaGuide)

    【Java学习+面试指南】 一份涵盖大部分Java程序员所需要掌握的核心知识。 

  - [jobbole / awesome-java-cn](https://github.com/jobbole/awesome-java-cn)

    Java资源大全中文版，包括开发库、开发工具、网站、博客、微信、微博等，由伯乐在线持续更新。

    同时他还有 [jobbole / awesome-python-cn](https://github.com/jobbole/awesome-python-cn)，[jobbole / awesome-cpp-cn](https://github.com/jobbole/awesome-cpp-cn)

  - [AobingJava / JavaFamily](https://github.com/AobingJava/JavaFamily)

    【互联网一线大厂面试+学习指南】进阶知识完全扫盲：涵盖高并发、分布式、高可用、微服务等领域知识，作者风格幽默，看起来津津有味，把学习当做一种乐趣，何乐而不为，后端同学必看 

    附其CSDN博客（《吊打面试官》系列）：[https://me.csdn.net/qq_35190492](https://me.csdn.net/qq_35190492)

  - [xingshaocheng / architect-awesome](https://github.com/xingshaocheng/architect-awesome)  

    后端架构师技术图谱

  - [crossoverJie / JCSprout](https://github.com/crossoverJie/JCSprout) 

    Java Core Sprout : basic, concurrent, algorithm 

  - [javagrowing / JGrowing](https://github.com/javagrowing/JGrowing)

    Java成长路线，但学到不仅仅是Java。 

- Java高并发

  - [seckill](https://github.com/liyifeng1994/seckill)

- Android

  - [henrymorgen / android-advanced-decode](https://github.com/henrymorgen/android-advanced-decode)  《Android进阶解密》源码 

- 游戏相关

  - [hstcscolor / awesome-gameserver-cn](https://github.com/hstcscolor/awesome-gameserver-cn)  中文游戏服务器资源大全 

  - ARPG

    - 永恒之塔开源服务器架构 [https://github.com/Aion-server/Aion-unique](https://github.com/Aion-server/Aion-unique)

    - 天堂2 l2jserver2 

      [https://github.com/oonym/l2InterludeServer](https://github.com/oonym/l2InterludeServer)

      [https://github.com/Rogiel/l2jserver2](https://github.com/Rogiel/l2jserver2)

    - 魔兽世界server TrinityCore [https://github.com/TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore)

  - tinyHeart [https://github.com/luckykun/tinyHeart](https://github.com/luckykun/tinyHeart)

- 常见框架源码

  - tomcat
  - dubbo
  - spring
  - zookeeper

- 源码解读

  - [huangz1990 / redis-3.0-annotated](https://github.com/huangz1990/redis-3.0-annotated)  带有详细注释的 Redis 3.0 代码 

### 知识体系

- [Github优秀java项目集合（中文版） - 涉及java所有的知识体系](https://blog.csdn.net/aa1215018028/article/details/80951389)
- [伯乐在线](http://hao.jobbole.com/)
- [《成神之路系列文章》](http://www.hollischuang.com/archives/1001)
- [超强总结！Github上那些Java面试、学习相关仓库](https://mp.weixin.qq.com/s?__biz=Mzg3MjA4MTExMw==&mid=2247486161&idx=1&sn=b7bd2bffc50f5f64368d6e40c973619a&chksm=cef5f967f9827071badcdcf1ae8c162ad2176e44978ce3d8d138a94ae843e9232c1fb6d4ec36&scene=21#wechat_redirect)

### 面经汇总

- Java面经汇总
  - ImportNew
    1. [Java线程面试题 Top 50 - ImportNew](http://www.importnew.com/12773.html)
  - 掘金
    1. [Java面试通关要点汇总集 - 掘金](https://juejin.im/post/5a94a8ca6fb9a0635c049e67?utm_source=gold_browser_extension#heading-1)
    2. [Java面试通关要点汇总集(基础篇之基本功，非原作者) - 掘金](https://juejin.im/post/5a9690fc5188257a865da3ee?utm_source=gold_browser_extension)
    3. [JavaEE面试题收集 - 掘金](https://juejin.im/post/58a6ad5461ff4b78fca442eb)
    4. [一个两年Java的面试总结 - 掘金](https://juejin.im/post/5a9f5ce86fb9a028de443ed9?utm_source=gold_browser_extension)
  - 2020年收录：
    - [强烈推荐！15 个 Github 顶级 Java 教程类开源项目推荐！](https://blog.csdn.net/qq_34337272/article/details/104423823)
    - [Java基础知识面试题（2020最新版）](https://blog.csdn.net/ThinkWon/article/details/104390612)
    - [【阿里P6面经】二本，curd两年，疯狂复习，拿下阿里offer](https://blog.csdn.net/qq_35190492/article/details/105186878)
- 大厂面经
  - [面试心得与总结—BAT、网易、蘑菇街 - ImportNew](http://www.importnew.com/22637.html)
- 经历分享
  - [2017年秋季校招面经 - CSDN博客](http://blog.csdn.net/huachao1001/article/details/52247268)
  - [我的求职经历——遍览国内一流IT企业(转） - lonelycatcher - 博客园](http://www.cnblogs.com/lonelycatcher/archive/2012/03/10/2388889.html)

### 博客

- 职场
  - [如何入职心仪的游戏公司？ 游戏策划从入门到入行](https://www.gameres.com/840718.html)
- 年度报告
  - [阿里研究院：2016年校园快递行业发展报告](http://www.199it.com/archives/530127.html)
  - [SegmentFault 年度内容盘点 - 2016](https://summary.segmentfault.com/2016/#/)
- 其他
  - [究竟怎样写代码才算是好代码 - CSDN博客](http://blog.csdn.net/u013970991/article/details/52609083)
  - [成为Java顶尖程序员 ，看这11本书就够了 - CSDN博客](https://blog.csdn.net/u012410733/article/details/51869105)

### 常用社区

- [慕课网手记](http://www.imooc.com/article)

### 必备软件

- everything
- wox（window快速搜索文件启动程序软件）
- 系统镜像
  - [https://msdn.itellyou.cn/](https://msdn.itellyou.cn/)
- HTTP接口测试工具
  - Postman
- PanDownload



### Linux常用服务搭建

（Shadowsocks，Ngrok，Nginx...）

- CentOS7
  - [Linux如何查看端口状态_百度经验](https://jingyan.baidu.com/article/59703552c2fd838fc1074046.html)
  - [Linux Yum 命令使用举例_Linux教程_Linux公社-Linux系统门户网站](http://www.linuxidc.com/Linux/2011-09/42108.htm)
  - [CentOS7使用firewalld打开关闭防火墙与端口 - 莫小安 - 博客园](https://www.cnblogs.com/moxiaoan/p/5683743.html)
- MySql
  - [CentOS下的Mysql的安装和使用 - suxiaoman - 博客园](https://www.cnblogs.com/suxiaoman/p/7693066.html)
- Jetty
  - [Centos6.8 Jetty 安装配置 - 那个汪 - 博客园](https://www.cnblogs.com/wzalex/p/6912500.html)
- Shadowsocks
  - [记一次搭建SS服务器，完整的过程。，搭建ss_Linux教程 · 帮客之家](http://www.bkjia.com/Linuxjc/1202867.html)
  - [Centos 7下搭建SS - CSDN博客](http://blog.csdn.net/u013309540/article/details/74330305)
  - [Shadowsocks - Clients](https://shadowsocks.org/en/download/clients.html)
  - [锐速ServerSpeeder无限带宽破解版一键安装包(2017.6.23更新 )-蜗牛789](https://www.wn789.com/4678.html)
- Nginx
  - [Nginx的一些基本功能 - CSDN博客](http://blog.csdn.net/zhongguozhichuang/article/details/52816887)
- Ngrok
  - [CentOS7.3编译安装go1.8.1 - Aliang Log](https://www.aliang.org/golang/go1-8-1.html)
  - [CentOS下部署Ngrok服务器 - YE_NICKNAME - CSDN博客](http://blog.csdn.net/y534560449/article/details/53513046)
  - [Centos下自己架设ngrok服务器（内网测试神器） - 个人文章 - SegmentFault](https://segmentfault.com/a/1190000010338848)



### 前端

- HTML/CSS/JS
- ECMAScript
- Bootstrap 教程 - 菜鸟教程
- [Vue](https://cn.vuejs.org/)
  - 双向数据绑定与单向数据绑定
  - [Vuex](https://vuex.vuejs.org/zh/)，[Weex](http://weex.apache.org/cn/)
- React
  - [React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)
- Flux 架构
  - [Flux 架构入门教程](http://www.ruanyifeng.com/blog/2016/01/flux.html)
- 状态管理
  - [聊一聊主流前端框架的状态管理](https://www.cnblogs.com/axel10/archive/2018/03/15/8571757.html)
  - [前端状态管理请三思](https://juejin.im/post/59fd94475188254115703461)
- 其他
  - [给2019前端的5个建议](https://juejin.im/post/5c617c576fb9a049e93d33a4)
  - [浏览器原理系列10篇正式完结](https://juejin.im/post/5c6d3e026fb9a04a0d576f98)



### Android

- [Gradle Distributions](https://services.gradle.org/distributions)
- 图表
  - hellocharts
- 学习网站
  - [开发者指南 · Android 开发者 · Android Developers](https://developer.android.com/guide)
    - [使用 NavigationUI 更新界面组件](https://developer.android.com/guide/navigation/navigation-ui#add_a_navigation_drawer)
      - 抽屉式导航栏
  - [AndroidDevTools - Android开发工具 Android SDK下载 Android Studio下载 Gradle下载 SDK Tools下载](https://www.androiddevtools.cn/)
  - [android-open-source-project-analysis](https://github.com/sucese/android-open-source-project-analysis)
- Material Design
  - NavigationView FlaotingActionBar SnackBar
  - Design Support Library
  - RecyclerView
  - SwipeRefreshLayout
  - 控件点击水波纹



### Unity3d

- 愤怒的小鸟 [https://www.bilibili.com/video/av35565116/](https://www.bilibili.com/video/av35565116/)

### 游戏技术

- **AI**（状态机 行为树）
- 游戏框架
  - skynet
  - Pinus

### 游戏相关

- [游戏学院 - 腾讯大学](https://daxue.qq.com/game)
- 安全
  - [游戏安全实验室](https://gslab.qq.com/portal.php?mod=view&aid=94)
- [GameRes游资网-游戏开发者门户](https://www.gameres.com/)



### 相关书籍

- Java
  - 《深入理解Java虚拟机（第3版）（周志明）》
  - 《Java并发编程实战》
  - 《Effective Java》
- Redis
  - 《Redis 深度历险：核心原理与应用实践 （钱文品）》
  - 《Redis设计与实现》
- Spring
  - 《Spring 源码深度解析 第二版》《Spring实战》
  - 《Spring Boot编程思想（核心篇）》![书籍·图1](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3621582485,3050859261&fm=58&bpow=800&bpoh=940)
  - 《Spring Boot实战》
  - 《Spring 微服务实战》
- Netty
  - 《Netty权威指南》
- Tomcat
  - 《Tomcat架构解析 （刘光瑞）》
- 《漫画算法：小灰的算法之旅》
- 《架构探险分布式服务框架 （李业兵）》
- 《高性能MySQL》