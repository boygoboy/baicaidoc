---
title: 解构与赋值
description: 数组、对象、函数等的解构与赋值
published: 1
date: 2022-05-09T07:57:50.490Z
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
+ 基本使用
``` js
let obj={
       name:'张飞'，
       age:18
}
let {name,age}=obj
console.log(name) //张飞
console.log(age)  //18
```
+ 为解构变量起别名
``` js
let obj={
       name:'张飞'，
       age:18
}
let {name:f,age}=obj
console.log(f) //张飞
```
+ 复杂对象的解构
核心：根据对象的结构进行解构
``` js
let obj={
    f:[
     '张三',
    {
    age:18
    }
  ]
}
let {f:[name,{age}]}=obj
console.log(name)  //张三
console.log(age) //18
```
> 解构对象
{.is-info}

``` js
let obj={
    f:[
     '张三',
    {
    age:18
    }
  ]
}
let {f,f:[name,{age}]}=obj
console.log(f) //['张三',{age:18}]
```
+ 设置默认值
``` js
let {x='aaa'}={}
console.log(x) //aaa
```
> 拓展：如果先声明变量然后解构需要添加括号：
{.is-warning}

``` js
let name;
{name}={name:'aaa'} //这样解构会报错
//正确做法
({name}={name:'aaa'})
```
# 函数解构
函数的解构主要是参数设置默认值，使用的依然是数组和对象的解构
+ 设置默认值
``` js
function test([a,b,c=3]){
   return a+b+c
}
```
+ 解构函数的返回值
``` js
//数组
function fn(){
   return [1,2,3]
}
let [a,b,c]=fn()
console.log(a) //1
//对象
function fn(){
     return {
         name:'aaa',
         age:18
     }
}
let {name,age}=fn()
```
# 字符串的解构
将字符串转化为类似数组的解构
``` js
let str='abcd'
let [a,b,c]=str
console.log(a) //a
```
[第三节-解构赋值.docx](/es6/第三节-解构赋值.docx)
