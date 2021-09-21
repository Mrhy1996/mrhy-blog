

# MySql数据库的知识总结

## 1. mysql数据库常用的存储引擎

MyISAM、InnoDB、MERGE、MEMORY(HEAP)、BDB(BerkeleyDB)、EXAMPLE、FEDERATED、ARCHIVE、CSV、BLACKHOLE。

## 2. MyISAM 和InnoDB的区别

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210324102037337.png" alt="image-20210324102037337" style="zoom:50%;" />

## 3. mysql 的架构图

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210324101741686.png" alt="image-20210324101741686" style="zoom:50%;" />

特点：可插拔式

## 4. Mysql 索引

### 导致性能下降的原因有

1. 查询磁盘问题  top

2. sql写的烂

3. 索引失效(单值索引、复合索引)

   id name email weixinNumber

   单值索引

   create index idx_user_name on user(name);

   复合索引

   create index idx_user_name_email on user(name,email);

4. 关联查询太多join（设计缺陷）

5. 服务器调优及各个参数设置

### 常见的join查询

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210324103152493.png" alt="image-20210324103152493" style="zoom:50%;" />

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210324103248647.png" alt="image-20210324103248647" style="zoom:50%;" />

### 七种join理论

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210324105825223.png" alt="image-20210324105825223" style="zoom:50%;" />

### 索引是什么？

Mysql官方的定义为索引（Index）是帮助MySql高效获取数据的数据结构，本质就是数据结构

索引的目的就是为了提高查询效率，类似于字典

### 索引的创建方式

  主键

  alter table tableName add primary key(column_list);

  唯一索引

  alter table tableName add unique index index_name(column_list);

  普通索引

  alter table tableName add index index_name(column_list);

  全文索引

  alter table tableName add fullText index_name index_name(column_list);

### 哪些字段适合创建索引？

主键自动创建索引
频繁作为查询条件的应该作为索引
查询中与其他表关联的字段
大部分情况下选择组合索引
查询中排序的字段
查询中统计或者分组的字段

### 哪些情况不适合建索引

表记录太少   300万
经常增删改的列
数据重复且分布平均的字段

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210325173119006.png" alt="image-20210325173119006" style="zoom:50%;" />

### 索引的类型

聚集索引、非聚集索引、聚簇索引、稀疏索引、稠密索引

#### 聚集索引



#### 非聚集索引

#### 聚簇索引

#### 稀疏索引

#### 稠密索引





## 5. mysql 性能分析explain关键字

1. 能干嘛

   表的读取顺序

   数据读取的读取操作类型

   哪些索引可以被使用

   哪些索引实际被使用

   表之间的引用

   每张表有多少行被优化器查询

   <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210325180332331.png" alt="image-20210325180332331" style="zoom:50%;" />

   <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210325180416947.png" alt="image-20210325180416947" style="zoom:50%;" />

2. id 介绍

   谁的顺序高，谁先执行，如果相同则从上到下执行

3. select_type介绍

   simple：简单的select查询，不包含子查询或者union

   primary：查询中包含复杂的子部分，最外层被标记为

   subquery：在select 或者where列表中包含子查询

   derived：临时表

   union：若第二个select出现在union之后，被标记为union，若union包含在from子句的子查询中，外层select奖杯标记为derived

   union result：从union表获取结果的select。

4. type介绍

   all：     全表扫描

   index：全索引扫描

   range：只检索给定范围内的检索

   ref：非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，他返回所有匹配某个单独值的行。

   eq_ref：唯一性索引扫描，对于每个索引建，表中只有一条记录与之匹配，常见主键或者唯一索引扫描

   const：表示通过索引一次就找到了，const用于比较primary key 或者unique索引。因为只匹配一行数据，所以很快。

   system ：表只有一行数据，const类型的特例，平时不会出现。

   null

  从好到差依次为：system>const>eq_ref>ref>range>index>all

5. possible_keys key key_len

possible_keys:显示可能应用到这张表中的索引，一个或多个，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用

key：实际使用的索引

key_len:表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度，在不损失精确性的情况下，长度越短越好

6. rows 介绍

根据表统计信息及索引选用情况，大致估算出找出所需的记录所需要读取的行数。

7. extra介绍

   Using filesort：文件内排序

   using temporary：使用临时表

   using index：表示不错

## 6.索引优化及失效的情况

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210326101743536.png" alt="image-20210326101743536" style="zoom:50%;" />

最佳左前缀法则：1.带头大哥不能死2.中间兄弟不能断

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210326133458510.png" alt="image-20210326133458510" style="zoom:50%;" />

==小表驱动大表==

Exists 或者in 的使用

如果 A小于b 则用 in

select * from a where id=(select id from b)

如果 a表的数据集大约b，则用exists

select * from a where exists (select 1 from b where a.id=b.id)

==为排序增加索引==

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210326155852987.png" alt="image-20210326155852987" style="zoom:50%;" />







## 7.实际过程中分析sql

1. 观察，至少跑一天，查看生产环境的慢sql

2. 开启慢查询日志

3. explain+慢sql分析

4. show profile

5. 运维经理或者dba性能调优

## 8. 慢sql日志查询

  默认mysql数据库没有开启慢查询日志

配置一下

借助 mysqldumpslow去查看

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210326212257952.png" alt="image-20210326212257952" style="zoom:50%;" />

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210326212349462.png" alt="image-20210326212349462" style="zoom:50%;" />

## 9. Show profiles 使用（记录曾经执行过的sql）

使用步骤

1. 查询profiles 状态

   show VARIABLES like 'profiling%'

   <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329091508015.png" alt="image-20210329091508015" style="zoom:50%;" />

2. 开启profiles

   set profiling=on;(已经开启过的可以不开启)

3. show profiles; 

查询曾经使用过的sql

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329085942082.png" alt="image-20210329085942082" style="zoom:50%;" />

4. show profile cpu,block io for query **(Query_id);

   <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329091856611.png" alt="image-20210329091856611" style="zoom:50%;" />

   5. 结论

      <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329110501104.png" alt="image-20210329110501104" style="zoom:50%;" />

如果sql中出现这4个问题，则表示问题有点大

## 10.全局查询日志

==永远不用在生产环境启用这个功能==

set global general_log =1;

Set global log_output='TABLE';

select * from mysql.general_log

## 11.Mysql锁机制(MyIsam)

查看锁的状态

   show open tables;  

   unlock tables;

按照锁的类型来分

- 读锁（共享锁）

  命令 lock table * read;

  加上读锁，自己能写这个表，不能读写其他表，不能update这个表

  其他 能读 不能写（死锁）

- 写锁（排他锁）

  命令 lock table * write;

  <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329154023885.png" alt="image-20210329154023885" style="zoom:50%;" />

  <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329155557747.png" alt="image-20210329155557747" style="zoom:50%;" />

  下面的table locks waited 越高，证明性能越不好。

##   12. mysql 事务

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329155829159.png" alt="image-20210329155829159" style="zoom:50%;" />

## 13. mysql  事务隔离级别

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329160118303.png" alt="image-20210329160118303" style="zoom:50%;" />

##  14. mysql 行锁

##  15. 索引操作不当，行锁变成表锁

## 16. 间隙锁

<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329223734308.png" alt="image-20210329223734308" style="zoom:50%;" />

## 17. 如何锁定一行？

for update

## 18 性能总结

​	<img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210329225912024.png" alt="image-20210329225912024" style="zoom:50%;" />

## 19 mysql 主从复制  

> 原理

slave会从master读取binlog日志来进行同步。

>  三步骤+原理图

  <img src="/Users/mrhy/Library/Application Support/typora-user-images/image-20210330090221517.png" alt="image-20210330090221517" style="zoom:50%;" />

>  步骤

1. 修改主机配置文件

   server-id=1;

   log-bin=自己本地路径/mysqlin

2. 从机修改id

   server-id=2;

   log-bin=mysql-bin

3. 重启服务

4. 主机授权

   ```sql
   grant replication slave, replication client on *.* to 'rep'@'192.168.159.%' identified by 'Mysql@123';
   flush privileges
   ```

5. 查看主机状态

   ```sql
   show master status;
   ```

6. 从机配置

   ```sql
   CHANGE MASTER TO 
   MASTER_HOST='172.16.1.52',  #这是主库的IP（域名也可以需要做解析）
   MASTER_PORT=3306,              #主库的端口，从库端口和主库不可以相同
   MASTER_USER='rep',            #这是主库上创建用来复制的用户rep
   MASTER_PASSWORD='123456'    #rep的密码
   MASTER_LOG_FILE='mysql-bin.000025', #这里是show master
   status时看到的查询二进制日志文件名称，这里不能多空格
   MASTER_LOG_POS=9155;       #这里是show master status时看到的二进制日志偏移量，不能多空格
   ```

7. 开启从机复制

   ```sql
   start slave ;
   ```

8. 查看从机状态

   ```sql
   show slave status\G
   ```

   如果Slave_IO_RUNING:YES 和 SLAVE_SQL_RUNING:YES 则证明配置成功;

9. 停从机
	stop slave;

   