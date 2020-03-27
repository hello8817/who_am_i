<h1 align="center">编码思考</h1>  
## 阅读前闲聊几句  
　　今天看到了一篇文章,讲述了作为一个程序员,应该如何注意自己的技术成长路线.自己想到了主要有三个方面:
1. 基础知识: 对于一个技术人员来讲,首先要了解自己领域的核心知识能力图谱,而不是将过多的经历放在新出现技术的具体实现方式,以及使用细节上面.  
对于一个技术人员的核心能力来说,不是针对具体学习哪些语言,哪些框架,或者某些具体语言的细节实现,这些能力不长久.  而是另外核心原理性的东西,比如Java语言,就需要认真了解 jvm 的原理,真正动了 jvm 的原理之后,才能从底层实现的角度协助进行问题排查等.

2. 算法知识:  作为技术人员,需要对一些常见的算法比较熟悉,因此需要通过不断刷题的方式进行相关算法的熟悉.一定需要注意不断的刷题

3. 编码规范:  编码规范需要不断进行编码,养成编码习惯才可以,这个编码习惯不是看两遍才可以行程,一定要不断的有意识 **训练** 才可以



　　打算将编码学习过程中学习到的一些东西分模块的记录下来，主要是需求实现过程中的 [设计想法](#codingThought) 、 [Java相关技术知识](#coffee-Java) 、 [设计模式](#设计模式) 等开发技能，以及 [Redis](#Redis) 、 [Mysql](#Mysql) 、 [Nginx](#Nginx) 等中间件组件，和[Linux](#Linux) 操作系统等等相关的内容做一个系统性的个人能力树结构的总结


再看技术文档的时候,发现了一个牛人的源码解析博客,记录下来,作为未来的目标去走
[田小波-非著名程序员](http://www.tianxiaobo.com)
牛人的博客里面有非常多的源码解析文章,针对刚开始的源码解析入门,有很大的借鉴

## 目录
* [codingThought](#codingThought)
  * [功能设计](#功能设计)
  * [数据库设计](#数据库设计)
  * [接口设计](#接口设计)
* [Java](#coffee-Java)
  * [基础](#基础)  
* [设计模式](#设计模式)
* [数据结构与算法](#数据结构与算法)
  * [数据结构](#数据结构)
  * [算法](#算法)
* [中间件技术](#中间件技术)
  * [Redis](#Redis)
  * [Mysql](#Mysql)
  * [Nginx](#Nginx)
* [操作系统](#操作系统)
  * [Linux](#Linux)
* [计算机网络与数据通信](#计算机网络与数据通信)
* [杂谈](#杂谈)
* [挑战架构](#挑战架构)
  * [多活技术](#多活技术)

## codingThought

### 功能设计  

### 数据库设计  

### 接口设计  
关于 RESTful 接口设计相关资料,发现另外有一个小伙伴的仓库中有很多不错的资料,分享一下
- [restful-api-design-references](https://github.com/aisuhua/restful-api-design-references)

下面的是自己整理了一下的相关资料

* [RESTful 接口设计规约](https://github.com/hello8817/codingThought/blob/master/codingThought/接口设计/RESTful接口设计规约.md)

## :coffee: Java

### 基础

## 设计模式

## 数据结构与算法

### 数据结构

### 算法

- bloom过滤器
  缺点：有一定的误识别率，删除困难
  优点：空间效率和查询时间都比一般的算法要好的多，另外如果判断出不在集合中，那么就一定不在集合中

## 前端技术
作为后端，经常与前端进行联调工作，因此也需要了解一些前端知识，这样才能更好的进行问题定位于解决

### VUE

### Cordova

### dom


## 中间件技术
- [ElasticSearch](https://github.com/hello8817/codingThought/blob/master/中间件技术/ElasticSearch.md)

### Redis  

### Mysql  

### Nginx   

## 操作系统  
### Linux  

## 计算机网络与数据通信  
网络相关技术与协议解析内容，也可以包括一些网络抓包工具使用说明文档等等

## 杂谈
杂谈模块主要记录一些工作中的经验与一些非技术方面的感悟随笔
- [技术成长](https://github.com/hello8817/codingThought/blob/master/杂谈/技术成长.md)
- [面试小结](https://github.com/hello8817/codingThought/blob/master/杂谈/面试小结.md)

## 挑战架构
这个模块中主要记录系统架构方面的相关内容

- [架构的演进路程](https://www.cnblogs.com/zhy-1992/p/9233789.html)

### 多活技术

* [多活技术探讨](https://github.com/hello8817/codingThought/blob/master/挑战架构/多活技术/多活技术探讨.md)

### 高可用
- [websocket与dubbo](https://github.com/hello8817/codingThought/blob/master/挑战架构/高可用/websocket与dubbo.md)
- [分布式锁](https://github.com/hello8817/codingThought/blob/master/挑战架构/高可用/分布式锁.md)
- [高可用探索](https://github.com/hello8817/codingThought/blob/master/挑战架构/高可用/高可用探索.md)

### 代码实现目录
　　上述目录均为探索过程中不断补充的知识体系结构,但是知识体系结构脱离代码实现,给人的印象不深刻,因此对应相关的知识体系中,如果可以进行代码实现以及模拟,是最好不过的了,列举一下自己目前想到的一些计划来进行实现
- **短期小目标**  
  - [ ] 多线程的代码实现
  - [ ] 反射原理解析相关的代码实现
  - [ ] 动态代理相关的代码实现
  - [ ] 设计模式相关的代码实现

- **长期的大目标**
  - [ ] 自己动手编写 Spring 框架
  - [ ] 自己动手编写 Mybatis 框架
  - [ ] 自己动手编写 netty 框架
  - [ ] 微信小程序(技术能力体现相关)编写实现
- **技术文档沉淀**
  - [ ] 维护个人公众号



## 技术路线学习目录调整 

作为后端研发工程师，重新整理一下整体研发体系的知识，let's go

目前将如下内容放置在 AscendLadder 文件夹中

**初级阶段**

- [ ] **Level_01_JavaSE:** java 基础语法相关知识
  - [ ] Java基础
- [ ] **Level_02_DataBase: **数据库基础知识
  - [ ] 数据库基础
- [ ] **Level_03_FrameWork:** spring && springMVC 等框架知识
  - [ ] 框架基础
- [ ] **Level_04_WEB:** 前端基础知识
  - [ ] 前端基础

**中级阶段**

- [ ] **Level_05设计模式:** 常用设计模式相关知识
- [ ] **Level_06多线程&&高并发:** 多线程与高并发编程
- [ ] **Level_07_JVM:** Java虚拟机的核心原理以及 Java 内存回收机制
- [ ] **Level_08_网络到分布式:** 网络负载以及分布式开发知识
- [ ] **Level_09_NIO&&NETTY:** 网络编程
- [ ] **Level_10_MySQL:** 数据库调优

**高级阶段**

- [ ] **Level_11消息中间件:** 消息中间件知识，rabbitMQ,roketMQ,kafka,etc...
- [ ] **Level_12源码分析:** Spring 框架，SpringMVC 框架，Mybatis 框架，Netty 框架等源码实现原理分析
- [ ] **Level_13_亿级流量系统设计:** 互联网亿级流量项目系统设计

**Extra 技能**

- [ ] **Extra_skills:** git,svn,maven,gradle,jenkins,ansible ... 
- [ ] **Extra_操作系统原理:** 底层实现原理解析
- [ ] **Extra_区块链技术:** 热门技术区块链
- [ ] **Extra_大数据:** BigData
- [ ] **Extra_AI:** 人工智能