---
title: selenium web自动化
description: 使用selenium自动化操作浏览器
published: 1
date: 2022-09-20T08:03:59.433Z
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
## 普通方式
1. id:find_element_by_id
2. name:find_element_by_name
3. class_name:find_element_by_class_name
4. tag_name:find_element_by_tag_name

**例子：**
``` py
# 查找 元素(标签,标记 节点)  通过id
 driver.find_element_by_id("kw").send_keys("美女")
 driver.find_element_by_id("su").click()

# 通过name查找元素
 driver.find_element_by_name("wd").send_keys("美女")

# 通过class name
 driver.find_element_by_class_name("s_ipt").send_keys("美女")
 driver.find_element_by_id("su").click()
```
## 查找超链接
1. link_text
根据超链接的文字定位元素
2. partial_link_text
根据超链接的部分文字定位元素
> 相同的规则会对应多个标签，那么方法只会返回第一个
{.is-warning}

**例子：**
``` py
# 定位a标签 link text  partial linktext
 driver.find_element_by_link_text("hao123").click()
 driver.find_element_by_partial_link_text("hao123").click()
```
## 通过css选择器定位
1. css选择器语法

![css选择器语法](/images/snipaste_2022-09-20_14-20-06.png)
2. 使用方法
find_element_by_css_selector(CSS选择器)
3. 例子
``` py
driver.find_element_by_css_selector("#kw").send_keys("美女")
driver.find_element_by_css_selector(".s_ipt").send_keys("美女")
driver.find_element_by_css_selector("[name=wd]").send_keys("美女")
driver.find_element_by_css_selector("[value=百度一下]").click()
```
## 通过xpath获取元素
1. 描述
xpath即标签元素的路径，通过标签的路径可以定位到元素
2. xpath表达式

![snipaste_2022-09-20_14-30-44.png](/images/snipaste_2022-09-20_14-30-44.png)
3. 获取内容

![snipaste_2022-09-20_14-32-18.png](/images/snipaste_2022-09-20_14-32-18.png)
4. 进阶表达式

![snipaste_2022-09-20_14-33-47.png](/images/snipaste_2022-09-20_14-33-47.png)

5. 辅助方式
+ 通过Chrome 开发者工具赋值xpath值
+ 使用xpath helper插件进行校验

6. 例子
``` py
driver.find_element_by_xpath("//*[@id='kw']").send_keys("美女")
driver.find_element_by_xpath("//*[@id='su']").click()
```
# 元素的操作
+ 元素操作
1. clear()清除文本
2. send_keys()模拟输入
3. click()单击元素
+ 元素属性获取
1. size
返回元素大小
2. text
获取元素的文本
3. get_attribute()
获取属性值
4. is_display()
判断元素是否可见
5. is_enabled()
判断元素是否可用
+ 例子
``` py
driver.find_element_by_id("kw").clear()
driver.find_element_by_id("kw").send_keys("美女")
driver.find_element_by_id("su").click()
print(driver.find_element_by_id("kw").size)
print(driver.find_element_by_id("kw").text)
print(driver.find_element_by_id("kw").is_enabled())
print(driver.find_element_by_id("kw").is_displayed())
print(driver.find_element_by_xpath("//*[text()='新闻']").get_attribute("href"))
```
# 浏览器的其他操作
+ 浏览器操作方法
1. maximize_window()最大化浏览器
2. set_window_size(width,height) 设置浏览器宽高
3. set_window_position(x,y) 设置浏览器位置，浏览器左上角相对于屏幕左上角位置
4. back()后退
5. forward()前进
6. refresh()刷新
7. close()关闭当前页面
8. quit()关闭浏览器
+ 浏览器信息（属性）
1. title获取页面title
2. current_url 获取当前页面URL
+ 例子
``` py
from selenium import webdriver
import time

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
url = "https://www.baidu.com/"

driver.get(url)

# 最大化浏览器
driver.maximize_window()

# 设置浏览器 1920* 1080
# time.sleep(1)
# driver.set_window_size(800,600)
# time.sleep(2)
# driver.set_window_position(200,200)
print(driver.title)
print(driver.current_url)
driver.find_element_by_id("kw").send_keys("美女")
driver.find_element_by_id("su").click()

time.sleep(2)
print(driver.title)
print(driver.current_url)
# 浏览器后退
driver.back()
time.sleep(2)
print(driver.title)
print(driver.current_url)
# 浏览器 前进
driver.forward()

time.sleep(2)
print(driver.title)
print(driver.current_url)

# 刷新浏览器
driver.refresh()

time.sleep(2)

driver.back()

driver.find_element_by_xpath("//*[text()='hao123']").click()

time.sleep(2)
driver.close()

time.sleep(5)

# 回收资源
driver.quit()
```
# 页面等待
+ 为什么要等待
代码执行完毕但是页面元素可能还未加载出来，此时就会报错因此要进行页面等待。
+ 等待方式
1. 强制等待
2. 显示等待
3. 隐式等待
+ 强制等待
通过调用time.sleep()方法让程序停止执行一段时间，此方法不太灵活适合网速稳定情况
+ 显示等待
1. 使用WebDriverWait包装webDriver对象
2. 使用webDriverWait的util 方法，传入可调用对象（通常是presence_of_element_located函数的返回值）
3. 例子
``` py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait
url = r'E:\测试\课件\Web自动化\Web自动化课件\02img\注册A.html'
driver = webdriver.Chrome()
driver.get(url)
element = WebDriverWait(driver, 5).until(EC.presence_of_element_located((By.ID, 'userA')))
element.send_keys("admin")
```
4. 此方法导入的包太多使用比较麻烦
+ 隐式等待（推荐）
1. 使用方法
在创建好webDriver对象之后，调用implicitly_wait方法传入超时时间即可
2. 例子
``` py
driver.implicitly_wait(5)
driver.find_element_by_id("1").click()
```
> 注意：每次页面刷新翻页等重新加载时要进行页面等待操作
{.is-warning}

# 鼠标操作
+ 模拟鼠标操作的使用步骤：
1. 创建ActionChains对象
2. 使用ActionChains对象的方法进行操作
   + context_click()
   右击---此方法模拟鼠标右键点击效果
   + double_click()
   双击---此方法模拟鼠标双击效果
   + drag_and_drop()
   拖动---此方法模拟鼠标拖动效果
   + move_to_element()
   悬停---次方法模拟鼠标悬停效果
3. 通过ActionChains提交这些操作
perform()执行---此方法用来执行以上所有鼠标方法
+ 例子

**拖动**
``` py
from selenium import webdriver
from time import sleep
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import  By
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome(executable_path="./chromedriver.exe")

url = r'C:\Users\T\PycharmProjects\testdemo\drag.html'
driver.get(url)

sleep(3)
chains = ActionChains(driver)
chains.drag_and_drop(driver.find_element(By.ID,'div1'), driver.find_element(By.ID,'div2') )
chains.perform()

sleep(3)
driver.quit()
```

**移动鼠标**
``` py
from selenium import webdriver
from time import sleep
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import  By

driver = webdriver.Chrome(executable_path="./chromedriver.exe")

url = r'C:\Users\T\PycharmProjects\testdemo\注册A.html'
driver.get(url)

sleep(3)
chains = ActionChains(driver)
chains.move_to_element(driver.find_element(By.XPATH,'//*[@id="zc"]/fieldset/button'))
chains.perform()

sleep(3)
driver.quit()
```
**双击**
``` py
from selenium import webdriver
from time import sleep
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import  By

driver = webdriver.Chrome(executable_path="./chromedriver.exe")

url = r'C:\Users\T\PycharmProjects\testdemo\注册A.html'
driver.get(url)

sleep(3)
chains = ActionChains(driver)
chains.double_click(driver.find_element(By.ID,'laotie'))
chains.perform()

sleep(3)
driver.quit()
```
# 键盘操作
+ 使用方法
element.send_keys()
+ 参数
1. 普通字符串
2. 键盘按键
+ 例子
``` py
from selenium import webdriver
import time
from selenium.webdriver.common.keys import Keys

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
url = "https://www.baidu.com/"

driver.get(url)

el = driver.find_element_by_id("kw")
# 输入python
el.send_keys("python")
time.sleep(2)
# 全选
el.send_keys(Keys.CONTROL,"a")
time.sleep(2)
# 删除
el.send_keys(Keys.BACKSPACE)
time.sleep(2)
# 输入美女
el.send_keys("美女")
time.sleep(2)
# 全选
el.send_keys(Keys.CONTROL,"a")
time.sleep(2)
# 复制
el.send_keys(Keys.CONTROL,"c")
time.sleep(2)

el.send_keys(Keys.CONTROL,"v")
time.sleep(2)



# 回收资源
driver.quit()
```
# 控制下拉框
下拉框即html中的select元素，主要操作下拉框中的option
+ 使用步骤
1. 通过select元素创建出Select对象
2. 通过Select对象的方法选中选项
  （1）select_by_index()根据option索引来定位，从0开始
  （2）select_by_value()根据option属性value值来定位
  （3）select_by_visible_text()根据option显示文本来定位
+ 例子
``` py
from selenium import webdriver
import time
from selenium.webdriver.support.select import Select

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
# url = "https://www.baidu.com/"

url = "file:///D:/ship/kc/9.selenium/%E8%B5%84%E6%96%99/%E6%B3%A8%E5%86%8CA.html"

driver.get(url)

# driver.find_element_by_id("selectA").se
select = Select(driver.find_element_by_id("selectA"))
time.sleep(2)
select.select_by_index(3)
time.sleep(2)
select.select_by_value("bj")
time.sleep(2)
select.select_by_visible_text("A广州")
time.sleep(5)
# 回收资源
driver.quit()
```
# 操作滚动条
+ 滚动条的限制与突破
WebDriver类库中并没有直接对滚动条进行操作的方法，但提供了调用JavaScript脚本的方法，通过调用JavaScript脚本的方法来达到操作滚动条
+ 使用方法
1. 让浏览器执行js代码
driver.execute_script(js代码字符串)
2. 滚动的js代码
   + 绝对滚动
   window.scrollTo(x,y)
   + 相对滚动
   window.scrollBy(x,y)
+ 例子
``` py
from selenium import webdriver
import time

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
url = "https://www.toutiao.com/"

driver.get(url)
driver.maximize_window()
time.sleep(2)

js_str="window.scrollTo(0,10000)"

driver.execute_script(js_str)


# time.sleep(2)
# driver.find_element_by_xpath("//*[text()='下一页']").click()
#
# time.sleep(2)
# driver.find_element_by_xpath("//*[text()='下一页']").click()

time.sleep(5)

# 回收资源
driver.quit()
```
# 操作警告框
+ 警告框
通过js中的alert、confirm、prompt方法弹出的框
+ 操作方法
1. 获得警告框 alert=driver.switch_to.alert
2. 关闭警告框：适用于三种警告框 alert.dismiss()
3. 确认（也会自动关闭）：适用于confirm和prompt和prompt alert.accept()
4. 输入文字：适用于 prompt alert.send_keys()
5. 获得警告框中的文字
alert.text
+ 例子
``` py
from selenium import webdriver
import time

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
# url = "https://www.baidu.com/"

url = "file:///D:/ship/kc/9.selenium/%E8%B5%84%E6%96%99/%E6%B3%A8%E5%86%8CA.html"

driver.get(url)

time.sleep(2)
driver.find_element_by_id("alerta").click()

# 警告框 切换到警告框 然后操作警告框

alert = driver.switch_to.alert

print(alert.text)
alert.dismiss()

time.sleep(5)

# 回收资源
driver.quit()
```
# frame切换
+ 适用场景
当页面中嵌套frame页面时，定位元素会无法找到元素，此时需要通过frame切换到指定网页才可进行元素定位
+ frame框架
指 ‘iframe frameset frame’等html标签引入的网页
+ 使用方法
1. 切换到子frame
driver.switch_to.frame(frame的名称或者id)
2. 切换到根页面
driver.switch_to.default_content()
+ 例子
``` py
from selenium import webdriver
import time

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
url = "https://mail.qq.com/"

driver.get(url)

time.sleep(2)

print(driver.find_element_by_class_name("login_pictures_title").text)

# 切换frame界面
driver.switch_to.frame("login_frame")

driver.find_element_by_id("u").send_keys("100001")

time.sleep(2)
# 切换回原始的页面
driver.switch_to.default_content()

print(driver.find_element_by_class_name("login_pictures_title").text)

time.sleep(5)

# 回收资源
driver.quit()
```
> 注意：适用frame切换要记得把窗体切回原始页面
{.is-warning}

# 窗口切换
+ 背景
当浏览器中出现多窗口时需要进行窗口切换，下面介绍窗口切换的方法
+ 适用方法
1. 获取所有窗口标识
通过driver.window_handles方法获取返回的是列表
2. 获取单个
通过driver.current_window_handle方法获取返回的是当前窗口标识
3. 切换
通过driver.switch_to.window(handle)方法切换窗口，handle是窗口的标识
+ 例子
``` py
from selenium import webdriver
import time

# webdriver 获取浏览器的对象
driver = webdriver.Chrome("chromedriver.exe")

# 准备一个网址

# https://www.baidu.com/
url = "https://www.baidu.com/"

driver.get(url)
driver.maximize_window()

time.sleep(2)

print(driver.window_handles)
print(driver.current_window_handle)

driver.find_element_by_id("kw").send_keys("美女")
driver.find_element_by_id("su").click()

time.sleep(2)
driver.find_element_by_id("1").click()

time.sleep(2)
print(driver.window_handles)
print(driver.current_window_handle)

# driver.close()
# 切换页面   获取当前 handle  获取所有的handle  新打开的页面在所有handle 列表的最后面
driver.switch_to.window(driver.window_handles[1])

driver.find_element_by_id("currentImg").click()

time.sleep(1)
driver.get_screenshot_as_file("img.png")


time.sleep(5)

# 回收资源
driver.quit()
```
> 注意：超链接打开的窗口，当前窗口仍然是之前的窗口
{.is-warning}

# 截图
截图使用driver.get_screenshot_as_file(保存路径)即可对当前网页进行截图
# cookie的处理
+ 背景
现在网站为了防止爬虫设置了很多人机验证码，为解决这个问题可使用获取设置cookie的方式绕过校验登录
+ 设置cookie的方法
driver.add_cookie({"name":'BDUSS',"value":'通过chrome浏览器调试工具查看'})方法可以设置浏览器cookie
+ 例子
``` py
import time

from selenium import webdriver

driver = webdriver.Chrome('./chromedriver.exe')
driver.get("https://www.baidu.com")

driver.maximize_window()
time.sleep(5)
driver.add_cookie({"name":'BDUSS',"value":'通过chrome浏览器调试工具查看'})
driver.add_cookie({"name":'BAIDUID',"value":'通过chrome浏览器调试工具查看'})
time.sleep(5)
driver.refresh()
time.sleep(5)
```
> 实际场景cookie很难获取，破解cookie以及智能识别验证码仍需探索
{.is-warning}
