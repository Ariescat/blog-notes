# 《Game》



## 游戏技术

### 网络

- 可靠 UDP 的实现：KCP

- TCP

  启用和禁用TCP_NODELAY有什么影响？



### AI

- 状态机
- 行为树
  - 入门
    - [Java游戏服务器开发之行为树_cmqwan的博客-CSDN](https://blog.csdn.net/cmqwan/article/details/80453352)
  - 进阶
    - 事件行为树



### 寻路

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



### 碰撞检测

[碰撞检测的向量实现 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903928501387277#heading-11)

这里面的参考链接也可以看看



### 游戏框架

- Akka
  - **Actor 模型**
  - [akka 设计模式系列-基础模式](https://yq.aliyun.com/articles/616951?spm=a2c4e.11153940.blogcont616952.14.28751adcybgYqt)
- skynet
  
  - [LuaAPI · cloudwu/skynet Wiki (github.com)](https://github.com/cloudwu/skynet/wiki/LuaAPI)
  - [skynet源码赏析 (manistein.github.io)](https://manistein.github.io/blog/post/server/skynet/skynet源码赏析/)
  - [skynet教程（1）--服务的编写 - 简书 (jianshu.com)](https://www.jianshu.com/p/d843fe686fc0)
- Pinus



### 服务端

#### 架构

- [MMORPG服务器架构 - I want to fly higher - BlogJava](http://www.blogjava.net/landon/archive/2012/07/14/383092.html)
- [高性能分布式游戏服务器框架_剑心！的博客-CSDN](https://blog.csdn.net/dcba2014/article/details/72615487)



#### 压测

TODO robot



### 工具

- 弱网环境

  Clumsy



### 安全

- [游戏安全实验室](https://gslab.qq.com/portal.php?mod=view&aid=94)
- [“黑客”深度学习之“游戏外挂原理与实现”](https://t.cj.sina.com.cn/articles/view/6497007699/18340785300100cnws?qq-pf-to=pcqq.c2c)



### 源码学习

- [hstcscolor / awesome-gameserver-cn](https://github.com/hstcscolor/awesome-gameserver-cn)  中文游戏服务器资源大全

- ARPG

  - 永恒之塔开源服务器架构 [https://github.com/Aion-server/Aion-unique](https://github.com/Aion-server/Aion-unique)

  - 天堂 2 l2jserver2

    [https://github.com/oonym/l2InterludeServer](https://github.com/oonym/l2InterludeServer)

    [https://github.com/Rogiel/l2jserver2](https://github.com/Rogiel/l2jserver2)

  - 魔兽世界 server TrinityCore [https://github.com/TrinityCore/TrinityCore](https://github.com/TrinityCore/TrinityCore)

- tinyHeart [https://github.com/luckykun/tinyHeart](https://github.com/luckykun/tinyHeart)



### Unity3d

- 愤怒的小鸟 [https://www.bilibili.com/video/av35565116/](https://www.bilibili.com/video/av35565116/)





## 游戏门户

- [GameRes 游资网-游戏开发者门户](https://www.gameres.com/)
- [游戏学院 - 腾讯大学](https://daxue.qq.com/game)





## 书籍

- 2018腾讯移动游戏 技术评审标准与实践案例
- 《腾讯游戏开发精粹》
- 《腾讯游戏开发精粹Ⅱ》
