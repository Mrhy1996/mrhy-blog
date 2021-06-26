---
title: mysql常见的优化
toc: true
date: 2021-04-28 21:41:54
tags: mysql
url: mysql-optimize
---

sql的优化主要是围绕着在查询语句的时候尽量使用索引避免全表扫描。

此贴记录一下mysql中常见的优化语句

<!--more-->

> 使用索引

- 对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引。

> 避免判断null值

mysql 在使用is null 和is not null 的时候均不使用索引

> 不要使用%like

mysql 在使用%like 的时候不会使用索引

> 复合索引要符合最左前缀法则(带头大哥不能死，中间兄弟不能断)

> 小表驱动大表

Exists 或者in 的使用

如果 A小于b 则用 in

select * from a where id in (select id from b)

如果 a表的数据集大约b，则用exists

select * from a where exists (select 1 from b where a.id=b.id)

> 不要使用!= 或者<>

> 尽量避免使用or 

```sql
select id from table where a=1 or b=2;

# 修改为

select id from table where a=1 
union all
select id from table where b=2
```



> 常见的sql写法优化

```sql
# 大数据分页
# 低性能
SELECT c1,c2,cn… FROM table LIMIT n,m
# 高性能
select c1,c2,cn... from table where id in(select id from table limit n,m)

# 尽量避免使用selec * 

# 注意 exist 和 in 使用的时机

select num from a where num in(select num from b)

#用下面的语句替换：

select num from a where exists(select 1 from b where num=a.num)
```

