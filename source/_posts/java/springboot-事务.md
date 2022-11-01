---
title: springboot 事务
date: 2021-09-13 20:18:56
tags:
- java
- springboot
- 事务
categories:
- java

auto_excerpt:
  enable: true
  length: 150
---

[toc]

## @Transactional 注解的属性介绍

### value 和 transactionManager 属性

它们两个是一样的意思。当配置了多个事务管理器时，可以使用该属性指定选择哪个事务管理器。

### propagation 属性

事务的传播行为，默认值为 Propagation.REQUIRED。

* REQUIRED

支持事务，如果业务方法执行时在一个事务中，则加入当前事务，否则则重新开始一个事务。

* REQUIRES_NEW

支持事务。每次都是创建一个新事物，如果当前已经在事务中了，会挂起当前事务。

* NESTED

支持事务。如果当前已经在一个事务中了，则嵌套在已有的事务中作为一个子事务。如果当前没在事务中则开启一个事务。

* SUPPORTS

支持事务。当前有事务就加入当前事务。当前没有事务就算了，不会开启一个事物。

* MANDATORY

支持事务，如果业务方法执行时已经在一个事务中，则加入当前事务。否则抛出异常。

* NOT_SUPPORTED

不支持事务，如果业务方法执行时已经在一个事务中，则挂起当前事务，等方法执行完毕后，事务恢复进行。

* NEVER

不支持事务。如果当前已经在一个事务中了，抛出异常。数据回滚。

### isolation 属性

事务的隔离级别，默认值为 Isolation.DEFAULT。

* Isolation.DEFAULT

  使用底层数据库默认的隔离级别。

* Isolation.READ_UNCOMMITTED

* Isolation.READ_COMMITTED

* Isolation.REPEATABLE_READ

* Isolation.SERIALIZABLE

### timeout 属性

事务的超时时间，默认值为-1。如果超过该时间限制但事务还没有完成，则自动回滚事务。

### readOnly 属性

指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置 read-only 为 true。

### rollbackFor 属性

用于指定能够触发事务回滚的异常类型，可以指定多个异常类型。

### noRollbackFor 属性

抛出指定的异常类型，不回滚事务，也可以指定多个异常类型。

## 事务不生效的情况

### 1. 数据库不支持事物

mysql 中 MyISAM 是不支持事物的。

### 2. 类内部访问

简单来讲就是指非直接访问带注解标记的方法 B，而是通过类普通方法 A，然后由 A 访问 B

### 3. 私有方法

在私有方法上，添加`@Transactional`注解也不会生效，私有方法外部不能访问，所以只能内部访问。

### 4. 异常不匹配

`@Transactional`注解默认处理运行时异常，即只有抛出运行时异常时，才会触发事务回滚，否则并不会

### 5. 多线程

这个场景可能并不多见，在标记事务的方法内部，另起子线程执行 db 操作，此时事务同样不会生效

### 6. 传播属性设置异常

有些传播属性是不支持事物的，见上文中呢
