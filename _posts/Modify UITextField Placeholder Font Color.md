---
title: 修改UITextField占位符字体颜色的几个方法
date: 2016-07-27 13:31:38
categories:
- 术业专攻
tags:
- iOS
- UITextField
- Placeholder
- Font
---
最近看到几篇博客，说到Apple公司希望开发者们利用Xcode提供的storyboard来开发界面，并且在storyboard中提供了大量的功能，给开发带来方便，当然也有些问题目前仅用storyboard还是解决不了的。最近在学习的过程中，就修改UITextField的占位符文字颜色给大家做个总结。

<!-- more -->

修改UITextField的占位符文字颜色主要有三个方法：
### 使用attributedPlaceholder属性
{% codeblock lang:objc %}
@property(nullable, nonatomic,copy)   NSAttributedString     *attributedPlaceholder NS_AVAILABLE_IOS(6_0); // default is nil
{% endcodeblock %}

### 重写drawPlaceholderInRect方法
{% codeblock lang:objc %}
- (void)drawPlaceholderInRect:(CGRect)rect;
{% endcodeblock %}

### 修改UITextField内部placeholderLaber的颜色
{% codeblock lang:objc %}
[textField setValue:[UIColor grayColor] forKeyPath@"placeholderLaber.textColor"];
{% endcodeblock %}

**具体实现**
>给定场景，如在注册登录中，要修改手机号和密码TextField的placeholder的文字颜色。

**效果对比**
![使用前](http://ww3.sinaimg.cn/large/b36cd9dbgw1f68iwocywdj20fe05kglm.jpg)
![使用后](http://ww4.sinaimg.cn/large/b36cd9dbgw1f68ix24fszj20fc05qq31.jpg)

## 使用attributedPlaceholder
自定义GYLLoginRegisterTextField类，继承自UITextField；实现awakeFromNib()方法，如果使用storyboard，那么修改对应的UITextField的CustomClass为GYLLoginRegisterTextField即可，具体代码如下：
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
@implementation GYLLoginRegisterTextField

- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
    
    //修改占位符文字颜色
    NSMutableDictionary *attrs = [NSMutableDictionary dictionary];
    attrs[NSForegroundColorAttributeName] = [UIColor whiteColor];
    self.attributedPlaceholder = [[NSAttributedString alloc] initWithString:self.placeholder attributes:attrs];
}

@end
{% endcodeblock %}

## 重写drawPlaceholderInRect方法
与方法一同样，自定义GYLLoginRegisterTextField，继承自UITextField，重写drawPlaceholderInRect方法，后续相同，代码如下：
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"

@implementation GYLLoginRegisterTextField

- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
}

- (void)drawPlaceholderInRect:(CGRect)rect
{
    NSMutableDictionary *attrs = [NSMutableDictionary dictionary];
    attrs[NSForegroundColorAttributeName] = [UIColor whiteColor];
    attrs[NSFontAttributeName] = self.font;
    
    //画出占位符
    CGRect placeholderRect;
    placeholderRect.size.width = rect.size.width;
    placeholderRect.size.height = rect.size.height;
    placeholderRect.origin.x = 0;
    placeholderRect.origin.y = (rect.size.height - self.font.lineHeight) * 0.5;
    [self.placeholder drawInRect:placeholderRect withAttributes:attrs];
    
    //或者
    /*
    CGPoint placeholderPoint = CGPointMake(0, (rect.size.height - self.font.lineHeight) * 0.5);
    [self.placeholder drawAtPoint:placeholderPoint withAttributes:attrs];
    */
}

@end
{% endcodeblock %}

## 修改UITextField内部placeholderLaber的颜色
使用KVC机制，找到UITextField内部的修改站位文字颜色的属性：**placeholderLaber.textColor**，代码如下：
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
@implementation GYLLoginRegisterTextField

- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
    
    //修改占位符文字颜色
    [self setValue:[UIColor grayColor] forKeyPath@"placeholderLaber.textColor"];
}

@end
{% endcodeblock %}

>第三种方法比较简单，建议可以将此封装起来：扩展UITextField，新建category，添加placeholderColor属性，使用KVC重写set和get方法。有需要的同学可以[邮箱](mailto:reclusegu1218@gmail.com)联系。监听UITextField事件，可查看下一篇[文章]()。