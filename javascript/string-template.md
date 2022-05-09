---
title: 字符串模板和字符串新增方法
description: 字符串模板的使用
published: 1
date: 2022-05-09T08:46:43.990Z
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
# 标签模板
``` js
alert('yes')
//使用标签模板
alert`yes`

let func=(m)=>`${m}`
console.log(func(aaa))
```
# 字符串新增方法
+ 查询字符串
```js
//es5
let str='this is a student'
if(str.indexOf('this')!=-1){
   console.log(true)
}else{
   console.log(false)
}
//es6
let str='this is a student'
let ret=str.includes('this')
console.log(ret) //true
```
+ 判断字符串开头
使用startsWith判断字符串开头
``` js
let url='https://www.baidu.com'
let result=url.startsWith('https')
console.log(result) //true
```
+ 判断字符串结尾
使用endsWith判断字符串结尾
``` js
let url='https://www.baidu.com'
let result=url.endsWith('com')
console.log(result) //true
```
+ 复制方法
``` js
let str='d'
let result=str.repeat(5)
console.log(result) //ddddd
```
+ 字符串填充
padStart('字符串长度','填充的字符')
```js
let str='a'
let result=str.padStart(5,'d')
console.log(result) //dddda
```
padEnd('字符串长度','填充的字符')
```js
let str='d'
let result=str.padEnd(5,'a')
console.log(result) //daaaa
```
+ 去除空格
```js
let str='         e    '
console.log(str)
//去除所有空格
console.log(str.trim())
//去除左面空格
console.log(str.trimLeft())
//去除右面空格
console.log(str.trimRight())
```
+ 字符串模板可嵌套
``` js
let str=`<h1>`${fn()}`</h1>`
```

