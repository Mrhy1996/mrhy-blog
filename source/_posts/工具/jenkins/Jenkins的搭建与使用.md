---
title: Jenkins的搭建与使用
toc: true
date: 2021-11-26 15:48:59
tags: jenkins
url: jenkins
---

最近老是往服务器部署东西，每次都是scp之类的非常麻烦，于是决定研究一下jenkins，来解决自己的这个难题

<!--more-->

# 1. Jenkins官网

https://www.jenkins.io/

# 2. 下载与安装

https://www.jenkins.io/download/lts/macos/

我的电脑是macos，找到了mac的安装方法，借助了brew，来安装

安装jenkins之前要记得安装openjdk11

> brew install jenkins-lts

启动jenkins

> brew services start jenkins-lts

重启jenkins

> brew services restart jenkins-lts

升级jenkins

> brew upgrade jenkins-lts



sendFile  mmap
