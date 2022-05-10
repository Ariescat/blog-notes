# 《Game》



## 网络

### 信道

- 丢包率
- 延时
- 带宽 单位时间通过的数据量



### 网络协议

**TCP**

启用和禁用TCP_NODELAY有什么影响？

<br/>

**UDP**

可靠 UDP 的实现：

- KCP

- FEC（牺牲带宽换取效率）
  - kcp + fec
  - 自适应模块
    - 丢包探测
    - grp协议 开关
  - *RS编码*，又称里所码，即Reed-solomon codes



### 应用层协议

- Json，MessagePack

- ProtoBuf



### 字节压缩

**deflate**

deflate算法就是基于LZ77算法和Huffman编码基础上实现的。

可以指定算法 的压缩级别，这样你可以在压缩时间和输出文件大小上进行平衡。可选的级别有0（不压缩），以及1(快速压缩)到9（慢速压缩）。

deflate是zip压缩文件的默认算法。其实deflate现在不光用在zip文件中，在7z、xz等其他的压缩文件中都用。

<br/>

**gzip**

当键入 `tar -zcf src.tar.gz src` 时，就可以将 `src` 下的所有文件打包成一个 tar.gz 格式的压缩包。这里的 “tar” 是归档格式，将多个文件组合成一个文件；而 “gz” 指的就是 gzip 压缩格式，使用 deflate 算法压缩得到。

deflate 是最基础的算法，gzip 在 deflate 的 raw data 前增加了 10 个字节的 gzheader，尾部添加了 8 个字节的校验字节（可选 crc32 和 adler32） 和长度标识字节。

<br/>

**Snappy**

snappy是google基于LZ77的思路编写的快速数据压缩与解压程序库。它的目标并非最大压缩率或与其他压缩程序库的兼容性，而是非常高的速度和合理的压缩率。





## 数据同步

### 帧同步

**驱动帧模式**

- 传统模式，即客户端上传指令，服务端转发后才能生效的模式。此模式主要适用于操作连贯、写实的游戏中，例如足球等。
- 预测回滚模式，即客户端本地立即执行指令，再上传指令到服务端，发现不同步后再回滚的模式。此模式主要适用于有效输入频率较少、动作可不连贯、不太写实的游戏中，例如拳皇等。
- 外部输入模式，即驱动帧由外部提供，客户端被动执行的模式。此模式常用在直播需求中，例如足球比赛的全网直播等。

<br/>

**相关算法**

- 帧锁定同步算法

  [帧锁定同步算法 - Skywind Inside](http://www.skywind.me/blog/archives/131)

  早期 RTS，XBOX360 LIVE游戏常用同步策略是什么？格斗游戏多人联机如何保证流畅性和一致性？如何才能像单机游戏一样编写网游？

  **算法概念**

  该算法普遍要求网速RTT要在100ms以内，一般人数不超过8人，在这样的情况下，可以像单机游戏一样编写网络游戏。所有客户端任意时刻逻辑都是统一的，缺点是一个人卡机，所有人等待。

  1. 客户端定时（比如每五帧）上传控制信息。
  2. 服务器收到所有控制信息后广播给所有客户。
  3. 客户端用服务器发来的更新消息中的控制信息进行游戏。
  4. 如果客户端进行到下一个关键帧（5帧后）时没有收到服务器的更新消息则等待。
  5. 如果客户端进行到下一个关键帧时已经接收到了服务器的更新消息，则将上面的数据用于游戏，并采集当前鼠标键盘输入发送给服务器，同时继续进行下去。
  6. 服务端采集到所有数据后再次发送下一个关键帧更新消息。

  这个等待关键帧更新数据的过程称为“帧锁定”

  应用案例：大部分RTS游戏，街霸II(xbox360)，Callus模拟器。

<br/>

- 预测回滚的帧同步

  简单来介绍下这种同步机制：

  假设游戏运行时帧率为 60 帧，每帧用时 16 ms。

  - 每个客户端不阻塞等待远端输入
    - 每一帧在拿到本地输入后，如果远端输入还没有达到本地，则**猜测这次的远端输入与上一次一样**，用猜测的远端输入+本地输入更新游戏状态，继续进行游戏
  - 当收到某帧的远端收入后，对比历史中对该帧的输入猜测和实际输入是否一样
    - **注意接下来的所有步骤都在一帧的物理时间（16ms）内处理完，下面步骤的帧都是逻辑意义上的帧**
    - 不一样的话将本地客户端的游戏状态回滚到输入不一样的帧的上一帧 `Frame 1`
    - 利用真实的远端输入和本地的历史输入重新追算出来当前帧 `Frame N`的游戏状态（`Frame N` 和 `Frame 1` 之间可能会差几帧，即需要重新计算多个逻辑帧的状态变化）
      - 相当于坐上时光机，重回历史，改变历史，回到新的现在（这一切都是命运石之门的选择）
    - 基于错误的预测输入可能渲染多余的画面/音频，需要把这些清理干净
    - 基于新的游戏状态重新渲染出来正确的游戏画面/音频

<br/>

**问题**

一致性保证

> - 解决浮点数计算误差的问题
>
>   可使用 fixmath 库，该库使用整数类型代替浮点数（不过使用 Fix16 类型是否会精度不够？扩展为 Fix32?）
>
> - 解决随机数一致性问题
>
>   可使用用基于定点数的 Wichmann Hill 算法
>
> - 解决物理计算误差问题
>
>   可用基于定点数的Box2D
>
> - 逻辑调用的一致性问题
>
>   严格时序，并提供基于帧号的定点数timer，其实现方式类似于时间轮

存在网络延迟时，客户端没收到服务端数据时怎么办

> 客户端预测、插值、缓存池、不一致回退等方式。
>
> 大部分帧同步都不会进行逻辑预测，只会进行表现预测，比如模型先往前走，逻辑位置数据保持原地，网络延迟卡顿会等待帧数据的到来。

如何防作弊

> 1. 服务端运行和客户端一样或核心逻辑，对于关键数据进行同步验证比对；或是战斗结束后对指令进行快速播放校验。但无法避免客户端利用所有玩家信息的全图挂。
> 2. 客户端之间的比对校验，环境相同，结果不一样的客户端是异常的。
> 3. 战局回放和举报机制，帧同步的回放实现很容易，让玩家举报玩家也是一种好办法。

<br/>

**基础**

- 解决快照生成、校验、现场恢复等问题
- 动态调整帧率和倍速，动态可调节，状态可自动记录。
- 提供暂停和软暂停功能，适应多种使用场景。

<br/>

**拓展**

属性

> 定期为场景和各个logic_entity的属性生成快照并序列化，然后将快照md5上传至Server进行对比，若发现快照不一致，将使用验证端最新的快照作为标准进行现场恢复。

AI计算分配

> 对于帧同步的游戏而言，AI的工作通常由各个客户端自己完成行为树，状态机的计算，无需通过服务端计算和命令收发。但如果客户端需要计算的AI比较多，计算复杂度又比较高，这时由客户端完成所有AI的策略计算，AI的开销对于性能不佳的机器而言就是不小的负担了。
>
> 在这种情况下，如果在客户端≥2个，就可以对AI的计算工作进行分配，每个客户端负责一定数量的AI计算，并通过帧命令的方式 (与普通玩家操作相似) 确保AI的行为一致。
>
> 考虑参数：客户端基数、客户端性能、客户端网络、客户端断线



### 状态同步

#### 移动同步

摇杆同步和坐标同步：

<table>
   <tr>
      <th></th>
      <th>摇杆同步</th>
      <th>坐标同步</th>
   </tr>
   <tr>
      <td>移动延迟</td>
      <td>低</td>
      <td>高</td>
   </tr>
   <tr>
      <td>消息流程</td>
      <td>摇杆移动->服务端收到摇杆信息进行广播->客户端收到摇杆消息开始移动</td>
      <td>摇杆移动->服务端收到摇杆信息->服务端对象移动->客户端收到坐标开始移动</td>
   </tr>
   <tr>
      <td>网络波动/丢包表现</td>
      <td>好，人物可以进行相对流畅的移动表现</td>
      <td>差，网络包间隔不均匀会导致人物移动也变得卡顿感很重</td>
   </tr>
   <tr>
      <td>位置误差</td>
      <td>差，可能会产生累积误差，需要进行额外的校正</td>
      <td>基本无误差</td>
   </tr>
</table>

做加法？服务端向客户端发送的不在是摇杆信息，而是坐标和摇杆的组合信息。



### 网络延迟处理

无论是状态同步还是帧同步，由于网络延迟、网络波动，客户端需要对位置数据进行预测，避免移动卡顿。





## AI

- 状态机
- 行为树

  - 入门

    - [Java游戏服务器开发之行为树_cmqwan的博客-CSDN](https://blog.csdn.net/cmqwan/article/details/80453352)
- 事件行为树





## 寻路

- A* 及其变种算法

  参考：

  [路径规划之 A* 算法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/54510444)

- JPS

  参考：

  《2018腾讯移动游戏技术评审标准与实践案例》：寻路算法 JPS 优化章节

  [「游戏」寻路算法之JPS原理和实现__Echo_-CSDN博客](https://blog.csdn.net/qq_37005831/article/details/115091377)

  [JPS（Jump Point Search）寻路及实现代码分析_燕临江下的蛋-CSDN博客](https://blog.csdn.net/u011265162/article/details/91048927)

  [[算法\]小学堂：JPS寻路算法浅析 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/25093275)

- 导航网格（navmesh）

  导航网格的生成会分为下面几个步骤：

  1. 场景模型体素化（Voxelization），或者叫“栅格化”（Rasterization）
  2. 过滤出可行走面（Walkable Suface）
  3. 生成 Region
  4. 生成 Contour（边缘）
  5. 生成 Poly Mesh
  6. 生成 Detailed Mesh

  参考：

  - 《腾讯游戏开发精粹》：第5章 3D游戏碰撞之体素内存、效率优化
  - [游戏的寻路导航 1：导航网格 - 简书 (jianshu.com)](https://www.jianshu.com/p/490a9128b248)
  - [NMGen Study](http://www.critterai.org/projects/nmgen_study/) 项目，NMGen研究是Java中 [Recast静态网格](https://github.com/memononen/recastnavigation) 功能的改编，用于研究和实验目的。
  - [Recast&Detour_游蓝海 的专栏-CSDN博客](https://blog.csdn.net/you_lan_hai/category_7114516.html)
  - [寻路_长三月的游戏开发-CSDN博客](https://blog.csdn.net/needmorecode/category_7909722.html)
  - [detour 寻路核心逻辑 CrowdToolState::updateTick dtCrowd::update_只要你在的博客-CSDN博客](https://blog.csdn.net/icebergliu1234/article/details/80381342)

- **抗锯齿**

  - A* 上：

    1. 延长寻路目标

    2. a* 增加拐角代价函数





## 碰撞检测

[碰撞检测的向量实现 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903928501387277#heading-11)

这里面的参考链接也可以看看





## 包围盒

- BVH（Bounding Volume Hierarchies 层次包围盒）





## 服务端

### 架构

- [MMORPG服务器架构 - I want to fly higher - BlogJava](http://www.blogjava.net/landon/archive/2012/07/14/383092.html)

- [高性能分布式游戏服务器框架_剑心！的博客-CSDN](https://blog.csdn.net/dcba2014/article/details/72615487)



### 第三方框架

- Akka

  - **Actor 模型**

  - [akka 设计模式系列-基础模式](https://yq.aliyun.com/articles/616951?spm=a2c4e.11153940.blogcont616952.14.28751adcybgYqt)

- skynet

  - [LuaAPI · cloudwu/skynet Wiki (github.com)](https://github.com/cloudwu/skynet/wiki/LuaAPI)

  - [skynet源码赏析 (manistein.github.io)](https://manistein.github.io/blog/post/server/skynet/skynet源码赏析/)

  - [skynet教程（1）--服务的编写 - 简书 (jianshu.com)](https://www.jianshu.com/p/d843fe686fc0)

- Pinus



### 压测

robot



### 服务端源码学习

- [hstcscolor / awesome-gameserver-cn](https://github.com/hstcscolor/awesome-gameserver-cn)  中文游戏服务器资源大全

- ARPG

  - 永恒之塔开源服务器架构 [https://github.com/Aion-server/Aion-unique](https://github.com/Aion-server/Aion-unique)

  - 天堂 2 l2jserver2

    [https://github.com/oonym/l2InterludeServer](https://github.com/oonym/l2InterludeServer)

    [https://github.com/Rogiel/l2jserver2](https://github.com/Rogiel/l2jserver2)

  - 魔兽世界 server TrinityCore [https://github.com/TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore)

- tinyHeart [https://github.com/luckykun/tinyHeart](https://github.com/luckykun/tinyHeart)





## 游戏引擎

### Unity3d

- 愤怒的小鸟 [https://www.bilibili.com/video/av35565116/](https://www.bilibili.com/video/av35565116/)





## 工具

- 弱网环境

  Clumsy





## 游戏安全

- [游戏安全实验室](https://gslab.qq.com/portal.php?mod=view&aid=94)

- [“黑客”深度学习之“游戏外挂原理与实现”](https://t.cj.sina.com.cn/articles/view/6497007699/18340785300100cnws?qq-pf-to=pcqq.c2c)





## 游戏门户

- [GameRes 游资网-游戏开发者门户](https://www.gameres.com/)

- [游戏学院 - 腾讯大学](https://daxue.qq.com/game)





## 思考

- [你为什么会离开游戏行业？ - Skywind Inside](http://www.skywind.me/blog/archives/1259)





## 书单

- 2018腾讯移动游戏 技术评审标准与实践案例

- 《腾讯游戏开发精粹》

- 《腾讯游戏开发精粹Ⅱ》
