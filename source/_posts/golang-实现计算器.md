---
title: golang 实现计算器
date: 2021-09-24 16:58:16
tags:
- golang
- 算法
categories:
- golang

auto_excerpt:
  enable: true
  length: 150
---
golang 实现一个计算的功能，并支持括号计算

github地址：[https://github.com/huangxingx/go-utils.git](https://github.com/huangxingx/go-utils.git)

#### 实现重点：后缀表达式
一般的表达式都是中缀表达式，操作符在两个操作项之间。而后缀表达式的操作符在两个操作项后面

#### 后缀表达式的实现
利用栈 `stack` 和队列 `queue` 实现**后缀表达式**
![后缀表达式流程图](/img/puml/suffix_express/suffix_express.svg)

**流程：**
> 1. 先把字符串转换为列表 比如 "1+2+3\*4" 转换为 ["1", "+", "2", "+", "3", "*", "4"]
> 2. 上面的字符串列表转换为后缀表达式
> 3. 遍历后缀表达式进行计算

`computer.go`

```golang
package go_utils

import "strconv"

// Computer 计算后缀表达式
func Computer(expression string) float64 {
	mpnExpression := parse2mpn(expression)
	rpnExpression := parse2rpn(mpnExpression)
	resultStack := NewStack()
	for _, v := range rpnExpression {
		switch v {
		case "+", "-", "*", "/":
			v1, _ := resultStack.Pop().(float64)
			v2, _ := resultStack.Pop().(float64)
			switch v {
			case "+":
				resultStack.Push(v1 + v2)
			case "-":
				resultStack.Push(v2 - v1)
			case "*":
				resultStack.Push(v2 * v1)
			case "/":
				resultStack.Push(v2 / v1)
			}
		default:
			float, err := strconv.ParseFloat(v, 64)
			if err != nil {
				panic(err)
			}
			resultStack.Push(float)
		}
	}
	return resultStack.Pop().(float64)
}

// 解析中缀表达式数组
func parse2mpn(express string) []string {
	//compile, _ := regexp.Compile("[\\d\\+\\-\\*\\/\\(\\)]")
	//return compile.FindAllString(express, -1)
	result := make([]string, 0)
	s := ""
	for i, v := range express {
		if v == 32 {
			continue
		}
		if v > 47 && v < 59 || v == 46 {
			s += string(v)
			if i == len(express)-1 {
				result = append(result, s)
			}
		} else {
			// +-*/ ( )
			if s != "" {
				result = append(result, s)
				s = ""
			}
			result = append(result, string(v))
		}
	}
	return result
}

// parse2rpn 解析中缀表达式-> 后缀表达式
func parse2rpn(express []string) []string {
	stact := NewStack()
	rpnList := make([]string, 0, len(express))
	var s string
	for i := 0; i < len(express); i++ {
		s = express[i]
		switch s {
		case "+", "-":
			for {
				if stact.IsEmpty() || stact.Peek().(string) == "(" {
					stact.Push(s)
					break
				} else {
					rpnList = append(rpnList, stact.Pop().(string))
				}
			}
		case "*", "/":
			for {
				if stact.IsEmpty() || (stact.Peek().(string) != "*" && stact.Peek().(string) != "/") {
					stact.Push(s)
					break
				} else {
					rpnList = append(rpnList, stact.Pop().(string))
				}
			}

		case "(":
			stact.Push(s)

		case ")":

			for stact.Peek().(string) != "(" {
				rpnList = append(rpnList, stact.Pop().(string))
			}
			stact.Pop()
		default:
			rpnList = append(rpnList, s)
		}
	}

	for stact.Size() > 0 {
		rpnList = append(rpnList, stact.Pop().(string))
	}
	return rpnList
}

```

