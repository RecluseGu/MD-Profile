---
title: 在项目中添加PCH文件
date: 2016-07-19 17:48:35
categories:
- 术业专攻
tags:
- iOS
- PCH
- 自定义Log
- 颜色
---
>在Xcode6之前，创建一个新工程是Xcode会在Supporting files文件夹下自动创建一个“工程名-Prefix.pch”文件，pch文件的内容能被项目中其他所有文件访问，它是一个预编译文件。

<!-- more -->
![](http://ww1.sinaimg.cn/large/b36cd9dbgw1f5zf70wfo8j20cu08emxf.jpg)

## PCH文件的作用
> 1. 存放整个项目中使用的上得宏定义，即全局的宏
> 2. 存放整个项目中所有文件都用得上的头文件

## Xcode中添加PCH文件
1. Command + N，打开新建文件窗口：依次选择iOS->other->PCH File
![](http://ww3.sinaimg.cn/large/b36cd9dbgw1f5zeydzugpj214k0ssdko.jpg)

2. 在工程的Targets中的Build Settings搜索Prefix Header
![](http://ww1.sinaimg.cn/large/b36cd9dbgw1f5zeyxm9i8j21cm0s2n4s.jpg)

3. 在Prefix Header右边双击，添加刚刚创建的pch文件的路径（右击pch文件->Show in Finder->将文件拖入弹出的白框内）
![](http://ww4.sinaimg.cn/large/b36cd9dbgw1f5zez4cw8rj21340fctc1.jpg)

## 添加自定义LOG、颜色宏定义
>PCH文件中添加代码

{% codeblock lang:objc %}
//
//  PrefixHeader.pch
//  GUBaiSI
//
//  Created by 永亮 谷 on 7/19/16.
//  Copyright © 2016 Recluse. All rights reserved.
//
#ifndef PrefixHeader_pch
#define PrefixHeader_pch

#ifdef __OBJC__

//自定义LOG
#ifdef DEBUG
#define GYLLog(...) NSLog(__VA_ARGS__)
#else
#define GYLLog(...)
#endif

//颜色
#define GYLCOLORA(r,g,b,a) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:(a)/255.0]
#define GYLCOLOR(r,g,b) GYLCOLORA(r,g,b,255)
#define GYLRANDOMCOLOR GYLCOLOR(arc4random_uniform(255),arc4random_uniform(255),arc4random_uniform(255))

#endif

#endif /* PrefixHeader_pch */
{% endcodeblock %}