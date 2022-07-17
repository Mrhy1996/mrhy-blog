---
title: dubbo的简介与实际开发
toc: true
date: 2022-06-05 11:11:18
tags:
url: dubbo-introduce
---

今天，我们再次来看dubbo3的一些东西，从头开始复习介绍一下吧

<!--more-->

# 1. 什么是服务发现

服务发现，即消费端自动发现服务地址列表的能力，是微服务框架需要具备的关键能力，借助于自动化的服务发现，微服务之间可以在无需感知对端部署位置与 IP 地址的情况下实现通信。

实现服务发现的方式有很多种，Dubbo 提供的是一种 Client-Based 的服务发现机制，通常还需要部署额外的第三方注册中心组件来协调服务发现过程，如常用的 Nacos、Consul、Zookeeper 等，Dubbo 自身也提供了对多种注册中心组件的对接，用户可以灵活选择。

![//imgs/architecture.png](dubbo的简介与实际开发/architecture.png)

# 2. 什么是RPC通信协议

RPC是远程过程调用（Remote Procedure Call）的缩写形式。SAP系统RPC调用的原理其实很简单，有一些类似于三层构架的C/S系统，第三方的客户程序通过接口调用SAP内部的标准或自定义函数，获得函数返回的数据进行处理后显示或打印。

java  rmi 1.1提供

