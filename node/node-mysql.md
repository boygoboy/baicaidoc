---
title: node中操作mysql
description: 在node中使用mysql
published: 1
date: 2022-04-21T04:01:04.525Z
tags: mysql
editor: markdown
dateCreated: 2022-04-21T04:01:04.525Z
---

# 在项目中操作mysql的基本步骤
1. 安装操作mysql数据库的第三方模块
通过以下命令安装mysql
``` shell
npm install mysql
```
2. 通过mysql模块连接到mysql数据库
``` js
//1.导入模块
const mysql=require('mysql')
//2.建立与mysql数据库的连接
const db=mysql.createPool({
      host:'127.0.0.1'，
      user:'root',
      password:'admin',
      database:'mydatabase'
})
```
3. 通过mysql模块执行sql语句
这里测试mysql模块能否正常工作，调用db.query()函数，指定要执行的sql语句通过回调函数拿到执行结果
``` js
db.query('select 1',(err,results)=>{
if(err) return console.log('err')
  console.log(results)
})
```
