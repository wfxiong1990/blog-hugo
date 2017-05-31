---
title: 【LeetCode算法训练】 二叉树路径之和
date: 2014-11-05 16:35:07
tags: [python,leetcode]

---

## 二叉树结构
二叉树的定义如下：<http://zh.wikipedia.org/wiki/二叉树>。

	二叉树是每个节点最多有两个子树的树结构。通常子树被称作“左子树”（left subtree）和“右子树”（right subtree）。二叉树常被用于实现二叉查找树和二叉堆。

	二叉树的每个结点至多只有二棵子树(不存在度大于2的结点)，二叉树的子树有左右之分，次序不能颠倒。

二叉树是最容易叫人在白板上写算法的数据结构，基本上等同于中学语文里鲁迅的地位了。

首先，你得用python来实现一个二叉树节点的结构，实现方法如下：

```
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

python是动态脚本语言，写法上跟C#和java区别还是蛮大的，python初始化本节点的值为val，左右节点可由形参传入，也可不传。

比较一下二叉树C#的实现：

```
class Node
{
    public Node Left;
    public Node Right;
    public int Data;

    public Node(int data)
    {
        Data = data;
        Left =  null;
		Right = null;
    }
}

```

C#初始化节点时就只能用int值传入，但是python貌似都可以，只是后面解析起来要做预判了。

## 难度★★☆☆☆
写代码来递归好像木有自己数数字容易。


## 二叉树的节点插入
如果root节点的左节点为空，那么`root.left=Node(你的左节点值)`；如果左节点不为空，那么类似链表插入的方法：

```
tree = BinaryTree(newValue)
self.left = tree
tree.left = self.left
```

先让当前节点的左边指向新建的实例tree,然后让tree的左边指向当前节点原先左边的节点。瞬间脑补了当年C语言程序设计时链表插入时的无知和迷茫了。

由于本程序直接初始化的时候传入了左节点和右节点，所以这种方法仅供参考，[stackoverflow上的例子](http://stackoverflow.com/posts/25859412/revisions)。

## 题解
leetcode上题目如下：
	Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

	For example:
	Given the below binary tree and sum = 22,
	              5
	             / \
	            4   8
	           /   / \
	          11  13  4
	         /  \      \
	        7    2      1
	return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

首先要实现这个二叉树，我的方法如下：

```python
node = TreeNode(5, TreeNode(4, TreeNode(11, 7, 2)), TreeNode(8, 13, TreeNode(4, None, 1)))

```

根节点是5，左节点是个子二叉树，那么左节点是一个二叉树的类，其中左节点的值是4，那么左节点的二叉树的根节点为4，如果这个根节点的左节点又是二叉树的话，那么又是一个`TreeNode`的类。

要判断22是不是二叉树某条路径节点之和，这个二叉树简单，可以自己数数。
>	              5
>	             /
>	            4
>	           /
>	          11
>	            \
>	             2

很明显，这条路径节点之和就是22，所以如果sum=22，应该return true。
那么讲一下计算机怎么判断是否return true。

## 寻找所有二叉树的路径是否存在路径上节点和
基本思路是：如果该节点的value不等于给定的sum值，那么从左节点（如果有）开始，判断左节点为root节点的二叉树的是否存在路径之和等于sum-父节点的value。如果左节点没有该路径，那么切换到右节点，继续递归。

好了，基本的思路就是这样子的。下面是这部分的代码，并验证这个solution是否正确：


```python
# coding=utf-8
__author__ = 'wfxiong1990'


class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    # @param root, a tree node
    # @param sum, an integer
    # @return a boolean
    def hasPathSum(self, root, sum):
        if isinstance(root, TreeNode): #如果是节点是二叉树结构，遍历左右节点值
            result = sum - root.val
            if root.left and self.hasPathSum(root.left, result):
                return True
            if root.right and self.hasPathSum(root.right, result):
                return True
        elif isinstance(root, int): #如果节点是int类型，即最下面的节点，检查是否等于sum-该路径上面的所有其他节点值
            result = sum - root
            return result == 0
        return False


node = TreeNode(5, TreeNode(4, TreeNode(11, 7, 2)), TreeNode(8, 13, TreeNode(4, None, 1)))
solution = Solution()
result = solution.hasPathSum(node, 22)
print result

```

## 总结
二叉树，屡战屡崩溃，其实就是基本的递归，好吧，这一题我偷看了答案，又让我想起了前序遍历 中序遍历 后序遍历 的那些神奇的代码。