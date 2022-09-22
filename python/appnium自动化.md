---
title: appnium自动化
description: 自动化操作手机app
published: 1
date: 2022-09-22T08:08:21.584Z
tags: appnium
editor: markdown
dateCreated: 2022-09-22T02:12:03.355Z
---

# appnium环境搭建
## java sdk环境配置
1. 下载java jdk 8u版本
去官网直接下载:https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html
2. 配置java jsk环境变量
+ 打开电脑系统高级配置
+ 配置JAVA_HOME变量
创建JAVA_HOME变量，变量值输入java jdk安装路径，例如：C:\Program Files\Java\jdk\bin
+ 配置path变量
编辑path变量，添加jdk bin 所在目录，例如：C:\Program Files\Java\jdk\bin
## Andriod sdk环境配置
1. 下载andriod sdk
安装包可从此处下载：[andriod sdk 下载](https://pan.baicai.blog/alist/%E8%BD%AF%E4%BB%B6%E5%AE%89%E8%A3%85%E5%8C%85/(%E5%BE%AE%E4%BF%A1-ceshi068)%E2%80%8Bandroid_sdk_test%E2%80%8B.7z)
2. 配置andriod sdk环境变量
将andriod sdk文件放到合适地方，需要配置ANDROID_HOME和path
+ 配置ANDROID_HOME
创建ANDROID_HOME变量，变量值为andriod sdk文件所在目录路径，例如：D:\enviroment\andriodsdk
+ 配置path
编辑path添加platform-tools和tools文件夹的路径，例如：D:\enviroment\andriodsdk\platform-tools,D:\enviroment\andriodsdk\tools
## 配置手机模拟器
1. 下载模拟器
这里下载夜神模拟器，直接去官网下载
2. 替换adb.exe和nox_adb.exe为androidsdk下的platform-tools的adb.exe.
## 下载appnium-client
``` shell
pip install Appium-Python-Client
```
## 使用