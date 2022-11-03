---
title: Java双亲委派机制及用法
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-03
tags:
- java
categories:
- java
typora-root-url: ../..
---

# Java双亲委派机制及用法

## 一、JVM类加载过程

整个生命周期包括如下7个阶段

1. 加载(loading)也称为装载
2. 验证、准备、解析3个部分统称为链接(Linking)
3. 初始化
4. 使用
5. 销毁

![java类加载过程](/img/java类加载过程.png)

### 二、类加载器介绍

JVM中有内置了三个初始的类加载器。

1. **BootstrapClassLoader(引导类、启动类加载器)**，顶层类加载器，C++实现。加载%JAVA_HOME%lib目录下jar包和类或者或被 `-Xbootclasspath`参数指定的路径中的所有类，加载java.*。
2. **ExtensionClassLoader(扩展类加载器)** ：负责加载目录 `%JRE_HOME%/lib/ext` 目录下的jar包和类，或被 `java.ext.dirs` 系统变量所指定的路径下的jar包。
3. **AppClassLoader(应用程序类加载器)** :加载当前应用classpath下的所有jar包和类（在spring加载中起到了重要作用）。

### 三、双亲委派模型

一个类加载的时候先交由父类加载器去加载，如果父类加载器也存在父类加载器，继续交由父类加载，以此递归，如果父类加载无法加载再由自己去加载。

![双亲委派模型](/img/java双亲委派模型.png)

**如果想打破双亲委派机制可以自定义一个类加载器继承java.lang.ClassLoader，重写loadClass方法，不交由父类去加载。**

**如果只是想自定义加载class的方法，比如读取二进制流后进行解密等。只需要重写findClass就行了。**

**loadClass方法负责加载流程，findClass方法负责具体加载细节。**

### 四、双亲委派模式的特点

- 避免重复加载`class`，保证数据安全。
- 保护核心class无法修改。
- 不同加载器加载同一个class不是同一个class对象，保证class执行安全。