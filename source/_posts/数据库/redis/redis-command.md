---
++title: redis知识点
date: 2020-09-21 19:47:10
tags: redis
---
# redis 的数据类型
1. String字符串（可以为整形）
2. hash （hash散列值 key必须唯一）
3. list（实现队列，元素不唯一，先入先出原则）
4. set （集合）各不相同
5. sort set（有序集合）

## 五大基本数据类型
### String
```bash
########
master:0>set name majiju
"OK"
master:0>get name
"majiju"
master:0>exists name
"1"
master:0>exists mingzi
"0"
master:0>get name
"majiju"
master:0>strlen name
"6"
master:0>append name "，hello"
"14"
master:0>get name
"majiju，hello"

##########
master:0>keys * #获取全部的key值
 1)  "name"
 2)  "fans"
master:0>get fans #获取fans 的值
"0"
master:0>incr fans # 自增+1
"1"
master:0>incr fans
"2"
master:0>incr fans
"3"
master:0>decr fans #自减 -1
"2"
master:0>decr fans
"1"
master:0>decr fans
"0"
master:0>decr fans
"-1"
master:0>decr fans
"-2"
master:0>decr fans
"-3"
master:0>incrby fans 9 #增加9
"6"
master:0>decrby fans 2 # 减2
"4"
###############
# 字符串范围
master:0>getrange name 0 -1
"majiju，hello"
master:0>getrange name 0 3
"maji"
###############
# 替换
master:0>setrange name 1 zhang
"14"
master:0>get name
"mzhang，hello"
###############
setex(set with expire) ##设置过期时间
setnx(set if not exist) ## 不存在时设置

master:0>setex key1 30 "zhanghongqin"
"OK"
master:0>ttl key1
"25"
master:0>ttl key1
"22"
master:0>ttl key1
"21"
master:0>get key1
"zhanghongqin"
master:0>setnx key2 majiju # 设置key2的值，第一次设置成功
"1"
master:0>setnx key2 majiju1 # 设置 key2的值，第二次设置失败
"0"
master:0>get key1
null
master:0>ttl key1
"-2"
#######################
master:0>mset k1 v1 k2 v2 k3 v3 #批量设置key value
"OK"
master:0>keys * 
 1)  "k2"
 2)  "k1"
 3)  "k3"
master:0>mget k1 k2 k3 #批量获取值
 1)  "v1"
 2)  "v2"
 3)  "v3"
master:0>msetnx k1 v2 k4 v4 # 批量setnx 原子性操作
"0"

##############
#对象
master:0>mset user:1:name "majiju" user:1:age 12
"OK"
master:0>mget user:1:name user:1:age
 1)  "majiju"
 2)  "12"


master:0>set user:2 {name:majiju,age:12}
"OK"
master:0>get user:2
"{name:majiju,age:12}"

############
getset（获取前一个值，并设置下一个）

master:0>getset db redis
null
master:0>getset db mongo
"redis"


```
### list（集合）
```bash
# redis中可以把list玩成栈 队列 阻塞队列
# 所有的list的命令都是l或者r开头的
# list中左边为上面的
#############################################
lpush
rpush
master:0>lpush list one 
"1"
master:0>lpush list two
"2"
master:0>lpush list three
"3"
master:0>lrange list 0 -1 #获取list中的
 1)  "three"
 2)  "two"
 3)  "one"
master:0>lrange list 0 1
 1)  "three"
 2)  "two"
master:0>rpush list xx
"4"
master:0>lrange list 0 -1
 1)  "three"
 2)  "two"
 3)  "one"
 4)  "xx"
#############################################
lpop
rpop
master:0>lpop list # 抛出头部
"three"
master:0>rpop list # 抛出尾部
"xx"
master:0>lrange list 0 -1 
 1)  "two"
 2)  "one"
#############################################
lindex（通过下标去查询list中的元素）
master:0>lindex list 0
"two"
master:0>lindex list 1
"one"
master:0>lindex list 2
null
#############################################
llen(查询redis中list的长度)
master:0>flushdb
"OK"
master:0>lpush list one
"1"
master:0>lpush list two
"2"
master:0>lpush list three
"3"
master:0>lpush list four
"4"
master:0>llen list
"4"
#############################################
lrem(移除指定的值)
master:0>lrem list 1 one
"1"
master:0>lrem list 1 two
"1"
master:0>lrem list 1 two
"0"
master:0>lpush list three
"3"
master:0>lrange list 0 -1
 1)  "three"
 2)  "four"
 3)  "three"
master:0>rpush list one
"4"
master:0>lrem list 1 three
"1"
master:0>lrange list 0 -1
 1)  "four"
 2)  "three"
 3)  "one"
master:0>rpush list one
"4"
master:0>lrem list 2 one
"2"
#############################################
ltrim(截取list的长度)
master:0>lpush list one 
"1"
master:0>lpush list one1
"2"
master:0>lpush list one2
"3"
master:0>lpush list one3
"4"
master:0>ltrim list 0 1 # 通过下标截取指定的长度
"OK"
master:0>lrange list 0 -1
 1)  "one3"
 2)  "one2"
##############################################
 rpoplpush(移除列表的最后一个元素，并把它移动到新的列表中)
 master:0>rpush list one
"1"
master:0>rpush list one1
"2"
master:0>rpush list one2
"3"
master:0>rpush list one3
"4"
master:0>rpoplpush list  list1
"one3"
master:0>lrange list 0 -1
 1)  "one"
 2)  "one1"
 3)  "one2"
master:0>lrange list1 0 -1
 1)  "one3"
##############################################
 lset(更新数组中指定位置的值)
 master:0>rpush list one1
"1"
master:0>rpush list one2
"2"
master:0>rpush list one3
"3"
master:0>lrange list  0 -1
 1)  "one1"
 2)  "one2"
 3)  "one3"
master:0>lset list 0 one888 # 替换指定下标的值
"OK"
master:0>lrange list 0 -1
 1)  "one888"
 2)  "one2"
 3)  "one3"
master:0>lset list 88 one888 # 如果不存在就会报错
"ERR index out of range"


##############################################
linsert(在指定的某个值的前面或者后面插入)
master:0>lrange list 0 -1
 1)  "one888"
 2)  "one2"
 3)  "one3"
master:0>linsert list before one2 one1 #在one2的前面插入one1
"4"
master:0>lrange list 0 -1
 1)  "one888"
 2)  "one1"
 3)  "one2"
 4)  "one3"
 master:0>linsert list after one3 one5 # 在one3的后面插入one5
"5"
master:0>lrange list 0 -1
 1)  "one888"
 2)  "one1"
 3)  "one2"
 4)  "one3"
 5)  "one5"
 ##############################################
```
### set（集合）

```bash
set 集合中的值不能重复
set中的命令都是以s开头的
##############################################
master:0>sadd myset 'hello' # set中新增元素
"1"
master:0>sadd myset 'tom'
"1"
master:0>smembers myset #查看set集合中的成员
 1)  "hello"
 2)  "tom"
master:0>sismember myset hello #判断某个值是否是成员中的
"1"
master:0>sismember myset world
"0" 
master:0>scard myset # 获取set集合中元素的个数
"2"
##############################################
srem 移除指定集合中特定的元素
master:0>srem myset aa
"0"
master:0>srem myset hello
"1"
master:0>scard myset
"1"
master:0>smembers myset
 1)  "tom"
##############################################
srandmember (随机获取set集合中某个元素)
master:0>smembers myset
 1)  "zhanghua"
 2)  "lihua"
 3)  "lilei"
 4)  "tom"
master:0>srandmember myset 
"zhanghua"
master:0>srandmember myset 
"lilei"
master:0>srandmember myset 
"tom"
master:0>srandmember myset 
"tom"
master:0>srandmember myset 2 #随机获取set集合中的2个元素
 1)  "tom"
 2)  "lilei"
##############################################
spop(随机弹出集合中的某个元素)
master:0>smembers myset
 1)  "zhanghua"
 2)  "lihua"
 3)  "lilei"
 4)  "tom"
master:0>spop myset
"lilei"
master:0>smembers myset
 1)  "zhanghua"
 2)  "lihua"
 3)  "tom"
##############################################
smove (移动指定的值到另外一个集合中)
master:0>smembers myset
 1)  "zhanghua"
 2)  "lihua"
 3)  "tom"
master:0>smove myset myset2 tom # 移动指定的值到另外的集合中
"1"
master:0>smembers myset
 1)  "zhanghua"
 2)  "lihua"
master:0>smembers myset2
 1)  "tom"
##############################################
微博 b站 的共同关注 
set集合中也可以去做
	- 交集
	- 并集
	- 差集
master:0>sadd key1 a
"1"
master:0>sadd key1 b
"1"
master:0>sadd key1 c
"1"
master:0>sadd key2 c
"1"
master:0>sadd key2 d
"1"
master:0>sdiff key1 key2 # 取a集合与b集合的差集
 1)  "a"
 2)  "b"
master:0>sinter key1 key2 # 取a集合与b集合的交集
 1)  "c"
master:0>sunion key1 key2 # 取a集合与b集合的并集
 1)  "a"
 2)  "c"
 3)  "b"
 4)  "d"
master:0>sunion key1 key3 # key3 不存在 但是不会报错
 1)  "a"
 2)  "c"
 3)  "b"
master:0>sdiff key1 key3
 1)  "a"
 2)  "c"
 3)  "b"
master:0>sinter key1 key3
```

### Hash（哈希）

数据结构类似于 k-（k，v）

所有的hash的命令都是以h开头 本质和String没有太大区别

```bash
########################################################
master:0>hset myhash name majiju # 往hash中set值
"1"
master:0>hset myhash age 12 
"1"
master:0>hget myhash name # 获取hash中某个值
"majiju"
master:0>hget myhash age
"12"
master:0>hgetall myhash # 获取
 1)  "name"
 2)  "majiju"
 3)  "age"
 4)  "12"
master:0>hmset myhash name zhanghongqin age 11
"OK"
master:0>hget myhash name age
"ERR wrong number of arguments for 'hget' command"
master:0>hmget myhash name age
 1)  "zhanghongqin"
 2)  "11"
master:0>hgetall myhash
 1)  "name"
 2)  "zhanghongqin"
 3)  "age"
 4)  "11"
 master:0>hdel myhash name # 删除指定的key
"1"
master:0>hgetall  myhash
 1)  "age"
 2)  "11"
 ########################################################
 hlen 查看有多少键值对
 master:0>hlen myhash
"1"
 ########################################################
 hexists(查看hash中是否存在指定的key)
 master:0>hexists myhash age
"1"
master:0>hexists myhash name
"0"
########################################################
hkeys(查询hash中所有的key)
hvals（查询hash中所有的value）
master:0>hkeys myhash
 1)  "age"
 2)  "name"
master:0>hvals myhash
 1)  "11"
 2)  "majiju"

########################################################
hincrby (hash中的decrby就是后面的步长变成负数)
master:0>hincrby myhash age 1
"12"
master:0>hincrby myhash age 1
"13"
master:0>hincrby myhash age -1
"12"
########################################################
 hsetnx(set if not exist)
master:0>hsetnx myhash grade 2
"1"
master:0>hsetnx myhash grade 2
"0" 
```

Hash 中一般用键值对  比如用户信息 hash更适合对象的存储，string 更适合值的存储

### Zset（有序集合）

在set的基础上增加了一个值 set key1 v1

Zset k1 score v1

```bash
master:0>zadd myset 1 one # 添加值
"1"
master:0>zadd myset 2 two 
"1"
master:0>zadd myset 3 three 4 froue # 添加多个值
"2"
master:0>zrange myset 0 -1 #查找值
 1)  "one"
 2)  "two"
 3)  "three"
 4)  "froue"
########################################################
zrangebyscore(给zset进行排序 -inf 表示负无穷 +inf 表示正无穷)
master:0>zrangebyscore myset -inf +inf # 升序排列
 1)  "one"
 2)  "two"
 3)  "three"
 4)  "froue"
master:0>zrangebyscore myset -inf +inf withscores # 带着key value 
 1)  "one"
 2)  "1"
 3)  "two"
 4)  "2"
 5)  "three"
 6)  "3"
 7)  "froue"
 8)  "4"
 master:0>ZREVRANGE myset 0 -1 # 从大到小排列
 1)  "eight"
 2)  "six"
 3)  "froue"
 4)  "three"
 5)  "two"
 
########################################################
master:0>zrange myset 0 -1 # 查询集合中的范围
 1)  "one"
 2)  "two"
 3)  "three"
 4)  "froue"
master:0>zrem myset one #移除
"1"
master:0>zrange myset 0 -1
 1)  "two"
 2)  "three"
 3)  "froue"
master:0>zcard myset
"3"
########################################################
zcount 查询指定区间的数量
master:0>zcount myset 6 88
"2"
```

案例：

存储班级成绩 

存储工资表排列 

带权重执行

排行榜实现

## 三种特殊数据类型

### [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html)

redis 在3.2版本推出的

朋友的定位、附近的人、打车距离

> geoadd
>
> 规则：不能直接添加南北两极的数据
>
> 将指定的地理空间位置（经度、纬度、名称）添加到指定的`key`中

```bash
master:0>geoadd china:city 116.405285 39.904989 beijing
"1"
master:0>geoadd china:city 121.472644 31.231706 shanghai
"1"
master:0>geoadd china:city 106.504962 29.533155 chongqing
"1"
master:0>geoadd china:city 114.085947 22.547 shenzhen
"1"
master:0>geoadd china:city 120.153576 30.287459 hangzhou 108.948024 34.263161 xian
"2"
```

> geopos(获取指定的经度 纬度)

```bash
master:0>geopos china:city shenzhen hangzhou
 1)    1)   "114.08594459295272827"
  2)   "22.54699993773966327"

 2)    1)   "120.15357345342636108"
  2)   "30.28745790721532671"
```

> Geodist (两个人之间的指定距离) 
>
> - **m** 表示单位为米。
> - **km** 表示单位为千米。
> - **mi** 表示单位为英里。
> - **ft** 表示单位为英尺。
>
> 如果用户没有显式地指定单位参数， 那么 `GEODIST` 默认使用米作为单位。
>
> `GEODIST` 命令在计算距离时会假设地球为完美的球形， 在极限情况下， 这一假设最大会造成 0.5% 的误差。

```bash
master:0>geodist china:city beijing hangzhou # 查看北京到杭州的直线距离
"1122483.2754"
master:0>geodist china:city beijing shanghai km
"1067.5980"
```

> GEORADIUS 命令 - 以给定的经纬度为中心， 找出某一半径内的元素

```bash
master:0>GEORADIUS china:city 110 30 1000 km 获取经度 longtitude 110 纬度 30 附近范围内 半径为1000km以内的城市
 1)  "chongqing"
 2)  "xian"
 3)  "shenzhen"
 4)  "hangzhou"
master:0>GEORADIUS china:city 110 30 1000 km withcoord count 1 #获取指定数量的城市  返回编码
 1)    1)   "chongqing"
  2)      1)    "106.50495976209640503"
   2)    "29.53315530684997015"
master:0>GEORADIUS china:city 110 30 1000 km withdist count 1 #显示到中心的距离
 1)    1)   "chongqing"
  2)   "341.4052"
 
```

> GEORADIUSBYMEMBER  找出位于指定范围内的元素，中心点是由给定的位置元素决定

```bash
master:0>GEORADIUSBYMEMBEr china:city beijing 1000 km # 找出指定范围内的元素
 1)  "beijing"
 2)  "xian"
```

> Redis GEOHASH 命令 - 返回一个或多个位置元素的 Geohash 表示

```bash
# 将二维的经纬度转换为一维的字符串对象
master:0>geohash china:city beijing chongqing
 1)  "wx4g0b7xrt0"
 2)  "wm78p86e170"
```

> Geo 底层是基于zset 进行封装的，所以zset的命令可以使用在geo身上

```bash
master:0>zrange china:city 0 -1
 1)  "chongqing"
 2)  "xian"
 3)  "shenzhen"
 4)  "hangzhou"
 5)  "shanghai"
 6)  "beijing"
master:0>zrem china:city chongqing
"1"
master:0>zrange china:city 0 -1
 1)  "xian"
 2)  "shenzhen"
 3)  "hangzhou"
 4)  "shanghai"
 5)  "beijing"
```

### HyperLogLog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

> ## 什么是基数?

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

```bash
master:0>pfadd mykey a b c d e a # 添加元素
"1"
master:0>pfcount mykey
"5"
master:0>pfadd mykey2 a b e
"1"
master:0>pfmerge mykey3 mykey mykey2 # 合并key1 key2 到mykey3
"OK"
master:0>pfcount mykey3
"5"
```

应用场景：统计网站的登录人数

### BitMap

>  位存储,只有 0 1 两种状态

```bash
master:0>setbit mykey 0 1
"0"
master:0>setbit mykey 1 1
"0"
master:0>setbit mykey 2 1
"0"
master:0>setbit mykey 3 1
"0"
master:0>setbit mykey 4 1
"0"
master:0>setbit mykey 5 1
"0"
master:0>setbit mykey 6 0
"0"
master:0>getbit mykey 3
"1"
master:0>bitcount mykey # 查看为1的数据
"6"
```

# redis 事务

redis事务本质：一组命令的集合（所有的事务都需要序列化）

一次性、顺序性、排他性

```
------队列 set set set ---执行
```

--redis事务没有隔离的概念--

redis事务

- 开启事务（multi）

- 命令入队

- 执行事务（exec）

> 正常执行事务

```bash
master:0>multi #开启事务
"OK"
master:0>set k1 v1
"QUEUED"
master:0>set k2 v2
"QUEUED"
master:0>get k2
"QUEUED"
master:0>exec #执行事务
 1)  "OK"
 2)  "OK"
 3)  "OK"
 4)  "OK"
 5)  "OK"
 6)  "v2"
 7)  "OK"
```

> 取消事务

```bash
master:0>multi
"OK"
master:0>set k1 v1
"QUEUED"
master:0>discard # 取消事务
"OK"
master:0>get k1 
null
```

> 事务错误

```bash
master:0>multi
"OK"
master:0>set key1 aa
"QUEUED"
master:0>incr key1
"QUEUED"
master:0>set key2 v2
"QUEUED"
master:0>get key2
"QUEUED"
master:0>exec
 1)  "OK"
 2)  "OK"
 3)  "OK"
 4)  "ERR value is not an integer or out of range"
 5)  "OK"
 6)  "OK"
 7)  "OK"
 8)  "v2"
 9)  "OK"
```

# 监控

悲观锁

- 很悲观，认为什么时候都会出问题

乐观锁

- 乐观锁 很乐观 认为什么时候都不会出问题，所以不会加锁，更新的时候判断一下版本号
- 获取version
- 更新时比较version

> 正常执行

```bash
master:0>set money 100
"OK"
master:0>set out 0
"OK"
master:0>watch money #监视money对象
"OK"
master:0>multi
"OK"
master:0>decrby money 20
"QUEUED"
master:0>incrby out 20
"QUEUED"
master:0>exec
 1)  "OK"
 2)  "80"
 3)  "OK"
 4)  "20"
 5)  "OK"
```

> 多线程异常执行 使用watch可以当做redis乐观锁

# Redis持久化

## RDB(Redis data base)

> 是什么

rdb通俗一点来讲就是快照，类似于我们虚拟机中的快照功能，可以把某一时刻的状态全都保存下来。


> 优点

1. 适合大规模的数据恢复。
2.  如果业务对数据完整性和一致性要求不高，RDB是很好的选择。


> 缺点

1. 数据的完整性和一致性不高，因为RDB可能在最后一次备份时宕机了。
2.  备份时占用内存，因为Redis 在备份时会独立创建一个子进程，将数据写入到一个临时文件（此时内存中的数据是原来的两倍哦），最后再将临时文件替换之前的备份文件。所以Redis 的持久化和数据的恢复要选择在夜深人静的时候执行是比较合理的。


> 什么时候触发

- 达到执行条件
- shutdown
- save
- bgsave
- flushall

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210409132906037.png" alt="image-20210409132906037" style="zoom:50%;" />

## AOF(Append only File)

Redis 默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个**写操作**，并**追加**到文件中。Redis 重启的会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

### 从配置文件了解AOF

打开 redis.conf 文件，找到 APPEND ONLY MODE 对应内容
1 redis 默认关闭，开启需要手动把no改为yes

```
appendonly yes
```

2 指定本地数据库文件名，默认值为 appendonly.aof

```
appendfilename "appendonly.aof"
```

3 指定更新日志条件

```
# appendfsync always
appendfsync everysec
# appendfsync no
```

解说：
always：同步持久化，每次发生数据变化会立刻写入到磁盘中。性能较差当数据完整性比较好（慢，安全）
everysec：出厂默认推荐，每秒异步记录一次（默认值）
no：不同步

4 配置重写触发机制

```
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```

解说：当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。一般都设置为3G，64M太小了。

### 触发AOF快照

根据配置文件触发，可以是每次执行触发，可以是每秒触发，可以不同步。

### 根据AOF文件恢复数据

正常情况下，将appendonly.aof 文件拷贝到redis的安装目录的bin目录下，重启redis服务即可。但在实际开发中，可能因为某些原因导致appendonly.aof 文件格式异常，从而导致数据还原失败，可以通过命令redis-check-aof --fix appendonly.aof 进行修复 。从下面的操作演示中体会。

### AOF的重写机制

前面也说到了，AOF的工作原理是将写操作追加到文件中，文件的冗余内容会越来越多。所以聪明的 Redis 新增了重写机制。当AOF文件的大小超过所设定的阈值时，Redis就会对AOF文件的内容压缩。

重写的原理：Redis 会fork出一条新进程，读取内存中的数据，并重新写到一个临时文件中。并没有读取旧文件。最后替换旧的aof文件。

触发机制：当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。这里的“一倍”和“64M” 可以通过配置文件修改。

### AOF 的优缺点

优点：数据的完整性和一致性更高
缺点：因为AOF记录的内容多，文件会越来越大，数据恢复也会越来越慢

# Redis主从复制

### 概念

主从复制：是指将一台redis服务器上的数据复制到其他redis服务器，前者称为master（主）服务器，后者称为slaver（从）；数据库的复制是单向的，意思是：只能从主服务器复制到从服务器。一般来讲，主服务器负责写，从服务器负责读。

默认情况下，每一台redis都是一个主节点；且一个主节点可以有多个从节点，但是从节点只能有1个主节点

### 作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化的另外一种表现形式。
2. 数据恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复
3. 负载均衡：在主从复制的模式下，特别是针对于一些写少读多的场景，主节点负责写，多个从节点负责读，大大减轻了单个节点的压力。提高redis的并发量
4. 高可用基石：除了上述的作用之外，主从复制还是哨兵和集群能够实施的基础

只要在公司中一定要进行主从复制

### 环境配置

```bash
master:0>info replication #查看当前服务器配置信息
"# Replication
role:master
connected_slaves:0
master_replid:c5e8d7364f28b7b341759951496479915967b745
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
"
```

> 子节点找老大

```bash
slaver1:0>slaveof 127.0.0.1 6379
```

<font color="rgb(255,255,255)">这种的主从配置是临时的，实际情况中应该按照配置文件的方式去配置，如果从机重启了，这时候重启从机就会立马变成主机，只要是变成从机，立马会同步</font>

> 复制原理

从机连接到主机之后，发送一个sync信号到master，master收到信号之后立马同步一份数据传输到从机，从机拿到数据之后进行

**全量复制**，进行数据恢复，之后的数据都进行**增量复制**

模式一：

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20201012215928154.png" alt="image-20201012215928154" style="zoom:50%;" />

模式二：

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20201012220200519.png" alt="image-20201012220200519" style="zoom:50%;" />

以上两种在工作中都不会使用

> 如果没有老大了，这时候如何选择出老大呢？手动！(哨兵模式恢复之前)

```bash
slaver1:0>slaveof no one
"OK"
slaver1:0>info replication
"# Replication
role:master
connected_slaves:0
master_replid:b890a8a5cb5a6e7fc259c08b94106cd16496b9b6
master_replid2:4d46c2b14ceb5fc1c4e43dc2b53ec6338aa8de68
master_repl_offset:4966
second_repl_offset:4967
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:4966
```

### 哨兵模式（自动选取老大的模式）

> 概念

主从切换技术的方法是：主机宕机之后需要手动选举出新的老大当做主机，费时费力。redis从2.8之后开始提供哨兵模式

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20201012222839130.png" alt="image-20201012222839130" style="zoom:50%;" />

1. 配置哨兵文件sentinel.conf

```bash
# sentinel monitor 被监控的名称 host port 1
sentinel monitor redis-master 127.0.0.1 16379 1
```

后面的数字1，代表主机挂了，slave投票看让谁成为主机，票数最多的就会成为哨兵



# 缓存穿透和雪崩

## 缓存穿透概念（查不到）

用户想要查询一个值，先去redis中请求，发现redis中没有，也就是所谓的redis没有命中，然后往数据库去查询，数据库中也没有，，于是本次查询失败。当用户很多，并且有大量的这种请求的时候，会对持久化的数据库造成压力，这就相当于出现了缓存穿透。

## 解决方案

> 布隆过滤器

布隆过滤器是一种数据结构，使用起来也比较方便，只需要导入包即可

> 缓存空值

# 缓存击穿（针对于某个点集中攻击）

## 解决方案

> 热点数据不过期

> 分布式锁

# 缓存雪崩（集体失效）

## 解决方案

> redis高可用

> 限流降级

> 数据预热