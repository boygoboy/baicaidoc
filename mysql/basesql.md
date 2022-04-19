---
title: sql基本操作
description: 对mysql的基本操作
published: 1
date: 2022-04-19T14:15:21.544Z
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
# sql的where语句
1. 语法
where 子句用于限定选择的标准。在select、update、delete语句中都可使用它来限定选择的标准
``` sql
select 列名称 from 表名称 where 列 运算符 值
update 表名称 set 列=新值 where 列 运算符 值
delete from 表名称 where 列 运算符 值
```
2. 可以在where子句中使用的运算符
下面的运算符可在where子句中使用：
|操作符|描述|
|----|----|
|=|等于|
|<>|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|在某个范围内|
|LIKE|搜索某种模式|
3. 例子
``` sql
//查询status为1的所有用户
select * from users where status=1
//查询id 大于 2 的所有用户
select * from users where id>2
//查询username 不等于admin 的所有用户
select * from users where username <>'admin'
```
# sql的and和or运算符
1. 语法
and 和or可在where子句中把两个或多个条件结合起来，and表示必须同时满足多个条件，or表示只要满足任意一个条件即可
2. 例子
+ 使用and来显示所有status为0，并且id小于3的用户
``` sql
select * from users where status=0 and id<3
```
+ 使用or来显示所有status为1，或者username为zs的用户
``` sql
select *from users where status=1 or username='zs
```
# sql的order by子句
1. 语法
order by 语句用云根据指定的列对结果集进行排序
order by 语句默认按照升序对记录进行排序
如果按照降序对记录进行排序可以使用desc关键字
2. 例子
+ order by升序排序
对user表中的数据按照status 字段进行升序排序
``` sql
select * from users order by status 
//或者
select * from users order by status as asc
```
+ order by降序排序
对users表中的数据按照id字段进行降序排序
``` sql
select * from users order by id desc
```
+ order by多重排序
对users表中的数据先按照status字段进行降序排序再按照username的字母顺序进行升序排序
``` sql
select * from users order by status desc,username asc
```
