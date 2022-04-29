---
title: js中的函数
description: 函数使用
published: 1
date: 2022-04-29T08:59:37.203Z
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
# 函数作为值
不仅可以把函数作为参数传给另一个函数也可在一个函数中返回另一个函数
```js
function callSomeFunction(someFunction,someArgument){
   return someFunction(someArgument)
}
function add(num){
return num+10
}
let result1=callSomeFunction(add,10)
console.log(result1) //20
```
> 如果是访问函数而不是调用函数，那必须不带括号
{.is-warning}

从一个函数返回另一个函数用处很大比如利用sort()方法对对象中某个属性排序
``` js
function createComparisonFunction(propertyName){
        return function(obj1,obj2){
           let value1=obj1[propertyName]
           let value2=obj2[propertyName]
           if(value1<value2){
            return -1
           }else if(value1>value2){
             return 1
           }else{
           return 0
           }
        }
}
let data=[
  {name:'a',age:10},
  {name:'b',age:5}
]
data.sort(createComparisonFunction('age'))
console.log(data[0].age) //5
```
> 拓展：这里返回的内部函数调用了外部函数propertyName变量，这种称为函数的闭包
{.is-info}

# 函数内部
## arguments
+ arguments对象是一个类数组包含调用参数时传入的所有参数
+ 只有以function关键字定义函数时才会有
+ arguments对象还有一个callee属性,用于指向arguments对象所在函数的指针
比如经典的阶乘函数
``` js
//优化前
function factorial(num){
if(mum<=1){
   return 1
}else{
    return num*factorial(num-1)
}
}
//优化后
function factorial(num){
     if(num<=1){
       return 1
     }else{
       return num*arguments.callee(num-1)
     }
}
let trueFactorial=factorial
factorial=function(){
      return 0
}
console.log(trueFactorial(5)) //120
console.log(factorial(5)) //0
```
上述如果用优化前的阶乘函数调用trueFactorial(5)输出的将会是0
## 函数中的this问题
+ 标准函数的this
this引用的是把函数当成方法调用的上下文对象，这时通常称其为this值，在网页全局上下文调用函数时，this指向windows
``` js
window.color='red'
let o={
   color:'blue'
}
function sayColor(){
 console.log(this.color)
}
sayColor() //red
o.sayColor=sayColor
o.sayColor() //blue
```
+ 箭头函数的this
箭头函数中this引用的是定义箭头函数的上下文
``` js
window.color='red'
let o={
 color:'blue'
}
let sayColor=()=>{console.log(this.color)}
sayColor() //red
o.sayColor=sayColor
o.sayColor() //red
```
这次调用o.sayColor()输出的任然是red因为这次用箭头函数定义的，this引用的是定义箭头函数的上下文即为window
> 箭头函数this实际应用：在回调函数或者定时器中使用可以保留定义箭头函数的上下文this
{.is-info}

``` js
function king(){
   this.name='jack'
  setTimeout(()=>{console.log(this.name)},1000)
}
function queen(){
   this.name='jack'
  setTimeout(function(){console.log(this.name)},1000)
}
new king() //jack
new queen() //undefined
```
这里new queen()过后输出undefined是因为定时器中的函数为普通函数，其this指向并非是queen
## caller
+ 描述
这个属性引用的是调用当前函数的函数，如果在全局作用域中调用的是null
``` js
function outer(){
  inner()
}
function inner(){
   console.log(inner.caller)
}
outer()
```
> 这里代码会输出outer()函数的源代码，因为inner.caller指向了outer()
{.is-info}

降低代码的耦合度可以使用如下方式
``` js
function outer(){
  inner()
}
function inner(){
   console.log(arguments.callee.caller)
}
outer()
```
> 严格模式下不能使用此方式会报错，也不能给caller属性赋值
{.is-warning}

## new.target
+ 描述
函数还可以作为构造函数实例化为一个新对象，也可作为普通函数被调用。new.target属性可以检测函数是否用new关键字调用的：函数正常调用则new.target值为undefined，如果new关键字调用则new.target将引用被调用的构造函数
+ 应用
``` js
function King(){
if(!new.target){
 throw 'error'
}
  console.log('success')
}
new King()
King()
```
> 这里只允许函数通过new被调用
{.is-info}

# 函数属性和方法
## length属性
+ 描述
保存函数定义的命名参数的个数
``` js
function sayName(a,b,c){
}
function sayAge(){}
console.log(sayName.length) //3
console.log(sayAge.length)  //0
```
## prototype属性
+ 描述
该属性时保存引用类型所有实例方法的地方意味着toString()、valueOf()方法都保存在prototype上,prototype不可枚举不能使用for-in循环
## 方法
函数还有两个方法：apply()和call()方法，这两个方法都会以指定的this值来调用函数，会设置调用函数时函数体内的this对象的值
+ apply()
该方法接收两个参数：函数内this的值和一个参数数组，第二个参数可以是Array的实例也可以是arguments对象
``` js
function sum(num1,num2){
   return num1+num2
}
function callSum1(num1,num2){
  return sum.apply(this,arguments)
}
console.log(callSum1(10,10)) //20
```
调用callSum1()时将函数体内的this传给sum，此时在全局作用域中调用callSum1，this指向window
> 注意：在严格模式下调用函数时如果没有指定上下文对象，this值不会指向window，除非使用apply()或call()把函数指定给一个对象，否则this的值为undefined。
{.is-warning}

+ call()
call()方法的作用一样，第一个传递的参数是this，而剩下的要传给被调用函数的参数则是逐个传递的即通过call()向函数传参必须将参数一个个列出来。
``` js
function sum(num1,num2){
    return num1+num2
}
function callSum(num1,num2){
   return sum.call(this,num1,num2)
}
console.log(callSum(10,10)) //20
```
> apply()和call()方法实际的应用是控制函数调用上下文即函数体内this值的能力
{.is-info}

``` js
window.color='red'
let o={
  color:'blue'
}
function sayColor(){
      console.log(this.color)
}
sayColor.call(this) //red
sayColor.call(window) //red
sayColor.call(o) //blue
```
+ bind()
bind()方法会创建一个新的函数实例，其this值会被绑定到传给bind()的对象：
``` js
window.color='red'
let o={
   color:'blue'
}'
function sayColor(){
       console.log(this.color)
}
let objectSayColor=sayColor.bind(o)
objectSayColor() //blue
```

