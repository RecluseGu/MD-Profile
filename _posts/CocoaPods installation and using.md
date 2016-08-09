---
title: CocoaPods安装与使用
date: 2016-08-09 13:33:39
categories:
- 术业专攻
tags:
- iOS
- CocoaPods
---
在我们项目开发的过程中，时不时的会用到第三方库，并且在一个项目中会用到多个第三方库，于是我们可以使用CocoaPods来安装和管理第三方库，这使我们在整个开发环境中对第三方库的版本管理非常方便。 CocoaPods 的原理是将所有的第三方库都放到另一个名为 Pods 项目中，并且让主项目依赖 Pods 项目，于是，源码管理工作都从主项目移到了 Pods 项目中。本文中我将介绍如何安装和使用CocoaPods。

<!-- more -->

## CocoaPods 安装
1.升级Ruby环境

``` 
$ sudo gem update --system
```
环境升级的过程比较慢，静心等待吧（不要吐槽我的网速渣）！

2.使用淘宝的RubyGems镜像来代替官方版本

```
$ gem sources --remove https://rubygems.org/
//等到移除后再敲入以下命令
$ gem sources -a https://ruby.taobao.org/
```
验证Ruby镜像只有taobao，使用命令：

```
$ gem sources -l
```
出现以下信息说明安装成功，具体可以查看官方文档[https://ruby.taobao.org](https://ruby.taobao.org/)：
![](http://upload-images.jianshu.io/upload_images/2013105-80742f8aa900dcce.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.安装CocoaPods

* 系统在10.11以下：

```
$ sudo gem install cocoapods
```

* 系统在10.11以后

```
$ sudo gem install -n /usr/local/bin cocoapods
//执行完后
$ sudo xcode-select --switch /Applications/Xcode.app
```

在安装时，可能会出现错误，提示需要2.2.2以上版本Ruby，我在安装时就碰到这个错误，若遇到同样问题，请参考[MAC OS Ruby升级](http://www.jianshu.com/p/a575aff064e3)。

输入以下完成安装：

```
$ pod setup
```
`注意：`
在执行`pod setup`的时候会输出`Setting up CocoaPods master repo`，文件比较大，下载慢，等待的时间会非常久，还可能会出现错误：
![](http://upload-images.jianshu.io/upload_images/2013105-342db580c87dc40e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
执行`pod setup`其实是CocoaPods会将pod spec索引文件信息更新到~/.cocoapods目录下，可以通过`du -sh *`查看下载进度。有一位大神在国内的gitcafe、oschina上建立了CocoaPods的索引库镜像，因此可以使用国内的镜像，这样会快很多，执行步骤如下：
```
$ pod repo remove master
$ pod repo add master https://gitcafe.com/akuandev/Specs.git
$ pod repo update
```
将`https://gitcafe.com/akuandev/Specs.git`替换成`http://git.oschina.net/akuandev/Specs.git`就是oschina上的索引库镜像。

以下界面说明setup成功：
![](http://upload-images.jianshu.io/upload_images/2013105-955a78f4a9ecedb5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## CocoaPods 使用
1.使用search搜索类库名
```
$ pod search AFNetworking
```
效果如下：
![](http://upload-images.jianshu.io/upload_images/2013105-df64bea95768d4d5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2.新建项目CocoaPodsSample，切换到项目根目录，并建立Podfile文件
```
$ cd "your project home"
$ touch Podfile
```
效果如下：
![](http://upload-images.jianshu.io/upload_images/2013105-d1797a71263317a3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3.使用vim编辑并保存pod file
```
$ vim Podfile
```
输入以下内容：
```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '9.0'

target 'CocoaPodsSample' do
pod 'AFNetworking', '~> 3.1'
end
```
4.下载安装AFNetworking
```
$ pod install
```
安装成功后：
![](http://upload-images.jianshu.io/upload_images/2013105-f1479690d64b6b3b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/2013105-aea918dd84cf0267.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5.双击`CocoaPodsSample.xcworkspace`打开工程，会发现AFNetworking已经安装好了，在ViewController中引入`#import "AFNetworking" `，再编译，不出错的话就可以使用啦。
![](http://upload-images.jianshu.io/upload_images/2013105-d77edefa0b84aa12.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>注意：在执行`pod install`之后会生成`Podfile.lock`文件，在官方文档中提到`Podfile.lock`应该放到按本控制中，因为`Podfile.lock`会锁定当前各依赖库的版本，之后如果多次执行`pod install`不会更改版本，`pod update`才会改Podfile.lock了。于是在多人协作的时候，能`防止第三方库升级时造成大家各自的第三方库版本不一致`，详情请查看[官方文档](http://guides.cocoapods.org/using/using-cocoapods.html#should-i-ignore-the-pods-directory-in-source-control)。