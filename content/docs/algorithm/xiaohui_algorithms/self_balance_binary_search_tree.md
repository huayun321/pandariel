---
weight: 1
bookFlatSection: true
title: 平衡二叉树
---

# 平衡二叉树

```shell
package main

import "fmt"

type AVLTreeNode struct {
	key         int
	height      int
	left, right *AVLTreeNode
}

func NewNode(key int) *AVLTreeNode {
	return &AVLTreeNode{
		key:    key,
		left:   nil,
		right:  nil,
		height: 0,
	}
}

func GetHeight(node *AVLTreeNode) int {
	if node == nil {
		return 0
	}
	return node.height
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

// 定义左旋
func RotateLeft(root *AVLTreeNode) *AVLTreeNode {
	//当前节点的右节点会作为新树的根结点
	//当前节点root会作为根节点newRoot的左子树
	//如果新的树根，原来有左子树，则原来的左子树，会作为旧根节点的右子树
	newRoot := root.right
	t2 := newRoot.left
	newRoot.left = root
	root.right = t2

	//更新树高
	root.height = 1 + max(GetHeight(root.left), GetHeight(root.right))
	newRoot.height = 1 + max(GetHeight(newRoot.left), GetHeight(newRoot.right))

	return newRoot
}

// 定义右旋
func RotateRight(root *AVLTreeNode) *AVLTreeNode {
	newRoot := root.left
	t2 := newRoot.right
	newRoot.right = root
	root.left = t2

	//更新树高
	root.height = 1 + max(GetHeight(root.left), GetHeight(root.right))
	newRoot.height = 1 + max(GetHeight(newRoot.left), GetHeight(newRoot.right))

	return newRoot
}

// 定义获取平衡因子的函数
func GetBalance(node *AVLTreeNode) int {
	return GetHeight(node.left) - GetHeight(node.right)
}

// 定义插入节点的函数
func InsertNode(node *AVLTreeNode, key int) *AVLTreeNode {
	if node == nil {
		return NewNode(key)
	}

	if key < node.key {
		node.left = InsertNode(node.left, key)
	} else if key > node.key {
		node.right = InsertNode(node.right, key)
	} else {
		return node
	}

	// 更新树高
	node.height = 1 + max(GetHeight(node.left), GetHeight(node.right))

	//获取当前节点的平衡因子
	balance := GetBalance(node)
	//我们是否要调整这个树，是看平衡因子是不是绝对值大于1
	//LL型失衡
	if balance > 1 && GetBalance(node.left) > 0 {
		return RotateRight(node)
	}
	//LR型失衡
	if balance > 1 && GetBalance(node.left) < 0 {
		//先对node的左子树进行左旋
		node.left = RotateLeft(node.left)
		return RotateRight(node)
	}

	//RR型失衡
	if balance < -1 && GetBalance(node.right) < 0 {
		return RotateLeft(node)
	}

	//RL型失衡
	if balance < -1 && GetBalance(node.right) > 0 {
		//先对node的右子树进行右旋
		node.right = RotateRight(node.right)
		return RotateLeft(node)
	}

	return node
}

// 定义先序遍历
func PreOrderTraversal(node *AVLTreeNode) {
	if node == nil {
		return
	}

	fmt.Println(node.key)
	PreOrderTraversal(node.left)
	PreOrderTraversal(node.right)
}

//定义中序遍历

func InOrderTraversal(node *AVLTreeNode) {
	if node == nil {
		return
	}

	InOrderTraversal(node.left)
	fmt.Println(node.key)
	InOrderTraversal(node.right)
}

func Search(root *AVLTreeNode, key int) (node *AVLTreeNode, counter int) {
	cur := root
	for cur != nil {
		if key < cur.key {
			cur = cur.left
			counter++
		} else if key > cur.key {
			cur = cur.right
			counter++
		} else {
			return cur, counter
		}
	}

	return nil, counter
}

func test() {
	var root *AVLTreeNode
	root = InsertNode(root, 10)
	root = InsertNode(root, 20)
	root = InsertNode(root, 30)
	root = InsertNode(root, 40)
	root = InsertNode(root, 50)
	root = InsertNode(root, 60)
	root = InsertNode(root, 70)

	_, count := Search(root, 70)
	fmt.Printf("找了几次 %d \n", count)
	fmt.Println("=====打印先序遍历的结果 ======")
	PreOrderTraversal(root)
	fmt.Println("=====打印中序遍历的结果 ======")
	InOrderTraversal(root)
}

func main() {
	test()
}


```

### 资源
* [平衡二叉树(AVL树)](https://www.bilibili.com/video/BV1tZ421q72h/?spm_id_from=333.337.search-card.all.click&vd_source=5d0a312dde384d3900a4eed32188b8f2)
* [平衡二叉树（AVL树）](https://www.bilibili.com/video/BV1S94y1V7wL/?spm_id_from=333.337.search-card.all.click&vd_source=5d0a312dde384d3900a4eed32188b8f2)