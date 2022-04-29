---
title: js中的函数
description: 函数使用
published: 1
date: 2022-04-29T03:46:50.076Z
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
若函数是一个获取函数、设置函数、或者使用bind()实例化，那么标识符前面会加上一个前缀
``` js
function foo(){}
console.log(foo.bind(null).name) //bound foo
let dog={
    year:1,
    get age(){
        return this.years
    }
    set age(newAge){
        this.years=newAge
    }
}
let propertyDescriptor=Object.getOwnPropertyDescriptor(dog,'age')
console.log(propertyDescriptor.get.name) //get age
console.log(propertyDescriptor.set.name) //set age
```
# 函数参数
## arguments
函数中传入的参数没有类型和个数限制，因为函数的参数在内部表现一个数组用arguments对象接收，arguments为一个伪数组：可以通过arguments[i]访问传入的参数也可通过arguments.length访问传入参数的个数，但不可使用数组的一些内置方法。
``` js
function doAdd(){
  if(arguments.length==1){
   console.log(arguments[0]+10)
  }else if(arguments.length==2){
   console.log(arguments[0]+arguments[1])
  }
}
doAdd(10) //20
doAdd(20,30) //50
```
arguments对象可以和命名参数一起使用
``` js
function doAdd(){
  if(arguments.length==1){
   console.log(arguments[0])
  }else if(arguments.length==2){
   console.log(arguments[0]+num2)
  }
}
```
arguments的值与对应的名称参数同步（非严格模式）
``` js
function doAdd(num1,num2){
    arguments[1]=10
    console.log(arguments[0]+num2)
}
doAdd(10,0) //20
```
修改了arguments[1]的值也会同步修改num2的值，这两个值在内存中是分开的
## 箭头函数中的参数
+ 箭头函数中传递的参数不能使用arguments关键字访问只能通过定义的命名参数访问
``` js
function foo(){
   console.log(arguments[0]) 
}
foo(5) //5
let bar=()=>{
  console.log(arguments[0])
}
bar(5) //报错
```
+ 箭头函数没有arguments对象，但可以在包装函数中把它提供给箭头函数
``` js
function foo(){
 let bar=()=>{
    console.log(arguments[0])
 }
 bar()
}
foo(5) //5
```
> 函数中所有参数都按值传递，不可能按引用传递参数，如果把对象作为参数传递，那么传递的值是这个对象的引用
{.is-warning}

## 没有重载
js中没有重载定义两个同名的函数后者会覆盖前者
``` js
function addSum(num){
 return num+100
}
function addSum(num){
 return num+200
}
addSum(100) //300
````
# 默认参数值
## 为传入参数设置默认值
当传递的参数为undefined时就为设置的默认值
``` js
function makeKing(name='jack'){
 return `king ${name}`
}
makeKing('hill') //king hill
makeKing()  // king jack
```
## arguments对象的值不反映参数的默认值
``` js
function makeKing(name='hill'){
    return `king ${name}`
}
console.log(makeKing()) //King undefined
```
## 默认参数值类型
原始值或对象类型，也可使用调用函数返回的值
``` js
let romanNum=['A','B','C']
let index=0
function getNum(){
    return romanNum(index++)
}
function makeKing(name=getNum()){
  return name
}
console.log(makeKing()) //A
console.log(makeKing()) //B
console.log(makeKing()) //C
```
箭头函数同样也可使用默认参数，当只有一个参数时必须使用括号不能省略
``` js
let makeKing=(name='hill')=>`king ${name}`
```
> 计算默认值的函数只有在调用函数但未传相应参数时才会调用
{.is-warning}

## 默认参数作用域与暂时性死区
参数定义默认值按先后定义的顺序规则初始化，因此后定义的默认值的参数可以引用先定义的参数
``` js
function makeKing(name='hill' nametem=name){
       return `king ${name}${nametem}`
}
console.log(makeKing()) //King hillhill
```
暂时性死区规则即前面定义的参数不能引用后面定义的
``` js
//调用时不传第一个参数会报错
function makeKing(name=nametem,nametem='hill'){
    return ·king ${name} ${nametem}·
}
```
参数也存在于自己的作用域中，他们不能引用函数体的作用域
``` js
//调用时不传第二个参数会报错
function makeKing(name='hill' namtem=defaultname){
    let defaultname='jack'
    return `king ${name} ${nametem}`
}
```
# 参数拓展与收集
## 拓展参数
传递的参数不是一个数组而是分别传入数组的元素
``` js
let values=[1,2,3,4]
function getSum(){
  let sum=0
  for(let i=0;i<arguments.length;i++){
    sum+=arguments[i]
  }
  return sum
}
console.log(getSum(...values)) //10
```
使用拓展操作符时不妨碍前后再传其他值
``` js
console.log(1,...values,2) //13
```
在普通函数或者箭头函数中可将拓展操作符用于命名参数，当然同时也可使用默认参数
``` js
let getSum=(a,b,c=0)=>a+b+c
console.log(getSum(...[1,2,3])) //6
console.log(getSum(...[1,2])) //3
```
## 收集参数
使用拓展操作符把不同长度的独立参数组合成一个数组
``` js
function getSum(...values){
  return values.reduce((x,y)=>x+y,0)
}
console.log(getSum(1,2,3)) //6
```
收集的参数前面如果还有命名参数则只会收集其余的参数，没有则得到空数组，收集的参数结果可变只能把它作为最后一个参数
``` js
//可以
function getValue(firstvalue,...values){}
//不可以
function getValue(...values,lastvalue){}
```
箭头函数可以使用收集参数的方法模拟arguments
``` js
let getSum=(...values)=>{
   return values.reduce((x,y)=>x+y,0)
}
console.log(getSum(1,2,3))
```
# 函数声明与函数表达式
函数声明与函数表达式区别
+ js代码执行之前会先读取函数声明并在执行上下文中生成函数定义
+ 函数表达式必须等到代码执行到那一行才会在执行上下文中生成函数定义
因此函数表达式不能像函数声明那样调用否则报错
``` js
console.log(sum(10,10))
function sum(num1,num2){
  return num1+num2
}
//报错
console.log(sum(10,10))
let sum=function(num1,num2){
  return num1+num2
}
```
