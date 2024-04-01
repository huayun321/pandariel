---
weight: 1
bookFlatSection: true
title: 快排
---

# 分治法与快排

## 列表使用分治法
递归缩小问题范围
注意基线问题和递归条件

```shell
package main

import "fmt"

func main() {
	//sum test
	//list := []int{1, 2, 3, 4, 5, 8}
	//fmt.Println(len(list))
	//fmt.Println(cap(list))
	//fmt.Println(sum(list))

	//count test
	//list := []int{1, 2, 3, 4, 5, 8}
	//fmt.Println(len(list))
	//fmt.Println(cap(list))
	//fmt.Println(count(list))

	//max test
	//list := []int{112, 2, 333, 4, 5, 8}
	//fmt.Println(len(list))
	//fmt.Println(cap(list))
	//fmt.Println(max(list))

	//quick sort test
	list := []int{112, 2, 333, 4, 5, 8, 4}
	fmt.Println(len(list))
	fmt.Println(cap(list))
	fmt.Println(quickSort(list))
}

func sum(list []int) int {
	if len(list) == 1 {
		return list[0]
	}
	fmt.Println(list)
	fmt.Println("len ", len(list[1:]))
	fmt.Println("cap ", cap(list[1:]))

	return list[0] + sum(list[1:])
}

func count(list []int) int {
	if len(list) == 1 {
		return 1
	}

	return 1 + count(list[1:])
}

func max(list []int) int {
	if len(list) == 2 {
		if list[0] > list[1] {
			return list[0]
		} else {
			return list[1]
		}
	}
	subMax := max(list[1:])

	if list[0] > subMax {
		return list[0]
	} else {
		return subMax
	}
}

func quickSort(list []int) []int {
	if len(list) < 2 {
		return list
	}

	pivot := list[0]
	var less []int
	var greater []int
	var res []int
	for _, v := range list[1:] {
		if v <= pivot {
			less = append(less, v)
		} else {
			greater = append(greater, v)
		}
	}

	res = append(res, quickSort(less)...)
	res = append(res, pivot)
	res = append(res, quickSort(greater)...)

	return res
}

```