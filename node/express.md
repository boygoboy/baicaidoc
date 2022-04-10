---
title: express模块
description: express的使用
published: 1
date: 2022-04-10T14:36:33.052Z
tags: express
editor: markdown
dateCreated: 2022-04-10T11:04:52.890Z
---

# express的基本使用
+ express模块的介绍
Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。使用 Express 可以快速地搭建一个完整功能的网站。
+ express的引入使用
1. 安装express模块
``` shell
npm install express @4.17.1
```
2. 引入express模块并启动
``` javascript
const express=require('express')
//创建express实例
const app=express()
//启动express服务
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
# express的具体使用
## 监听get请求
1. 介绍
通过app.get()方法可以监听get请求，具体语法如下：
``` javascript
//url为get请求的路径
//回调函数req为请求对象，res为响应对象
app.get(url,function(req,res){

})
```
2. 使用
``` js
const express=require('express')
const app=express()
app.get('/get',(req,res)=>{
res.send({id:'1',name:'abc'})
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```

## 监听post请求
1. 介绍
通过app.post()方法可以监听post请求，具体语法如下：
``` js
//url为post请求路径
//req为请求对象
//res为响应对象
app.post(url,function(req,res){})
```
2. 使用
``` js
const express=require('express')
const app=express()
app.post('/post',(req,res)=>{
res.send('发送请求成功！')
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
## 响应对象send()方法
send()方法可以返回响应信息
## 获取url中携带的查询参数
例如：get请求的url为：http://127.0.0.1/?name=aa&age=18 那么可用通过响应对象中的query属性获取查询参数
``` js
const express=require('express')
const app=express()
app.get('/get',(req,res)=>{
console.log(res.query) //输出：{name:aa,age:18}
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
## 获取url中的动态参数
通过req.params对象可以获取到:匹配到的动态参数，使用案例如下：
1. 发送get请求url
url：http://127.0.0.1/33 
``` js
const express=require('express')
const app=express()
app.get('/get/:id(req,res)=>{
console.log(res.params) //输出：{id:33}
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
# 托管静态资源
## express.static()用法
+ 描述
express.static()方法可以创建一个静态资源服务器，将js、css、image等资源开放供外部访问,比如public目录下有index.html文件，使用```app.use(express.static('public'))```即可创建静态资源服务器。浏览器输入http://127.0.0.1/index.html即可访问该资源。
> 注意：public目录并不会出现在访问路径里面
{.is-warning}
+ 案例
将clock目录下的资源开放为静态资源，实现代码如下：
``` js
const express=require('express')
const app=express()
app.use(express.static('./clock'))
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
浏览器中输入：http://127.0.0.1/index.html即可访问资源
## 托管多个静态资源目录
+ 描述
通过多次调用express.static()方法可创建多个静态资源目录
``` js
app.use(express.static('public'))
app.use(express.static('clock'))
```
以上即可创建两个静态资源目录
> 注意：访问静态资源文件时，express.static()会根据添加资源目录的顺序查找所需文件，例如：public和clock目录下都有index.html文件，那么当浏览器中输入：http://127.0.0.1/index.html会访问public目录下的index.html
{.is-warning}
## 挂载路径前缀
+ 描述
如果想在访问的静态资源文件前挂载路径前缀可通过``` app.use('/public',express.static('public'))``` 方式实现
+ 案例
``` js
const express=require('express')
const app=express()
app.use('/public',express.static('./public'))
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
此时浏览器中需要输入：http://127.0.0.1/public/index.html才可以访问index.html网页
