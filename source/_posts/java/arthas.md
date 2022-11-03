---
title: Arthas
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-03
tags:
- java
categories:
- java
---

# Arthas

## 快速入门

GitHub：https://github.com/alibaba/arthas

文档地址：https://arthas.aliyun.com/

下载并启动arthas

```shell
curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
```

**使用`as.sh`**

Arthas 支持在 Linux/Unix/Mac 等平台上一键安装，请复制以下内容，并粘贴到命令行中，敲 `回车` 执行即可：

```shell
curl -L https://arthas.aliyun.com/install.sh | sh		
```

## 常用命令

### logger

更新 logger lever

```shell
logger --name ROOT --level debug
```

### jad

反编译指定已加载类的源码

使用参考 https://arthas.aliyun.com/doc/jad.html

**参数说明**

|              参数名称 | 参数说明                                   |
| --------------------: | :----------------------------------------- |
|       *class-pattern* | 类名表达式匹配                             |
|                `[c:]` | 类所属 ClassLoader 的 hashcode             |
| `[classLoaderClass:]` | 指定执行表达式的 ClassLoader 的 class name |
|                   [E] | 开启正则表达式匹配，默认为通配符匹配       |

### tt

方法执行数据的时空隧道，记录下指定方法每次调用的入参和返回信息，并能对这些不同的时间下调用进行观测

```shell
tt -t demo.MathGame primeFactors
```

**表格字段说明**

| 表格字段  | 字段解释                                                     |
| --------- | ------------------------------------------------------------ |
| INDEX     | 时间片段记录编号，每一个编号代表着一次调用，后续 tt 还有很多命令都是基于此编号指定记录操作，非常重要。 |
| TIMESTAMP | 方法执行的本机时间，记录了这个时间片段所发生的本机时间       |
| COST(ms)  | 方法执行的耗时                                               |
| IS-RET    | 方法是否以正常返回的形式结束                                 |
| IS-EXP    | 方法是否以抛异常的形式结束                                   |
| OBJECT    | 执行对象的`hashCode()`，注意，曾经有人误认为是对象在 JVM 中的内存地址，但很遗憾他不是。但他能帮助你简单的标记当前执行方法的类实体 |
| CLASS     | 执行的类名                                                   |
| METHOD    | 执行的方法名                                                 |

### trace

`trace` 命令能主动搜索 `class-pattern`／`method-pattern` 对应的方法调用路径，渲染和统计整个调用链路上的所有性能开销和追踪调用链路。

**参数说明**

|            参数名称 | 参数说明                             |
| ------------------: | :----------------------------------- |
|     *class-pattern* | 类名表达式匹配                       |
|    *method-pattern* | 方法名表达式匹配                     |
| *condition-express* | 条件表达式                           |
|                 [E] | 开启正则表达式匹配，默认为通配符匹配 |
|              `[n:]` | 命令执行次数                         |
|             `#cost` | 方法执行耗时                         |

```shell
trace demo.MathGame run -n 10 '#cost > 10'
```



### watch

让你能方便的观察到指定函数的调用情况。能观察到的范围为：`返回值`、`抛出异常`、`入参`，通过编写 OGNL 表达式进行对应变量的查看。

**参数说明**

|            参数名称 | 参数说明                                          |
| ------------------: | :------------------------------------------------ |
|     *class-pattern* | 类名表达式匹配                                    |
|    *method-pattern* | 函数名表达式匹配                                  |
|           *express* | 观察表达式，默认值：`{params, target, returnObj}` |
| *condition-express* | 条件表达式                                        |
|                 [b] | 在**函数调用之前**观察                            |
|                 [e] | 在**函数异常之后**观察                            |
|                 [s] | 在**函数返回之后**观察                            |
|                 [f] | 在**函数结束之后**(正常返回和异常返回)观察        |
|                 [E] | 开启正则表达式匹配，默认为通配符匹配              |
|                [x:] | 指定输出结果的属性遍历深度，默认为 1，最大值是 4  |

```shell
watch demo.MathGame primeFactors -x 2
```

### dashboard

当运行在 Ali-tomcat 时，会显示当前 tomcat 的实时信息，如 HTTP 请求的 qps, rt, 错误数, 线程池信息等等。

**参数说明**

| 参数名称 | 参数说明                                 |
| -------: | :--------------------------------------- |
|     [i:] | 刷新实时数据的时间间隔 (ms)，默认 5000ms |
|     [n:] | 刷新实时数据的次数                       |

