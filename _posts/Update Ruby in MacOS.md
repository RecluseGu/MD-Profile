---
title: Mac OS 中升级Ruby
date: 2016-08-09 13:37:58
categories:
- 术业专攻
tags:
- iOS
- Mac
- Ruby
---
【洪荒之力】
<!-- more -->

在安装CocoaPods时，要求Ruby的版本高于2.2.2，如下图所示；而系统的Ruby的版本只有2.0。于是就查了些资料，由于网速和其他错误，也花了不少时间，在这里给大家做个总结。在这里使用RVM升级Ruby，RVM可以让你有多个版本的Ruby，并且可以自由切换，过程如下：

![](http://upload-images.jianshu.io/upload_images/2013105-cdbccba8719ce34f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.安装Homebrew

>在安装Homebrew的时候，通常会使用以下命令来安装，但是在我使用该命令安装的时候时常会有错误，例如没有文件权限(permission denied)，返回400等等，于是我就选用了手动安装Homebrew，我也推荐使用该方法：

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

>手动安装Homebrew，安装时要确保要移动到的目录文件中的相同文件要被移除：

* clone brew项目到本地

```
$ cd ~    //在根目录（强迫症）
$ git clone https://github.com/Homebrew/brew   //克隆项目到根目录
```
* 移动文件

```
$ cd brew   //进入brew文件夹中
$ sudo mv bin/brew /usr/local/bin    //移动brew到/usr/local/bin目录中
$ sudo mv Library /usr/local    //移动Library到/usr/local目录中
$ sudo mv share /usr/local    //移动share到/usr/local目录中
$ cd ..
$ rm -rf brew    //删除多余的brew文件(强迫症)
```
* 查看安装是否成功，使用以下命令，如果如下所示，说明安装成功！

```
$ brew
```

![](http://upload-images.jianshu.io/upload_images/2013105-d9280e61d062800f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.安装rvm

```
$ curl -L get.rvm.io | bash -s stable
//安装完成后装载rvm
$ source ~/.rvm/scripts/rvm
```

3.查看版本，测试是否安装成功

```
$ rvm -v    //如果没有成功，建议彻底退出Terminal再试试
```

4.安装Ruby:相关命令

```
$ rvm list known   //列出rvm可安装的Ruby版本信息

$ rvm install 2.2.2  //安装2.2.2版本

$ rvm use 2.2.2 --default  //设置2.2.2版本为默认

$ rvm list  //查看已安装的Ruby版本

$ rvm remove 2.2.2  //卸载已安装的2.2.2版本
```
安装成功如下所示：
![](http://upload-images.jianshu.io/upload_images/2013105-b5a5d0257905e3f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.其他
Ruby升级完成后，可能会遇到openssl问题，例如安装CocoaPods时会有如下错误，提示openssl没有安装，如图所示：
![](http://upload-images.jianshu.io/upload_images/2013105-68c64fa1d746a9ef.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
解决方案：
* 删除所有，重新安装；
* 安装openssl

```
$ rvm pkg install openssl
$ rvm reinstall 2.2.2 --with-openssl-dir=/usr/local
```
>安装完之后，可能还会有证书问题，错误如下：

```
Faraday::SSLError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
from ruby/2.2.0/net/http.rb:923:in `connect'
```
>解决方案：

```
$ cd $rvm_path/usr/ssl
$ sudo curl -O http://curl.haxx.se/ca/cacert.pem
$ sudo mv cacert.pem cert.pem
```