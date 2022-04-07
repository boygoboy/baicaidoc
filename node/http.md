---
title: http模块
description: http模块
published: 1
date: 2022-04-07T12:43:26.543Z
tags: http
editor: markdown
dateCreated: 2022-04-07T12:43:26.543Z
---

# 创建web服务器
+ 创建web服务器的基本步骤
1. 导入http模块
2. 创建web服务器实例
3. 为服务器绑定request事件，监听客户端的请求
4. 启动服务器
+ 代码实现
``` javascript
//1. 导入http模块
const http=require('http')
//2. 创建web服务器实例
const server=http.createServer()
//3. 为服务器绑定request事件，监听客户端的请求
server.on('request',(req,res)=>{
console.log("someone request")
})
//4. 启动服务器
server.listen(8080,()=>{
console.log('started')
})
```
# request事件回调函数介绍
+ 描述
回调函数中接收两个参数，request和response。request为客户端请求的对象，其中包含了url和method方法等。response为服务端响应对象，可以发送响应内容和设置响应头等。
## request请求对象
1. url
通过 request.url可以获取客户端请求的路径url。
2. method
通过 request.method可以获取客户端请求的方法
**代码如下**
``` javascript
const http=require('http')
const server=http.createServer()
server.on('request',(req,res)=>{
console.log(req.url) //客户端请求的url
console.log(req.method) //客户端请求的方法
})
server.listen(8080,()=>{
})
```
## response响应对象
1. end
服务器调用response.end()方法可以向客户端发送响应内容并结束这次请求的处理过程
2. setHeader
服务器调用response.setHeader()方法可以设置响应头解决中文乱码问题
**代码如下**
``` javascript
const http=require('http')
const server=http.createServer()
server.on('request',(req,res)=>{
const mes=`内容:${req.url}/${req.method}`
res.setHeader('Content-Type','text/html;charset=utf-8')
res.end(mes)
})
server.listen(8080,()=>{
})
```
# 案例
[时钟案例](https://pan.baicai.blog/%E7%94%B2%E9%AA%A8%E6%96%87%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8/node%E5%AD%A6%E4%B9%A0%E6%A1%88%E4%BE%8B)

