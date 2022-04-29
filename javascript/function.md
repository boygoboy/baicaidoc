---
title: js中的函数
description: 函数使用
published: 1
date: 2022-04-29T01:39:01.609Z
tags: function
editor: markdown
dateCreated: 2022-04-29T01:28:58.641Z
---

# 箭头函数
## 任何可以使用函数表达式的地方都可以使用箭头函数
``` js
let arrowSum=(a,b)=>{
   return a+b
}
let funNum=function(a,b){return a+b}
console.log(arrowSum(5,5)) //10
console.log(funNum(5,5)) //10
```
## 箭头函数简洁写法适合嵌入式
``` js
let ints=[1,2,3]
console.log(ints.map(function(i){return i+1})) //[2,3,4]
console.log(ints.map((i)=>{return i+1}))      //[2,3,4]
```
## 箭头函数括号省略规则
只有一个参数可以不用括号，没有参数或者多个参数需要使用括号
``` js
//一下两者都有效
let double=(x)=>{return 2*x}
let double=x=>{return 2*x}
//没有参数需要使用括号
let getRandom=()=>{Math.random()}
//多个参数需要括号
let sum=(a,b)=>{return a+b}
```
## 箭头函数省略大括号规则
省略大括号箭头后面只能有一行代码比如赋值操作或者一个表达式，并且省略大括号会隐式返回这行代码的值
``` js
//以下两种写法都可返回相应的值
let double=x=>{return 2*x}
let double1=x=>2*x
//可以赋值
let value={}
let setName=x=>x.name='k'
setName(value)
console.log(value.name)  //k
//无效写法：
let mul=(a,b)=>return a+b
```
> 注意：箭头函数不能使用arguments、super和new.target，也不能用作构造函数，此外箭头函数没有prototype属性
{.is-warning}

# 函数名
## 函数名指向指针
函数名指向函数指针，因此跟其他包含对象指针的变量具有相同行为。一个函数名可以有多个名称
``` js
function sum(num1,num2){
    return num1+num2
}
console.log(sum(10,10))
let anothersum=sum
console.log(anothersum(10,10)) //20
sum=null
console.log(anothersum(10,10))  //20
```
anothersum被赋值为sum后指向函数的指针可以调用函数，当sum被赋值为null后切断了与函数的联系
## 函数的名称属性
函数对外暴露一个只读的name属性，该属性保存函数标识符或者是一个字符串化的变量名，如函数没有名称显示空字符串，如是Function构造函数创建的则会标识成'anonymous'
``` js
function foo(){}
let bar=function(){}
let baz=()=>{}
console.log(foo.name)  //foo
console.log(bar.name)  //bar
console.log(baz.name)  //baz
console.log((()=>{}).name) //空字符串
console.log((new Function()).name) //anonymous
```


