---
title: java mapstruct 详解
auto_excerpt:
  enable: true
  length: 150
date: 2021-10-25 09:58:50
tags:
- java
- mapstruct
- plugins
categories:
- java
---

官网：[https://mapstruct.org/](https://mapstruct.org/)

GitHub: [https://github.com/mapstruct/mapstruct](https://github.com/mapstruct/mapstruct)

### 关于 BeanUtil

平时我经常使用Hutool中的BeanUtil类来实现对象转换，用多了之后就发现有些缺点：

- 对象属性映射使用反射来实现，性能比较低；
- 对于不同名称或不同类型的属性无法转换，还得单独写Getter、Setter方法；
- 对于嵌套的子对象也需要转换的情况，也得自行处理；
- 集合对象转换时，得使用循环，一个个拷贝。

对于这些不足，MapStruct都能解决，不愧为一款功能强大的对象映射工具！

### IDEA插件支持

![](/img/mapstruct-plugins.png)

### 项目集成

在SpingBoot中集成MapStruct非常简单，仅续添加如下两个依赖即可，这里使用的是`1.4.2.Final`版本

```xml
<dependency>
    <!--MapStruct相关依赖-->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${mapstruct.version}</version>
    </dependency>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct-processor</artifactId>
        <version>${mapstruct.version}</version>
        <scope>compile</scope>
    </dependency>
</dependencies
```

