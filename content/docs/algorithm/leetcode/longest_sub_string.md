---
weight: 1
bookFlatSection: true
title: "3. 无重复字符的最长子串"
---

# 3. 无重复字符的最长子串

## 题目
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

### 示例 1：
```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

### 示例 2：
```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
### 示例 3：
```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

### 示例 4：
```
输入: s = ""
输出: 0
```

### 提示：

* 0 <= s.length <= 5 * 10^4
* s 由英文字母、数字、符号和空格组成

## 思路
```
刚开始的思路是利用集合将字符串分割成不重复的字串，
但是这样过不了测试用例“dvdf”。
为了过dvdf，逐个字符查找不重复字串。
时间复杂度O(n^2)。
不过看提交结果算法有很大的提升空间。
下次试试将map的集合改成下标。

```

## 题解
```golang

func lengthOfLongestSubstring(s string) (l int) {
	m := make(map[uint8]bool)
	for i := range s {
		for j := i; j < len(s); j++ {
			if _, ok := m[s[j]]; ok {
				if len(m) > l {
					l = len(m)
				}
				m = make(map[uint8]bool)
				break
			}
			m[s[j]] = true
		}
	}
	if l > len(m) {
		return l
	} else {
		return len(m)
	}
}

```

