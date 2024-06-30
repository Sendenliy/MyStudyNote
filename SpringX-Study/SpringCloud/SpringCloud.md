# 微服务

## 引入

```bash
三层架构：MVC
框架：
	Spring IOC+AOP
	SpringBoot	新一代的JavaEE开发标准，自动装配
	
微服务架构 ---> 新架构
    模块化！功能化！
    用户，支付，签到...；
    
    人越来越多：一台服务器解决不了了；我们可以再增加一台服务器， 横向扩展
    假设A服务器占用了98%的资源，B服务器只占用了10%。 --负载均衡
    将原来的整体项目（all in one），分成模块化
    
    用户管理就是一个单独的项目，签到也是一个单独的项目；项目之间需要进行通信？那如何通信？
	用户非常多，签到十分少！ 给用户多一点服务器，给签到少一点服务器！
	
微服务架构问题？
    分布式会遇到的四个核心问题？
    1. 这么多服务，客户端该如何去访问？
    2. 这么多服务，服务之间如何进行通信？
    3. 这么多服务，如何治理呢？
    4. 服务挂了，怎么办？
    
解决方案：
    SpringCloud，是一套生态。就是来解决以上分布式架构的4个问题
    想使用SpringCloud，必须要掌握SpringBoot，因为SpringCloud是基于SpringBoot；
    1.Spring Cloud NetFlix，出来了一套解决方案！一站式解决方案。我们需要的东西它都有。
        api网关，zuul组件
        Feign-->HttpClient-->Http的通信方式，同步并阻塞
        服务注册与发现：Eureka
        熔断机制：Hystrix
        
        2018年年底，NetFlix宣布无限期停止维护，生态不再维护，就会脱节。
        
    2.Apache Dubbo zookeeper，第二套解决方案
        API：没有！要么找第三方组件，要么自己实现
        Dubbo：是一个高性能的基于Java实现的RPC通信框架！ 2.6.X
        服务注册与发现，zookeeper：动物园管理者（Hadoop，Hive）
        没有：借助Hystrix
        
        Dubbo这个并不完善！
        
    3.Spring Cloud Alibaba 一站式解决方案！
    
目前又推出服务网格的概念
    服务网格：下一代微服务标准，Server Mesh
    代表解决方案：istio
    
万变不离其宗，一通百通！
    1.服务路由：API网关
    2.服务通信：HTTP，RPC框架，异步调用
    3.服务注册与发现：高可用
    4.熔断机制：服务降级
    
如果，你们基于这些问题，研发出一套解决方案，也可叫SpringCloud！
为什么要解决这些问题？ 本质：网络是不可靠的！
```

1. 什么是微服务？
2. 微服务之间是如何独立通讯的？
3. SpringCloud 和 Dubbo有哪些区别？
4. SpringBoot和SpringCloud，请你谈谈对他们的理解
5. 什么是服务熔断？什么是服务降级
6. 微服务的优缺点是分别是什么？说下你在项目开发中遇到的坑
7. 你所知道的微服务技术栈有哪些？请列举一二
8. eureka和zookeeper都可以提供服务注册与发现的功能，请说说两个的区别？

## 概述

### 什么是微服务

微服务（Microservice Architecture）是近几年流行的一种架构思想。究竟什么是微服务呢？我们在此引用 ThoughtWorks 公司的首席科学家 Martin Fowler于2014年提出的一段话：

- 原文：https://martinfowler.com/articles/microservices.html
- 汉化：https://www.cnblogs.com/liuning8023/p/4493156.html

就目前而言，对于微服务，业界并没有一个统一的，标准的定义

但通常而言，

- 微服务架构是一种架构模式，或者说是一种架构风格， 
- 它提倡将单一的应用程序划分成一组小的服务，
- 每个服务运行在其独立的自己的进程内，服务之间互相协调，互相配置，为用户提供最终价值。

服务之间采用轻量级的通信机制互相沟通，

- 每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，
- 另外，应尽量避免统一的，集中式的服务管理机制；对具体的一个服务而言，应根据业务上下文，选择合适的语言，工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，
- 可以使用不同的语言来编写服务，也可以使用不同的数据存储；

**我们从技术维度来理解下：**

- 微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，`彻底地去耦合`，
- 每一个微服务提供单个业务功能的服务，一个服务做一件事情，
- 从技术角度看就是一种小而独立的处理过程，类似进程的概念，能够自行单独启动或销毁，拥有自己独立的数据库。  

### 微服务与微服务架构

微服务

- 强调的是服务的大小，他关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，
- 狭义的看，可以看做是IDEA中的一个个微服务工程，或者Moudel

IDEA 工具里面使用Maven开发的一个个独立的小Moudel，它具体是使用SpringBoot开发的一个小模块，专业的事情交给专业的模块来做，一个模块就做着一件事情。强调的是一个个的个体，每个个体完成一个具体的任务或者功能。

微服务架构

- 微服务架构是一种新的架构形式，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调，互相配合，为用户提供最终价值。
- 每个服务运行在其独立的进程中，服务于服务间采用轻量级的通信机制互相协作，每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言，工具对其进行构建。

### 微服务优缺点

优点

- 【单一职责原则】每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求;
- 开发简单，开发效率提高，一个服务可能就是专一的只干一件事;
- 微服务能够被小团队单独开发，这个小团队是2~5人的开发人员组成;
- 微服务是`松耦合`的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的。
- 微服务能使用不同的语言开发。
- 易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如jenkins,Hudson,bamboo
- 微服务易于被一个开发人员理解，修改和维护，这样小团队能够更关注自己的工作成果。无需通过合作才能体现价值。
- 微服务允许你利用融合最新技术。
- `微服务只是业务逻辑的代码`，不会和HTML , CSS 或其他界面混合
- `每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一数据库`

缺点:

- 开发人员要处理分布式系统的复杂性
- 多服务运维难度，随着服务的增加，运维的压力也在增大
- 系统部署依赖
- 服务间通信成本·数据一致性·系统集成测试
- 性能监控......

### 微服务技术栈

| 微服务条目                               | 落地技术                                                     |
| ---------------------------------------- | ------------------------------------------------------------ |
| 服务开发                                 | SpringBoot,Spring,SpringMVC                                  |
| 服务配置与管理                           | Netflix公司的Archaius、阿里的Diamond等                       |
| 服务注册与发现                           | Eureka、Consul、Zookeeper等                                  |
| 服务调用                                 | Rest、RPC、gRPC                                              |
| 服务熔断器                               | Hystrix、Envoy等                                             |
| 负载均衡                                 | Ribbon、Nginx等                                              |
| 服务接口调用（客户端调用服务的简化工具） | Feign等                                                      |
| 消息队列                                 | Kafka、RabbitMQ、ActiveMQ等                                  |
| 服务配置中心管理                         | SpringCloudConfig、Chef等                                    |
| 服务路由（API网关）                      | Zuul等                                                       |
| 服务监控                                 | Zabbix、Nagios、Metrics、Specatator等                        |
| 全链路追踪                               | Zipkin、Brave、Dapper等                                      |
| 服务部署                                 | Docker、OpenStack、Kubernetes等                              |
| 数据流操作开发包                         | SpringCloud Stream(封装与Redis，Rabbit，Kafka等发送接收消息) |
| 事件消息总线                             | SpringCloud Bus                                              |

Spring Cloud Alibaba  

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609221001486-878474857.png" alt="image-20240609221001643" style="zoom:40%;" />

### 为什么选择SpringCloud作为微服务架构  

1. 选型依据

   - 整体解决方案和框架成熟度
   - 社区热度
   - 可维护性
   - 学习曲线

2. 当前各大IT公司用的微服务架构有哪些？

   - 阿里：dubbo+HFS
   - 京东：JSF
   - 新浪：Motan
   - 当当网 DubboX .......

3. 各微服务框架对比

   | 功能点/服务框架 | Netflix/SpringCloud                                          | Motan                                                        | gRPC    | Thrift   | Dubbo/DubboX                        |
   | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------- | -------- | ----------------------------------- |
   | 功能定位        | 完整的微服务框架                                             | RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册/发现 | RPC框架 | RPC框架  | 服务框架                            |
   | 支持Rest        | 是，Ribbon支持多种可插拔的序列化选择                         | 否                                                           | 否      | 否       | 否                                  |
   | 支持RPC         | 否                                                           | 是（Hession2）                                               | 是      | 是       | 是                                  |
   | 支持多语言      | 是（Rest形式）？                                             | 否                                                           | 是      | 是       | 否                                  |
   | 负载均衡        | 是（服务端zuul+客户端Ribbon），zuul-服务，动态路由，云端负载均衡Eureka（针对中间层服务器） | 是（客户端）                                                 | 否      | 否       | 是（客户端）                        |
   | 配置服务        | Netfix Archaius，Spring Cloud Config Server集中配置          | 是（zookeeper提供）                                          | 否      | 否       | 否                                  |
   | 服务调用链监控  | 是（zuul），zuul提供边缘服务，API网关                        | 否                                                           | 否      | 否       | 否                                  |
   | 高可用/容错     | 是（服务端Hystrix+客户端Ribbon）                             | 是（客户端）                                                 | 否      | 否       | 是（客户端）                        |
   | 典型应用案例    | Netflix                                                      | Sina                                                         | Google  | Facebook |                                     |
   | 社区活跃程度    | 高                                                           | 一般                                                         | 高      | 一般     | 2017年后重新开始维护，之前中断了5年 |
   | 学习难度        | 中断                                                         | 低                                                           | 高      | 高       | 低                                  |
   | 文档丰富程度    | 高                                                           | 一般                                                         | 一般    | 一般     | 高                                  |
   | 其他            | Spring Cloud Bus为我们的应用程序带来了更多管理端点 | 支持降级 | Netflix内部在开发集成 gR PC | IDL定义 | 实践的公司比较多 |

# SpringCloud入门

## SpringCloud是什么

- Spring官网:https://spring.io/
- 中文网：https://dev-cloud.gitcode.host/spring/guide/spring-cloud/documentation-overview.html

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610104300509-446474773.png" alt="image-20240610104300928" style="zoom: 33%;" />

SpringCloud, 基于SpringBoot提供了一套微服务解决方案，包括：服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于NetFlix的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。

SpringCloud利用SpringBoot的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式系统的一些工具，**包括配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等**，他们都可以用SpringBoot的开发风格做到一键启动和部署。

SpringBoot并没有重复造轮子，它只是将目前各家公司开发的比较成熟，经得起实际考研的服务框架组合起来，通过SpringBoot风格进行再封装，屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包。

SpringCloud 是 分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。

## SpringCloud和SpringBoot关系

- SpringBoot专注于快速方便的`开发构建`单个个体微服务。jar

- SpringCloud是关注全局的微服务`协调整理治理框架`，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等集成服务。


- SpringBoot可以离开SpringCloud独立使用，开发项目；但是SpringCloud离不开SpringBoot，属于依赖关系


- SpringBoot专注于快速、方便的开发单个个体微服务；SpringCloud关注全局的服务治理框架


## Dubbo 和 SpringCloud 对比

### 传统的互联网架构

分布式+服务治理Dubbo：目前成熟的互联网架构，应用服务化拆分 + 消息中间件

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610110858720-380961324.png" alt="image-20240610110859071" style="zoom:50%;" />

### 对比

|              | Dubbo         | Spring                           |
| ------------ | ------------- | -------------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netfilx Eureka      |
| 服务调用方式 | RPC           | REST API  【基于http的Rest方式】 |
| 服务监控     | Dubbo-monitor | Spring Boot Admin                |
| 断路器       | 不完善        | Spring Cloud Netflix Hystrix     |
| 服务网关     | 无            | Spring Cloud Netflix Zuul        |
| 分布式配置   | 无            | Spring Cloud Config              |
| 服务跟踪     | 无            | Spring Cloud Sleuth              |
| 消息总线     | 无            | Spring Cloud Bus                 |
| 数据流       | 无            | Spring Cloud Stream              |
| 批量任务     | 无            | Spring Cloud Task                |

**最大区别: SpringCloud抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式。**

严格来说，这两种方式各有优劣。

- 虽然从一定程度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题。
- 而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸http契约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更加合适。

**品牌机与组装机的区别**

- 很明显，Spring Cloud的功能比DUBBO更加强大，涵盖面更广，而且作为Spring的拳头项目，它也能够与Spring Framework、Spring Boot、Spring Data、Spring Batch等其他Spring项目完美融合，这些对于微服务而言是至关重要的。
- 使用Dubbo构建的微服务架构就像组装电脑，各环节我们的选择自由度很高，但是最终结果很有可能因为一条内存质量不行就点不亮了，总是让人不怎么放心，但是如果你是一名高手，那这些都不是问题;
- 而Spring Cloud就像品牌机，在Spring Source的整合下，做了大量的兼容性测试，保证了机器拥有更高的稳定性，但是如果要在使用非原装组件外的东西，就需要对其基础有足够的了解。

**社区支持与更新力度**

- SpringCloud：https://github.com/spring-cloud
- Dubbo：https://github.com/dubbo

最为重要的是，DUBBO停止了5年左右的更新，虽然2017.7重启了。对于技术发展的新需求，需要由开发者自行拓展升级(比如当当网弄出了DubboX)，这对于很多想要采用微服务架构的中小软件组织，显然是不太合适的，中小公司没有这么强大的技术能力去修改Dubbo源码+周边的一整套解决方案，并不是每一个公司都有阿里的大牛+真实的线上生产环境测试过。

### **总结**

曾风靡国内的开源RPC服务框架Dubbo在重启维护后，令许多用户为之雀跃，但同时，也迎来了一些质疑的声音。互联网技术发展迅速，Dubbo是否还能跟上时代?Dubbo与Spring Cloud相比又有何优势和差异?是否会有相关举措保证Dubbo的后续更新频率?

人物:Dubbo重启维护开发的刘军，主要负责人之一。刘军，阿里巴巴中间件高级研发工程师，主导了Dubbo重启维护以后的几个发版计划，专注于高性能RPC框架和微服务相关领域。曾负责网易考拉RPC框架的研发及指导在内部使用，参与了服务治理平台、分布式跟踪系统、分布式一致性框架等从无到有的设计与开发过程。

**解决的问题域不一样:**

- Dubbo的定位是一款RPC框架，
- Spring Cloud的目标是微服务架构下的一站式解决方案

## SpringCloud的作用

- Distributed/versioned configuration （分布式/版本控制配置） 
- Service registration and discovery（服务注册与发现） 
- Routing（路由）
- Service-to-service calls（服务到服务的调用） 
- Load balancing（负载均衡配置）
- Circuit Breakers（断路器）
- Distributed messaging（分布式消息管理）
- Short lived microservices (tasks)
- Consumer-driven and producer-driven contract testing

## SpringCloud下载

官网：http://projects.spring.io/spring-cloud/

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610112347678-1128057881.png" alt="image-20240610112348169" style="zoom:33%;" /><img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610112550950-1905435879.png" alt="image-20240610112552025" style="zoom:33%;" />

- Spring Cloud是一个由众多独立子项目组成的大型综合项目，每个子项目有不同的发行节奏，都维护着自己的发布版本号。Spring Cloud通过一个资源清单BOM（Bill of Materials）来管理每个版本的子项目清单。为避免与子项目的发布号混淆，所以没有采用版本号的方式，而是通过命名的方式。
- 这些版本名称的命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Release版本：Brixton，然后是Camden、Dalston、Edgware，Finchley
- 不建议使用快照SANPSHOT版本，使用GA通用稳定版本

参考：

- Netfilx：https://springcloud.cc/spring-cloud-netflix.html
- spring-cloud中文API文档：https://springcloud.cc/spring-cloud-dalston.html
- 中文网： https://springcloud.cc
- 官网：https://spring.io/projects/spring-cloud#overview

# Rest微服务构建

## 环境搭建

我们会使用一个Dept部门模块做一个微服务通用案例。Consumer消费者（Client）通过REST调用Provider提供者（Server）提供的服务

Maven的分包分模块架构复习

```bash
一个简单的Maven模块结构是这样的：
    
-- app-parent：一个父项目（app-parent）聚合很多子项目（app-util，app-dao，appweb...）
    -- pom.xml
    ||
    -- app-core
    ||----pom.xml
    | |
    -- app-web
    ||----pom.xml
    ......
```

一个父工程带着多个子Module子模块

SpringCloud父工程（Project）下初次带着3个子模块（Module）

- springcloud-api 【封装的整体entity / 接口 / 公共配置等】
- springcloud-provider-dept-8001【服务提供者】
- springcloud-consumer-dept-80【服务消费者】

一般步骤：

1. 导入依赖
2. 编写配置文件
3. 开启这个功能@Enablexxx
4. 配置类

## 创建父工程

新建父工程Maven项目 springcloud

- 切记Packageing是pom模式
- 主要是定义POM文件，将后续各个子模块公用的jar包等统一提取出来，类似一个抽象父类

pom.xml

```xml
<!--打包方式 pom-->
<packaging>pom</packaging>

<!--版本号-->
<properties>
    <maven.compiler.source>22</maven.compiler.source>
    <maven.compiler.target>22</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <junit.version>4.13.2</junit.version>
    <lombok.version>1.18.32</lombok.version>
    <log4j.version>2.23.1</log4j.version>
</properties>

<!--依赖管理-->
<dependencyManagement>
    <!--依赖-->
    <dependencies>
        <!--SpringCloud-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--SpringBoot-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>3.2.5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--数据库-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.22</version>
        </dependency>
        <!--MyBatis的SpringBoot启动器-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>3.0.3</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <!--logback-->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.4.14</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 创建api公共模块

新建springcloud-api模块，普通Maven项目。可以观察发现，在父工程中多了一个Modules

1. 编写springcloud-api 的 pom.xml

   ```xml
   <!--当前Module自己需要的依赖，如果父依赖已经配置了版本，这里就不用写了-->
   <dependencies>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
       </dependency>
   </dependencies>
   ```

2. 创建部门数据库脚本，数据库名：db01

   ```mysql
   create database db01;
   use db01;
   CREATE TABLE dept(
                        deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
                        dname VARCHAR(60),
                        db_source VARCHAR(60)
   );
   INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
   SELECT * FROM dept;
   ```

3. 编写实体类，注意：实体类都序列化！

   ```java
   @Data
   @NoArgsConstructor
   @Accessors(chain = true) //链式写法
   public class Dept implements Serializable {//实体类必须实现序列化
       private Long deptno;    //主键
       private String dname;
   
       //这个数据存在哪个数据库 微服务：一个服务对应一个数据库，同一个信息可以存在不同的数据库
       private String db_ource;
   
       //有参构造
       public Dept(String dname) {
           this.dname = dname;
       }
   }
   ```

## 创建provider模块

新建springcloud-provider-dept-8001模块

1. 编辑pom.xml

   ```xml
   <dependencies>
       <!--需要拿到实体类，所以要配置api module-->
       <dependency>
           <groupId>com.sunm</groupId>
           <artifactId>springcloud-api</artifactId>
           <version>1.0-SNAPSHOT</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
       </dependency>
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
       </dependency>
       <dependency>
           <groupId>ch.qos.logback</groupId>
           <artifactId>logback-core</artifactId>
       </dependency>
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
           <version>1.2.17</version>
       </dependency>
       <!--Mybatis-->
       <dependency>
           <groupId>org.mybatis.spring.boot</groupId>
           <artifactId>mybatis-spring-boot-starter</artifactId>
       </dependency>
       <!--test-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
       </dependency>
       <!--spring-boot-starter-web-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <!--jetty: 应用服务器，和Tomcat差不多-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-jetty</artifactId>
       </dependency>
       <!--热部署工具-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
       </dependency>
   </dependencies>
   ```

2. 编辑 application.yml

   ```yaml
   server:
     port: 8081
   #mybatis配置
   mybatis:
     type-aliases-package: com.sunm.springcloud.pojo
     #config-location: classpath:mybatis/mybatis-config.xml
     mapper-locations: classpath:mybatis/mapper/*.xml
     configuration:
       map-underscore-to-camel-case: true
   #spring配置
   spring:
     application:
       name: Spring-Cloud-Provider-Dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.jdbc.Driver
       url: jdbc:mysql://localhost:3306/db01?userUnicode=true&characterEncoding=utf-8
       password: root
       username: root
       dbcp2:
         min-idle: 5 #数据库连接池的最小维持连接数
         initial-size: 5 #初始化连接数
         max-total: 5 #最大连接数
         max-wait-millis: 200 #等待连接获取的最大超时时间
   ```

3. 根据配置新建mybatis-config.xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <settings>
           <!--开启二级缓存-->
           <setting name="cacheEnabled" value="true"/>
       </settings>
   </configuration>
   ```

4. DAO层

   编写部门的dao接口

   ```java
   @Mapper
   @Repository
   public interface DeptMapper {
       public boolean addDept(Dept dept);
   
       public Dept getDeptById(Long id);
   
       public List<Dept> queryAll();
   }
   ```

   接口对应的Mapper.xml文件 mybatis\mapper\DeptMapper.xml

   ```xml
   <insert id="addDept">
       insert into dept(dname, db_source) values (#{dname}, DATABASE());
   </insert>
   <select id="getDeptById" resultType="Dept">
       select * from dept where deptno = #{id}
   </select>
   <select id="queryAll" resultType="Dept">
       select * from dept;
   </select>
   ```

5. Service层

   创建Service服务层接口

   ```java
   public interface DeptService {
       public boolean addDept(Dept dept);
   
       public Dept getDeptById(Long id);
   
       public List<Dept> queryAll();
   }
   ```

   ServiceImpl实现类

   ```java
   @Service
   public class DeptServiceImpl implements DeptService {
       @Autowired
       private DeptMapper deptMapper;
   
       @Override
       public boolean addDept(Dept dept) {
           return deptMapper.addDept(dept);
       }
   
       @Override
       public Dept getDeptById(Long id) {
           return deptMapper.getDeptById(id);
       }
   
       @Override
       public List<Dept> queryAll() {
           return deptMapper.queryAll();
       }
   }
   ```

6. DeptController：提供REST服务

   ```java
   //提供Restful服务
   @RestController
   public class DeptController {
       @Autowired
       private DeptService deptService;
   
       @PostMapping("/dept/add")
       public boolean addDept(@Requestbody Dept dept) {
           return deptService.addDept(dept);
       }
   
       @GetMapping("/dept/get/{id}")
       public Dept get(@PathVariable Long id) {
           return deptService.getDeptById(id);
       }
   
       @PostMapping("/dept/list")
       public List<Dept> addDept() {
           return deptService.queryAll();
       }
   }
   ```

7. 编写DeptProvider的主启动类

   ```java
   //启动类
   @SpringBootApplication
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class, args);
       }
   }
   ```

启动测试，注意编写细节：

## 创建consumer模块

新建springcloud-consumer-dept-80模块

1. 编辑pom.xml

   ```xml
   <dependencies>
       <!--实体类+web+热部署-->
       <dependency>
           <groupId>com.sunm</groupId>
           <artifactId>springcloud-api</artifactId>
           <version>1.0-SNAPSHOT</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
       </dependency>
   </dependencies>
   ```

2. application.yml 配置文件

   ```yaml
   server:
     port: 80
   ```

3. 新建一个ConfigBean包注入 RestTemplate！

   ```java
   @Configuration
   public class ConfigBean {//spring : application.xml
       @Bean
       public RestTemplate getRestTemplate() {
           return new RestTemplate();
       }
   }
   ```

4. 创建Controller包，编写DeptConsumerController类

   RestTemplate : 提供多种便捷访问远程http服务的方法，简单的Restful服务模版

   ```java
   @Controller
   public class DeptConsumerController {
       //消费者：没有Service层
       //RestFul  RestTemplate
   
       @Autowired
       private RestTemplate restTemplate;  //提供多种便捷访问远程http服务的方法，简单的Restful服务模版
   
       private static final String REST_URL_PREFIX = "http://localhost:8001";
   
       //postForObject(URI url, @Nullable Object request, Class<T> responseType)
       @RequestMapping("/consumer/dept/add")
       public Boolean addDept(Dept dept) {
           return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
       }
   
       @RequestMapping("/consumer/dept/get/{id}")
       public Dept get(@PathVariable("id") Long id) {
           return restTemplate.getForObject(REST_URL_PREFIX + "/dept/get/" + id, Dept.class);
       }
   
       @RequestMapping("/consumer/dept/list")
       public List<Dept> list() {
           return restTemplate.getForObject(REST_URL_PREFIX + "/dept/list", List.class);
       }
   
   }
   ```

5. 主启动类

   ```java
   @SpringBootApplication
   public class DeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumer_80.class, args);
       }
   }
   ```

访问测试

- 先启动provider模块
- 再启动consumer模块
- 访问：http://localhost:81/consumer/dept/list

# Eureka服务注册与发现

## 什么是Eureka

Eureka：[juˈriːkə]	。Netflix 在设计Eureka 时，遵循的就是AP原则

- CAP原则又称CAP定理，指的是在一个分布式系统中一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）。这三个要素最多只能同时实现两点，不可能三者兼顾。

Eureka是Netflix的一个子模块，也是核心模块之一。

- Eureka是一个基于REST的服务，用于定位服务，以实现云端中间层服务发现和故障转移。
- 服务注册与发现对于微服务来说是非常重要的，有了服务发现与注册，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了。
- 功能类似于Dubbo的注册中心，比如Zookeeper；

## 原理讲解

Eureka的基本架构：

- SpringCloud 封装了NetFlix公司开发的Eureka模块来实现服务注册和发现。

- Eureka采用了C-S的架构设计，EurekaServer 作为服务注册功能的服务器，他是服务注册中心

  <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610153153333-1865753145.png" alt="image-20240610153154437" style="zoom:33%;" />

- 而系统中的其他微服务。使用Eureka的客户端连接到EurekaServer并维持心跳连接。这样系统的维护人员就可以通过EurekaServer来监控系统中各个微服务是否正常运行，SpringCloud的一些其他模块（比如Zuul）就可以通过EurekaServer来发现系统中的其他微服务，并执行相关的逻辑；

- 和Dubbo架构对比：

  Eureka 三大角色

  - Eureka Server：提供服务的注册与发现。
  - Service Provider：将自身服务注册到Eureka中，从而使消费方能够找到。
  - Service Consumer：服务消费方从Eureka中获取注册服务列表，从而找到消费服务。

  <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609222036723-608120124.png" alt="image-20240609222037393" style="zoom:50%;" /><img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609222041910-106164003.png" alt="image-20240609222042639" style="zoom:50%;" />

- Eureka 包含两个组件：Eureka Server 和 Eureka Client 。

- Eureka Server 提供服务注册服务，各个节点启动后，会在EurekaServer中进行注册。这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

- Eureka Client是一个Java客户端，用于简化EurekaServer的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向EurekaServer发送心跳（默认周期为30秒）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除掉（默认周期为90秒）

## 构建eureka模块

建立springcloud-eureka-7001模块

一般步骤：导入依赖、编写配置文件、开启这个功能@Enablexxx、配置类

1. 编辑pom.xml

   ```xml
   <dependencies>
       <!--spring-cloud-starter-netflix-eureka-server提供注册服务-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
           <version>4.1.2</version>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
       </dependency>
   </dependencies>
   ```

2. application.yml

   ```yaml
   server:
     port: 7001
   
   #Eureka配置
   eureka:
     instance:
       hostname: localhost #Eureka服务端的实例名称
     client:
       register-with-eureka: false #是否向Eureka注册中心注册自己
       fetch-registry: false #false，表示自己为注册中心
       service-url:  #监控页面
         defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
   ```

3. 编写主启动类

   ```java
   @SpringBootApplication
   @EnableEurekaServer	//开启Eureka功能 服务端的主启动类，可以接受别人注册进来
   public class EurekaServer_7001 {
       public static void main(String[] args) {
           SpringApplication.run(EurekaServer_7001.class, args);
       }
   }
   ```

启动，测试访问：http://localhost:7001/

- System Status：系统信息
- DS Replicas：服务器副本
- Instances currently registered with Eureka：已注册的微服务列表
- General Info：一般信息
- Instance Info：实例信息

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610160234327-2032270909.png" alt="image-20240610160235253" style="zoom: 40%;" />

## 构建Eureka-Client

### Provider环境搭建

Service Provider：将 8001 的服务入驻到 7001 的eureka中！

1. 修改8001服务的pom文件，增加eureka的支持！

   ```xml
   <!--Eureka-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-eureka-client</artifactId>
       <version>4.1.2</version>
   </dependency>
   ```

2. yaml 中添加对 eureka 的支持配置

   ```yaml
   #Eureka配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
   ```

3. 8001 的主启动类注解支持

   ```java
   //启动类
   @SpringBootApplication
   @EnableDiscoveryClient 
   //@EnableEurekaClient失效 //在服务启动后自动注册到Eureka中
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class, args);
       }
   }
   ```

   截止目前：服务端也有了，客户端也有了，启动7001，再启动8001，

测试访问：http://localhost:7001/

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610170918804-1673819375.png" alt="image-20240610170919654" style="zoom:40%;" />

### actuator与注册微服务信息完善

1. 修改Eureka的服务状态名称

   主机名称：服务名称修改。在8001的yaml中修改一下配置

   ```yaml
   #Eureka配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
     instance:
       instance-id: springcloud-provider-dept8001 #修改Eureka上的默认描述信息
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610173944676-1017408777.png" alt="image-20240610173945651" style="zoom:33%;" />

   重启，刷新后查看结果！

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610174016797-193246157.png" alt="image-20240610174018486" style="zoom: 33%;" />

2. 修改IP提示信息

   访问信息有IP信息提示，yaml中在增加一个配置

   ```yaml
   #eureka配置
   eureka:
       client:
       	service-url:
       		defaultZone: http://localhost:7001/eureka
       instance:
           instance-id: springcloud-provider-dept8001
           prefer-ip-address: true # true访问路径可以显示IP地址
   ```

   info内容构建，现在点击info，出现ERROR页面

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610174157392-1459146553.png" alt="image-20240610174158611" style="zoom:50%;" />

   修改8001的pom文件，新增依赖！

   ```xml
   <!--完善监控信息-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-actuator</artifactId>
       <version>3.3.0</version>
   </dependency>
   ```

   然后回到我们的8001的yaml配置文件中修改增加信息

   ```yaml
   management:
     endpoints:
       web:
         exposure:
           include: info,health
     info:
       env:
         enabled: true
   #info配置
   info:
     app.name: sunm-springcloud
     comany.name: blog.sunm.com
   ```

   重启项目测试：7001、8001.

   访问：http://192.168.10.1:8001/actuator/info

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610175337682-907068353.png" alt="image-20240610175338888" style="zoom:50%;" />

### Eureka的自我保护机制

之前出现的这些红色情况，没出现的，修改一个服务名，故意制造错误！

![image-20240609222332580](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609222331516-385557814.png)

**自我保护机制：好死不如赖活着**

一句话总结：某时刻某一个微服务不可以用了，eureka不会立刻清理，依旧会对该微服务的信息进行保存！

- 默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是当网络分区故障发生时，微服务与Eureka之间无法正常通行，以上行为可能变得非常危险了--因为微服务本身其实是健康的，**此时本不应该注销这个服务**。Eureka通过 **自我保护机制** 来解决这个问题--当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。一旦进入该模式，EurekaServer就会保护服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。当网络故障恢复后，该EurekaServer节点会自动退出自我保护模式。
- 在自我保护模式中，EurekaServer会保护服务注册表中的信息，不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该EurekaServer节点就会自动退出自我保护模式。它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话：好死不如赖活着。
- 综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是**宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留），也不盲目注销任何健康的微服务**。使用自我保护模式，可以让Eureka集群更加的健壮和稳定。
- 在SpringCloud中，可以使用 `eureka.server.enable-self-preservation = false` 禁用自我保护模式 【不推荐关闭自我保护机制】

### 8001服务发现 Discovery

对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息。【对外暴露服务】

1. 修改springcloud-provider-dept-8001工程中的DeptController

   新增一个方法

   ```java
   //获取一些配置信息，得到具体的微服务
   @Autowired
   private EurekaDiscoveryClient discoveryClient;
   
   //注册进来的微服务，获取一些消息
   @GetMapping("/dept/discovery")
   public Object discovery() {
       //获得微服务列表的清单
       List<String> services = discoveryClient.getServices();
       System.out.println("services：" + services);
       //services：[springcloud-provider-dept]
   
       //得到一个具体的微服务信息
       List<ServiceInstance> instances = discoveryClient.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
       for (ServiceInstance instance : instances) {
           System.out.println(instance.getHost() + "\t" +
                   instance.getPort() + "\t" +
                   instance.getUri() + "\t" +
                   instance.getServiceId() + "\t"
           );
       }
       //192.168.10.1	8001	http://192.168.10.1:8001	SPRINGCLOUD-PROVIDER-DEPT
       return this.discoveryClient;
   }
   ```

2. 主启动类增加一个注解

   ```java
   @EnableDiscoveryClient//服务发现
   ```

启动Eureka服务，启动8001提供者。访问测试 http://localhost:8001/dept/discovery

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610185156255-594224324.png" alt="image-20240610185157249" style="zoom:40%;" />

### consumer访问服务

springcloud-consumer-dept-80

1. 修改DeptConsumerController增加一个方法

   ```java
   @GetMapping("/consumer/dept/discovery")
   public Object discovery(){
       return restTemplate.getForObject(REST_URL_PREFIX+"/dept/discovery",Object.class);
   }
   ```

2. 启动 80 项目进行测试！

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610190059629-1878746884.png" alt="image-20240610190100968" style="zoom:50%;" />

## 集群配置

新建工程springcloud-eureka-7002、springcloud-eureka-7003

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610190904059-1701740227.png" alt="image-20240610190905357" style="zoom:33%;" />

1. 按照7001为模板，复制POM文件

2. 复制并修改主启动类

3. 修改映射配置 , windows域名映射。C:\Windows\System32\drivers\etc\hosts

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609222455631-1998605297.png" alt="image-20240609222456535" style="zoom:50%;" />

4. 修改3个EurekaServer的yaml文件属性值，hostname和defaultZone

   ```yaml
   server:
     port: 7001
   #Eureka配置
   eureka:
     instance:
       hostname: eureka7001.com #Eureka服务端的实例名称
     client:
       register-with-eureka: false #是否向Eureka注册中心注册自己
       fetch-registry: false #false，表示自己为注册中心
       service-url: #监控页面
         defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   
   server:
     port: 7002
   #Eureka配置
   eureka:
     instance:
       hostname: eureka7002.com #Eureka服务端的实例名称
     client:
       register-with-eureka: false #是否向Eureka注册中心注册自己
       fetch-registry: false #false，表示自己为注册中心
       service-url: #监控页面
         #单机：defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
         #集群(关联)：
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7003.com:7003/eureka/
         
   server:
     port: 7003
   #Eureka配置
   eureka:
     instance:
       hostname: eureka7003.com #Eureka服务端的实例名称
     client:
       register-with-eureka: false #是否向Eureka注册中心注册自己
       fetch-registry: false #false，表示自己为注册中心
       service-url: #监控页面
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
   ```

5. 将8001微服务发布到1台eureka集群配置中，发现在集群中的其余注册中心也可以看到，但是平时我们保险起见，都发布！

   ```java
   #Eureka配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
       register-with-eureka: true
       fetch-registry: true
     instance:
       instance-id: springcloud-provider-dept8001 #修改Eureka上的默认描述信息
       prefer-ip-address: true # true访问路径可以显示IP地址
   ```

6. 启动集群测试：启动三个注册中心和一个微服务

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610192704705-1657793378.png" alt="image-20240610192706240" style="zoom:33%;" />

   关掉7003

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610192915302-2046410762.png" alt="image-20240610192916347" style="zoom: 33%;" />

## 对比Zookeeper

### 回顾CAP原则

- RDBMS(Mysql、Oracle、sqlServer) ===>ACID


- NoSQL (redis、mongdb) ===>CAP


### ACID是什么

- A (Atomicity)原子性
- C (Consistency)一致性
- I（lsolation)隔离性
- D (Durability)持久性

### CAP是什么

- C (Consistency)强一致性
- A (Availability)可用性
- P(Partition tolerance)分区容错性

CAP的三进二:CA、AP、CP

### CAP理论的核心

- 一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
- 根据CAP原理，将NoSQL数据库分成了满足CA原则，满足CP原则和满足AP原则三大类:
  - CA:单点集群，满足一致性，可用性的系统，通常可扩展性较差
  - CP:满足一致性，分区容错性的系统，通常性能不是特别高
  - AP:满足可用性，分区容错性的系统，通常可能对—致性要求低一些

## 作为服务注册中心，Eureka比Zookeeper好在哪里?

著名的CAP理论指出，一个分布式系统不可能同时满足C（一致性)、A (可用性)、P(容错性)。由于`分区容错性P在分布式系统中是必须要保证的`，因此我们只能在A和C之间进行权衡。

- zookeeper保证的是CP;
- Eureka保证的是AP;

**Zookeeper保证的是CP**

- 当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，此时服务注册功能对一致性的要求要高于可用性。
- 但是zk会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30~120s，且选举期间整个zk集群都是不可用的，这就导致在选举期间注册服务瘫痪。
- 在云部署的环境下，因为网络问题使得zk集群失去master节点是较大概率会发生的事件，虽然服务最终能够恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

**Eureka保证的是AP**

- Eureka看明白了这一点，因此在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。
- 而Eureka的客户端在向某个Eureka注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台Eureka还在，就能保住注册服务的可用性，只不过查到的信息可能不是最新的
- 除此之外，Eureka还有一种自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：
  1. Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务
  2. Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点依然可用）
  3. 当网络稳定时，当前实例新的注册信息会被同步到其他节点中

因此，`Eureka可以很好的应对因网络故障导致部分节点失去联系的情况`，而不会像zookeeper那样使整个注册服务瘫痪。

# Ribbon负载均衡

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240612110225238-1621673214.png" alt="image-20240612110226829" style="zoom:60%;" />

## 概述

### Ribbon是什么

- Spring Cloud Ribbon是基于Netflix Ribbon实现的一套`客户端负载均衡的工具`。
- 简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将NetFlix的中间层服务连接在一起。Ribbon的客户端组件提供一系列完整的配置项如：连接超时、重试等等。简单的说，就是在配置文件中列出LoadBalancer（简称LB：负载均衡）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等等）去连接这些机器。我们也很容易使用Ribbon实现自定义的负载均衡算法！
- Ribbon的github地址 ： https://github.com/NetFlix/ribbon

### Ribbon能干嘛

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240610194730053-36855738.png" alt="image-20240610194731090" style="zoom:33%;" />

- LB，即负载均衡（Load Balance），在微服务或分布式集群中经常用的一种应用。
- 负载均衡简单的说就是：将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）。
- 常见的负载均衡软件有 Nginx，Lvs 等等。Apache+TomCat也可以
- Dubbo、SpringCloud中均给我们提供了负载均衡，**SpringCloud的负载均衡算法可以自定义**

负载均衡简单分类：

- 集中式LB

  即在服务的消费方和提供方之间使用`独立的LB设施`，如之前学习的Nginx：反向代理服务器。由该设施负责把访问请求通过某种策略转发至服务的提供方！

- 进程式LB

  - 将LB逻辑`集成到消费方`，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
  - Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！


## Ribbon配置初步

修改springcloud-consumer-dept-80工程

1. 修改pom.xml，添加Ribbon相关的配置

   ```xml
   <!-- ribbon -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
       <version>2.2.10.RELEASE</version>
   </dependency>
   <!--eureka-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-eureka</artifactId>
       <version>1.4.7.RELEASE</version>
   </dependency>
   ```

2. 修改application.yml ， 追加Eureka的服务注册地址

   ```yaml
   Eureka配置
   eureka:
     client:
       register-with-eureka: false #不注册自己
       fetch-registry: true	#搜索注册中心，要注册为true
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ```

3. 对里面的ConfigBean方法加上注解@LoadBalanced在 获得Rest时加入Ribbon的配置；

   ```java
   //配置负载均衡实现Restful
   @LoadBalanced   //Ribbon
   @Bean
   public RestTemplate getRestTemplate() {
       return new RestTemplate();
   }
   ```

4. 主启动类DeptConsumerRibbon80添加@EnableEurekaClient

   ```java
   @EnableDiscoveryClient
   ```

5. 修改DeptConsumerController客户端访问类，之前的写的地址是写死的，现在需要变化！

   ```java
   //private static final String REST_URL_PREFIX = "http://localhost:8001";
   //应该是一个变量，通过服务名来访问  spring.application.name
   private static final String REST_URL_PREFIX = "Spring-Cloud-Provider-Dept";
   ```

6. 先启动3个Eureka集群后，再启动springcloud-provider-dept-8001并注册进eureka，启动 DeptConsumerRibbon80

   测试：

   - http://localhost/consumer/dept/get/1
   - http://localhost/consumer/dept/list

小结

- Ribbon和Eureka整合后，客户端Consumer可以直接调用服务，而不用再关心IP地址和端口号

## Ribbon负载均衡

### 架构说明：

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240611103519475-521820703.png" alt="image-20240611103520675" style="zoom:40%;" />

Ribbon在工作时分成两步

- 第一步先选择EurekaServer，它优先选择在同一个区域内负载均衡较少的Server。
- 第二步在根据用户指定的策略，在从server去到的服务注册列表中选择一个地址。其中Ribbon提供了多种策略，比如轮询（默认），随机和根据响应时间加权重,,,等等

### 修改：

1. 参考springcloud-provider-dept-8001，新建两份，分别为8002,8003！

   - 拷贝pom依赖
   - resource: yaml的服务名称spring.application.name一致是前提
     - 修改8002/8003各自的YML文件
       - 端口
       - 数据库连接
       - 实例名也需要修改
       - 对外暴露的统一的服务实例名【三个服务名字必须一致！】
   - src：全部复制完毕，修改启动类名称，修改端口号名称！

2. 新建8002/8003数据库，各自微服务分别连接各自的数据库，复制DB1！

   ```mysql
   create database db03;
   use db03;
   
   DROP TABLE IF EXISTS `dept`;
   CREATE TABLE dept(
                        deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
                        dname VARCHAR(60) DEFAULT NULL,
                        db_source VARCHAR(60) DEFAULT NULL
   )ENGINE = INNODB AUTO_INCREMENT = 1 CHARACTER SET = utf8 COMMENT='部门表'
   
   INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
   INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
   ```

### 测试：

- 启动3个Eureka集群配置区，启动3个Dept微服务并都测试通过

  ![image-20240611125957073](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240611125955434-1867213833.png)

- 访问：http://localhost:8001/dept/list、http://localhost:8002/dept/list、http://localhost:8003/dept/list

- 启动springcloud-consumer-dept-ribbon-80

- 客户端通过Ribbon完成负载均衡并访问：http://localhost/consumer/dept/list

  多刷新几次注意观察结果：`轮询`

### 总结：

Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和Eureka结合只是其中的一个实例。

## Ribbon核心组件IRuleI

Rule：根据特定算法从服务列表中选取一个要访问的服务！

### IRule

| 策略名                    | 策略描述                                                     | 实现说明                                                     |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| RandomRule                | **随机**选择一个 server                                      | 在 index 上随机，选择 index 对应位置的 Server                |
| RoundRobinRule            | **轮询**选择 server                                          | 轮询 index，选择 index 对应位置的 server                     |
| ZoneAvoidanceRule         | 复合判断 server 所在**区域的性能**和 server 的**可用性**选择 server | 使用 ZoneAvoidancePredicate 和 AvailabilityPredicate 来判断是否选择某个  server。ZoneAvoidancePredicate 判断判定一个 zone 的运行性能是否可用，剔除不可用的 zone（的所有  server）；AvailabilityPredicate 用于过滤掉连接数过多的 server。 |
| BestAvailableRule         | 选择一个**最小并发请求**的 server                            | 逐个考察 server，如果 server 被 tripped 了则忽略，在选择其中activeRequestsCount 最小的 server |
| AvailabilityFilteringRule | 过滤掉那些因为一直**连接失败**的被标记为 circuit tripped 的后端 server，并过滤掉那些**高并发**的的后端 server（activeConnections 超过配置的阈值） | 使用一个 AvailabilityPredicate 来包含过滤 server 的逻辑，其实就就是检查 status 里记录的各个 server 的运行状态 |
| WeightedResponseTimeRule  | 根据 server 的**响应时间**分配一个 **weight**，响应时间越长，weight 越小，被选中的可能性越低 | 一个后台线程定期的从 status 里面读取评价响应时间，为每个 server 计算一个 weight。weight 的计算也比较简单，responseTime  减去每个 server 自己平均的 responseTime 是 server 的权重。当刚开始运行，没有形成 status 时，使用  RoundRobinRule 策略选择 server。 |
| RetryRule                 | 对选定的负载均衡策略机上**重试**机制                         | 在一个配置时间段内当选择 server 不成功，则一直尝试使用 subRule 的方式选择一个可用的 server |

1. RoundRobinRule【轮询】

2. RandomRule【随机】

3. AvailabilityFilterRule：

   - 会先过滤掉由于多次访问故障而处于断路器跳闸的服务，还有并发的连接数量超过阈值的服务，
   - 然后对剩余的服务列表按照`轮询`策略进行访问

4. WeightedResponseTimeRule：

   - 根据平均响应时间计算所有服务的权重，响应时间越快服务权重越大，被选中的概率越高
   - 刚启动时如果统计信息不足，则使用RoundRobinRule策略，等待统计信息足够，会切换到WeightedResponseTimeRule

5. RetryRule：

   【先按照RoundRobinRule的策略获取服务，如果获取服务失败，则在指定时间内会进行重试，获取可用的服务】

6. BestAvailableRule

   【会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务】

7. ZoneAvoidanceRule：

   【默认规则，复合判断server所在区域的性能和server的可用性选择服务器】

### ReactorServiceInstanceLoadBalancer

- RoundRobinLoadBalancer：轮询

  ```java
  private Response<ServiceInstance> getInstanceResponse(List<ServiceInstance> instances) {
      if (instances.isEmpty()) {
         if (log.isWarnEnabled()) {
            log.warn("No servers available for service: " + serviceId);
         }
         return new EmptyResponse();
      }
  
  
      if (instances.size() == 1) {
         return new DefaultResponse(instances.get(0));
      }
  
  	/*this.position.incrementAndGet() 方法等价于 "++随机数 "。
  	因为incrementAndGet是一个原子操作，保证了每次调用都会得到一个唯一的递增数值，
  	position在构造方法中利用new Random()).nextInt(1000)进行赋值
  	*/
      int pos = this.position.incrementAndGet() & Integer.MAX_VALUE;
  
      // 进行轮询选择
      ServiceInstance instance = instances.get(pos % instances.size());
  
      return new DefaultResponse(instance);
  }
  ```

- RandomLoadBalancer

  ```java
  @SuppressWarnings("rawtypes")
  @Override
  public Mono<Response<ServiceInstance>> choose(Request request) {
      ServiceInstanceListSupplier supplier = serviceInstanceListSupplierProvider
            .getIfAvailable(NoopServiceInstanceListSupplier::new);
      //返回请求的所有实例
      return supplier.get(request).next()
            .map(serviceInstances -> processInstanceResponse(supplier, serviceInstances));
  }
  
  private Response<ServiceInstance> processInstanceResponse(ServiceInstanceListSupplier supplier,
         List<ServiceInstance> serviceInstances) {
      Response<ServiceInstance> serviceInstanceResponse = getInstanceResponse(serviceInstances);
      if (supplier instanceof SelectedInstanceCallback && serviceInstanceResponse.hasServer()) {
         ((SelectedInstanceCallback) supplier).selectedServiceInstance(serviceInstanceResponse.getServer());
      }
      return serviceInstanceResponse;
  }
  
  private Response<ServiceInstance> getInstanceResponse(List<ServiceInstance> instances) {
      if (instances.isEmpty()) {	//实例为空
         if (log.isWarnEnabled()) {
            log.warn("No servers available for service: " + serviceId);
         }
         return new EmptyResponse();
      }
      //生成区间随机数
      int index = ThreadLocalRandom.current().nextInt(instances.size());
  
      ServiceInstance instance = instances.get(index);
  
      return new DefaultResponse(instance);
  }
  ```

### 测试切换为随机策略

- 切换为随机策略实现试试，编写负载均衡配置类

  作为`@LoadBalancerClient`或`@LoadBalancerClients`配置参数传递的类不应使用`@Configuration`进行注释，也`不应在组件扫描范围之外`

  ```java
  public class CustomerLoadBalancerConfigration {
      // 随机的负载均衡策略
      @Bean
      ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(Environment environment,
                                                              LoadBalancerClientFactory loadBalancerClientFactory) {
          String name = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);
          return new RandomLoadBalancer(loadBalancerClientFactory
                  .getLazyProvider(name, ServiceInstanceListSupplier.class),
                  name);
      }
  }
  ```

- 在主启动类上开启全局配置

  ```java
  @LoadBalancerClients(defaultConfiguration = CustomerLoadBalancerConfigration.class)
  
  // 设置局部的负载均衡策略 在@Service上
  @LoadBalancerClient(name = "Spring-Cloud-Provider-Dept",
      configuration = RandomLoadBalancerConfig.class)
  ```

- 重启80服务进行访问测试，查看运行结果！【注意，可能服务长时间不使用会崩】

  访问http://localhost/consumer/dept/list

## 自定义负载均衡器

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240611165826978-1282819015.png" alt="image-20240611165828041" style="zoom:50%;" />

### 创建自定义负载均衡器

1. 创建一个负载均衡类， 并让其实现 ReactorServiceInstanceLoadBalancer 接口；
2. 复制 RandomLoadBalancer 的整个方法体，粘贴到自定义负载均衡类中，并修改构造方法名称
3. 在关键方法 getInstanceResponse 中实现自定义负载均衡策略

```java
public class CustomerLoadBalancer implements ReactorServiceInstanceLoadBalancer {
    private int total = 0;  //被调用的次数
    private int currentIndex = 0;   //当前是谁在提供服务
    private static final Log log = LogFactory.getLog(RandomLoadBalancer.class);

    private final String serviceId;

    private ObjectProvider<ServiceInstanceListSupplier> serviceInstanceListSupplierProvider;

    public CustomerLoadBalancer(ObjectProvider<ServiceInstanceListSupplier> serviceInstanceListSupplierProvider,
                                String serviceId) {
        this.serviceId = serviceId;
        this.serviceInstanceListSupplierProvider = serviceInstanceListSupplierProvider;
    }

    @SuppressWarnings("rawtypes")
    @Override
    public Mono<Response<ServiceInstance>> choose(Request request) {
        ServiceInstanceListSupplier supplier = serviceInstanceListSupplierProvider
                .getIfAvailable(NoopServiceInstanceListSupplier::new);
        return supplier.get(request).next()
                .map(serviceInstances -> processInstanceResponse(supplier, serviceInstances));
    }

    private Response<ServiceInstance> processInstanceResponse(ServiceInstanceListSupplier supplier,
                                                              List<ServiceInstance> serviceInstances) {
        Response<ServiceInstance> serviceInstanceResponse = getInstanceResponse(serviceInstances);
        if (supplier instanceof SelectedInstanceCallback && serviceInstanceResponse.hasServer()) {
            ((SelectedInstanceCallback) supplier).selectedServiceInstance(serviceInstanceResponse.getServer());
        }
        return serviceInstanceResponse;
    }
	//自定义算法
    private Response<ServiceInstance> getInstanceResponse(List<ServiceInstance> instances) {
        if (instances.isEmpty()) {
            if (log.isWarnEnabled()) {
                log.warn("No servers available for service: " + serviceId);
            }
            return new EmptyResponse();
        }
        // int index = ThreadLocalRandom.current().nextInt(instances.size());
        // ServiceInstance instance = instances.get(index);

        /*  每个服务访问五次后，换下一个服务
            total=0，默认=0；如果=5，指向下一个服务节点
            index=0，默认0；如果total=5，index+1
         */
        ServiceInstance instance = null;

        if (total < 5) {
            instance = instances.get(currentIndex);
            total++;
        } else {
            total = 0;
            currentIndex++;
            if (currentIndex >= instances.size()) {
                currentIndex = 0;
            }
            instance = instances.get(currentIndex);
        }
       
        return new DefaultResponse(instance);
    }

}
```

### 封装自定义负载均衡器

```java
public class CustomerLoadBalancerConfigration {//默认是轮询
    @Bean
    ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(Environment environment,
                                                            LoadBalancerClientFactory loadBalancerClientFactory) {
        String name = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);
//        return new RandomLoadBalancer(loadBalancerClientFactory
//                .getLazyProvider(name, ServiceInstanceListSupplier.class),
//                name);
        return new CustomerLoadBalancer(loadBalancerClientFactory
                .getLazyProvider(name, ServiceInstanceListSupplier.class), name);
    }
}
```

### 为服务设置自定义负载均衡策略

```java
@LoadBalancerClients(defaultConfiguration = CustomerLoadBalancerConfigration.class)
```

### Ribbon

1. 主启动类添加@RibbonClient注解

2. 在启动该微服务的时候就能去加载我们自定义的Ribbon配置类，从而使配置类生效，例如:

   注意配置细节

官方文档明确给出了警告：这个自定义配置类不能放在@ComponentScan所扫描的当前包以及子包下，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，也就是说达不到特殊化定制的目的了！

步骤

1. 由于有以上配置细节原因，我们建立一个包 com.kuang.myrule

2. 在这里新建一个自定义规则的Rubbion类

   参考源码修改为我们需求要求的KuangRondomRule.java  

3. 在Myrule中return该类

4. 在主启动类上配置我们自定义的Ribbon  

5. 启动所有项目，访问测试，看看我们编写的随机算法，现在是否生效  

# Feign负载均衡

## 简介

### Feign

feign是声明式的`web service客户端`，它让微服务之间的调用变得更简单了，类似controller调用service。

Spring Cloud集成了Ribbon和Eureka，可在使用Feign时提供负载均衡的http客户端。只需要创建一个接口，然后添加注解即可！

feign，主要是社区，大家都习惯面向接口编程。这个是很多开发人员的规范。调用微服务访问两种方法：

- 微服务名字【ribbon】
- 接口和注解【feign ]

### Feign能干什么

- Feign旨在使编写Java Http客户端变得更容易
- 前面在使用Ribbon + RestTemplate时，利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。
- 所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义，**在Feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它(类似于以前Dao接口上标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解即可。）**即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon时，自动封装服务调用客户端的开发量。

### Feign集成了Ribbon

利用Ribbon维护了springcloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而与Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式的方法，优雅而且简单的实现了服务调用。

`通过Feign实现了负载均衡，利用Ribbon实现远程调用`

## Feign使用步骤

1. 参考springcloud-consumer-dept-ribbon-80，新建springcloud-consumer-dept-feign-80

   - 将springcloud-consumer-dept-80的内容都拷贝到 feign项目中
   - 删除myrule文件夹
   - 修改主启动类名称为 DeptConsumerFeign80

2. springcloud-consumer-dept-feign-80修改pom.xml ， 添加对Feign的支持。

   ```xml
   <!-- openfeign -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-openfeign</artifactId>
       <version>4.1.2</version>
   </dependency>
   ```

3. 修改springcloud-api工程

   - pom.xml添加feign的支持

   - 新建一个Service包，编写接口 DeptClientService，并增加新的注解@FeignClient

     ```java
     @Component
     @FeignClient(value = "Spring-Cloud-Provider-Dept")
     public interface DeptClientService {
         @GetMapping("/dept/get/{id}")
         public Dept queryById(@PathVariable("id") Long id);
     
         @GetMapping("/dept/list")
         public List<Dept> queryAll();
     
         @PostMapping("/dept/add")
         public Boolean addDept(Dept dept);
     }
     ```

4. springcloud-consumer-dept-feign-80工程修改Controller，添加上一步新建的DeptClientService

   ```java
   @RestController
   public class DeptConsumerController {
   
   //    @Autowired
   //    private RestTemplate restTemplate;  //提供多种便捷访问远程http服务的方法，简单的Restful服务模版
   
   //    private static final String REST_URL_PREFIX = "http://Spring-Cloud-Provider-Dept";
       @Autowired
       private DeptClientService deptClientService = null;
   
       @RequestMapping("/consumer/dept/add")
       public Boolean addDept(Dept dept) {
   //        return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
           return this.deptClientService.addDept(dept);
       }
   
       @RequestMapping("/consumer/dept/get/{id}")
       public Dept get(@PathVariable("id") Long id) {
   //        return restTemplate.getForObject(REST_URL_PREFIX + "/dept/get/" + id, Dept.class);
           return this.deptClientService.queryById(id);
       }
   
       @RequestMapping("/consumer/dept/list")
       public List<Dept> list() {
   //        return restTemplate.getForObject(REST_URL_PREFIX + "/dept/list", List.class);
           return this.deptClientService.queryAll();
       }
   
   }
   ```

5. microservicecloud-consumer-dept-feign工程修改主启动类，开启Feign使用！

   ```java
   @SpringBootApplication
   @EnableDiscoveryClient
   //@LoadBalancerClients(defaultConfiguration = CustomerLoadBalancerConfigration.class)
   @EnableFeignClients(basePackages = {"com.sunm.springcloud"})
   public class FeignDeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(FeignDeptConsumer_80.class, args);
       }
   }
   ```

6. 测试

   启动eureka集群；启动8001，8002，8003；启动feign客户端

   测试： http://localhost/consumer/dept/list

   结论：Feign自带负载均衡配置项-轮询

## 小结

Feign通过接口的方法调用Rest服务 ( 之前是Ribbon+RestTemplate )

- 该请求发送给Eureka服务器 （http://Spring-Cloud-Provider-Dept/dept/list）
- 通过Feign直接找到服务接口，由于在进行服务调用的时候融合了Ribbon技术，所以也支持负载均衡作用！
- feign其实不是做负载均衡的，负载均衡是ribbon的功能。feign只是集成了ribbon而已，但是负载均衡的功能还是feign内置的ribbon再做，而不是feign。

feign的作用：替代RestTemplate，性能比较低，但是可以使代码可读性很强。

# Hystrix断路器

## 概述

- 官网：https://github.com/Netflix/Hystrix/wiki


### 分布式系统面临的问题

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免的失败！

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609223722441-355958427.png" alt="image-20240609223722766" style="zoom: 50%;" />

### 服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B 和微服务C又调用其他的微服务，这就是所谓的 “扇出”，如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的 “雪崩效应”。

对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒中内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

我们需要`弃车保帅`

## 什么是Hystrix

Hystrix是一个用于处理分布式系统的延迟和容错的开源库。在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等。Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控(类似熔断保险丝)，**向调用方返回一个服务预期的，可处理的备选响应(FallBack)，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用，**从而避免了故障在分布式系统中的蔓延，乃至雪崩。

能干嘛

- 服务降级
- 服务熔断
- 服务限流
- 接近实时的监控 
- ...….

## 服务熔断

### 是什么

熔断机制是对应雪崩效应的一种微服务链路保护机制。熔断机制的注解是`@HystrixCommand`

当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，**进而熔断该节点微服务的调用，快速返回错误的响应信息**。当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败就会启动熔断机制。

### 测试

参考springcloud-provider-dept-8001

- 新建springcloud-provider-dept-hystrix-8001
- 将之前8001的所有东西拷贝一份

建立熔灾机制：

1. 修改pom，添加Hystrix的依赖

   ```xml
   <!-- hystrix -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       <version>2.2.10.RELEASE</version>
   </dependency>
   ```

2. 修改yml，修改eureka实例的id

   ```yaml
   #Eureka配置
   eureka:
     instance:
       instance-id: springcloud-provider-dept-hystrix-8001 #修改Eureka上的默认描述信息
   ```

3. 修改DeptController

   - @HystrixCommand报异常后如何处理
   - 代码内容

   ```java
   //提供Restful服务
   @RestController
   public class DeptController {
       @Autowired
       private DeptService deptService;
       
       @GetMapping("/dept/get/{id}")
       @HystrixCommand(fallbackMethod = "hystrixGet")	//注解
       public Dept get(@PathVariable("id") Long id) {
           Dept dept = deptService.getDeptById(id);
           if (dept == null) {
               throw new RuntimeException("id=>" + id + ",不存在该用户，或者信息无法找到");
           }
           return dept;
       }
   
       //备选方法
       public Dept hystrixGet(@PathVariable("id") Long id) {
           return new Dept()
                   .setDeptno(id)
                   .setDname("id=>" + id + ",没有对应的信息，null-->Hystrix")
                   .setDbSource("no database in Mysql");
       }
   }
   ```

4. 修改主启动类添加新注解 @EnableCircuitBreaker，修改主启动类的名称为 DeptProviderHystrix8001

   ```java
   //启动类
   @SpringBootApplication
   @EnableDiscoveryClient//服务发现
   @EnableHystrix  //已经包含了@EnableCircuitBreaker断路器
   public class HystrixDeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(HystrixDeptProvider_8001.class, args);
       }
   }
   ```

### resilience4j

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
   </dependency>
   ```

2. 修改配置

   ```yaml
   resilience4j:
     circuitbreaker:
       instances:
         myCircuitBreaker:
           registerHealthIndicator: true
           slidingWindowSize: 1
           minimumNumberOfCalls: 1
           permittedNumberOfCallsInHalfOpenState: 1
           automaticTransitionFromOpenToHalfOpenEnabled: true
           waitDurationInOpenState: 1s
           failureRateThreshold: 5
   ```

3. 增加注解

   ```java
   @GetMapping("/dept/get/{id}")
   @CircuitBreaker(name = "myCircuitBreaker", fallbackMethod = "hystrixGet")
   public Dept get(@PathVariable("id") Long id) {
       Dept dept = deptService.getDeptById(id);
       if (dept == null) {
           throw new RuntimeException("id=>" + id + ",不存在该用户，或者信息无法找到");
       }
       return dept;
   }
   
   //备选方法
   public Dept hystrixGet(Long id, RuntimeException e) {
       return new Dept()
               .setDeptno(id)
               .setDname("id=>" + id + ",没有对应的信息，null-->Hystrix")
               .setDbSource("no database in Mysql");
   }
   ```

测试

1. 启动Eureka集群

2. 启动主启动类 DeptProviderHystrix8001

3. 启动客户端 springcloud-consumer-dept-80

4. 访问 http://localhost/consumer/dept/get/8

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240612112034163-206530758.png" alt="image-20240612112035723" style="zoom:50%;" />

## 服务降级

### 是什么

整体资源快不够了，忍痛将某些服务先关掉，待渡过难关，再开启回来。服务降级处理是在`客户端实现完成的`，与服务端没有关系。

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240609224004223-1729321696.png" alt="image-20240609224004593" style="zoom: 33%;" />



修改springcloud-api工程

1. 根据已经有的DeptClientService接口，新建一个实现了FallbackFactory接口的类DeptClientServiceFallbackFactory

   【注意：这个类上需要@Component注解！！！】

   ```java
   @Component
   public class DeptClientFallBackFactory implements FallbackFactory<DeptClientService> {
       @Override
       public DeptClientService create(Throwable cause) {
           return new DeptClientService() {
               @Override
               public Dept queryById(Long id) {
                   return new Dept()
                           .setDeptno(id)
                           .setDname("id=>" + id + "没有对应的信息，客户端提供服务降级，这个服务现在已经被关闭了")
                           .setDbSource("no database in Mysql");
               }
               @Override
               public List<Dept> queryAll() {
                   return null;
               }
               @Override
               public Boolean addDept(Dept dept) {
                   return false;
               }
           };
       }
   }
   ```

2. 修改DeptClientService接口

   在注解 @FeignClient中添加fallbackFactory属性值

   ```java
   @FeignClient(value = "Spring-Cloud-Provider-Dept", fallbackFactory = DeptClientFallBackFactory.class)
   ```

3. springcloud-api工程 mvn clean install 

4. springcloud-consumer-dept-feign-80工程修改YML 

   ```yaml
   spring:
     cloud:
       openfeign:
         circuitbreaker:
           enabled: true
   ```

### 测试

1. 启动eureka集群，启动 springcloud-provider-dept-hystrix-8001，启动 springcloud-consumer-dept-feign-80
2. 正常访问测试http://localhost/consumer/dept/get/1

故意关闭微服务启动 springcloud-provider-dept-hystrix-8001

1. 客户端自己调用提示：http://localhost/consumer/dept/get/1
2. 此时服务端provider已经down了，但是我们做了服务降级处理，让客户端在服务端不可用时也会获得提示信息而不会挂起耗死服务器

## 小结

- 服务熔断：
  - 生产者，服务提供端
  - 一般是某个`服务故障或者异常`引起，类似现实世界中的 “保险丝” 
  - 当某个异常条件被触发，直接`熔断整个服务`，而不是一直等到此服务超时！

- 服务降级：
  - 消费者，客户端
  - 所谓降级，一般是从`整体负荷`考虑，
  - 当某个服务熔断之后，`服务器将不再被调用`，此时客户端可以自己准备一个本地的FallbackFactory回调，返回一个缺省值。
  - 这样做，虽然服务水平下降，但好歹可用，比直接挂掉要强。

## 服务监控

服务监控 hystrixDashboard

除了隔离依赖服务的调用以外，Hystrix还提供了准实时的调用监控（Hystrix Dashboard），Hystrix会持续地记录所有通过Hystrix发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求，多少成功，多少失败等等。

Netflix通过hystrix-metrics-event-stream项目实现了对以上指标的监控，SpringCloud也提供了Hystrix Dashboard的整合，对监控内容转化成可视化界面！

### 环境搭建

1. 新建工程springcloud-consumer-hystrix-dashboard-9001 

2. Pom.xml

   复制之前80项目的pom文件，新增以下依赖！

   ```xml
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       <version>2.2.10.RELEASE</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix-dashboard -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
       <version>2.2.10.RELEASE</version>
   </dependency>
   ```

3. application.yaml配置

   ```yaml
   server:
     port: 9001
   ```

4. 主启动类改名 + 新注解@EnableHystrixDashboard

   ```java
   @SpringBootApplication
   @EnableHystrixDashboard
   public class DeptConsumerDashboard_9001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumerDashboard_9001.class, args);
       }
   }
   ```

5. 所有的Provider微服务提供类(8001/8002/8003) 都需要监控依赖配置

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-actuator</artifactId>
       <version>3.3.0</version>
   </dependency>
   ```

6. 启动springcloud-consumer-hystrix-dashboard-9001，该微服务监控消费端http://localhost:9001/hystrix

### 测试一

1. 启动eureka集群

2. 启动springcloud-consumer-hystrix-dashboard-9001

3. 在 springcloud-provider-dept-hystrix-8001 启动类中增加一个bean

   ```java
   @Bean
   public ServletRegistrationBean hystrixMetricsStreamServlet() {
       ServletRegistrationBean registration = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
       registration.addUrlMappings("/actuator/hystrix.stream");
       return registration;
   }
   ```

4. 启动 springcloud-provider-dept-hystrix-8001

5. 访问：http://localhost:8001/dept/get/1、 http://localhost:8001/actuator/hystrix.stream 【查看1秒一动的数据流】

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613081952686-1564648188.png" alt="image-20240613081949433" style="zoom:50%;" />

### 监控测试

- 多次刷新 http://localhost:8001/dept/get/1

- 观察监控窗口，就是那个豪猪页面
  - 添加监控地址
  - Delay : 该参数用来控制服务器上轮询监控信息的延迟时间，默认为2000毫秒，可以通过配置该属性来降低客户端的网络和CPU消耗
  - Title ： 该参数对应了头部标题HystrixStream之后的内容，默认会使用具体监控实例URL，可以通过配置该信息来展示更合适的标题。

- 监控结果

  ![image-20240613083530977](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613083532612-1339689638.png)

  - 7色

    <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613083554321-304250056.png" alt="image-20240613083552854" style="zoom:50%;" />

  - 一圈

    - 实心圆：有两种含义，他通过颜色的变化代表了实例的健康程度。它的健康程度从绿色<黄色<橙色<红色 递减
    - 该实心圆除了颜色的变化之外，它的大小也会根据实例的请求流量发生变化，流量越大，该实心圆就越大，所以通过该实心圆的展示，就可以在大量的实例中快速发现故障实例和高压力实例。

  - 一线

    - 曲线：用来记录2分钟内流量的相对变化，可以通过它来观察到流量的上升和下降趋势！整图说明

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613083715317-1095992932.png" alt="image-20240613083713363" style="zoom:40%;" />

# Zuul路由网关

## 概述

Zuul包含了`对请求的路由和过滤`两个最主要的功能。提供：代理 + 路由 + 过滤 三大功能！

- 官网文档：https://github.com/Netflix/zuul
- 其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础，
- 而过滤器功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础。
- Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他微服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。

注意：Zuul服务最终还是会注册进Eureka

## 路由的基本配置

新建Module模块springcloud-zuul-gateway-9527 

1. pom文件，添加Eureka和zuul的配置

   ```xml
   <!--Zuul-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-zuul</artifactId>
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-gateway</artifactId>
   </dependency>
   ```

2. application.yaml配置

   ```yaml
   server:
     port: 9527
   spring:
     application:
       name: spring-cloud-gateway
     cloud:
       gateway:
         routes:
           - id: account
             uri: http://127.0.0.1:9501
             predicates:
               - Path=/account/**
   #Eureka配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
     instance:
       instance-id: spring-cloud-gateway.com #修改Eureka上的默认描述信息
       prefer-ip-address: true # true访问路径可以显示IP地址
   #info配置
   info:
     app.name: sunm-springcloud
     comany.name: blog.sunm.com
   ```

3. hosts修改

   路径：C:\Windows\System32\drivers\etc\hosts

   127.0.0.1 myzuul.com  

4. 主启动类

   ```java
   @SpringBootApplication
   //@EnableZuulProxy
   public class GatewayApplication_9527 {
       public static void main(String[] args) {
           SpringApplication.run(GatewayApplication_9527.class, args);
       }
   }
   ```

启动

- Eureka集群

- 一个服务提供类：springcloud-provider-dept-8001 

- zuul路由

- 访问 ：http://localhost:7001/

  <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613090719511-992445869.png" alt="image-20240613090717700" style="zoom:50%;" />

测试

- 不用路由 ：http://localhost:8001/dept/get/2

- 使用路由 ：http://myzuul.com:9527/springcloud-provider-dept/dept/get/2

  网关 / 微服务名字 / 具体的服务

## 路由访问映射规则

问题：http://myzuul.com:9527/springcloud-provider-dept/dept/get/2 这样去访问的话，就暴露了我们真实微服务的名称！这不是我们需要的！怎么处理呢?

1. 修改：springcloud-zuul-gateway-9527 工程

2. 代理名称

   yml配置修改，增加Zuul路由映射！

   ```yaml
   #Zuul路由映射
   zuul:
       routes:
           mydept.serviceId: springcloud-provider-dept
           mydept.path: /mydept/**
   ```

3. 配置前访问：http://myzuul.com:9527/springcloud-provider-dept/dept/get/2

4. 配置后访问：http://myzuul.com:9527/mydept/dept/get/2

问题，现在访问原路径依旧可以访问！这不是我们所希望的！

**原真实服务名忽略**

```yaml
# Zuul路由映射
zuul:
    ignored-services: springcloud-provider-dept # 不能再使用这个服务名访问；
    #ignored：忽略
    routes:
        mydept.serviceId: springcloud-provider-dept
        mydept.path: /mydept/**
```

- 测试：现在访问http://myzuul.com:9527/springcloud-provider-dept/dept/get/2 就访问不了了

上面的例子中，我们只写了一个，那要是有多个需要隐藏，怎么办呢?

```yaml
# Zuul路由映射
zuul:
    ignored-services: "*" # 通配符 * ， 隐藏全部的！
    #ignored：忽略
    routes:
        mydept.serviceId: springcloud-provider-dept
        mydept.path: /mydept/**
```

**设置统一公共前缀**

```yaml
# Zuul路由映射
zuul:
    prefix: /kuang
	ignored-services: springcloud-provider-dept
    #ignored：忽略
    routes:
        mydept.serviceId: springcloud-provider-dept
        mydept.path: /mydept/**
```

- 访问：http://myzuul.com:9527/kuang/mydept/dept/get/2 ，加上统一的前缀！kuang，否则，就访问不了了

# SpringCloud Config

## 概述

1. 分布式系统面临的配置文件的问题：微

   服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务，由于每个服务都需要必要的配置信息才能运行，所以一套集中式的、动态的配置管理设施是必不可少的。

   SpringCloud提供了ConfigServer来解决这个问题，我们每一个微服务自己带着一个application.yml，那上百的配置文件要修改起来，岂不是要疯。

2. 什么是SpringCloud config分布式配置中心

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613102140126-1326003607.png" alt="image-20240613102138004" style="zoom:50%;" />

   - SpringCloud Config为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为各个不同微服务应用的所有环节提供了一个中心化的外部配置。
   - SpringCloud Config分为服务端和客户端两部分；
     - 服务端也称为：分布式配置中心，它是一个独立的微服务应用，用来连接配置服务器并为客户端提供获取配置信息，加密，解密信息等访问接口。
     - 客户端则是：通过制定的配置中心来管理应用资源以及与业务相关的配置内容， 并在启动的时候从配置中心获取和加载配置信息，配置服务器默认采用git来存储配置信息，这样有助于对环境配置进行版本管理，并且可以通过git客户端工具来方便的管理和访问配置内容。

3. SpringCloud Config分布式配置中心能干什么

   - 集中管理配置文件
   - 不同环境，不同配置，动态化的配置更新，分环境部署，比如/dev /test/ /prod /beta /release通过—三个横杠隔开
   - 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息
   - 当配置发生变动是，服务不需要重启即可感知到配置的变化，并应用新的配置。这个得需要热部署插件才可以做到。
   - 将配置信息以REST借口的形式暴露

SpringCloud Config分布式配置中心与GitHub整合

- 由于SpringCloud Config默认使用Git来存储配置文件（也有其他方式，比如支持SVN和本地文件）

- 但是最推荐的还是Git，而且使用的是http/https访问的形式；

- 下载远程代码到本地

  <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613174825856-190220442.png" alt="image-20240613174823774" style="zoom:50%;" />

1. 编写application.yaml

   ```yaml
   spring:
    profile:
    active: dev
   
   ---
   spring:
    profile: dev
    application:
     name: springcloud-config-dev
   
   ---
   spring:
    profile: test
    application:
     name: springcloud-config-test
   ```

2. 提交代码到本地

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613175909778-670394100.png" alt="image-20240613175908209" style="zoom:50%;" />

3. 推送到远程

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613175937222-854603961.png" alt="image-20240613175935674" style="zoom:50%;" />

## Config-Server服务端配置

新建普通Maven模块springcloud-config-server-3344

1. 添加依赖

   ```xml
   <dependencies>
       <!-- spring-cloud-config-server -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-config-server</artifactId>
           <version>4.1.2</version>
       </dependency>
       <!-- eureka-client -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           <version>4.1.2</version>
       </dependency>
       <!-- web -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   </dependencies>
   ```

2. 编写配置

   ```yaml
   server:
     port: 3344
   spring:
     application:
       name: springcloud-config-server
     #连接远程仓库
     cloud:
       config:
         server:
           git:
             uri: https://gitee.com/Sunflower0312/springcloud-config.git #https地址
   ```

3. 开启功能

   ```java
   @SpringBootApplication
   @EnableConfigServer
   public class Config_server_3344 {
       public static void main(String[] args) {
           SpringApplication.run(Config_server_3344.class, args);
       }
   }
   ```

测试访问：

- http://localhost:3344/

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613181207316-457962287.png" alt="image-20240613181205748" style="zoom:33%;" />

- http://localhost:3344/applicaton-dev.yml或者http://localhost:3344/master/applicaton-dev.yml

  <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240613204611883-736302802.png" alt="image-20240613204609918" style="zoom:50%;" />

  ```bash
  /{application}/{profile}[/{label}]
  /{application}-{profile}.yml
  /{label}/{application}-{profile}.yml
  /{application}-{profile}.properties
  /{label}/{application}-{profile}.properties
  #例如
  curl localhost:8888/foo/development
  curl localhost:8888/foo/development/master
  curl localhost:8888/foo/development,db/master
  curl localhost:8888/foo-development.yml
  curl localhost:8888/foo-db.properties
  curl localhost:8888/master/foo-db.properties
  ```

## Config-Server客户端配置

1. 在远程**新建一个config-client.yml文件**

   ```yml
   #spring配置文件
   spring:
     profiles:
       active: dev
   
   ---
   server:
     port: 8201
   spring:
     profiles: dev
     application:
       name: springcloud-provider-dept
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
   ---
   server:
     port: 8202
   spring:
     profiles: test
     application:
       name: springcloud-provider-dept
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
   ```

2. 新建一个module，**springcloud-config-client-3355**

   1. 导入依赖

      ```xml
      <dependencies>
          <!--config-->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-config</artifactId>
          </dependency>
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-bootstrap</artifactId>
          </dependency>
          <!--actuator 完善监控信息-->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-actuator</artifactId>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
      </dependencies>
      ```

   2. 编写BootStrap.yml

      - bootstrap.yml是系统级别的配置。bootstrap.yml的优先级高
      - 而application.yml是用户级别的配置。

      ```yml
      # 系统级别的配置
      #客户端连接远程项目。先去连接3344中的服务，再通过3344这个服务去连接GitHub上的获取
      spring:
        cloud:
          config:
            name: config-client #需要从git上获取的资源名称，无需后缀
            profile: dev
            label: master
            uri: http://localhost:3344
      #uri + name + '-' + profile + '.yml'
      ```

   3. application.yml

      ```yml
      # 用户级别的配置
      spring:
        application:
          name: springcloud-config-client-3355
      ```

   4. 主启动类

      ```java
      package com.springcloud;
      import org.springframework.boot.SpringApplication;
      import org.springframework.boot.autoconfigure.SpringBootApplication;
      //客户端去拿服务端的东西
      @SpringBootApplication
      public class ConfigClient_3355 {
          public static void main(String[] args) {
              SpringApplication.run(ConfigClient_3355.class,args);
          }
      }
      ```

   5. 控制层

      ```java
      package com.springcloud.controller;
      import org.springframework.beans.factory.annotation.Value;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RestController;
      @RestController
      public class ConfigClientController {
          /*参照gitee上的config-client.yml文件注入进去*/
          @Value("${spring.application.name}")
          private String applicationName;
          @Value("${eureka.client.service-url.defaultZone}")
          private String eurekaServer;
          @Value("${server.port}")
          private String port;
          //如果能打印出来这段话证明成功
          @RequestMapping("/config")
          public String getConfig(){
              return "applicationName:" + applicationName+
                      "eurekaServer:"+ eurekaServer+
                      "port:" + port;
          }
      }
      ```

启动3355这个服务，因为没有配置端口号，默认走的是8201这个分支。

- 访问：`http://localhost:8201/`
- http://localhost:8201/config

**总结：**

- 可以写上多个客户端，把配置统一放在GitHub上，通过server去连接GitHub。
- 有问题可以在GitHub去调节，然后代码还是代码，server还是server，远程的变化对于这两个没有影响。

## ConfigServer实战

整合原来Eureka7001和Provider8001

1. 在远程配置项目上写config-eureka.yml

   ```yml
   spring:
     profiles:
       active: dev #激活dev
   #---
   server:
     port: 7001
   #spring配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-eureka
   # eureka配置
   eureka:
     instance:
       hostname: eureka7001.com  #eureka服务端的实例名称
     client:
       register-with-eureka: false  # 表示是否向eureka注册中心注册自己
       fetch-registry: false #fetch-registry如果为false，则表示自己为注册中心
       service-url:
         # 单机
         #defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/   #监控页面
         # 集群（关联）在7001中挂载7002
         defultZone: http://eureka7002.com:7002/eureka/,http://eureka7003:com:7003/eureka/
   #----
   server:
     port: 7001
   #spring配置
   spring:
     profiles: test
     application:
       name: springcloud-config-eureka
   # eureka配置
   eureka:
     instance:
       hostname: eureka7001.com  #eureka服务端的实例名称
     client:
       register-with-eureka: false  # 表示是否向eureka注册中心注册自己
       fetch-registry: false #fetch-registry如果为false，则表示自己为注册中心
       service-url:
         # 单机
         #defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/   #监控页面
         # 集群（关联）在7001中挂载7002
         defultZone: http://eureka7002.com:7002/eureka/,http://eureka7003:com:7003/eureka/
   
   application.yml
   spring:
     profiles:
       active: dev #激活dev
   #----
   server:
     port: 8001
   #mybatis配置
   mybatis:
     type-aliases-package: com.springcloud.pojo
     config-location: classpath:mybatis/mybatis-config.xml    #mybatis的核心配置
     mapper-locations: classpath:mybatis/mapper/*.xml
   #spring配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.jdbc.Driver      #org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/cloud01?useUnicode=true&characterEncoding=utf-8
       username: root
       password: sa123
   #eureka的配置,defualtZone  服务注册到7001中的yml的路径上
   eureka:
     client:
       service-url:
         defualtZone: http://eureka7001.com :7001/eureka/,http://eureka7002.com :7002/eureka/,http://eureka7003.com :7003/eureka/   #http://localhost:7001/eureka/
     instance:
       instance-id: spring-cloud-provider-dept8001   #修改eureka上的默认描述信息
   #    prefer-ip-address: true   #可以显示服务ip地址，可以定位到远程的IP地址
   #info配置
   info:
     app.name: study-springcloud
     company.name: gd.familychen.com
   #----
   server:
     port: 8001
   #mybatis配置
   mybatis:
     type-aliases-package: com.springcloud.pojo
     config-location: classpath:mybatis/mybatis-config.xml    #mybatis的核心配置
     mapper-locations: classpath:mybatis/mapper/*.xml
   #spring配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.jdbc.Driver      #org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/cloud02?useUnicode=true&characterEncoding=utf-8
       username: root
       password: sa123
   #eureka的配置,defualtZone  服务注册到7001中的yml的路径上
   eureka:
     client:
       service-url:
         defualtZone: http://eureka7001.com :7001/eureka/,http://eureka7002.com :7002/eureka/,http://eureka7003.com :7003/eureka/   #http://localhost:7001/eureka/
     instance:
       instance-id: spring-cloud-provider-dept8001   #修改eureka上的默认描述信息
   #    prefer-ip-address: true   #可以显示服务ip地址，可以定位到远程的IP地址
   #info配置
   info:
     app.name: study-springcloud
     company.name: gd.familychen.com
   ```

2. 创建springcloud-config-eureka-7001。把7001的内容复制过来改

3. 导入config-server jar包

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-eureka-server</artifactId>
           <version>1.4.6.RELEASE</version>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
       </dependency>
       <!--config-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-config</artifactId>
           <version>2.2.7.RELEASE</version>
       </dependency>
   </dependencies>
   ```

4. 主启动类

   ```java
   package com.springcloud;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
   @SpringBootApplication
   @EnableEurekaServer//开启eureka服务  EnableEurekaServer服务端的启动类，可以接受别人注册进来
   public class ConfigEurekaServer_7001 {
       public static void main(String[] args) {
           SpringApplication.run(ConfigEurekaServer_7001.class,args);
       }
   }
   ```

5. 写一个bootstrap.yml 里面写配置要从哪个地方获取

   ```yml
   spring:
     cloud:
       config:
         name: config-eureka
         label: master
         profile: dev
         uri: http://localhost:6666
   ```

6. application.yml

   ```yml
   #配置信息，代表是哪个(客户端)module的配置
   spring:
     application:
       name: springcloud-config-eureka-7001
   ```

启动项目:

- 访问路径：`localhost：3344/master/config-eureka-dev.yml`
- 测试成功后，启动springcloud-config-eureka-7001，访问路径：`localhost:7001`

新建一个module，**springcloud-config-dept-8001**，复制8001修改一下配置。

1. 添加依赖

2. 添加bootstrap.yml

   ```yml
   spring:
     cloud:
       config:
         name: config-dept
         label: master
         profile: dev
         uri: http://localhost:3344
   ```

启动springcloud-config-dept-8001，能放到eureka注册中心证明成功。

- 访问路径：localhost:8001/dept/get/1
- `http://localhost:3344/master/config-dept-dev.yml`

假设在gitee上改一些配置信息，改config-dept.yml文件:

- 下面那个spring配置改成test
- 然后在最上面把数据库cloud01改成cloud03
- 如果查询数据的话就会走cloud03这个数据库的数据