---
title: 用python实现图灵聊天机器人的接口
date: "2014-10-25 00:13:03"
tags: [python,]

---

## 码这坨代码目的
在知乎上看到，[微信公众平台的聊天机器人是怎么搭建？](http://www.zhihu.com/question/20492916)的主题,对图灵机器人感兴趣起来，官方有php和java的实现，那么，python的简单实现，就我了。

## 开始码代码啦
首先，需要import一下urllib2这个组件，用于以GET的方式请求。
发请求的部分：

```python
req = urllib2.Request(url)
```

收到请求

```python
res_data = urllib2.urlopen(req)
res = res_data.read()
```

收到的是以字符串的形式，当然也可以转换成字典或者json，个人比较喜好json（一种类xml的储存和交换文本信息的语法），那么再import一下json的库

```python
dic=eval(res)  #转换为字典
myJson2=json.loads(res)  #转换为json
```

好了，大功告成，但是问题来了，如何在命令行里面一直保持程序能接受到用户输入的状态，经stackoverflow的指点，用while True 然后如果用户直接回车那么break出来。最后程序如下，太短了，自己都看不下去了。。。
ps: 根据图灵机器人给出的借口文档，如果请求的链接里面带userid的话会联系上下文返回，所以建议加上。

```python
# coding=UTF-8
import json
import urllib2

url = "http://www.tuling123.com/openapi/api?key=09e28a9f654a341ad11601cd0e1bf44_&userid＝hackerxiong&info="  #key的最后一位省略了，想知道的请留言找我推荐注册哈；输入参数info就是要请求的内容
while True:  ##一直保持循环输入状态
    entered = raw_input("Please enter your input or leave a blank line to quit: \n")  ##拿到用户的输入，默认是string的类型
    if not entered:
        break  ##直到空回车break出循环
    url+=entered
    req = urllib2.Request(url)
    # print req
    res_data = urllib2.urlopen(req)
    res = res_data.read()  ##拿到的响应也是string类型
    # print res
    dic=eval(res)  #转换为字典
    myJson2=json.loads(res)  #转换为json
    print(res+"\n")
```

最后，来看看效果如何，哈哈哈（我输了好多次才有这效果，看来机器人还是不够智能）![](http://ww1.sinaimg.cn/large/6788d2b1jw1elmpnt5rfqj20zk0dfad2.jpg)

附上常用的返回code 的说明，其他的详见：<http://www.tuling123.com/openapi/cloud/api.jsp?section=8>

| code | 说明 |
 ----------------- | ----------------- 
| 100000 | 文本类数据 |
| 200000| 网址类数据 |
| 40002 | 请求内容为空 |
| 40007|服务器数据格式异常|


## Todolist

貌似api接入的效果不太好，问了几个问题后开始有重复的回复出现，不知道是我发请求不对还是他们服务器返回就是这样子（不要找借口），有空再研究研究。