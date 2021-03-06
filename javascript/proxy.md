---
title: js中的代理与反射
description: 代理与反射的介绍
published: 1
date: 2022-04-27T06:56:00.333Z
tags: proxy
editor: markdown
dateCreated: 2022-04-25T04:01:10.925Z
---

# 代理与反射
## 代理简述
代理是对目标对象的抽象，可以通过代理操作目标对象，对目标对象的一些底层操作进行拦截操作相当于门卫的作用。代理同时又独立于目标对象，直接操作目标对象会直接绕过代理所给予的行为。

## 创建空代理
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
## 定义捕获器
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

## 捕获器参数和反射api
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
## 捕获器不变式
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
## 可撤销代理
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
# 实用反射api
## 反射api与对象api
实用反射api需要记住：
+ 反射api不限于捕获处理程序
+ 大多数反射api方法在Object类型上有对应的方法
通常Object上的方法适用于通用程序，而反射方法适用于细粒度的对象控制与操作
## 状态标记
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
## 代理另一个代理
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
# 代理的问题与不足
## 代理中的this
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
## 代理与内部槽位
某些内置类型可能会依赖代理无法控制的机制导致代理上调用某些方法会出错，比如Date类型的方法依赖this值上的内部槽位[[NumberDate]]代理上不存在这个内部槽位，且这些方法不能通过get()、set()方法操作因此代理进行拦截就会报错
``` js
const target=new Data()
const proxy=new Proxy(target,{})
console.log(proxy instanceof Date) //true
proxy.getDate() //报错
```
# 代理捕获器13种方法
## get()
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

## set()
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
## has()
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
## defineProperty()
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
## getOwnPropertyDescriptor()
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
2. 返回值
getOwnPropertyDescriptor()必须返回对象，或者属性不存在时返回undefined
3. 拦截的操作
+ Object.getOwnPropertyDescriptor(proxy,property)
+ Reflect.getOwnPropertyDescriptor(proxy,property)
4. 捕获器处理程序参数
+ target:目标对象
+ property：引用目标对象上的字符串键属性
5. 捕获器不变式
自有的target.property特征(A)对应处理程序返回值(B)如下：
+ A:存在且不可配置，B:返回属性存在的对象
+ A：存在且target不可拓展，B:返回一个标识该属性存在的对象
+ A:不存在且target不可拓展，B:返回undefined标识该属性不存在
+ A:target.property不存在，B:不能返回表示该属性可配置的对象
## deleteProperty()
1. 描述
deleteProperty()捕获器会在delete操作符中被调用，对应的反射api为Reflect.deleteProperty()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
        deleteProperty(target,property){
           console.log('deleteproperty')
         return Reflect.deleteProperty(...arguments)
        }
})
delete proxy.foo //deleteProperty
```
2. 返回值
deleteProperty()必须返回布尔值表示删除是否成功，不是布尔值转化为布尔值
3. 捕获器处理程序参数
+ target:目标对象
+ property：引用的目标对象上的字符串键属性
4. 捕获器不变式
如果自有的target.property存在且不可配置，则处理程序不能删除这个属性
## ownKeys()
1. 描述
ownKeys()捕获器会在Object.keys()及类似方法中被调用。对应的反射api为Reflect.ownKeys()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
      ownKeys(target){
       console.log('ownkeys')
        return Reflect.ownKeys(...arguments)
      }
})
Object.keys(proxy) //ownkeys
```
2. 返回值
ownKeys()必须必须返回包含字符串或符号的可枚举对象
3. 拦截的操作
+ Object.getOwnPropertyNames(proxy)
+ Object.getOwnPropertySymbols(proxy)
+ Object.keys(proxy)
+ Reflect.ownKeys(proxy)
4. 捕获器处理程序参数
+ target：目标对象
5. 捕获器不变式
返回的可枚举对象必须包含target的所有不可配置的自有属性，target不可拓展，则返回可枚举对象必须准确包含自有属性键
## getPrototypeOf()
1. 描述
getPrototypeOf()捕获器会在Object.getPrototypeOf()中被调用，对应的反射api方法为Reflect.getPrototypeOf()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
   getPrototypeOf(target){
      console.log('getPrototypeOf')
     return Reflect.getPrototypeOf(...arguments)
   }
})
Object.getPrototypeOf(proxy) // getPrototypeOf
```
2. 返回值
getPrototypeOf()必须返回对象或null
3. 拦截的操作
+ Object.getPrototypeOf(proxy)
+ Reflect.getPrototypeOf(proxy)
+ proxy._proto_
+ Object.prototype.isPrototypeOf(proxy)
+ proxy instanceof Object
4. 捕获器处理程序参数
+ target:目标对象
4. 捕获器不变式
如果target不可拓展，则Object.getPrototypeOf(proxy)唯一有效的返回值就是Object.getPrototypeOf(target)的返回值
## setPrototypeOf()
1. 描述
setPrototypeOf()捕获器会在Object.setPrototypeOf()中被调用，对应的反射api方法为Reflect.setPrototypeOf()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
    setPrototypeOf(target,prototype){
        console.log('setPrototypeOf')
      return Reflect.setPrototypeOf(...arguments)
    }
})
    Object.setPrototypeOf(proxy,Object) //setPrototypeOf
```
2. 返回值
setPrototypeOf必须返回布尔值，表示原型赋值是否成功，返回非布尔值会被转型为布尔值
3. 拦截的操作
+ Object.setPrototypeOf(proxy)
+ Reflect.setPrototypeOf(proxy)
4. 捕获器不变式
如果target不可拓展，则唯一有效的prototype参数就是Object.getPrototypeOf(target)的返回值
## isExtensible()
1. 描述
isExtensible()捕获器会在Object.isExtensible()中被调用。对应的反射api方法为Reflect.isExtensible()
```js
const myTarget={}
const proxy=new Proxy(myTarget,{
     isExtensible(target){
        console.log('isExtensible')
       return Reflect.isExtensible(...arguments)
     }
})
    Object.isExtensible(proxy) // isExtensible
```
2. 返回值
isExtensible()必须返回布尔值，表示target是否可拓展。返回非布尔值会被转型为布尔值
3. 拦截的操作
+ Object.isExtensible(proxy)
+ Reflect.isExtensible(proxy)
4. 捕获器处理程序参数
+ target:目标对象
5. 捕获器不变式
+ target可拓展，处理程序必须返回true
+ target不可拓展，则处理程序必须返回false
## preventExtensions()
1. 描述
preventExtensions()捕获器会在Object.preventExtensions()中被调用。对应的反射api方法为Reflect.preventExtensions()
``` js
const myTarget={}
const proxy=new Proxy(myTarget,{
        preventExtensions(target){
             console.log('preventExtensions')
          return Reflect.preventExtensions(...arguments)
        }
})
     Object.preventExtensions(proxy) // preventExtensions
```
2. 返回值
preventExtensions()必须返回布尔值，表示target是否已经不可拓展，不可拓展返回true,可拓展返回false,非布尔值转化为布尔值
3. 拦截的操作
+ Object.preventExtensions(proxy)
+ Reflect.preventExtensions(proxy)
4. 捕获器处理程序参数
+ target:目标对象
5. 捕获器不变式
Object.isExtensions(proxy)是false，则处理程序必须返回true
## apply()
1. 描述
apply()捕获器会在调用函数时中被调用，对应反射api方法为Reflect.apply()
``` js
const myTarget=()=>{}
const proxy=new Proxy(myTarget,{
   apply(target,thisArg,...arguments){
      console.log('apply')
     return Reflect.apply(...arguments)
   }
})
proxy()
//apply
```
2. 返回值
无限制
3. 拦截的操作
+ proxy(...argumentsList)
+ Function.prototype.apply(thisArg,...argumentsList)
+ Function.prototype.call(thisArg,...argumentsList)
+ Reflect.apply(target,thisArgument,argumentsList)
4. 捕获器处理程序参数
+ target:目标对象
+ thisArg:调用函数时的this对象
+ argumentsList:调用函数时的参数列表
5. 捕获器不变式
target必须是一个函数对象
## construct()
1. 描述
construct()捕获器会在new操作符中被调用。对应的反射api方法为Reflect.construct()
``` js
const myTarget=function(){}
const proxy=new Proxy(myTarget,{
     construct(target,argumentsList,newTarget){
       console.log('construct')
       return Reflect.construct(...arguments)
     }
})
new proxy //construct
```
2. 返回值
construct()必须返回一个对象
3. 拦截的操作
+ new proxy(...argumentsList)
+ Reflect.construct(target,argumentsList,newTarget)
4. 捕获器处理程序参数
+ target:目标构造函数
+ argumentsList:传给目标构造函数的参数列表
+ newTarget:最初被调用的构造函数
5. 捕获器不变式
target必须可以用作构造函数
# 代理的实际应用
## 跟踪属性访问
通过捕获get、set和has等操作，可以获取对象属性被访问查询记录，达到监控的作用
``` js
const user={name:'jack'}
const proxy=new Proxy(user,{
      get(target,property,receiver){
        console.log(`get ${property}`)
        return Reflect.get(...arguments)
      }
      set(target,property,value,receivr){
           console.log(`set ${property}=${value}`)
           return Reflect.set(...arguments)
        }
})
 proxy.name //get name
 proxy.age=27 // set age=27
```
## 隐藏属性
代理的内部实现代码对外部是不可见的，借此可以隐藏目标对象上的属性：
``` js
const hiddenProperties=['foo','bar']
const targetObject={
       foo:1,
       bar:2,
       baz:3
}
const proxy=new Proxy(targetObject,{
       get(target,property){
         if(hiddenProperties.includes(property)){
            return undefined
         }else{
          return Reflect.get(...arguments)
         }
       }
       has(target,property){
             if(hiddenProperties.includes(property)){
                return false
             }else{
                return Reflect.has(...arguments)
             }
}
})
console.log(proxy.foo) //undefined
console.log(proxy.bar) //undefined
console.log(proxy.baz) //3
console.log('foo' in proxy)  //false
console.log('bar' in proxy)  //false
console.log('baz' in proxy)  //true
```
## 属性验证
所有赋值操作都会触发set()捕获器，可以根据赋值情况决定是否允许赋值
``` js
const target={num:0}
const proxy=new Proxy(target,{
      set(target,property,value){
         if(typeof value !=='number'){
            return false
         }else{
          return Reflect.set(...arguments)
         }
      }
})
proxy.num=1 
console.log(target.num) //1
proxy.num='2'
console.log(target.num) //1
```
## 函数与构造函数参数验证
对函数和构造函数的参数进行审查，方法如下：
``` js
function median(...nums){
    return nums.sort()[Math.floor(nums.length/2)]
}
const proxy=new Proxy(mdian,{
      apply(target,thisArg,argumentsList){
         for(const arg of argumentsList){
           if(typeof arg !=='number'){
             throw 'error'
           }
           return Reflect.apply(...arguments)
         }
      }
})
```
**要求实例化时必须给构造函数传参**
``` js
class User{
    constructor(id){
       this.id_=id
    }
}
const proxy=new Proxy(User,{
     construct(target,argumentsList,newTarget){
          if(argumentsList[0]===undefined){
             throw 'error'
          }else{
           return Reflect.construct(...arguments)
          }
     }
})
new proxy(1)
new proxy() //报错
```
## 数据绑定与可观察对象
**让被代理的类绑定到一个全局实例集合，让所有创建的实例都被添加到集合中**
``` js
const userList=[]
class User{
    construct(name){
       this.name_=name
    }
}
const proxy=new Proxy(User,{
     construct(){
       const newUser=Reflect.construct(...arguments)
       userList.push(newUser)
       return newUser
     }
})
 new proxy('j')
 new proxy('k')
console.log(userList) //[User{},User{}]
```
**把集合绑定到一个事件分派程序，每次插入新实例发送新消息**
``` js
const userList=[]
function emit(newValue){
  console.log(newValue)
}
const proxy=new Proxy(userList,{
      set(target,property,value,receiver){
         const result=Reflect.set(...arguments)
         if(result){
          emit(Reflect.get(target,property,receiver))
         }
        return result
      }
})
proxy.push('j')  //j
proxy.push('k')  //k
```
# 总结
代理是对象的门卫，可以进行对象常用方法的拦截处理，实际应用中可以跟踪属性访问、隐藏属性、阻止修改或删除属性、函数参数验证、构造函数参数验证、数据绑定等。