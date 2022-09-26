---
title: appnium自动化
description: 自动化操作手机app
published: 1
date: 2022-09-26T01:59:36.970Z
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
## 使用appium
1. 下载
官网上下载appium
2. 启动appium
启动appium可以看到连接地址代码中需要用到
3. 测试appium是否连接到模拟器
打开cmd输入：
``` shell
adb devices
```
# adb的使用
+ 进入手机系统
``` shell
adb shell
```
+ 重新连接手机
当无法连接手机时可执行以下操作：
``` shell
adb kill-server
adb start-server
```
+ 获取信息
获取手机APP信息以及当前正在操作的程序以及界面
``` shell
adb shell getprop ro.build.version.release
adb shell dumpsys window windows | findstr mFocuseApp 
```
+ 其他命令
文件传输、软件安装卸载、获取其他信息命令执行百度或者使用电脑操作就可完成
# 入门案列
连接手机并打开指定APP然后关闭退出
``` py
from appium import webdriver
import time

# 连接移动设备所必须的参数
desired_caps = {}

# 当前要测试的设备的名称
desired_caps["deviceName"] = "127.0.0.1:62001"
# 系统
desired_caps["platformName"] = "Android"
# 系统的版本
desired_caps["platformVersion"] = "7.1"
# 要启动app的名称(包名)
desired_caps["appPackage"] = "com.android.settings"
# 要启动的app的哪个界面
desired_caps["appActivity"] = ".Settings"

driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_capabilities=desired_caps)

time.sleep(5)
print(driver.page_source)

# 关闭app
driver.close_app()
# 释放资源
driver.quit()
```
# 基础操作
1. 基础API
**driver**
+ close_app()关闭打开的应用
+ quit()断开连接（后续就不能发送指令）
+ install_app('apk在电脑的绝对路径')安装应用
+ remove_app('应用的包名')卸载应用
+ is_app_installed('应用的包名')判断应用是否安装
+ push_file(目标位置，base64编码的内容)
+ pull_file(来源位置)返回值是base64编码的内容
+ page_source 获取界面xml源码
+ find_element...
+ find_elements...
+ current_package 获取当前操作的应用的包名
+ current_activity 获取当前操作的界面的名称
**element**
+ text获取元素文本内容
+ click()点击元素对应位置
+ get_attribute(属性名称)获取属性值
+ location 获取元素左上角的坐标（相对于屏幕的左上角）
+ size获取元素的宽高（字典）
2. 输入文本
+ send_keys(文本内容)
可以输入文字，但对于中文，在连接设备的时候要加上配置
``` py
desired_caps['unicodeKeyboard']=True #unicode设置（允许中文输入）
desired_caps['resetKeyboard']=True #键盘设置（允许中文输入）
```
对于同一个元素，多次调用此方法会先一个个删掉原有内容，再输入
+ clear()
清空文本框中的内容
# 查找元素
1. 概述
查找元素的方法与selenium的元素定位方法类似，大多使用xpath定位的方式，其中查找元素需要借助Androidsdk安装目录下的tools文件夹下的**uiautomatorviewer可执行程序**。
2. 案列
``` py
from appium import webdriver
import time

# 连接移动设备所必须的参数
desired_caps = {}

# 当前要测试的设备的名称
desired_caps["deviceName"] = "127.0.0.1:62001"
# 系统
desired_caps["platformName"] = "Android"
# 系统的版本
desired_caps["platformVersion"] = "7.1"
# 要启动app的名称(包名)
desired_caps["appPackage"] = "com.android.settings"
# 要启动的app的哪个界面
desired_caps["appActivity"] = ".Settings"

driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_capabilities=desired_caps)

print(driver.current_package)
print(driver.current_activity)
time.sleep(1)
# print(driver.page_source)

# 在Android手机中 text并不是文本 而是属于标签的属性
# driver.find_element_by_xpath("//*[text()='显示']").click()

el= driver.find_element_by_xpath("//*[@text='显示']")

print(el.size)
print(el.text)
print(el.get_attribute("text"))
print(el.location)
time.sleep(1)
print(driver.current_package)
print(driver.current_activity)

time.sleep(3)
# 关闭app
driver.close_app()
# 释放资源
driver.quit()
```
# 模拟手势
## 滚动
+ 使用方法
``` py
swipe(self,start_x:int,start_y:int,end_x:int,end_y:int,duration:int=0)
```
> 注意duration默认是600ms,会影响实际滚动距离，此外click方法实际上不是点击元素，而是点元素所在位置。当滑动时点击元素可能点不准需要在点击之前睡一会等待滑动结束。
{.is-warning}

``` py
scroll(origin_el,destination_el,duration)
```
> scroll直接传递元素作为参数即可，不需要手动获取位置。
{.is-info}

## 拖拽
+ 概述
在安卓中，拖拽=按下 等待一定时间移动松手
+ 使用
``` py
drag_and_drop(origin_el,destination_el)
```
## TouchAction
+ 作用
构建相对比较复杂的、连续的触摸行为
+ 用法
1. 创建TouchAction对象，需要把driver作为参数传递
2. 通过各种方法添加动作
3. 执行操作perform()
+ 方法
1. 按下
``` py
press(self,el,x,y,pressure)
```
如果传递了el参数，x,y可以不传

