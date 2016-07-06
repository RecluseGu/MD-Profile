---
title: iOS如何在更新后第一次显示版本新特性
date: 2016-07-05 14:19:25
categories:
- 术业专攻
tags:
- iOS
- 版本更新
---
当我们使用iPhone时，所下载的APP时常会因为修改BUG、增加功能而需要更新，每次更新过后都会发现，当第一次打开APP时会有一个版本新特性来简单展示本次更新的内容。根据此需求来讨论下如何在iOS开发中实现这一功能。

<!-- more -->
## 问题相关
* 首先，考虑到是版本更新的问题，因此可以想到可以通过比较两次的版本号，通过判断得出APP是不是第一次打开，是否需要展示当前版本的新特性界面。

* 其次，版本号资源在Info.plist文件中，可以以*SourceCode*的方式打开，其中有一个是**CFBundleVersion**的字段，CFBundleVersion就是用来表示版本号。

## 代码思路
1. 在第一次打开APP时，将版本号存入用户偏好设置中。
2. 当再一次打开APP时，取出上次存入的版本号，与当前的版本号进行比较，如果是一样，则说明版本与上一次是一致的，不需要新特性引导，如果不一样，说明版本不同，需要打开新特性界面进行引导。

## 具体代码
>**AppDelegate.m**

{% codeblock lang:objc %}
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    application.statusBarHidden = NO;
    
    self.window = [[UIWindow alloc] init];
    self.window.frame = [UIScreen mainScreen].bounds;
    
    NSString *versionKey = @"CFBundleVersion";
    NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
    NSString *lastVersion = [defaults objectForKey:versionKey];//从用户偏好设置中获取上一次版本号
    
    NSString *currentVersion = [NSBundle mainBundle].infoDictionary[versionKey];//获取当前版本号
    
    if ([currentVersion isEqualToString:lastVersion]) {
        self.window.rootViewController = [[GUTabBarController alloc] init];
    }else{
        self.window.rootViewController = [[GUNewFeatureController alloc] init];
        
        [defaults setObject:currentVersion forKey:versionKey];//将当前版本号存储到偏好设置中
        [defaults synchronize];//立刻同步
    }
    
    [self.window makeKeyAndVisible];
    return YES;
}
{% endcodeblock %}