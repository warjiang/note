---
title: KKLog使用 
tags: iOS,log
---
1. 安装
    强烈推荐使用pod安装。(使用cocoapod，首先你得安装ruby,在用gem install cocoapod,如果gem 安装速度太慢,可以使用淘宝的ruby源)
    `pod 'KKLog'`([KKLog的github地址](https://github.com/Coneboy-k/KKLog))
2. 使用
    1. 在AppDelegate.m里面
        * 加上#import "KKLog.h"
        * 加上函数
          ```iOS
          void uncaughtExceptionHandler(NSException *exception)
          {
            [KKLog logCrash:exception];
          }
          ```
        * 在didFinishLaunchingWithOptions中添加KKLog的初始化函数
        ```iOS
        // signExceptionHandler
        NSSetUncaughtExceptionHandler(&uncaughtExceptionHandler);
        // install KKlog
        [KKLog logIntial];
        // set logLevel
        [KKLog setLogLevel:LOGLEVELE];
        ```
    2. 在需要打log得地方就可以使用
        ```iOS
        先引用#import "KKLog.h"
        [KKLog logI:@"info"];
        [KKLog logE:@"error"];
        [KKLog logW:@"worring"];

        ```
    ## 说明
    **KKLog**中日志分为五个等级
    ```
    LOGLEVELV = 0,  //wend
    LOGLEVELD = 1,  //Debug
    LOGLEVELI = 2,  //Info
    LOGLEVELW = 3,  //Warning
    LOGLEVELE = 4,  //Error
    ```
    如果配置日志等级为`LOGLEVELE`,则`LOGLEVELW、LOGLEVELI、LOGLEVELD、LOGLEVELV`等级的日志均不显示。