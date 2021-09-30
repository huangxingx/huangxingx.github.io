---
title: sync.Pool 对象池使用问题记录
auto_excerpt:
  enable: true
  length: 150
date: 2021-09-30 11:07:11
tags:

- golang
- sync.Pool
- concurrent

categories:
- golang
---

### sync.Pool 对象重置问题

1. 方式1: get -> operate-obj ->  reset -> put

   > 问题：get 的对象在操作时可能没有被reset 到

2. 方式2: get -> reset -> operate-obj -> put

   > 获取对象后，先 reset 在使用是没有问题的
   >
   > gin 就是这样使用的
   >
   > ```go
   > func (engine *Engine) ServeHTTP(w http.ResponseWriter, req *http.Request) {
   > 	c := engine.pool.Get().(*Context)
   > 	c.writermem.reset(w)
   > 	c.Request = req
   > 	c.reset() // 先reset 
   > 
   > 	engine.handleHTTPRequest(c) // 再使用
   > 
   > 	engine.pool.Put(c) // 使用之后直接 put
   > }
   > 
   > ```

**`cat test_syncpool.go`**

```go
package test

import (
	"strconv"
	"sync"
	"testing"
)

type A struct {
	Name string
}

func (a *A) putValue(i int) {
	a.Name = "A+" + strconv.Itoa(i)
}

func (a *A) reset() {
	a.Name = ""
}

func Benchmark_A(b *testing.B) {
	pool := &sync.Pool{
		New: func() interface{} {
			return new(A)
		},
	}
  pool2 := &sync.Pool{
		New: func() interface{} {
			return new(A)
		},
	}
	waitGroup := sync.WaitGroup{}

	waitGroup.Add(1)
	go func() {
		defer waitGroup.Done()
		for i := 0; i < b.N; i++ {

			a := pool.Get().(*A)

			//a.reset() // 方式2 没有问题

			if a.Name != "" {
				panic("name not empty")
			}
			a.putValue(i) // a.Name = "A+" + strconv.Itoa(i)
			if a.Name == "" {
				panic("name is empty")
			}

			a.reset() // 方式1 有问题的方式

			pool.Put(a)
		}
	}()

	waitGroup.Add(1)
	go func() {
		defer waitGroup.Done()
		for i := 0; i < b.N; i++ {
			a := pool.Get().(*A)
			a.reset()
			if a.Name != "" {
				panic("name not empty")
			}
			a.putValue(i)
			if a.Name == "" {
				panic("name not empty")
			}
			pool.Put(a)
		}
	}()
	waitGroup.Wait()
}

```

修改点

```go
func (pool *taskPool) get() *Task {
   task := pool.bp.Get().(*Task)
   task.Reset()
   return task
}

func (pool *taskPool) put(obj *Task) {
   pool.bp.Put(obj)
}
```



执行用例 BenchmarkTimeWheel

```shell
➜  go-timewheel (master) ✗ go test -v -bench . -test.bench='TimeWheel$' -test.run ^$
goos: darwin
goarch: amd64
pkg: github.com/rfyiamcool/go-timewheel
cpu: Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz
BenchmarkAdd-12                  1990670              1090 ns/op
BenchmarkTimeWheel/wheel-N-1m-12                 1000000              1049 ns/op
BenchmarkTimeWheel/wheel-N-5m-12                 1223064              1061 ns/op
BenchmarkTimeWheel/wheel-N-10m-12                1000000              1253 ns/op
PASS
ok      github.com/rfyiamcool/go-timewheel      100.504s
```



问题点

task.callback 有值，但是 task.callback == nil 分支却执行了，断点在 task.callback() 时去执行 task.callback 确实是有值的

![image-20210930143647762](/img/syncpool/image-20210930143647762.png)

Task.stop = true

![image-20210930145240966](/img/syncpool/image-20210930145240966.png)

![image-20210930145657424](/img/syncpool/image-20210930145657424.png)
