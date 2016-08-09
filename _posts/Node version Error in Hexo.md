---
title: Hexo使用的Node版本的问题
date: 2016-08-03 09:53:48
categories:
- 触类旁通
tags:
- Hexo
- Node
- Homebrew
---
前几天由于安装CocoaPods的时候，把Homebrew卸载重装了，于是之前安装的node没有了，于是再次安装了下node，但是在使用hexo命令的时候出错了，给大家说下原因和解决方案。

<!-- more -->
错误截图如下：
![](http://ww3.sinaimg.cn/large/b36cd9dbgw1f6gbtg6jaej20zg0ui4fx.jpg)

造成这个错误的原因是Node的版本问题，但是报错的最后命令还是执行了，当我再次安装的时候使用的是`brew install node`，使用该命令是安装最新版本的Node，于是我安装了6.3的，在Node6.3版本中对很多模块已经不再支持了，推荐安装Node 4.4.7(LTS)，这个版本适合绝大多数人。

```
$ brew install homebrew/versions/node    //查看可安装的版本

$ brew install homebrew/versions/node4-lts  //安装指定版本

//接下来安装Hexo所需要的模块
$ npm install hexo-cli -g

$ cd ~/Desktop/Hexo   //切换到博客根目录
$ npm install

$ hexo server  //可以继续使用而不用看到错误啦

```

相关截图：
![查看node版本](http://ww3.sinaimg.cn/large/b36cd9dbgw1f6gc71sk0dj20z80is7do.jpg)

![安装指定版本](http://ww2.sinaimg.cn/large/b36cd9dbgw1f6gc7loiglj20z809gtdq.jpg)

![继续使用啦 ~ ~](http://ww3.sinaimg.cn/large/b36cd9dbgw1f6gc87incsj20nk05k0ua.jpg)