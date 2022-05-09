---
title: 字符串模板和字符串新增方法
description: 字符串模板的使用
published: 1
date: 2022-05-09T08:10:39.944Z
tags: string
editor: markdown
dateCreated: 2022-05-09T08:10:39.944Z
---

# 字符串模板基本使用
+ 嵌入计算表达式
``` js
let obj={
    m:10,
    n:10
}
let sum=`${obj.m+obj.n}`
console.log(sum) //20
```
+ 嵌入函数
``` js
function fn(){
    return 'aaa'
}
let retsult=`name:${fn()}`
```
+ 拼接字符串
``` js
let name='aaa'
let str=`my name is:${name}`
console.log(str) //my name is:aaa
```