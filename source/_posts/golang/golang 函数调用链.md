---
title: golang 函数调用链
tags:
  - golang
categories:
  - golang
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---



[TOC]

### 利用 defer 实现函数出入口的跟踪

跟踪函数调用，我们首先想到的就是跟踪函数的出入口，而完成这一任务，当仁不让的就是利用defer

```golang
func trace() func() {
    pc, _, _, ok := runtime.Caller(1)
    if !ok {
        panic("not found caller")
    }

    fn := runtime.FuncForPC(pc)
    name := fn.Name()

    fmt.Printf("enter: %s\n", name)
    return func() { fmt.Printf("exit: %s\n", name) } 
}

func A1() {
    defer trace()()
    B1()
}

func B1() {
    defer trace()()
    C1()
}

func C1() {
    defer trace()()
    D()
}

func D() {
    defer trace()()
}

func main() {
    A1()
}
```

我们看到：以 A1 实现为例，当执行流来带 defer 语句时，首先会对 defer 后面的表达式进行求值。trace 函数会执行，输出函数入口信息，并返回一个 “打印出口信息” 的匿名函数。该函数在此并不会执行，而是被注册到函数 A1 的 defer 函数栈中，待 A1 函数执行结束后才会被弹出执行。也就是在 A1 结束后，会有一条函数的出口信息被输出

    $go build
    $./functrace-demo
    enter: main.A1
    enter: main.B1
    enter: main.C1
    enter: main.D
    exit: main.D
    exit: main.C1
    exit: main.B1
    exit: main.A1