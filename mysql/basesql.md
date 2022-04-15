---
title: sql基本操作
description: 对mysql的基本操作
published: 1
date: 2022-04-15T14:58:50.841Z
tags: mysql
editor: markdown
dateCreated: 2022-04-15T14:19:08.581Z
---

# sql的select语句
1. 使用select *查询表中所有的数据
``` sql
select * from user
```
2. 使用select 列名称查询表中指定列数据
比如想获取表中username和password的列内容：
``` sql
SELECT username,password FROM user
```
# sql的 insert into语句
1. 语法
insert into 语句用于向表中插入新的数据行，语法格式如下
``` sql
insert into table_name(列1，列2) values (值1,值2,......)
```
2. 示例
向user表中插入一条username为usertest，password为‘123456’的记录
``` sql
insert into user (username,password) values ('usertest','123456')
```
# sql的update语句
1. 语法
update语句用于修改表中的数据，语法格式如下：
``` sql
update 表名称 set 列名称=新值 where 列名称=新值
```
2. 示例
+ 把user表中的id号为1的password更新为'888888'
``` sql
update user set password = '888888' where id = 1
```
+ 把user表中的id号为2的username和password分别更新为bc和'654321'
``` sql
update user set username='bc',password='654321' where id = 2
```
# sql的delete语句
1. 语法
delete语句用于删除表中的行，语法格式如下：
``` sql
delete from 表名称 where 列名称 = 值
```
2. 示例
删除user表中id为3的用户
``` sql
delete from user where id = 3
```
