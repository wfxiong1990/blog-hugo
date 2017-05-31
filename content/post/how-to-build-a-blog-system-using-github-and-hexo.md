---
title: 利用GitHub Pages 和 Hexo 搭建静态博客
date: 2014-10-12 00:51:20
tags: [Hexo, Git]

---

无意中在知乎上看到了这样一个话题：[如何在 GitHub 上写博客？](http://www.zhihu.com/question/20962496) 于是开始自己捣腾，现在分享一下自己搭建博客的一点心得和笔记。

## GitHub Pages

给我的感觉就是，给我一个index.html，我能给你变出一个世界，其实不管什么语言什么架构搭建的项目，只要把index.html 放上去，并且本地能正常运行，这就是**静态网页程序**。那么github pages就能显示出你想要的网页来，一般github pages 的地址是`yourname.github.io`,*yourname*是你的github用户名，貌似一个github账号对应一个github pages，如果需要从自己的域名进入，那么只需要在项目的source目录下加一个CNAME文件即可，如果是Mac下面或者Linux的环境，一行命令搞定：

`vim CNAME` 或者`touch CNAME`

记得用vim的时候记得:wq 保存退出。

> ps:由于缓存的原因，第一次上传github pages 项目的时候需要等待几分钟左右，才能在yourname.github.io上看到。

## Git

说到GitHub，顾名思义，git是一个分布式的版本控制系统，这里不详细描述，hub在geek的心里就是集线器的意思吧，所以要在github上写博客，首先要学会用git，git不需要很深入的学习，但基本的命令还是要掌握的，这里推荐一下[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743858312764dca7ad6d0754f76aa562e3789478044000)。

首先你的电脑上要有git环境，如果输入`git --version`或者`whois git`如下图所示![git in terminal](http://ww3.sinaimg.cn/mw690/6788d2b1jw1elr9bsup7zj20jf0fpwg2.jpg)，那就是ok的，否则，需要在[这里](http://git-scm.com/downloads)下载对应系统的git安装包。

Git的基础操作很简单，无非是`git init` ,`git add`,`git commit`,`git status`,`git push`和`git pull`，对应的操作是当前文件夹初始化、把文件纳入版本管理，提交更改，检查状态，push status的变化到远程repository，把远程repository的更新拉下来，但是git 最牛的地方在对分支的管理，这方便我还掌握不够，大家可以参考上文中提到的廖雪峰的Git教程。

## Hexo

[Hexo](http://hexo.io/)是一个基于Node.js的静态博客程序，可以方便的生成静态网页托管在github和Heroku上。顾名思义，它是基于Node.js的，所以先要有Node.js的环境，[安装教程在此](http://www.infoq.com/cn/articles/nodejs-npm-install-config)。由于本人的macbook上安装了[Homebrew](http://brew.sh/index_zh-cn.html)，所以安装node只需要以下命令：

```
$ brew update
$ brew install node
```
不得不赞，macbook真的是开发者最好的玩具，比逼格更有生产力啊。
如果是windows，如果在cmd命令行里面输入`npm --version`没有显示，那么去[这里](http://nodejs.org/download/)下载nodejs的安装包，安装之。

Hexo虽然基于Node.js但是我们不需要了解Node.js编程，需要的只是4条命令：

```
$ hexo g  ##生成静态博客的public文件夹，也就是要上传到GitHub Pages的那部分文件
$ hexo s  ##运行本地的博客程序，比如你设置的端口号是4000的话，浏览器打开localhost:4000 就可以看到你搭建的博客是什么样子的了
$ hexo d  ##上传本地的修改到远程仓库 前提是你在_config.yml里面设置了git仓库的路径和分支
$ hexo n  ##新建一篇博客，就是new一个*.md的markdown文件，如果是在Mac下面可以直接用Mou程序打开
```
看上去简单吧～

推荐本地运行的时候把日志打开，这样哪里缺文件配置有问题一目了然。

```
$ logger: false
$ logger_format: dev
```
记住至少要在冒号后有一个空格，博客才能正确运行。

## Hexo主题优化

本博客采用了[Pacman的主题](https://github.com/A-limon/pacman)。[介绍在此](http://yangjian.me/pacman/hello/introducing-pacman-theme/)，不得不说，这个主题很漂亮，给人眼前一亮的感觉。用这个主题只要把github上的资源通过`git clone`到`本地的hexo目录\themes`下面，改一下hexo的_config.yml：

```
  theme: pacman
  ```

就可以了。

## Markdown
Markdown简直是程序猿的word编辑器，wikipedia里面介绍如下：

> Markdown 是一种轻量级标记语言，创始人为約翰·格魯伯（John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。

Markdown的学习可以参考作业部落的[CMD Markdown](https://www.zybuluo.com/mdeditor)在线的Markdown编辑器，很快就能上手的。

---
## 域名申请和绑定

很幸运，我通过github的student package 拿到了namecheap的一年 顶级域名 `xwf.me`的使用权，[申请方法](http://www.yd631.com/namecheap-free-me/)。拿到后把该域名的URL Forwarding 改成如下即可：
![image](http://ww4.sinaimg.cn/mw690/6788d2b1gw1elm37g1b7gj20ju07zdgr.jpg)

具体教程[戳这里](http://davidensinger.com/2013/03/setting-the-dns-for-github-pages-on-namecheap/)。

## 小结
自己搭建博客还是很拼的，各种google，百度还是算了，推荐一个网站：[通天塔](http://tmd123.com)，把google镜像到AWS亚马逊云上面，速度上还是靠谱的，~~隐私问题，反正应该比百度靠谱吧，我也不知道~~。
能有独立域名，网站还很干净，没广告，markdown熟练后也很方便，写博客也能慢慢改进自己描述问题不清楚，无法正确表达观点的问题。理科生，怀才不遇的大都连自己会啥都说不清楚。

好了，本篇就到这里吧，谢谢观赏，我不是[叫兽易小星](http://weibo.com/jiaoshoutv)，我从不做广告～