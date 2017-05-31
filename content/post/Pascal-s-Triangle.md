---
title: 【LeetCode算法训练】 输出杨辉三角形
date : "2014-11-04 23:42:48"
tags : [python,leetcode]

---

## 什么是杨辉三角形
如图所示吧，暂时还没来得及学习MarkDown的数学公式的写法
![](http://ww3.sinaimg.cn/large/6788d2b1jw1elzekp4nnvj20pz0drtb5.jpg)

## 题解
本题目[英文说明](https://oj.leetcode.com/problems/pascals-triangle/)如下：

	Given numRows, generate the first numRows of Pascal's triangle.

	For example, given numRows = 5,
	Return

	[
	     [1],
	    [1,1],
	   [1,2,1],
	  [1,3,3,1],
	 [1,4,6,4,1]
	]

输入数字5，那么出来的就是行数为5的杨辉三角形。可以分解为一行一行来看，每一行的第N个元素都是上面一行的N-1个元素加上第N个元素，如果N-1或者N元素不存在，那么用0代替，那么应该用函数递归来实现咯。想通了就是这么简单，take it easy!

## 难度★☆☆☆☆
算法不难，但是要把所有的情况考虑清楚，并用python来实现，大家可以自己试试，注意几种临界情况，比如没有加小于**等于**导致程序没输出预想的结果。

## 代码示例
代码如下：

```python
# coding=UTF-8
__author__ = 'weifeng'


def generatechild(numRows): #生成杨辉三角形的第numRows行
    if numRows<1: #输入非法时做判断
        return None
    if numRows == 1: #第一行
        return [1]
    else:
        last = generatechild(numRows - 1) #递归
        i = 0
        reslist = []
        while i < numRows:
            val1 = 0
            val2 = 0
            if 0 <= i < last.__len__(): #很神奇地发现python可以实现 0<i<n这种写法，注意把i换成i-1的时候要打括号
                val1 = last[i]
            if i-1>=0 and i - 1 < last.__len__(): #还要注意等于的情况
                val2 = last[i - 1]
            reslist.append(val1 + val2)
            i += 1
        return reslist


def generate(numRows): #生成行数为numRows的完整杨辉三角形
    i=1
    result=[]
    while i<=numRows: #目前没有发现 for(i=0;i<count;i++)的写法，只能用while来代替
        result.append(generatechild(i))
        i+=1
    return result

lst = generate(5)
print lst
```

## 小结
考察递归的使用，其实只要面向对象编程，思路总归是一样的，注意输入小于1的情况，有个预判比程序报错来的更为优雅。
