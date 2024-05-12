---
weight: 1
bookFlatSection: true
title: "The Go Programming Language"
---

# 《Go程序设计语言》

## 程序结构
### 变量
#### 变量声明
```go
package main

import "fmt"

func main() {
	var var1 int
	var var2 string
	var var3 bool

	fmt.Println(var1) //0
	fmt.Println(var2) //""
	fmt.Println(var3) //false

	var container1 []int
	var container2 map[string]int
	var container3 chan int
	var pointer *int
	var functional func()

	fmt.Println(container1 == nil) //true
	fmt.Println(container2 == nil) //true
	fmt.Println(container3 == nil) //true
	fmt.Println(pointer == nil)    //true
	fmt.Println(functional == nil) //true
}

```

#### new函数
```go
package main

import "fmt"

func main() {
	var sd []int
	fmt.Println(sd == nil)
	fmt.Println(len(sd))
	fmt.Println(cap(sd))
	// slice map chan 只声明的话为nil
	//true
	//0
	//0

	var sdd map[string]int
	fmt.Println(sdd == nil)

	nd := new([]int) //new 和make 的区别 只是声明，初始化零值， new不会初始化底层数组
	fmt.Println(*nd == nil)
	fmt.Println(len(*nd))
	fmt.Println(cap(*nd))
	//true
	//0
	//0

	md := make([]int, 10) //new 和make 的区别 声明，并初始化零值及引用类型， make会初始化底层数组
	fmt.Println(md == nil)
	fmt.Println(len(md))
	fmt.Println(cap(md))
	//false
	//10
	//10
}

```