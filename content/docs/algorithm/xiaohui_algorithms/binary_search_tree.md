---
weight: 1
bookFlatSection: true
title: 二叉查找树
---

# 二叉查找树

```shell
package main

import "fmt"

type Node struct {
	Key   int
	Val   interface{}
	Left  *Node
	Right *Node
}

type BST struct {
	Root *Node
}

func (t *BST) Insert(key int, val interface{}) {
	t.Root = t.insert(t.Root, key, val)
}

func (t *BST) insert(n *Node, key int, val interface{}) *Node {
	if n == nil {
		return &Node{Key: key, Val: val}
	}

	if key < n.Key {
		n.Left = t.insert(n.Left, key, val)
	} else if key > n.Key {
		n.Right = t.insert(n.Right, key, val)
	} else {
		n.Val = val
	}

	return n
}

func (t *BST) Search(key int) (interface{}, bool) {
	n := t.search(t.Root, key)
	if n == nil {
		return nil, false
	}
	return n.Val, true
}

func (t *BST) search(n *Node, key int) *Node {
	if n == nil {
		return nil
	}
	if key < n.Key {
		return t.search(n.Left, key)
	} else if key > n.Key {
		return t.search(n.Right, key)
	}
	return n
}

func preOrderTraversal(node *Node) {
	if node == nil {
		return
	}

	fmt.Println(node.Val)
	preOrderTraversal(node.Left)
	preOrderTraversal(node.Right)
}

func main() {
	bst := &BST{}
	bst.Insert(1, "a")
	bst.Insert(2, "b")
	bst.Insert(3, "c")
	bst.Insert(4, "d")
	bst.Insert(5, "e")

	preOrderTraversal(bst.Root)

	fmt.Println(bst.Search(2)) // Output: b true
	fmt.Println(bst.Search(6)) // Output: <nil> false
}

```