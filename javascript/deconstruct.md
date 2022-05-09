---
title: 解构与赋值
description: 数组、对象、函数等的解构与赋值
published: 1
date: 2022-05-09T06:39:14.035Z
tags: deconstruct
editor: markdown
dateCreated: 2022-05-09T06:39:14.035Z
---

# 数组的解构与赋值
+ 基本解构
``` js
   let arr=[1,2,3]
   let [a,b,c]=arr
   console.log(a,b,c) //1,2,3
```
+ 嵌套数组的解构
按数组的解构进行解构
``` js
let arr=[1,2,[3,4],5]
let [a,b,[c,d],e]
console.log(c) //3
```
+ 使用拓展运算符解构
``` js
let arr=[1,2,3,4,5,6]
let [a,...b]=arr
console.log(a) //1
console.log(b) //[2,3,4,5,6]
```
+ 解构数组中某个元素值
``` js
let arr=[1,2,3]
let [,a,]
console.log(a) //2
```
+ 解构设置默认值
解构时可设置默认值，当解构的值为undefined时则采用默认值，此外后面的解构变量可以使用前面解构变量作为默认值，反之不可以
``` js
let [a='111']=[]
console.log(a) //111
let [a='111']=[5]
console.log(a) //5
let [a=1,b=a]=[]
console.log(a) //1
console.log(b) //1
//下面写法会报错
let [a=b,b=3]=[]
console.log(a,b) //报错，因为b此时未定义
```
# 对象的解构赋值
