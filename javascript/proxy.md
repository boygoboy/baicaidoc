---
title: js中的代理与反射
description: 代理与反射的介绍
published: 1
date: 2022-04-26T09:22:26.302Z
tags: proxy
editor: markdown
dateCreated: 2022-04-25T04:01:10.925Z
---

# 代理与反射
## 代理简述
代理是对目标对象的抽象，可以通过代理操作目标对象，对目标对象的一些底层操作进行拦截操作相当于门卫的作用。代理同时又独立于目标对象，直接操作目标对象会直接绕过代理所给予的行为。
## 代理的操作
### 创建空代理
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
### 定义捕获器
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

### 捕获器参数和反射api
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
创建一个可以捕获所有方法，然后把每个方法转发给对应反射api的空代理：
``` js
const target={foo:'bar'}
const proxy=new Proxy(target,Reflect)
console.log(proxy.foo) //bar
```
反射api为开发者准备好样板代码可以用最少的代码修改捕获的方法
``` js
const target={
    foo:'bar',
    tool:'img'
}
const handler={
     get(trapTarget,property,receiver){
      let desc=''
      if(property=='foo'){
        desc='!'      
      }
       return Reflect.get(...arguments)+desc
     }
}
const proxy=new Proxy(target,handler) 
 console.log(proxy.foo) //bar!
```
### 捕获器不变式
捕获器不变式因方法不同而异，通常是防止捕获器的定义出现过于反常的行为，例如对象某个属性不可写不可配置，当捕获器返回一个与该属性不同的值时会报错
``` js
const target={}
Object.defineProperty(target,'foo',{
configurable:false,
writable:false,
  value:'bar'
})
const handler={
  get(){
    return 'tt'
  }
}
const proxy=new Proxy(target,proxy)
console.log(proxy.foo)  //TypeError
```
### 可撤销代理
通过new Proxy()创建的普通代理与目标对象的联系在生命周期内一直持续，用Proxy.revocable()方法创建出来的代理可以撤销代理与目标对象联系，并且不可逆。
``` js
const target={foo:'bar'}
const handler={
    get(){
     return 'tt'
    }
}
const {proxy,revoke}=Proxy.revocable(target,handler)
console.log(proxy.foo) //bar
revoke()
console.log(proxy.foo) //TypeError
```
## 实用反射api
### 反射api与对象api
实用反射api需要记住：
+ 反射api不限于捕获处理程序
+ 大多数反射api方法在Object类型上有对应的方法
通常Object上的方法适用于通用程序，而反射方法适用于细粒度的对象控制与操作
### 状态标记
反射方法返回的布尔值称作状态标记，状态标记比返回修改后的对象或者抛出错误的反射api方法更有用，例如
``` js
const o={}
try{
  Object.defineProperty(o,'foo','bar')
  console.log('success')
}catch(e){
 console.log('fail')
}
//使用状态标记优化
const o={}
if(Reflect.defineProperty(o,'foo','bar')){
  console.log('success')
}else{
  console.log('fail')
}
```
**以下反射方法提供状态标记**
+ Reflect.defineProperty()
+ Reflect.preventExtensions()
+ Reflect.setPrototypeOf()
+ Reflect.set()
+ Reflect.deleteProperty()
### 用一等函数代替操作符
反射方法提供只有操作符才能完成的操作
+ Reflect.get()：可以替代对象属性访问操作符
+ Reflect.set()：可以替代=赋值操作符
+ Reflect.has()：可以替代in操作符或with()
+ Reflect.deleteProperty()：可以替代delete操作符
+ Reflect.construct()：可以替代new操作符
### 安全地应用函数
函数自定义apply属性：
``` js
Function.prototype.apply.call(myFunc,thisVal,argumentList)
```
使用Reflect.apply:
``` js
Reflect.apply(myFunc,thisVal,argumentList)
```
### 代理另一个代理
代理还可以创建一个代理去代理另一个代理，相当于在一个对象上构建多个拦截网：
``` js
const target={
     foo:'bar'
}
const firstProxy=new Proxy(target,{
   get(){
     console.log('first proxy')
     return Reflect.get(...arguments)
   }
})
const secondProxy=new Proxy(firstproxy,{
get(){
console.log('second proxy')
  return Reflect.get(...arguments)
}
})
console.log(secondProxy.foo)
//second proxy
//first proxy
//bar
```
## 代理的问题与不足
### 代理中的this
代理中的this指向自己，所以代理某些对象可能存在问题，比如WeakMap保存私有变量
``` js
const wm=new WeakMap()
class User{
    constructor(userId){
       wm.set(this,userId)
    }
  set id(){
      wm.set(this,userId)
  }
  get id(){
      return wm.get(this)
  }
}
//由于这个实现依赖于实例对象标识，使用代理可能出现问题：
const user=new User(123)
console.log(user.id) //123
const userInstanceProxy=new Proxy(user,{})
console.log(userIntanceProxy.id)  //undefined
```
可以把代理User实例改为代理User类本身，然后创建代理的实例就可以访问WeakMap的键了
``` js
const UserClassProxy=new Proxy(user,{})
const proxyUser=new UserClassProxy(456)
console.log(proxyUser.id) //456
```
### 代理与内部槽位
某些内置类型可能会依赖代理无法控制的机制导致代理上调用某些方法会出错，比如Date类型的方法依赖this值上的内部槽位[[NumberDate]]代理上不存在这个内部槽位，且这些方法不能通过get()、set()方法操作因此代理进行拦截就会报错
``` js
const target=new Data()
const proxy=new Proxy(target,{})
console.log(proxy instanceof Date) //true
proxy.getDate() //报错
```
## 代理捕获器13种方法
### get()
1. 描述
get()捕获器在获取属性值的操作中被调用，对应的反射api方法为Reflect.get()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
    get(target,property,receiver){
     console.log('get')
      return Reflect.get(...arguments)
    }
})
proxy.foo //get
```
2. 返回值
返回值无限制
3. 拦截的操作
+ proxy.property
+ proxy[property]
+ Object.create(proxy)[property]
+ Reflect.get(proxy,property,receiver)
4. 捕获器处理程序对象
+ target:目标对象
+ property:引用的目标对象上的字符串属性
+ receiver：代理对象或继承代理对象的对象
5. 捕获器不变式
+ target.property不可写不可配置，则处理程序返回的值必须与target.property匹配
+ target.property不可配置且get特性为undefined处理程序的返回值必须是undefined

### set()
1. 描述
set()捕获器会在设置属性值的操作中被调用，对应的反射api方法为Reflect.set()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
   set(target,property,value,receiver){
       console.log('set()')
     return Reflect.set(...arguments)
   }
})
proxy.foo='bar' //set
```
2. 返回值
返回true表示成功，返回false表示失败，严格模式下会抛出错误
3. 拦截的操作
+ proxy.property=value
+ proxy[property]=value
+ Object.create(proxy)[property]=value
+ Reflect.set(proxy,property,value,receiver)
4. 捕获器处理程序参数
+ target：目标对象
+ property：引用的目标对象上的字符串键属性
+ value:要赋给属性的值
+ receiver：接收最初赋值的对象
5. 捕获器不变式
+ 如果target.property不可写且不可配置，则不能修改目标属性的值
+ 如果target.property不可配置且set特性为undefined，则不能修改目标属性的值
### has()
1. 描述
has()捕获器会在in操作符中被调用，对应的反射api方法为Reflect.has()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
    has(target,property){
    console.log('has')
      return Reflect.has(...arguments)
    }
})
'foo' in proxy
```
2. 返回值
has()必须返回布尔值，表示属性是否存在，返回非布尔值会被转化为布尔值
3. 拦截的操作
+ property in proxy
+ property in Object.create(proxy)
+ with(proxy){(property)}
+ Reflect.has(proxy,property)
3. 捕获器处理程序参数
+ target：目标对象
+ property：引用的目标对象上的字符串键属性
4. 捕获器不变式
+ 如果target.property存在且不可配置，则处理程序必须返回true
+ 如果target.propsrty存在且目标对象不可拓展，则处理程序必须返回true。
### defineProperty()
1. 描述
defineProperty()捕获器会在Object.defineProperty()中被调用。对应的反射api方法为Reflect.defineProperty()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
    defineProperty(target,property,descriptor){
      console.log('defineProperty')
      return Reflect.defineProperty(...arguments)
    }
})
Object.defineProperty(proxy,'foo',{value:'bar'}) //defineProperty
```
2. 返回值
defineProperty()必须返回布尔值，表示是否成功定义，返回非布尔值会被转型为布尔值
3. 拦截的操作
+ Object.defineProperty(proxy,property,descriptor)
+ Reflect.defineProperty(proxy,property,descriptor)
4. 捕获器处理程序参数
+ target：目标对象
+ property：引用的目标对象上的字符串键属性
+ descriptor：包含可选的enumerable、configurable、writable、value、get和set定义的对象
5. 捕获器不变式
+ 如果对象不可拓展，则无法定义属性
+ 如果目标对象有一个可配置的属性，则不能添加同名的不可配置属性
+ 如果目标对象有一个不可配置的属性，则不能添加同名的可配置属性
### getOwnPropertyDescriptor()
1. 描述
getOwnPropertyDescriptor()捕获器会在Object.getOwnPropertyDescriptor()中被调用，对应的反射api为Reflect.getOwnPropetyDescriptor()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
   getOwnPropertyDescriptor(target,property){
      console.log('getOwnPropertyDescriptor')
      Reflect.getOwnPropertyDescriptor(...arguments)
   }
})
Object.getOwnPropertyDescriptor(proxy,'foo')  //getOwnPropertyDescriptor
```
