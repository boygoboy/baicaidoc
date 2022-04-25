---
title: js中的代理与反射
description: 代理与反射的介绍
published: 1
date: 2022-04-25T07:16:46.785Z
tags: proxy
editor: markdown
dateCreated: 2022-04-25T04:01:10.925Z
---

# 代理与反射
## 代理简述
代理是对目标对象的抽象，可以通过代理操作目标对象，对目标对象的一些底层操作进行拦截操作相当于门卫的作用。代理同时又独立于目标对象，直接操作目标对象会直接绕过代理所给予的行为。
## 代理的操作
1. 创建空代理
创建空代理使代理与目标对象进行关联，什么也不会去做。代理使用Proxy构造函数创建接收两个参数：代理对象、处理程序对象。
``` js
const target={
    id:'target'
}
  const handler={}
  const proxy=new Proxy(target,handler)
  //id属性会访问同一个值
  console.log(target.id) //target
  console.log(proxy.id)  //target
  //给目标对象赋值代理与目标对象的值都会改变，因为它们指向同一个地址
   target.id='foo'
   console.log(target.id) //foo
   console.log(proxy.id)  //foo
  //同理给代理对象赋值，代理与目标对象的值都会改变
  proxy.id='foo'
  console.log(target.id) //foo
  console.log(proxy.id)  //foo
 //调用一些对象方法都会反映在代理与目标对象上，比如hasOwnProperty()方法
  console.log(target.hasOwnProperty('id')) //true
  console.log(proxy.hasOwnProperty('id'))  //true
 //Proxy.prototype为undefined所以不能使用instanceof操作符
 console.log(target instanceof Proxy) //error
 console.log(proxy instanceof Proxy)  //error
 //代理与目标对象不严格相等
  console.log(target===proxy)  //false
```
2. 定义捕获器
+ 介绍
使用代理的主要目的就是可以定义捕获器，一个处理程序中可以定义多个捕获器，每个捕获器对应一种基本操作。捕获器的作用是代理操作传播到目标对象之前进行拦截并修改相应的行为。
+ 例子
定义get()捕获器，在以某种形式操作调用get()时触发
``` js
const target={foo:'bar'}
const handler={
     get(){
      return 'hello'
     }
}
const proxy=new Proxy(target,handler)
```
触发get()捕获器条件：
（1）进行proxy.property操作
``` js
console.log(target.foo) //bar
console.log(proxy.foo)  //hello
```
（2）进行proxy[property]操作
``` js
console.log(target['foo']) //bar
console.log(proxy['foo'])  //hello
```
（3）进行Object.create(proxy)[property]操作
``` js
console.log(Object.create(target)['foo']) //bar
console.log(Object.create(proxy)['foo'])  //hello
```
> 只有在代理对象上执行这些操作才会触发捕获器
{.is-warning}

3. 捕获器参数和反射api
+ 捕获器参数
所有捕获器都可以访问相应的参数基于这些参数可以重建捕获器方法的原始行为，例如get()捕获器会接收三个参数：trapTarget目标对象、property要查询的属性、receiver代理对象。
``` js
const target={foo:'bar'}
const handler={
get(trapTarget,property,receiver){
   console.log(trapTarget==target)
   console.log(property)
   console.log(receiver==proxy)
}
}
const proxy=new Proxy(target,handler)
proxy.foo
//true
//foo
//true
```
利用这些参数可以重建捕获器方法的原始行为
``` js
const target={foo:'bar'}
const handler={
get(trapTarget,property,receiver){
  return trapTarget[property]
}
}
const proxy=new Proxy(target,handler)
proxy.foo //bar
```
+ 反射api
大多数情况通过调用全局Reflect对象上（封装了原始行为）的反射api方法来轻松进行重建
``` js
const target={foo:'bar'}
const handler={
   get(){
      return Reflect.get(..arguments)
   }
}
//简洁写法
const handler={
  get:Reflect.get
}
const proxy=new Proxy(target,handler)
console.log(proxy.foo) //bar
```

