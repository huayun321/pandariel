---
weight: 1
bookFlatSection: true
title: "2. 两数相加"
---

# 2. 两数相加

## 题目
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

### 示例 1：
```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

### 示例 2：
```
输入：l1 = [0], l2 = [0]
输出：[0]
```
### 示例 3：
```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

### 提示：

* 每个链表中的节点数在范围 [1, 100] 内
* 0 <= Node.val <= 9
* 题目数据保证列表表示的数字不含前导零


## 思路
```
看到链表就想到递归。
可是给定的函数只有两个参数，我就蒙了一会。
试了一下，可以添加其他函数。
感觉还有改进的余地。

```

## 题解
```golang

func recurseAdd(l1 *ListNode, l2 *ListNode, ln *ListNode) {
	if l1 == nil && l2 == nil {
		return
	}

	var first int
	var second int
	var l1n *ListNode
	var l2n *ListNode

	if l1 != nil {
		first = l1.Val
		l1n = l1.Next
	}

	if l2 != nil {
		second = l2.Val
		l2n = l2.Next
	}

	sum := first + second + ln.Val
	ln.Val = sum % 10

	ln.Next = &ListNode{}

	if sum >= 10 {
		ln.Next.Val = 1
	}

	if l1n == nil && l2n == nil && ln.Next.Val == 0 {
		ln.Next = nil
		return
	}

	recurseAdd(l1n, l2n, ln.Next)
}

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	ln := &ListNode{}
	recurseAdd(l1, l2, ln)
	return ln
}

```

