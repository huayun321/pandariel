---
weight: 1
bookFlatSection: true
title: 二叉树
---

# 二叉树

```shell
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func preOrderTraversal(node *TreeNode) {
	if node == nil {
		return
	}

	fmt.Println(node.Val)
	preOrderTraversal(node.Left)
	preOrderTraversal(node.Right)
}

func inOrderTraversal(node *TreeNode) {
	if node == nil {
		return
	}

	inOrderTraversal(node.Left)
	fmt.Println(node.Val)
	inOrderTraversal(node.Right)
}

func postOrderTraversal(node *TreeNode) {
	if node == nil {
		return
	}

	postOrderTraversal(node.Left)
	postOrderTraversal(node.Right)
	fmt.Println(node.Val)
}

func main() {
	// 示例二叉树:
	//    1
	//   / \
	//  2   3
	//     /\
	//    4  5
	root := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, &TreeNode{4, nil, nil}, &TreeNode{5, nil, nil}}}
	preOrderTraversal(root) // 输出: [1 2 3 4 5]
}

```