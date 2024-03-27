---
weight: 1
bookFlatSection: true
title: 选择查找
---

# 选择查找
时间复杂度O(n*n)

```shell
package main

import "fmt"

func selectionSort(arr []int) {
	for i := range arr {
		// 查找最小元素的索引
		minIndex := i
		for j := i + 1; j < len(arr); j++ {
			if arr[j] < arr[minIndex] {
				minIndex = j
			}
		}
		// 如果最小元素不是第一个，交换它们
		if minIndex != i {
			arr[i], arr[minIndex] = arr[minIndex], arr[i]
		}
	}
}

func main() {
	arr := []int{64, 25, 12, 22, 11}
	selectionSort(arr)
	fmt.Println("Sorted array:", arr)
}

```