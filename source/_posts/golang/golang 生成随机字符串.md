---
title: golang 生成随机字符串
date: 2019-06-13 15:41:22
tags: 
- golang

categories:
- golang

---

### 随机字符串

```go

//RandomStr 随机生成字符串
func RandomStr(length int) string {
	str := "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
	bytes := []byte(str)
	result := []byte{}
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	for i := 0; i < length; i++ {
		result = append(result, bytes[r.Intn(len(bytes))])
	}
	return string(result)
}
```

###  生成定长字符串


```go
//获得定长字符串
//str 填充字符串
//length 获得定长的长度
//char 不够长时填充的字符
func GetFixedLenString(str string, length int, char byte) string {
	if len(str) == 0 {
		return ""
	}

	if len(str) == length {
		return str
	}

	//超出切后面
	if len(str) > length {
		return string(str[:length])
	}

	//缺少添加char
	if len(str) < length {
		slice := make([]byte, length-len(str))
		for k := range slice {
			slice[k] = char
		}
		return string(append(slice, []byte(str)...))
	}

	return ""
}

```
### 获得定长byte slice
```go
//获得定长byte slice
//str 填充字符串
//length 获得定长的长度
//char 不够长时填充的字符
func GetFixedLenByte(b []byte, length int, char byte) (tb []byte) {
	if len(b) == 0 {
		return
	}

	if len(b) == length {
		return b
	}

	//超出切后面
	if len(b) > length {
		return b[:length]
	}

	//缺少添加char
	if len(b) < length {
		slice := make([]byte, length-len(b))
		for k := range slice {
			slice[k] = char
		}
		return append(slice, []byte(b)...)
	}

	return
}
```

### 生成随机验证码

```golang
package main
 
import (
	"fmt"
	"math/rand"
	"strings"
	"time"
)
 
func GenValidateCode(width int) string {
	numeric := [10]byte{0,1,2,3,4,5,6,7,8,9}
	r := len(numeric)
	rand.Seed(time.Now().UnixNano())
 
	var sb strings.Builder
	for i := 0; i < width; i++ {
		fmt.Fprintf(&sb, "%d", numeric[ rand.Intn(r) ])
	}
	return sb.String()
}
 
func main() {
	fmt.Println( GenValidateCode(6) )
}

```

