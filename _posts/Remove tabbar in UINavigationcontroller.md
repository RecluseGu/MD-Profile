---
title: 当push的时候隐藏UINavigationController的TabBar
date: 2016-06-06 21:33:47
categories:
- 术业专攻
tags:
- iOS
- UINavigationController
- TabBar
---
忙碌的学期期末，都没有时间学习了。做作业，做作业，做作业。美洲杯，美洲杯，美洲杯。
![阿根廷 VS 智利 2:1](http://ww3.sinaimg.cn/large/0060lm7Tgw1f4mj0jkh13j31ay0g8ahe.jpg)
<!-- more -->

## Method One
通常一个UITabBarController需要管理多个UINavigationController，当从每一个UINavigationController中push到某一个ViewController，如果不做处理，会在push到的Controller中显示TabBar，第一个做法是设置ViewController隐藏TabBar，假设在tableView中选中某一行跳转到另一个控制器，代码如下：
{% codeblock lang:objc %}
-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    GUTestController *testVc = [[GUTestController alloc] init];
    testVc.view.backgroundColor = [UIColor redColor];
    testVc.hidesBottomBarWhenPushed = YES;
    [self.navigationController pushViewController:testVc animated:YES];
}
{% endcodeblock %}

>也就是说，每次跳转到某个控制器的时候都要设置新控制器的hidesBottomBarWhenPushed属性为YES。

## Method Two
上面提到了Method One的缺点，所以得有新的方法来解决上述问题：
>解决方案：
1. 创建自定义UINavigationController--GUNavigationController；
2. 重写UINavigationController的push方法；
3. 如果最先push进来的控制器不是栈底栈底控制器，设置控制器的hidesBottomBarWhenPushed属性的值为YES。

代码如下：
{% codeblock lang:objc %}
-(void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    if (self.viewControllers.count > 0) {
        viewController.hidesBottomBarWhenPushed = YES;
    }
    [super pushViewController:viewController animated:animated];  
}
{% endcodeblock %}