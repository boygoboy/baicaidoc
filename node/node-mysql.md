---
title: node中操作mysql
description: 在node中使用mysql
published: 1
date: 2022-04-21T09:07:04.571Z
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
# 使用mysql模块操作mysql数据库
## 查询数据
**查询users表中所有数据**
``` js
const mysql=require('mysql')
const db=mysql.createPool({
      host:'127.0.0.1',
      user:'root',
      password:'admin',
      database:'mydatabase'
})
db.query('select * from users',(err,results)=>{
if(err) return console.log('查询失败！')
  console.log(results) //返回的是查询结果的数组
})
```
## 插入数据
**向users表中新增数据，其中username为cwj,password为123456**
``` js
const mysql=require('mysql')
const db=mysql.createPool({
      host:'127.0.0.1',
      user:'root',
      password:'admin',
      database:'mydatabase'
})
//要插入users表中的数据对象
const user={username:'cwj',password:'123456'}
//待执行的SQL语句英文？号代表占位符
const sqlStr=‘insert into users (username,password) values (?,?)’
//使用数组的形式依次为？占位符指定具体的值
db.query(sqlStr,[user.name,user.password],(err,results)=>{
if(err) return console.log(err.message)
  if(results.affectdRows==1){console.log('插入数据成功！')}
})
```
> 拓展：向表中新增数据时，如果数据对象的每个属性和数据表的字段一一对应，则可以通过如下方式快速插入数据：
{.is-info}

``` js
const mysql=require('mysql')
const db=mysql.createPool({
      host:'127.0.0.1',
      user:'root',
      password:'admin',
      database:'mydatabase'
})
//插入到users表中的数据对象
const user={username:'cwj',password:'123456'}
//待执行的SQL语句
const sqlStr='insert into users set ?'
//直接将数据对象当作占位符的值
db.query(sqlStr,user,(err,results)=>{
if(err) return console.log(err.message)
  if(results.affectedRows==1){
  console.log('插入新数据成功！')
  }
})
```
