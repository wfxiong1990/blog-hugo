---
title: 用beautifulsoup解析网页
date: 2014-11-19 09:37:59
tags: [python,beautifulsoup]

---

## 关于BeautifulSoup
BeautifulSoup是一个能抓取网页内容并解析的python类库，是学习python写爬虫的一个捷径，官网地址<http://www.crummy.com/software/BeautifulSoup/>。

![](http://www.crummy.com/software/BeautifulSoup/10.1.jpg)

有时候接触到一个新的技术或者模块，不是发自内心的求知欲而是被官网上的配图给萌cry才开始学习BeautifulSoup的。官网很简单直白地告诉你：
>You didn't write that awful page. You're just trying to get some data out of it. Beautiful Soup is here to help. Since 2004, it's been saving programmers hours or days of work on quick-turnaround screen scraping projects.

我写网页基础为0，但是想去解析网页内容，方便阅读（其实我开始想写一个iOS app来实现的，结果鬼使神差用python实现了，现在借用BeautifulSoup的思路来在iOS实现，半成品中）

BeautifulSoup语法很简洁，能很方便地抽取出tag，atrrs，text等网页元素，堪称py web 程序员必备神器。

## 解析网页
我要解析的是复旦的BBS手机版，以<http://bbs.fudan.edu.cn/m/bbs/tcon?board=Auto&f=3085889602>这个网址为例，web显示出来的效果![](http://hackerxiong.qiniudn.com/火狐截图_2014-11-20T06-59-49.365Z.png) 我需要解析的就是每一条帖子的主题，发信人，发信人昵称（括号里面的），发帖时间，发帖内容，然后保存在一个自定义的类面，然后以json输出。

以上面网址的网站为例，首先，我需要解析出它每个帖子的标题，如果iOS app的话可以直接用xpath路径去拿到该节点，可惜BreautifulSoup不支持，但是也有替代的方法，比如findall：

```
soup.find_all("title")
# [<title>The Dormouse's story</title>]

soup.find_all("p", "title")
# [<p class="title"><b>The Dormouse's story</b></p>]

soup.find_all("a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.find_all(id="link2")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

```

第一种方法用来寻找所有的`<title>`节点，第二种是寻找所有class=title的`<p>`节点，第三种是寻找所有节点，满足节点里面的id attribute＝“link2”的节点。

期待开发更多好玩好用的python脚本~