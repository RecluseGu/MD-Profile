---
title: UITextFile监听事件的几种方法总结
date: 2016-07-27 16:45:20
categories:
- 术业专攻
tags:
- iOS
- UITextFiled
- Event
---
上一篇文章提到了如何修改UITextField的占位符文字颜色，接下来继续总结关于UITextFiled的监听事件的几个方法，同样，本文基于给定场景中：让占位符的颜色随着光标的切换而切换，如光标在手机号textField中，手机号显示为白色，而密码还是灰色，当切换到密码时，手机号为灰色，密码为白色。

<!-- more -->

**效果如下图**
![](http://ww4.sinaimg.cn/large/b36cd9dbgw1f68ks7ero0j20fk05gq30.jpg)
![](http://ww2.sinaimg.cn/large/b36cd9dbgw1f68ksgg8jij20fi05it8s.jpg)

### 使用addTarget方法
因为UITextField是继承自UIControl的，那么就可以使用addTarget方法来实现监听。
{% codeblock lang:objc %}
NS_CLASS_AVAILABLE_IOS(2_0) @interface UITextField : UIControl <UITextInput, NSCoding>

- (void)addTarget:(nullable id)target action:(SEL)action forControlEvents:(UIControlEvents)controlEvents;
{% endcodeblock %}

代码如下：
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
#import "UITextField+GYLExtension.h"

@implementation GYLLoginRegisterTextField
- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
    
    //修改默认占位符文字颜色，使用category扩展UITextField，添加placeholderColor属性
    self.placeholderColor = [UIColor grayColor];
    
    [self addTarget:self action:@selector(beginEdit) forControlEvents:UIControlEventEditingDidBegin];
    [self addTarget:self action:@selector(endEdit) forControlEvents:UIControlEventEditingDidEnd];
}

- (void)beginEdit
{
    self.placeholderColor = [UIColor whiteColor];
}

- (void)endEdit
{
    self.placeholderColor = [UIColor grayColor];
}
@end
{% endcodeblock %}

### 使用UITextFieldDelegate
>使用代理时需要注意，代理只能设置一次，在自定义TextField已经设置代理，其他地方就设置其代理会出错。

代码如下：
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
#import "UITextField+GYLExtension.h"
@interface GYLLoginRegisterTextField () <UITextFieldDelegate>

@end

@implementation GYLLoginRegisterTextField
- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
    
    //修改默认占位符文字颜色
    self.placeholderColor = [UIColor grayColor];
    
    self.delegate = self;
}

- (void)textFieldDidBeginEditing:(UITextField *)textField
{
    self.placeholderColor = [UIColor whiteColor];
}

- (void)textFieldDidEndEditing:(UITextField *)textField
{
    self.placeholderColor = [UIColor grayColor];
}

@end
{% endcodeblock %}

### 使用NSNotificationCenter

{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
#import "UITextField+GYLExtension.h"
@interface GYLLoginRegisterTextField () <UITextFieldDelegate>
@end
@implementation GYLLoginRegisterTextField

- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色
    //修改默认占位符文字颜色
    self.placeholderColor = [UIColor grayColor];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(beginEdit) name:UITextFieldTextDidBeginEditingNotification object:self];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(endEdit) name:UITextViewTextDidEndEditingNotification object:self];
}

- (void)beginEdit
{
    self.placeholderColor = [UIColor whiteColor];
}

- (void)endEdit
{
    self.placeholderColor = [UIColor grayColor];
}
@end
{% endcodeblock %}

### 使用UITextField内部方法
{% codeblock lang:objc %}
#import "GYLLoginRegisterTextField.h"
#import "UITextField+GYLExtension.h"
@interface GYLLoginRegisterTextField ()

@end
@implementation GYLLoginRegisterTextField
- (void)awakeFromNib
{
    self.tintColor = [UIColor whiteColor];      //设置光标颜色    
    //修改默认占位符文字颜色
    self.placeholderColor = [UIColor grayColor];   
}

- (BOOL)becomeFirstResponder
{
    self.placeholderColor = [UIColor whiteColor];
    return [super becomeFirstResponder];
}

- (BOOL)resignFirstResponder
{
    self.placeholderColor = [UIColor grayColor];
    return [super resignFirstResponder];
}
@end
{% endcodeblock %}

