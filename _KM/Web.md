# 《Web》



## 框架



### 基础工具库

#### Apache commons

- Commons IO
  - `FileAlterationMonitor`和`FileAlterationObserver`（Alyx 曾发现这里每隔 10 秒会涨 10M 内存，待研究）
- Commons Lang3 等



#### Google Guava

Google Guava 是 Google 公司内部 Java 开发工具库的开源版本。Google 内部的很多 Java 项目都在使用它。它提供了一些 JDK 没有提供的功能，以及对 JDK 已有功能的增强功能。

- 主要包括了：
  1. 集合（Collections）
  2. 缓存（Caching）
  3. **原生类型支持**（Primitives Support）
  4. **并发库**（Concurrency Libraries）
  5. 通用注解（Common Annotation）
  6. 字符串处理（Strings Processing）
  7. 数学计算（Math）
  8. I/O 事件
  9. **总线（EventBus）**
- 一些有用的小工具：
  1. `BloomFilter`布隆过滤器的实现
  1. Ordering排序器
  
- 源码分析：[https://ifeve.com/google-guava](https://ifeve.com/google-guava/)



#### Json

- [关于 Gson 的几个坑](https://ariescat.top/2020/03/12/%E5%85%B3%E4%BA%8EGson%E7%9A%84%E5%87%A0%E4%B8%AA%E5%9D%91/)



### Spring

#### Spring

> 最好能抽空看看源码，最起码 bean 的生命周期，如何解决循环依赖，父子容器，还有 boot 的启动流程，事务实现原理，动态代理原理等，你知道越多越好。

- [Spring 源码浅析](https://blog.csdn.net/linuu/column/info/lovespring)

  - IOC 依赖注入，控制反转

    [Spring IOC 容器源码分析_Javadoop](https://javadoop.com/post/spring-ioc)

- 循环依赖及三级缓存

  [Spring 循环依赖及三级缓存](https://blog.csdn.net/u012098021/article/details/107352463/)

  弄清楚：

  1. 三级缓存是分别是什么，分别是什么时候起作用？

  2. 为何需要三级缓存，二级缓存不行吗？

  3. Spring 对 groovy 的生成的 bean 为何解决不了循环依赖？

     这里主要是：

     1. bean 生成的时机是 postProcessBeforeInstantiation，没有走到 doCreateBean，而 addSingletonFactory 是在 doCreateBean 调用的

        ```java
        protected void addSingletonFactory(String beanName, ObjectFactory singletonFactory) {
           Assert.notNull(singletonFactory, "Singleton factory must not be null");
           synchronized (this.singletonObjects) {
              if (!this.singletonObjects.containsKey(beanName)) {
                 this.singletonFactories.put(beanName, singletonFactory);
                 this.earlySingletonObjects.remove(beanName);
                 this.registeredSingletons.add(beanName);
              }
           }
        }
        ```

     2. bean 是由 ScriptFactoryPostProcessor#scriptBeanFactory 生成的，这个 scriptBeanFactory 是一个全新的，然后 copyConfigurationFrom 了一次 parent 的属性

- @Configuration

  @Configuration注解的类为什么被CGLIB增强？

  [@Configuration这注解为什么可以不加？加了和不加的区别，底层为什么使用cglib - 简书 (jianshu.com)](https://www.jianshu.com/p/0822470c1b85?utm_campaign=shakespeare&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

- Spring AOP

  - AOP 原理，ProxyFactory

  - AOP 中 Pointcut，Advice 和 Advisor 三个概念 还有 Advised

    * Advised
    
      在 Spring 中创建了 AOP 代理之后，就能够使用 org.springframework.aop.framework.Advised 接口对它们进行管理。 任何 AOP 代理都能够被转型为这个接口，不论它实现了哪些其它接口
    
    * Advisor
    
      类似使用 Aspect 的@Aspect 注解的类
    
    * Advice
    
      @Before、@After、@AfterReturning、@AfterThrowing、@Around
    
    * Pointcut
    
      @Pointcut

- Spring tx

  - `<tx:annotation-driven/>`的解析过程

  - `@Transactional` 

    代理过程，用的什么代理，怎么代理

    这里有一点需要注意的地方，由于 SpringAOP 的原因，@Transactional 注解只能用到 public 方法上，如果用到 private 方法上，将会被忽略，这也是面试经常问的考点之一。



#### 组件整合

- [Spring Cache 介绍](https://www.cnblogs.com/rollenholt/p/4202631.html)

- [Spring Webflux](https://www.jianshu.com/p/c029de45d23b) （reactive web 框架，与前端 Flux 架构名字相同）

  命令式编程 VS **响应式编程**

- Spring Data

  - [Spring Data JPA 简单查询--接口方法 - 如莲家园 - 博客园](https://www.cnblogs.com/rulian/p/6557471.html)
  - [JPA的查询语言—使用原生SQL_ChenAllen1025的专栏-CSDN博客](https://blog.csdn.net/chenallen1025/article/details/9169543)

- 与其他构架的整合

  - [企业大型互联网分布式架构 {Java 分布式架构 dubbo+springmvc+mybatis+ehcach+redis }-IT 未来-ITPUB 博客](http://m.blog.itpub.net/31452580/viewspace-2148363/)
  - [手把手教你从最基本的 Java 工程搭建 SpringMVC+SpringDataJPA+Hibernate(含源码下载) - anxpp 的博客 - CSDN 博客](http://blog.csdn.net/anxpp/article/details/51415366)



#### SpringBoot

- SpringBoot 自动配置机制
- SpringBoot 启动过程
- SpringBootStarter 依赖

简易教程

[Spring Boot教程™ (yiibai.com)](https://www.yiibai.com/spring-boot)



#### SpringCloud

核心子项目：

- Spring Cloud Netflix：核心组件，可以对多个 Netflix OSS 开源套件进行整合，包括以下几个组件：
  - Eureka：服务治理组件，包含服务注册与发现
  - Hystrix：容错管理组件，实现了熔断器
  - Ribbon：客户端负载均衡的服务调用组件
  - Feign：基于 Ribbon 和 Hystrix 的声明式服务调用组件
  - Zuul：网关组件，提供智能路由、访问过滤等功能
  - Archaius：外部化配置组件
- Spring Cloud Config：配置管理工具，实现应用配置的外部化存储，支持客户端配置信息刷新、加密/解密配置内容等。
- Spring Cloud Bus：事件、消息总线，用于传播集群中的状态变化或事件，以及触发后续的处理
- Spring Cloud Security：基于 spring security 的安全工具包，为我们的应用程序添加安全控制
- Spring Cloud Consul：封装了 Consul 操作，Consul 是一个服务发现与配置工具（与 Eureka 作用类似），与 Docker 容器可以无缝集成

简易教程：

[Spring Cloud 微服务架构学习笔记与示例 - EdisonZhou - 博客园 (cnblogs.com)](https://www.cnblogs.com/edisonchou/p/java_spring_cloud_foundation_sample_list.html)



### ASM 神器

spring-core 自带有 asm，org.ow2.asm 也是一个轻量级的 jar

还有 byte buddy 库，javassist 库



### JAX-RS

全称：Java API for RESTful Web Services，是一套用 java 实现 REST 服务的规范，提供了一些标注将一个资源类，一个 POJOJava 类，封装为 Web 资源。

包括：

- @Path，标注资源类或方法的相对路径
- @GET，@PUT，@POST，@DELETE，标注方法是用的 HTTP 请求的类型
- @Produces，标注返回的 MIME 媒体类型
- @Consumes，标注可接受请求的[MIME](http://liugang594.iteye.com/wiki/MIME) 媒体类型
- @PathParam，@QueryParam，@HeaderParam，@CookieParam，@MatrixParam，@FormParam，分别标注方法的参数来自于 HTTP 请求的不同位置，例如@PathParam 来自于 URL 的路径，@QueryParam 来自于 URL 的查询参数，@HeaderParam 来自于 HTTP 请求的头信息，@CookieParam 来自于 HTTP 请求的 Cookie

`Eureka`的`ApplicationResource`有用到



### 缓存

- Guava 的缓存

  Guava Cache 说简单点就是一个支持**LRU**的 ConcurrentHashMap

  1. [简析 guava cache 线程安全设计哲学 - 简书 (jianshu.com)](https://www.jianshu.com/p/699869cb5421)

- **Caffeine** 来自未来的缓存

  Caffeine 是基于 JAVA 1.8 Version 的高性能缓存库。Caffeine 提供的内存缓存使用参考 Google guava 的 API。Caffeine 是基于 Google Guava Cache 设计经验上改进的成果。



### 日志

- 区分`commons-logging`，`slf4j`，`log4j`，`logback`
  
  [Java日志，需要知道的几件事(commons-logging,log4j,slf4j,logback)_kobejayandy的专栏-CSDN博客](https://blog.csdn.net/kobejayandy/article/details/17335407)
  
  1. 了解`jcl-over-slf4j`，`jul-to-slf4j`这些 jar 的作用
  2. 了解`log4j`和`log4j2`的区别，**`lmax disruptor`**应用场景
  
- log4j
  
  - [log4j是如何拖慢你的系统的_veZunShao的专栏-CSDN博客](https://blog.csdn.net/king_is_everyone/article/details/78580924?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
  
- **`Flume`** 日志采集系统，一般用于日志聚合



### ORM 库

- hibernate

  查询：HQL 查询，QBC 查询，SQL 查询

  级联查询：一对一，一对多（多对一），多对多；懒加载，1+n 问题

  其他：

  1. session.get(): 非懒加载方法

     session.load(): 默认就是是懒加载

  2. 抓取策略（fetch）和 懒加载（lazy）

- mybatis

  [mybatis 3.x源码深度解析与最佳实践（最完整原创） - zhjh256 - 博客园 (cnblogs.com)](https://www.cnblogs.com/zhjh256/p/8512392.html)



### Netty

- [概述](https://www.jianshu.com/p/1a6d1a25e6cc)

- Netty 的线程模型

  通过**Reactor 模型**基于**多路复用器**接收并处理用户请求，内部实现了两个线程池，boss 线程池和 work 线程池，其中 boss 线程池的线程负责处理请求的 accept 事件，当接收到 accept 事件的请求时，把对应的 socket 封装到一个 NioSocketChannel 中，并交给 work 线程池，其中 work 线程池负责请求的 read 和 write 事件

- NioEventLoop 设计原理

- 定时任务的原理

- **netty 对象池使用与回收**

- 时间轮算法

  [HashedWheelTimer](http://novoland.github.io/%E5%B9%B6%E5%8F%91/2014/07/26/%E5%AE%9A%E6%97%B6%E5%99%A8%EF%BC%88Timer%EF%BC%89%E7%9A%84%E5%AE%9E%E7%8E%B0.html)

  > hashWheel 定时器和 Quartz 的区别：  
  > 1）Quartz 将定时任务分为任务和触发器，而 hashWheel 只有任务的概念
  >
  > 2）Quartz 通过一个 TreeSet 对所有的触发器进行管理，而 hashWheel 通过一个 hash 轮来对所有的任务进行管理
  >
  > 3）Quartz 能够非常方便的删除定时任务，而 netty 的 hashWheel 暂时没有删除任务的接口（除非自己实现一个 hashWheel 定时器）
  >
  > 4）Quartz 有一个专门的调度线程对任务进行管理，任务执行有另外专门的线程池，而 hashWheel 用一个线程实现对任务的管理和任务的执行。
  >
  > 5）Quartz 能够通过序列化，将定时任务保存在数据库，而 hashWheel 不能
  >
  > 总的来说，Quartz 的功能相对强大，而 hashWheel 相对要轻量级一点。

- 附：

  个人认为 netty 对用户来说是异步，但是实际底层 IO 是 IO 多路复用模型，本质上还是一种同步非阻塞（是的，个人认为 IO 多路复用模型还是**同步**非阻塞，并且[**真正的 IO 操作**都将**阻塞**应用线程](https://weread.qq.com/web/reader/1e732510718f63a11e7dee2k98f3284021498f137082c2e)），他只是多了一个 Selector（需要底层操作系统支持），如此一个线程就可以控制大量的通信（相比传统 IO，不管他是不是非阻塞）。

  另看 [IO#IO 概念](#io)，这里也收录了一些理解

- 面试

  [阿里大牛总结的Netty最全常见面试题，面试再也不怕被问Netty了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/148726453)



### Disruptor

Disruptor 是一个无锁、有界的队列框架，它的性能非常高。

- 背景

  1. [锁的缺点 - Disruptor 入门](http://wiki.jikexueyuan.com/project/disruptor-getting-started/lock-weak.html)

  2. 并发中的伪共享问题

  3. 代码的并发执行大约是两件事：互斥和变化的可见性。

     互斥是关于管理某些资源的竞争更新。

     变化的可见性是关于控制何时使这些更改对其他线程可见。

- 设计上的优势

  1. 内部数据存储使用环形缓冲（Ring Buffer），这样分配支持了**CPU 缓存位置预测**，**GC 的压力更小**
  2. **尽量使用无锁设计，合理使用 CAS**
  3. 优化数据结构（填充缓存行），**解决伪共享问题**
  4. 合理位运算（如 2 次方幂求模），**合理使用 Unsafe**

- 策略

  `WaitStrategy`可以选择`YieldingWaitStrategy`（无锁）

- 参考博客

  1. [解读 Disruptor 系列](https://www.jianshu.com/u/4c940e688e05)，这个系列挺好的，他每篇文章后面都有份参考资料，也可以认真看看

- 扩展

  1. AtomicXXX.lazySet 这个方法的作用（Sequence#set 相当于 AtomicLong#lazySet）
  2. Unsafe 类的作用？为什么要用这个类？除了 JDK，在 Netty、Spring、Kafka、Storm 等非常多的流行开源项目中都使用了 Unsafe



### 原子类型集合库

避免开销很大的装箱/拆箱操作，节省了原始类型装箱消耗的内存

- Koloboke

  [生成高性能的 JAVA 基本类型 map/set](https://blog.csdn.net/qinyongye/article/details/81282961)

- Eclipse Collections

- Fastutil



### 时间库

- **joda** 对时间的操作
- Quartz 定时任务



### RxJava

[➮详细](/2019/01/29/事件驱动编程 RxJava/)

" a library for composing asynchronous and event-based programs using observable sequences for the Java VM "  （一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库）





## 工具

- 构建工具
  - Maven
  - Gradle
    - [十分钟理解 Gradle - Bonker - 博客园](https://www.cnblogs.com/Bonker/p/5619458.html)
    - 慕课实战：Gradle3.0 自动化项目构建技术精讲+实战
    - [Gradle Distributions](https://services.gradle.org/distributions)
- 版本管理工具
  
  - Git
- 持续集成部署
  
- Jenkins
  
- 单元测试

  [玩转单元测试之 DBUnit - WadeXu - 博客园](https://www.cnblogs.com/wade-xu/p/4547381.html)

- 逆向工程

  [Java 代码生成利器之 rapid-generate 应用](http://blog.csdn.net/likeaboy_fire/article/details/44024987)





## Web容器

- tomcat





## 中间件

### Zookeeper

- 场景

  ZooKeeper来做：统一配置管理、统一命名服务、分布式锁、集群管理。

  使用分布式系统就无法避免对节点管理的问题（需要实时感知节点的状态、对节点进行统一管理等等），而由于这些问题处理起来可能相对麻烦和提高了系统的复杂性，ZooKeeper 作为一个能够**通用**解决这些问题的中间件就应运而生了。

- 原理

  - [Zookeeper 的功能以及工作原理](https://www.cnblogs.com/felixzh/p/5869212.html)
  - [Leader 选举-选举过程介绍比较清晰](https://blog.csdn.net/gaoshan12345678910/article/details/67638657)
  - [ZAB 协议理解](https://blog.csdn.net/junchenbb0430/article/details/77583955)



### Dubbo

Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。



### 消息队列

**主要使用场景：**

异步、削峰、解耦

**带来问题：**

1. 系统复杂性

   消息**重复消费**、**消息丢失**、**消息的顺序消费**等等

2. 数据一致性

   其他服务失败导致数据不一致？需要分布式事务？

3. 可用性

   MQ挂了咋办？

**主流：**

Kafka 和 RocketMQ



#### Kafka

- 场景

  - 想要保证消息（数据）是有序的，怎么做？

    Kafka会将数据写到 partition，单个 partition 的写入是有顺序的。如果要保证全局有序，那只能写入一个 partition 中。如果要消费也有序，消费者也只能有一个。

- **Kafka 性能优化：**

  1. 零拷贝网络和磁盘
  2. 优秀的网络模型，基于 Java NIO
  3. 高效的文件数据结构设计
  4. Parition 并行和可扩展
  5. 数据批量传输
  6. 数据压缩
  7. 顺序读写磁盘
  8. 无锁轻量级 offset

- 参考

  [Kafka性能篇：为何Kafka这么"快"？](https://mp.weixin.qq.com/s/kMIhPW2uLdy-mgS9sF6agw)



#### RocketMQ

TODO



#### RabbitMQ

了解其 Exchange (交换器)，常用的有四种：direct、topic、fanout、headers



### MySQL

#### sharding-jdbc

支持数据分片，分布式事务，数据库治理



#### 连接池

目的：解决建立数据库连接耗费资源和时间很多的问题，提高性能。

自定义数据库连接池要实现 javax.sql.DataSource 接口，一般都叫数据源。

**常用的数据源：**

DBCP：Apache推出的Database Connection Pool

C3P0：开源的 JDBC 连接池



#### 其他

- apache DBUtils

  DBUtils简化了JDBC的开发步骤，使得我们可以用更少量的代码实现连接数据库的功能。

- TDDL、cobar 等



### 搜索引擎

#### Elasticsearch

- [Elasticsearch 基础教程 - CSDN 博客](http://blog.csdn.net/cnweike/article/details/33736429)

- **倒排索引**

- 和 传统关系型数据库 的对比

  <table>
    <thead>
      <tr>
        <th>Relational DB</th>
        <th>Databases</th>
        <th>Tables</th>
        <th>Rows</th>
        <th>Columns</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>关系型数据库</td>
        <td>数据库</td>
        <td>表</td>
        <td>行</td>
        <td>列</td>
      </tr>
    </tbody>
  </table>

  <table>
    <thead>
      <tr>
        <th>Elasticsearch</th>
        <th>Indices</th>
        <th>Types</th>
        <th>Documents</th>
        <th>Fields</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>搜索引擎</td>
        <td>索引</td>
        <td>类型</td>
        <td>文档</td>
        <td>域（字段）</td>
      </tr>
    </tbody>
  </table>

- 使用

  - [Spring Data ElasticSearch_liuxigiant的专栏-CSDN博客](https://blog.csdn.net/liuxigiant/article/details/52105024)
  - [基于注解的配置 - Spring-Data-Elasticsearch](https://es.yemengying.com/5/5.1/5.1.2.html)



#### Logstash

[Logstash：收集、解析和转换日志 | Elastic](https://www.elastic.co/cn/logstash/)



#### Kibana

ELK：ELK 技术栈（ElasticSearch, Logstash, Kibana）搭建实时日志分析平台，将日志保存到 Elasticsearch 中，通过 Logstash 进行分析，并使用 Kibana 来展示和查询。



#### Lucene、Solr

TODO



### SOFAStack

[项目 · SOFAStack](https://www.sofastack.tech/projects/)，是一套用于快速构建金融级云原生架构的中间件，也是在金融场景里锤炼出来的最佳实践。

- SOFAJRaft

  SOFAJRaft 是一个基于 RAFT 一致性算法的生产级高性能 Java 实现，支持 MULTI-RAFT-GROUP，适用于高负载低延迟的场景。





## 分布式



### 理论基石

CAP 原理：

**C** - **C**onsistent ，一致性

**A** - **A**vailability ，可用性

**P** - **P**artition tolerance ，分区容忍性

但——**网络分区发生时，一致性和可用性两难全**



### 主要算法

- 一致性 Hash

  redis 分片

- 分布式集群中，生成全局唯一的ID

  - UUID

    String uuid = UUID.randomUUID().toString()

    虽然可以保证全局唯一，但占用32位太长，而且无序，入库时性能比较差。

    为什么无序的UUID会导致入库性能变差呢？

    这就涉及到 **B+树索引的分裂**：关系型数据库的索引大都是B+树的结构，拿ID字段来举例，索引树的每一个节点都存储着若干个ID。如果我们的ID按递增的顺序来插入，比如陆续插入8，9，10，新的ID都只会插入到最后一个节点当中。当最后一个节点满了，会裂变出新的节点。这样的插入是性能比较高的插入，因为这样节点的分裂次数最少，而且充分利用了每一个节点的空间。**但是，如果我们的插入完全无序，不但会导致一些中间节点产生分裂，也会白白创造出很多不饱和的节点，这样大大降低了数据库插入的性能。**

  - 数据库自增主键

    为了提高性能，在分布式系统中可以用DB proxy请求不同的分库，每个分库设置不同的初始值，步长和分库数量相等

    这样一来，DB1生成的ID是1,4,7,10,13....，DB2生成的ID是2,5,8,11,14.....

    但这样也不是很好。ID的生成对数据库严重依赖，影响性能，一旦数据库挂掉，服务将变得不可用。

  - SnowFlake

    [漫画：什么是SnowFlake算法？](https://blog.csdn.net/bjweimengshu/article/details/80162731)

    ![](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/NtO5sialJZGovUVwFkfA0yRdCYoer9mqxdkKsBd5aD96r6ygicrXlKjwmsIBCZpF4rrkUM7FR1U1zZdL4yjEF1Fw/640?wx_fmt=png)



### 网络通信

- RPC

  RPC 涉及：通讯，序列化，超时，重发（重复），消息顺序，负载 等等。（个人理解）

  - 协议：thrift、gRPC 等等
  - JavaRMI
    - [深究 Java 中的 RMI 底层原理](https://blog.csdn.net/sinat_34596644/article/details/52599688)
  - HSF
    阿里巴巴集团内部使用的分布式服务框架 High Speed Framework

- Dubbo



### 一致性

#### 分布式锁

分布式锁一般有三种实现方式：

1. 数据库乐观锁；
2. 基于 Redis 的分布式锁；（看数据库/Redis篇）
3. 基于 ZooKeeper 的分布式锁



#### 分布式事务

- 和分布式锁的区别

  要捋清一些概念：

  分布式事务指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。

  简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。

  本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

- 共识算法

  Paxos、Raft、Zab：

  [分布式事务与一致性算法 Paxos & raft & zab](https://blog.csdn.net/followmyinclinations/article/details/52870418)

- 分布式事务一致性的常见解决方案
  - 2PC，3PC
  - XA
  - 消息中间件最终一致性

  参考：

  - [分布式一致性算法2PC和3PC_fcj的技术博客_51CTO博客](https://blog.51cto.com/u_11821908/2058651)

- Java 分布式事务规范 JTA / XA

  JTA 是 Java 的事务管理器规范

  XA 是工业标准的 X/Open CAE 规范，可被两阶段提交及回滚的事务资源定义

  参考：

  - atomikos:[4.0 atomikos JTA/XA 全局事务](http://www.tianshouzhi.com/api/tutorials/distributed_transaction/386)
  - xaresource
  - [分布式事务](https://javatar.iteye.com/blog/981787)
  - [分布式事务系列（2.1）分布式事务的概念](https://yq.aliyun.com/articles/39047)



#### 分布式 session 一致性

1. session 复制，对 web 服务器 (例如 Tomcat) 进行搭建集群
2. session 绑定，使用 nginx `ip-hash 策略`，无论客户端发送多少次请求都被同一个服务器处理
3. 基于 redis 存储，spring 为我们封装好了 spring-session，直接引入依赖即可



### 高可用

- **缓存、降级、限流**

- 服务器端如何处理超大量合法请求？

  服务器架构层面，做负载均衡，将请求分发给其它服务器处理。

  软件服务架构层面，做请求队列，将 1w 个请求放入队列，业务处理完的请求再返回。

  代码层面，优化业务处理，把单机请求做到支持 1w 并发。

- 容量设计

  [互联网架构，如何进行容量设计？_w3cschool](https://www.w3cschool.cn/architectroad/architectroad-capacity-design.html)



### 架构演进

- 从All in one 到微服务
- [架构师之路_w3cschool](https://www.w3cschool.cn/architectroad/)





## 前沿技术

### 网络协议

#### RSocket

RSocket是一种二进制的点对点通信协议，是一种新的网络通信第七层协议。旨在用于分布式应用程序中。从这个意义上讲，RSocket是HTTP等其他协议的替代方案。它是一种基于Reactive Streams规范具有异步，背压的双向，多路复用，断线重连，基于消息等特性。它由Facebook，Netifi和Pivotal等工程师开发，提供Java，JavaScript，C ++和Kotlin等实现。

入门使用：

[RSocket协议初识-Java中使用（二）_后厂村老司机的博客-CSDN博客](https://blog.csdn.net/KouLouYiMaSi/article/details/106421826)



### 容器化

**Docker**

大家需要注意，**Docker本身并不是容器**，它是创建容器的工具，是应用容器引擎。

想要搞懂Docker，其实看它的两句口号就行。

第一句，是“**Build, Ship and Run**”。也就是，“搭建、发送、运行”，三板斧。

第二句口号就是：“**Build once，Run anywhere（搭建一次，到处能用）**”。



**K8S**

就在Docker容器技术被炒得热火朝天之时，大家发现，如果想要将Docker应用于具体的业务实现，是存在困难的——编排、管理和调度等各个方面，都不容易。于是，人们迫切需要一套管理系统，对Docker及容器进行更高级更灵活的管理。

就在这个时候，K8S出现了。

**K8S，就是基于容器的集群管理平台，它的全称，是kubernetes。**



### ServiceMesh

ServiceMesh，也叫服务网格，是一种概念。

**一言以蔽之：Service Mesh 是微服务时代的 TCP/IP 协议。**



### 中台

**构建中台的目的**

中台的目的是**构建企业级统一的服务接口**，不只是数据，包括技术、业务、组织架构等，其实质是整合企业内的软硬件资源，包括人力资源。传统单体系统，一个系统一套软硬件开发和运维人员，这些系统所采用的厂商、技术、开发语言、技术架构、数据库等可能各不相同。随着信息化系统越来越多，系统间面临着数据共享的要求。所以系统集成技术就应运而生。

[什么是中台？为什么需要中台？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/144497394)



### 思想

#### Reative 编程

Reactive 响应式 (反应式) 编程 是一种新的编程风格，其特点是异步或并发、事件驱动、推送 PUSH 机制以及观察者模式的衍生。

JVM 应用：RxJava、Akka、Actors 模型、Vert.x、Webflux



#### 领域驱动设计

> 他是综合软件系统分析和设计的面向对象**建模方法**，如今已经发展为一种针对大型复杂系统的领域建模与分析方法。
>
> 将要解决的**业务概念和业务规则**转换为**软件系统中的类型及类型的属性与行为**，**通过合理运用面向对象的封装、继承、多态等设计要素**，降低或隐藏整个系统的业务复杂性，并使得系统具有更好的扩展性，应对纷繁多变的现实业务问题。
>
> ——抄录于《高可用可伸缩微服务架构：基于 Dubbo、Spring Cloud 和 Service Mesh》2.1 节

- [领域驱动设计在互联网业务开发中的实践](https://tech.meituan.com/2017/12/22/ddd-in-practice.html)
- [美团 DDD 实践 示例项目](https://github.com/1987539447/draw-lottery)





## 代码评审

基本（规范，模块化，逻辑）

安全（表单校验，防攻击SQL注入，线程安全）

数据库（事务，sql优化）

性能





## 性能优化

- CPU 伪共享问题

  问题：二维数组按行和按列遍历效率（Java章节中有说明）

  应用：netty 中的 FastThreadLocal 中 InternalThreadLocalMap（Java章节中有说明）；lmax disruptor 等

- 对象池



## 安全

### Java 反序列化

[java反序列化漏洞的一些gadget](https://blog.csdn.net/whatday/article/details/107854348)



#### log4j

CVE-2019-17571：[log4j<=1.2.17反序列化漏洞](https://blog.csdn.net/weixin_39664746/article/details/111268312)

CVE-2021-44228：

- [(环境搭建+复现) CVE-2021-44228 Apache Log4j 远程代码执行漏洞](https://blog.csdn.net/qq_40989258/article/details/121862363)
- [安全漏洞之Log4j2漏洞复现绕过分析](https://baijiahao.baidu.com/s?id=1718946520876495065&wfr=spider&for=pc)



#### dubbo

[反序列化漏洞攻击原理(Dubbo反序列化漏洞剖析)](https://blog.csdn.net/hilaryfrank/article/details/104450106)



#### Gadgetinspector

一款针对Java应用程序/库的字节码分析工具，它可以帮助研究人员寻找和分析Java应用程序中的反序列化小工具链（Gadget Chain）

https://github.com/JackOfMostTrades/gadgetinspector

[Java 反序列化工具 gadgetinspector 初窥](https://blog.csdn.net/qq_43380549/article/details/100974531)







## HTML/CSS/JS

**HTML/CSS**

[Flex 布局教程：语法篇 - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

<br/>

**JavaScript**

- ECMAScript

- [阿里巴巴的这道题为什么选A？为什么new main()输出的都是undefined？ - SegmentFault 思否](https://segmentfault.com/q/1010000004573768/a-1020000004577401)

<br/>

**前端框架**

- Bootstrap 教程 - 菜鸟教程

- [Moment.js 中文网 (momentjs.cn)](http://momentjs.cn/)

- [Vue](https://cn.vuejs.org/)

  - 双向数据绑定与单向数据绑定

  - [Vuex](https://vuex.vuejs.org/zh/)，[Weex](http://weex.apache.org/cn/)

  - [vue 常用组件库_zhouzhiwengang的专栏-CSDN博客_vue组件库](https://blog.csdn.net/zhouzhiwengang/article/details/72621219)

- React

  [React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)

- Flux 架构

  [Flux 架构入门教程](http://www.ruanyifeng.com/blog/2016/01/flux.html)

- 状态管理

  - [聊一聊主流前端框架的状态管理](https://www.cnblogs.com/axel10/archive/2018/03/15/8571757.html)

  - [前端状态管理请三思](https://juejin.im/post/59fd94475188254115703461)

<br/>

**其他**

- [给 2019 前端的 5 个建议](https://juejin.im/post/5c617c576fb9a049e93d33a4)

- [浏览器原理系列 10 篇正式完结](https://juejin.im/post/5c6d3e026fb9a04a0d576f98)







## Weex

**前言**

移动端**跨平台 **UI 框架

<br/>

**Weex基础**

- [Weex-初次见到你-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/59341)

- [weex社区 - 专题 - 简书 (jianshu.com)](https://www.jianshu.com/c/f152a6d31479)

- [没有死！阿里公开Weex技术架构，还开源了一大波组件_我只是一个小小的搬运工的博客-CSDN博客](https://blog.csdn.net/zz901214/article/details/79168707)

- [（总结）Weex若干特性总结分析 - 大球和二憨 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lightsun/p/5880753.html)

- [Weex Android SDK源码分析之Module（modal）_王永迪的专栏-CSDN博客](https://blog.csdn.net/walid1992/article/details/51706199)

- [Weex Android交互篇_hzh839900的专栏-CSDN博客](https://blog.csdn.net/hzh839900/article/details/52779003)

<br/>

**Weex Ui**

[Weex Ui (apache.github.io)](https://apache.github.io/incubator-weex-ui/#/)

<br/>

**Eros**

[bmfe/eros: 📱 一套 Vue 代码，两端原生应用 ，或许可以叫我 weex-native。 (github.com)](https://github.com/bmfe/eros)

[EROS (bmfe.github.io)](https://bmfe.github.io/eros-docs/#/?id=介绍)

<br/>

**Nat**

[natjs/nat: A powerful kit for adding native functionalities to your weex app. (github.com)](https://github.com/natjs/nat)

<br/>

**EMAS组件**

待补充







## 源码学习

这些框架的源码都值得一看：

- spring
- dubbo
- zookeeper
- tomcat





## 书单

**Spring**

- 《Spring 源码深度解析 第二版》《Spring 实战》
- 《Spring Boot 编程思想（核心篇）》![书单·图 1](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3621582485,3050859261&fm=58&bpow=800&bpoh=940)
- 《Spring Boot 实战》
- 《Spring 微服务实战》

**MySQL**

- 《高性能 MySQL》

**Netty**

- 《Netty 权威指南》

**Tomcat**

- 《Tomcat 架构解析 （刘光瑞）》

**其他**

- 《架构探险分布式服务框架 （李业兵）》

