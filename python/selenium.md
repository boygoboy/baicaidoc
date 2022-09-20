---
title: selenium web自动化
description: 使用selenium自动化操作浏览器
published: 1
date: 2022-09-20T05:53:50.030Z
tags: selenium
editor: markdown
dateCreated: 2022-09-20T03:43:30.055Z
---

# selenium web自动化环境搭建
## python环境安装
python环境安装直接在官网下载软件，运行一直下一步操作即可（python版本在3.5以上）
## selenium安装
+ 这里安装的selenium 3.x版本，安装命令：
``` py
pip install selenium==3.14.0
```
+ 卸载
``` py
pip uninstall selenium
```
+ 查看
``` py
pip show selenium
```
## webdriver浏览器驱动环境
+ 浏览器版本查看
这里基于Chrome浏览器进行开发，版本查看方法可谷歌或百度
+ webdriver驱动下载
webdriver驱动的版本需要和Chrome浏览器版本对应，webdriver官方下载页有说明
+ 配置webdriver
创建一个selenium-study项目，将webdriver驱动放入项目的根目录，到此开发环境都准备完毕。
# 快速入门
+ 入门案例
+ 打开浏览器
``` py
from selenium import webdriver
import time
# 创建浏览器驱动对象
driver=webdriver.Chrome('./chromedriver.exe')
# 使用浏览器打开指定页面
url='https://www.baidu.com'
driver.get(url)
sleep(3)
#关闭浏览器
driver.quit()
```
# 元素查找

