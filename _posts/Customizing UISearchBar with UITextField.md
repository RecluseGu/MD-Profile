---
title: 使用UITextField自定义UISearchBar
date: 2016-06-29 13:19:33
categories:
- 术业专攻
tags:
- iOS
- UISearchBar
- UITextField
- Category
---
>有的时候，使用iOS自带的控件不符合我们的开发要求时，我们一般自定义控件来满足需求。在此处，我们通过创建一个GUSearchBar类，继承自UITextField类来自定义搜索框。

![I Believe you will come back!](http://ww3.sinaimg.cn/large/b36cd9dbgw1f5c2a6qcpdj20zc0neahy.jpg)

<!-- more -->
## 创建GUSearchBar
>代码如下：GUSearchBar.h

{% codeblock lang:objc %}
#import <UIKit/UIKit.h>
@interface GUSearchBar : UITextField

+(instancetype) searchBar;

@end
{% endcodeblock %}

>GUSearchBar.m

{% codeblock lang:objc %}
#import "GUSearchBar.h"

@implementation GUSearchBar

-(id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {

        self.background = [UIImage resizedImage:@"searchbar_textfield_background"];
        self.contentVerticalAlignment = UIControlContentVerticalAlignmentCenter;
        UIImageView *leftView = [[UIImageView alloc] init];
        leftView.image = [UIImage imageWithName:@"searchbar_textfield_search_icon"];

        leftView.width = leftView.image.size.width + 10;
        leftView.height = leftView.image.size.height;
        leftView.contentMode = UIViewContentModeCenter;
        
        self.leftView = leftView;
        self.leftViewMode = UITextFieldViewModeAlways;
        self.clearButtonMode = UITextFieldViewModeAlways;
        self.placeholder = @"搜索";
    }
    return self;
}

+(instancetype) searchBar
{
    return [[self alloc] init];
}

@end
{% endcodeblock %}

>注意：上述有类似leftView.width，leftView.height这样的语法是使用了自定义UIView的扩展分类，详情请点击[UIView扩展](http://reclusegu.github.io/2016/05/25/UIView-Extend-Property/)。

## 使用
{% codeblock lang:objc %}
GUSearchBar *searchBar = [[GUSearchBar alloc] init];
searchBar.width = 350;
searchBar.height = 30;
    
self.navigationItem.titleView = searchBar;
{% endcodeblock %}

## 使用效果
![GUSearchBar](http://ww3.sinaimg.cn/large/b36cd9dbgw1f5c2f4ghmtj20ks03mmx7.jpg)