---
title: 【LeetCode算法训练】 判断字符串是否是回文
date: "2014-11-04 22:45:29"
tags: [python,leetcode]

---
## 什么是LeetCode
LeetCode <http://weibo.com/leetcode>已是一个针对程序员招聘的颇具口碑的准备面试平台。虽然主要针对北美市场，但是内容也能很好的帮助大部分国内的IT面试者。

LeetCode上面有很多算法可以作为平时的脑力训练，适当做一做也可以提升程序猿打怪的经验值。

最近酷壳网的[左耳朵耗子](http://weibo.com/haoel) 在酷壳公布了所有LeetCode算法的答案（C++版）。为了追随大神的脚步，我决定一步步地用python来实现（实在做不出来可以抄一下思路么，一秒回到毕业前啊～）。

鄙人今年已经是第二个本命年了，有一种墙裂的不祥预感，俗称中年危机，所以要做做算法适当消遣来缓解压力。

## 题目解析
题目如下：

	Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

	For example,
	"A man, a plan, a canal: Panama" is a palindrome.
	"race a car" is not a palindrome.

Palindrome 直译过来叫回文。什么是回文？~~大波美人鱼人美波大~~，一句话解释，简单暴力，就是从左读和从右读是一样的，那程序思路就简单了，把字符串分成字符的数组，第一个和倒数第一个比较，第二个和倒数第二个比较，依此类推。

python里面字符串就是一个由字符组成的序列，所以字符串和序列不需要转换大部分方法可以通用。

## 难度★☆☆☆☆
如果你要鄙视我，这种算法都要写博客，请`ctrl+w`，不用谢。

## 代码示例
如下所示：

```python
# coding=UTF-8
__author__ = 'weifeng'


class Solution:
    def isPalindrome(self, s):
        sumstr = []
        for item in s:
            if not item.isalpha() or not item.isalnum():#判断是否是数字或者字符
                continue
            else:
                sumstr += item.lower()
        if sumstr.__len__() > 0:
            i = 0
            while i < sumstr.__len__() / 2:
                if sumstr[i] != sumstr[sumstr.__len__() - 1 - i]:
                    return False
                i += 1
            return True
        return False


if __name__ == '__main__':#主函数
    solu = Solution()
    validStr = u"A man, a plan, a canal: Panama"
    result = solu.isPalindrome(validStr)
    print result

```

## 小结
算法不难，但是如果是C#或者java的话可能要注意输入的字符串为空的情况，python可以直接转为长度为0的序列，目测不存在报错的问题。