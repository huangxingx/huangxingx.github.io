---
title: SQL中的in与not in、exists与not exists的区别以及性能分析
date: 2021-09-14 22:55:15
tags:
- sql
- 性能
categories:
- mysql
auto_excerpt:
  enable: true
  length: 150
---

### 1、in和exists

in是把外表和内表作hash连接，而exists是对外表作loop循环，每次loop循环再对内表进行查询，一直以来认为exists比in效率高的说法是不准确的。

**如果查询的两个表大小相当，那么用in和exists差别不大；如果两个表中一个较小一个较大，则子查询表大的用exists，子查询表小的用in；**

## **2、not in 和not exists**

not in 逻辑上不完全等同于not exists，如果你误用了not in，小心你的程序存在致命的BUG，请看下面的例子：

```sql
create table #t1(c1 int,c2 int);

create table #t2(c1 int,c2 int);

insert into #t1 values(1,2);

insert into #t1 values(1,3);

insert into #t2 values(1,2);

insert into #t2 values(1,null);

 

select * from #t1 where c2 not in(select c2 from #t2);  -->执行结果：无

select * from #t1 where not exists(select 1 from #t2 where #t2.c2=#t1.c2)  -->执行结果：1  3
```

正如所看到的，not in出现了不期望的结果集，存在逻辑错误。如果看一下上述两个select 语句的执行计划，也会不同，后者使用了hash_aj，所以，请尽量不要使用not in(它会调用子查询)，而尽量使用not exists（它会调用关联子查询）。

如果子查询中返回的任意一条记录含有空值，则查询将不返回任何记录。如果子查询字段有非空限制，这时可以使用not in，并且可以通过提示让它用hasg_aj或merge_aj连接。

如果查询语句使用了not in，那么对内外表都进行全表扫描，没有用到索引；而not exists的子查询依然能用到表上的索引。所以无论哪个表大，用not exists都比not in 要快。

## **3、in 与 = 的区别**

```sql
select name from student where name in('zhang','wang','zhao');
```

与

```sql
select name from student where name='zhang' or name='wang' or name='zhao'
```

的结果是相同的。

### 其他分析

**对于in 和 exists的性能区别:**

如果子查询得出的结果集记录较少，主查询中的表较大且又有索引时应该用in,反之如果外层的主查询记录较少，子查询中的表大，又有索引时使用exists。

其实我们区分in和exists主要是造成了驱动顺序的改变（这是性能变化的关键），如果是exists，那么以外层表为驱动表，先被访问，如果是IN，那么先执行子查询，所以我们会以驱动表的快速返回为目标，那么就会考虑到索引及结果集的关系了

**对于not in 和 not exists的性能区别：**

not in 只有当子查询中，select 关键字后的字段有not null约束或者有这种暗示时用not in,另外如果主查询中表大，子查询中的表小但是记录多，则应当使用not in,并使用anti hash join.

如果主查询表中记录少，子查询表中记录多，并有索引，可以使用not exists,另外not in最好也可以用`/*+ HASH_AJ */`或者外连接+is null

NOT IN 在基于成本的应用中较好

