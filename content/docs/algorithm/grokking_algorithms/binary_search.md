---
weight: 1
bookFlatSection: true
title: 二分查找
---

# 二分查找

## what
折半查找，
利用两个指针每次排除数组的一半去查找。

## why
大幅度减少时间复杂度。

## how

```shell
package main

func main() {
	l := []int{1, 3, 5, 7, 9, 11}
	result := binarySearch(l, 11)
	println(result)
	//println(1 / 2)
}

func binarySearch(list []int, item int) int {
	low := 0
	high := len(list) - 1
	println("high: ", high)

	for low <= high {
		mid := (high + low) / 2
		guess := list[mid]
		println("mid: ", mid)
		println("guess: ", guess)

		if guess == item {
			return mid
		}
		if guess > item {
			high = mid - 1
		} else {
			low = mid + 1
		}
	}

	return -1
}
```