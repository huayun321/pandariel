---
weight: 1
bookFlatSection: true
title: "1、两数之和"
---

# 1、两数之和

## 题目
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值的那两个整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。

### 示例 1：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

### 示例 2：
```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```
### 示例 3：
```
输入：nums = [3,3], target = 6
输出：[0,1]
```

### 提示：
* 2 <= nums.length <= 10^3
* -109 <= nums[i] <= 10^9
* -109 <= target <= 10^9
* 只会存在一个有效答案

## 思路
```
这是第一次正式解题并记录，一两年前注册过，可能也做过几个题，不太记得了。
看到这个题，直观的想法肯定是两次循环遍历，时间复杂度O(n^2)。
然后想怎么提高效率，如果数组有序那么在第二次循环的时候做二分。
最后想到可以利用tmp = target - nums[i], 那么如果将num元素放入map就可以O(1)获取第二个元素的下标。
在提交中发现，测试用例竟然有[0,3,4,0]....
没有注意到示例3。
好吧，map要用map[int][]int
```

## 题解
```golang
func twoSum(nums []int, target int) []int {
	m := make(map[int][]int, len(nums))
	for i, v := range nums {
		if len(m[v]) == 0 {
			m[v] = []int{}
		}
		m[v] = append(m[v], i)
	}

	for first, v := range nums {
		k := target - v
		second, _ := m[k]

		for _, v := range second {
			if first == v {
				continue
			}
			return []int{first, v}
		}
	}

	return []int{}
}
```

