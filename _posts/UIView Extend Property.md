---
title: UIView 扩展x,y,width,height,Size,Origin等属性
date: 2016-05-25 18:23:41
categories:
- 术业专攻
tags:
- iOS
- UIView
- Category
---
【将来的你 会感谢现在拼命的自己】
<!-- more -->

在定义一个view组件的位置时，大家都知道怎么设置该view组件的位置，是的我们通过view.frame来设置，代码如下：
{% codeblock lang:objc %}
[view.frame = CGRectMake(x,y,width,height)];
{% endcodeblock %}

当我们单独对view.frame.size进行赋值时，需要创建临时变量tempSize：
e{% codeblock lang:objc %}
CGSize tempSize = view.frame; 
temp.size = (CGSize){350,30};
view.frame = tempSize;
{% endcodeblock %}

而不能通过如下方式设置：以下方式无法通过编译，编译器会报错：**Expression is not assignable**;
{% codeblock lang:objc %}
view.frame.size = (CGSize){350,30}; //这个会报错，无法对size赋值
{% endcodeblock %}
>注意：view.frame.origin.x、view.frame.origin.y、view.frame.size.width、view.frame.size.height情况与上述类似，都不能直接赋值，具体原因查看[*Expression is not assignable*](http://reclusegu.github.io/2016/05/25/Expression-is-not-assignable/)。

通过上面的代码会发现，不能直接设置坐标和宽度高度的值，非常麻烦，若果可以直接一点，比如：
{% codeblock lang:objc %}
view.x = 0;
view.y = 0;
view.width = 100;
view.height = 100;
{% endcodeblock %}
这样的方式访问会简单，于是可以通过扩展UIView类，提供这些属性供用户直接访问。
新建Objective-C File选择Category，命名为Extension，继承自UIView；
**UIView + GUExtension.h**代码如下
{% codeblock lang:objc %}
#import <UIKit/UIKit.h>

@interface UIView (GUExtension)

@property(nonatomic,assign) CGFloat x; //坐标x
@property(nonatomic,assign) CGFloat y; //坐标y
@property(nonatomic,assign) CGFloat width; //宽度
@property(nonatomic,assign) CGFloat height; //高度
@property(nonatomic,assign) CGSize size; //大小
@property(nonatomic,assign) CGPoint origin; //位置
@property(nonatomic,assign) CGFloat centerX; //中心点x
@property(nonatomic,assign) CGFloat centerY; //中心点y

@end
{% endcodeblock %}

**UIView + GUExtension.m**代码如下：
{% codeblock lang:objc %}
#import "UIView+GUExtension.h"

@implementation UIView (GUExtension)

- (void)setX:(CGFloat)x
{
    CGRect frame = self.frame;
    frame.origin.x = x;
    self.frame = frame;
}

-(CGFloat)x
{
    return self.frame.origin.x;
}

-(void)setY:(CGFloat)y
{
    CGRect frame = self.frame;
    frame.origin.y = y;
    self.frame = frame;
}

-(CGFloat)y
{
    return self.frame.origin.y;
}

-(void)setWidth:(CGFloat)width
{
    CGRect frame = self.frame;
    frame.size.width = width;
    self.frame = frame;
}

-(CGFloat)width
{
    return self.frame.size.width;
}

-(void)setHeight:(CGFloat)height
{
    CGRect frame = self.frame;
    frame.size.height = height;
    self.frame = frame;
}

-(CGFloat)height
{
    return self.frame.size.height;
}

-(void)setSize:(CGSize)size
{
    CGRect frame = self.frame;
    frame.size.width = size.width;
    frame.size.height = size.height;
    self.frame = frame;
}

-(CGSize)size
{
    return self.frame.size;
}

-(void)setOrigin:(CGPoint)origin
{
    CGRect frame = self.frame;
    frame.origin.x = origin.x;
    frame.origin.y = origin.y;
    self.frame = frame;
}

-(CGPoint)origin
{
    return self.frame.origin;
}

-(void)setCenterX:(CGFloat)centerX
{
    CGPoint center = self.center;
    center.x = centerX;
    self.center = center;
}

-(CGFloat)centerX
{
    return self.center.x;
}

-(void)setCenterY:(CGFloat)centerY
{
    CGPoint center = self.center;
    center.y = centerY;
    self.center = center;
}

-(CGFloat)centerY
{
    return self.center.y;
}
@end
{% endcodeblock %}
