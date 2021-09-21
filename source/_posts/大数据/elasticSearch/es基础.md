---
title: es基础
toc: true
date: 2021-08-21 08:51:16
tags: es
url: es-common
---

今天开始学习es，先花费一天的时间了解es的基本，剩下的慢慢去弥补知识点。

<!--more-->

> es的技术主要跟着狂神说学习的，推荐一波。

https://www.bilibili.com/video/BV17a4y1x7zq?p=2&spm_id_from=pageDriver

> ES干什么的

两个字：搜索

# 聊一个人

## 聊聊Doug Cutting

生活中，可能所有人都间接用过他的作品，他是Lucene、Nutch 、Hadoop等项目的发起人。是他，把高深莫测的搜索技术形成产品，贡献给普罗大众；还是他，打造了目前在云计算和大数据领域里如日中天的Hadoop。他是某种意义上的盗火者，他就是Doug Cutting。

**从实习生做起**

1985年，Cutting毕业于美国斯坦福大学。他并不是一开始就决心投身IT行业的，在大学时代的头两年，Cutting学习了诸如物理、地理等常规课程。因为学费的压力，Cutting开始意识到，自己必须学习一些更加实用、有趣的技能。这样，一方面可以帮助自己还清贷款，另一方面，也是为自己未来的生活做打算。因为斯坦福大学座落在IT行业的“圣地”硅谷，所以学习软件对年轻人来说是再自然不过的事情了。

Cutting的第一份工作是在Xerox做实习生，Xerox当时的激光扫描仪上运行着三个不同的操作系统，其中的一个操作系统还没有屏幕保护程序。因此，Cutting就开始为这套系统开发屏幕保护程序。由于这套程序是基于系统底层开发的，所以 其他同事可以给这个程序添加不同的主题。这份工作给了Cutting一定的满足感，也是他最早的“平台”级的作品。

可以说，Xerox对 Cutting后来研究搜索技术起到了决定性的影响，除了短暂的在苏格兰工作的经历外，Cutting事业的起步阶段大部分都是在Xerox度过的，这段 时间让他在搜索技术的知识上有了很大提高。他花了四年的时间搞研发，这四年中，他阅读了大量的论文，同时，自己也发表了很多论文，用Cutting自己的 话说——“我的研究生是在Xerox读的。”

尽管Xerox让Cutting积累了不少技术知识，但他却认为，自己当时搞的这些研究只是纸 上谈兵，没有人试验过这些理论的可实践性。于是，他决定勇敢地迈出这一步，让搜索技术可以为更多人所用。1997年底，Cutting开始以每周两天的时间投入，在家里试着用Java把这个想法变成现实，不久之后，Lucene诞生了。作为第一个提供全文文本搜索的开源函数库，Lucene的伟大自不必多言。

**Hadoop的诞生**

之后，Cutting再接再厉，在 Lucene的基础上将开源的思想继续深化。2004年，Cutting和同为程序员出身的Mike Cafarella决定开发一款可以代替当时的主流搜索产品的开源搜索引擎，这个项目被命名为Nutch。在此之前，Cutting所在的公司 Architext（其主要产品为Excite搜索引擎）因没有顶住互联网经济泡沫的冲击而破产，那时的Cutting正处在Freelancer的生涯 中，所以他希望自己的项目能通过一种低开销的方式来构建网页中的大量算法。幸运的是，Google这时正好发布了一项研究报告，报告中介绍了两款 Google为支持自家的搜索引擎而开发的软件平台。这两个平台一个是GFS（Google File System），用于存储不同设备所产生的海量数据；另一个是MapReduce，它运行在GFS之上，负责分布式大规模数据。基于这两个平台，Cutting最引人瞩目的作品——Hadoop诞生了。谈到Google对他们的“帮助”，Cutting说：“我们开始设想用4~5台电脑来实现 这个项目，但在实际运行中牵涉了大量繁琐的步骤需要靠人工来完成。Google的平台让这些步骤得以自动化，为我们实现整体框架打下了良好的基础。”

说起Google，Cutting也是它成长的见证人之一，这里有一段鲜为人知的故事。早在Cutting供职于Architext期间，有两个年轻人曾去拜访这家公司，并向他们兜售自己的搜索技术，但当时他们的Demo只检索出几百万条网页，Excite的工程师们觉得他们的技术太小儿科，于是就在心里鄙 视一番，把他们给送走了。但故事并未到此结束，这两个年轻人回去之后痛定思痛，决定自己创业。于是，他们开了一家自己的搜索公司，取名为Google。这两个年轻人就是Larry Page和Sergey Brin。在Cutting看来，Google的成功主要取决于，反向排序之后再存储的设计和对自己技术的自信。

**让“开源”影响世界**

出于对时间成本的考虑，在从Architext离职四年后，Cutting决定结束这段Freelancer的生涯，找一家靠谱的公司，进一步完善 Hadoop的性能。他先后面试了几家公司，其中也包括IBM，但IBM似乎对他的早期项目Lucene更感兴趣，至于Hadoop则不置可否。就在此 时，Cutting接受了当时Yahoo!搜索项目负责人Raymie Stata的邀请，于2006年正式加入Yahoo!。在Yahoo!，有一支一百人的团队帮助他完善Hadoop项目，这期间开发工作进行得卓有成效。 不久之后，Yahoo!就宣布，将其旗下的搜索业务的架构迁移到Hadoop上来。两年后，Yahoo!便基于Hadoop启动了第一个应用项目 “webmap”——一个用来计算网页间链接关系的算法。Cutting的时任上司（后为Hortonworks CEO）Eric Baldeschwieler曾说：“在相同的硬件环境下，基于Hadoop的webmap的反应速度是之前系统的33倍。”

虽然 Hadoop的表现惊艳，但在当时并非所有公司都有条件使用，与此同时，用户需求却在日益增加。有些大公司（如银行、电信公司、大型零售商等）只关注于产品，却不想在技术工程和咨询服务上过多投入，它们需要一个可以帮助其解决问题的平台，这就是Cutting后来跳槽到Cloudera的初衷。从某种程度上说，Cloudera就是这么一个为那些在咨询和技术上有需求的公司提供服务的平台。它的客户大多来自于传统行业，希望通过Hadoop来处理之前只能 被直接抛弃的大规模数据。现在，除了这些传统行业之外，Yahoo!、Facebook、eBay、LinkedIn等公司都在使用Hadoop，用 Cutting的话说，他们的团队被“无形之中扩大了”。

目前，Cutting的目标是把Hadoop发展成云计算领域的RedHat。 “我从来没有想过，除了搜索引擎，Hadoop的作用还能在其他方面有所发挥，它今天所受到的关注程度，已超过了我之前的所有想象”。谈到成功，Cutting认为他的成功主要归功于两点，一是对自己工作的热情（Cutting在大学时就开始做Infrastracture类的程序，还用 Lisp为Emacs贡献过代码，他非常喜欢自己的程序被千万人使用的感觉）；二是目标不要定得过大，要踏踏实实，一步一个脚印。

文章参考：https://blog.csdn.net/weixin_34060741/article/details/85610704?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control

# ES的简介

***You know, for search (and analysis)***

![image-20210821141631259](es基础/image-20210821141631259.png)



# 货比三家

# 安装ES

> 声明：JDK1.8 最低要求

下载：https://www.elastic.co/cn/downloads/elasticsearch

![image-20210821144217572](es基础/image-20210821144217572.png)

## 目录结构

```shell
➜  elasticsearch-7.14.0 ll
total 1224
-rw-r--r--@  1 cooper  staff   3.8K  7 30 04:47 LICENSE.txt
-rw-r--r--@  1 cooper  staff   601K  7 30 04:51 NOTICE.txt
-rw-r--r--@  1 cooper  staff   2.6K  7 30 04:47 README.asciidoc
drwxr-xr-x@ 25 cooper  staff   800B  7 30 04:54 bin
drwxr-xr-x@ 11 cooper  staff   352B  8 20 22:06 config
drwxr-xr-x   3 cooper  staff    96B  8 20 22:06 data
drwxr-xr-x@  3 cooper  staff    96B  7 30 04:54 jdk.app
drwxr-xr-x@ 42 cooper  staff   1.3K  7 30 04:54 lib
drwxr-xr-x@ 15 cooper  staff   480B  8 21 00:00 logs
drwxr-xr-x@ 59 cooper  staff   1.8K  7 30 04:55 modules
drwxr-xr-x@  2 cooper  staff    64B  7 30 04:51 plugins
```

### bin

启动脚本、停止脚本

### config

![image-20210821145906126](es基础/image-20210821145906126.png)

### lib

包

### modules

模块

### plugins

插件包

## 启动

```shell
bin/elasticsearch
```

## ES的可视化程序head安装

### 下载地址

https://github.com/mobz/elasticsearch-head

### 安装

```shell
npm install

npm start
```

### 连接es

打开网址localhost:9100,连接es

![image-20210821214839146](es基础/image-20210821214839146.png)

点击连接，第一次连接的时候有跨域问题，需要去es中解决跨域问题

### 解决跨域问题

去es的config目录下修改配置文件，修改elasticsearch.yml

添加配置项

```yaml
http.cors.enabled: true
http.cors.allow-origin: '*'
```

重新启动es

```shell
bin/elasticsearch
```

点击连接，跨域问题解决

# 安装kibana

## kibana是什么

Kibana 是一款免费开源的前端应用程序，其基础是 Elastic Stack，可以为 Elasticsearch 中索引的数据提供搜索和数据可视化功能。尽管人们通常将 Kibana 视作 Elastic Stack（之前称作 ELK Stack，分别表示 Elasticsearch、Logstash 和 Kibana）的制图工具，但也可将 Kibana 作为用户界面来监测和管理 Elastic Stack 集群并确保集群安全性，还可将其作为基于 Elastic Stack 所开发内置解决方案的汇集中心。Elasticsearch 社区于 2013 年开发出了 Kibana，现在 Kibana 已发展成为 Elastic Stack 的窗口，是用户和公司的一个门户。

## Kibana用途

Kibana 与 Elasticsearch 和更广意义上的 Elastic Stack 紧密集成，这一点使其成为支持下列场景的理想之选：

1. 搜索、查看并可视化 Elasticsearch 中所索引的数据，并通过创建柱状图、饼状图、表格、直方图和地图对数据进行分析。仪表板视图能将这些可视化元素集中到一起，然后通过浏览器加以分享，以提供有关海量数据的实时分析视图，为下列用例提供支持：
   1. 日志处理和分析
   2. 基础设施指标和容器监测
   3. 应用程序性能监测 (APM)
   4. 地理空间数据分析和可视化
   5. 安全分析
   6. 业务分析
2. 借助网络界面来监测和管理 Elastic Stack 实例并确保实例的安全。
3. 针对基于 Elastic Stack 开发的内置解决方案（面向可观测性、安全和企业搜索应用程序），将其访问权限集中到一起。

## 下载

https://www.elastic.co/cn/downloads/kibana

![image-20210821221122883](es基础/image-20210821221122883.png)

## 配置汉化

去config目录下，修改kibana.yml

```yml
#i18n.locale: "en"
i18n.locale: "zh-CN"
```

## 启动

```shell
bin/kibana
```





# ES核心概念

> Es是面向文档的,关系型数据库和elasticsearch客观的对比，一切都是json！

| Relation DB | ElasticSearch   |
| ----------- | --------------- |
| 数据库      | 索引（indices） |
| 表          | types           |
| 行          | documents       |
| 字段        | fields          |

物理设计：

elasticSearch在后台把每个索引分成多个分片，每个分片



## 集群

## 节点

## 索引

库

## 类型

7.x 逐渐废弃

## 文档（documents）



## 分片（shard）



## 映射（mapping）

字段类型

# IK分词器

## 什么是IK分词器

分词:即把一段中文或者别的划分成一个个的关键字,我们在搜索时候会把自己的信息进行分词,会把数据库中或者索引库中的数据进行分词,然后进行一个匹配操作,默认的中文分词器是将每个字看成一个词,比如"我爱技术"会被分为"我","爱","技","术",这显然不符合要求,所以我们需要安装中文分词器IK来解决这个问题

IK提供了两个分词算法:ik_smart和ik_max_word

其中ik_smart为最少切分,ik_max_word为最细粒度划分

## 安装

地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

下载完毕，放到elasticSearch下面的插件即可

![image-20210822100440935](es基础/image-20210822100440935.png)

## 重启ik

![image-20210822100658333](es基础/image-20210822100658333.png)

## 实战

最少切分

![image-20210822101652303](es基础/image-20210822101652303.png)

最细粒度切分

![image-20210822101713575](es基础/image-20210822101713575.png)

## 实战2

![image-20210822101846224](es基础/image-20210822101846224.png)

如果想要将姜大合并成一个词，需要==手动配置字典==

## 手动配置ik字典

去ik分词器的config目录下，创建自己的字典

```shell
vim new.dic
```

修改配置文件

```shell
vim IKAnalyzer.cfg.xml
```

![image-20210822102538583](es基础/image-20210822102538583.png)

重启es

# RestFul操作ES

![image-20210822102936615](es基础/image-20210822102936615.png)

## 关于索引的操作

1. 创建（更新）一个索引

   ![image-20210822103736355](es基础/image-20210822103736355.png)

   ![image-20210822103822810](es基础/image-20210822103822810.png)

2. 创建一个索引类型

   ![image-20210822104444923](es基础/image-20210822104444923.png)

   

3. 获取数据、规则

   ![image-20210822104651428](es基础/image-20210822104651428.png)

4. 用POST实现修改

   ![image-20210822105856178](es基础/image-20210822105856178.png)

   ![image-20210822105923608](es基础/image-20210822105923608.png)

5. 删除索引

   ```shell
   DELETE test
   ```

## 关于文档的操作

### 存放数据

![image-20210822155132550](es基础/image-20210822155132550.png)

![image-20210822155208725](es基础/image-20210822155208725.png)

### 获取数据

![image-20210822155311426](es基础/image-20210822155311426.png)

### 更新数据

![image-20210822155358802](es基础/image-20210822155358802.png)

version代表着数据更新的次数

### 简单搜索

![image-20210822160450217](es基础/image-20210822160450217.png)

### 复杂搜索

- 构建复杂查询的json

![image-20210822161414986](es基础/image-20210822161414986.png)
- 指定要查询的字段

  ![image-20210822161756167](es基础/image-20210822161756167.png)

- 排序用sort

  ![image-20210822162003297](es基础/image-20210822162003297.png)

- 分页

  ![image-20210822162138892](es基础/image-20210822162138892.png)

  from：从第几个数据开始

  size：返回几条数据

- 多条件查询

  ![image-20210822162755137](es基础/image-20210822162755137.png)

  must 多条件必须都满足

  ![image-20210822163120425](es基础/image-20210822163120425.png)

should 是or的意思，代表着两个条件满足一个就行

![image-20210822163616279](es基础/image-20210822163616279.png)

must_not 代表着不匹配

### 高亮

![image-20210822165849651](es基础/image-20210822165849651.png)



