---
title: docker安装clickhouse
toc: true
date: 2021-05-21 11:14:07
tags: docker
url: docker-clickhouse
---

今天抽时间记一下docker安装clickhouse的过程，希望下次能用到的时候直接cv，不用走弯路

<!--more-->

安装clickhouse

```shell
 docker run -d --name clickhouse-server-1 --ulimit nofile=262144:262144 yandex/clickhouse-server
```

启动clickhouse

```shell
docker start clickhouse-server-1
```

启用cli  进入交互模式

```shell
docker run -it --rm --link clickhouse-server-1:clickhouse-server yandex/clickhouse-client --host clickhouse-server
```





