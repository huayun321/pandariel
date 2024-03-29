---
weight: 1
bookFlatSection: true
title: 递归
---

```shell
package main

import "fmt"

func factorial(n int) int {
	if n == 1 {
		return 1
	} else {
		return n * factorial(n-1)
	}
}

func main() {
	fmt.Println(factorial(5))
}

```