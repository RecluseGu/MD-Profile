---
title: Markdown 简介 & 使用
date: 2016-05-06 15:17:31
categories:
- 触类旁通
tags:
- Markdown
- IDE
---
> Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。 --[百度百科](http://baike.baidu.com/view/2311114.htm)

## Markdown 简介
Markdown具有一系列衍生版本，用于扩展Markdown的功能（如表格、脚注、内嵌HTML等等），这些功能原初的Markdown尚不具备，它们能让Markdown转换成更多的格式，例如LaTeX，Docbook。Markdown增强版中比较有名的有Markdown Extra、MultiMarkdown、Maruku等。这些衍生版本要么基于工具，如Pandoc；要么基于网站，如GitHub和Wikipedia，在语法上基本兼容，但在一些语法和渲染效果上有改动。
<!-- more -->

## Markdown 官方文档
> 在这里可以了解官方的Markdown的语法规则
 
* [Markdown英文语法说明](http://daringfireball.net/projects/markdown/syntax)
* [Markdown中文语法说明](http://wowubuntu.com/markdown/#list)

## 使用 Markdown 的优点
* 可以专注于位子的内容而非排版样式
* 能够导出HTML、PDF 和 md 文件
* 兼容所有的文本编辑器与字处理软件

## Mou
[Mou](http://25.io/mou/)是在Mac上十分好用的Markdown编辑器，支持实时预览功能，左边为编辑中的Markdown语言，右边实时生成预览效果。在Mou中，还支持通过偏好设置修改主题(Theme)和样式(CSS)。
![Mou](http://ww2.sinaimg.cn/large/b36cd9dbgw1f3lqyf1ihyj21cu100asy.jpg)

## Markdown 简单语法
### 标题
标题在每篇文章中都会使用到，在Markdown中，在文字前加 **#** 号，示例如下：
![标题](http://ww1.sinaimg.cn/large/b36cd9dbgw1f3lr1z1tnqj210c0lcgox.jpg)

### 引用
Markdown 标记区块引用使用 **>** 的方式，如下：
![引用](http://ww4.sinaimg.cn/large/b36cd9dbgw1f3lreahlyjj211o0ikdke.jpg)
在Markdown中也可以使用嵌套引用的方式，如下：
![嵌套引用](http://ww2.sinaimg.cn/large/b36cd9dbgw1f3lrg8h8s1j211o0ik0xi.jpg)

### 列表
Markdown支持有序列表和无序列表。  
无序列表可以使用星号、加号以及减号进行标记，如下：  
![无序列表](http://ww1.sinaimg.cn/large/b36cd9dbgw1f3lrkv5hjoj217w0patbk.jpg)  
有序列表使用数字加英文点号进行标记，如下：  
![有序列表](http://ww1.sinaimg.cn/large/b36cd9dbgw1f3lrmw9mioj20ri0ik0u1.jpg)

### 链接与图片
Markdown中链接与图片的语法类似，只是有无 **!** 的区别，如下：  
![图片和链接](http://ww4.sinaimg.cn/large/b36cd9dbgw1f3lrrd3uoxj218g0kugpi.jpg)  

### 强调
Markdown中使用星号（*）和下划线（_）进行标记，如下：  
![强调](http://ww2.sinaimg.cn/large/b36cd9dbgw1f3lrw8wx6yj218g0ku41b.jpg)  

### 代码块
使用反引号(`)进行标记，如下：
![代码块](http://ww4.sinaimg.cn/large/b36cd9dbgw1f3lsyvqfrfj218g0kuafz.jpg)
> **注意：**  
> 在Hexo中代码块可以使用其他方式，具体查看[此处](https://hexo.io/zh-cn/docs/tag-plugins.html)。

## 相关推荐
### 图床工具
用来上传图片获取url地址：
* [CloudApp](https://www.getcloudapp.com/)
* [围脖图床修复技术](http://weibotuchuang.sinaapp.com/)

### 相关文章阅读
* [Hexo官方文档](https://hexo.io/zh-cn/docs/)
* [马克飞象在线 Markdown 工具](https://maxiang.io/)
* [Mac下两款 Markdown 编辑器](http://www.jianshu.com/p/6c157af09e84)
* [图灵社区-怎样使用 Markdown](http://www.ituring.com.cn/article/23)
