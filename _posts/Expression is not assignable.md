---
title: Expression is not assignable
date: 2016-05-25 20:29:57
categories:
- 术业专攻
tags:
- iOS
- Xcode
- Error
---
【别管那些纷纷扰扰，别让不开心的事，停下了脚步，就怕你不说，就怕你不做，别让遗憾继续，一切都还来的及--思念是一种病】

<!-- more -->
例如如下代码：没法通过编译，编译器会报错：**Expression is not assignable**
{% codeblock lang:objc %}
self.view.frame.size.height = 100;
{% endcodeblock %}

这句代码中self.view.frame是Objective-C(以下简称OC)语法，是用来读取view属性的frame属性，在OC中使用点语法访问属性是一种语法糖，所以self.view.frame会被转化成：
{% codeblock lang:objc %}
[[self view] frame];
{% endcodeblock %}
实际上这是OC种的消息传递。

frame属性是一个CGRect结构，frame.size.height是C语言的语法，即访问CGRect结构的中的size，size也是一个结构为CGSize，再访问CGSize中的height。则self.view.frame.size.height = 100;可以写成：
[[self view] frame].size.height = 100;

但是我们知道OC是对C语言的一个扩展，所以上面这句话会被转换为C语言的函数调用形式，即：
getFrame().size.height = 100;

在C语言中函数的返回值是一个R-Value（只能出现在等号的右边），不能直接赋值，因此，这就是编译器报错的原因。

可行的解决办法是，用一个临时变量保存这个函数的返回值：
上面的代码可以写成：
{% codeblock lang:objc %}
CGRect tempFrame = self.view.frame;
tempFrame.size.height = 100;
self.view.frame = tempFrame;
{% endcodeblock %}
根据OC的语法就只能写成上述代码，于是我们可以通过[UIView扩展](http://reclusegu.github.io/2016/05/25/UIView-Extend-Property/)来解决这个问题。
