---
title: js中的this
description: 介绍this相关问题
published: 1
date: 2022-05-10T05:54:17.021Z
tags: this
editor: markdown
dateCreated: 2022-05-10T05:04:02.121Z
---

# es5中的this
## 普通函数中的this
+ 总是代表着它的直接调用者，如obj.fn，fn里的最外层this就是指向obj
+ 默认情况下，没有直接调用者，this指向window
+ 严格模式下（设置了'use strict'），this为undefined
+ 当使用call，apply，bind（ES5新增）绑定的，this指向绑定对象
1. call 方法第一个参数是this的指向，后面传入的是一个参数列表。当第一个参数为null、undefined的时候，默认指向window。如：getColor.call(obj, 'yellow', 'blue', 'red')
``` js
function Animal(name) {
    this.name = name;
    this.run = function() {
        console.log(`${this.name} is running!`)
    }
}

function Cat(name) {
    Animal.call(this, name);
}

let tom = new Cat("TOM");
tom.run(); // TOM is running!
```
2. apply 方法接受两个参数，第一个参数是this的指向，第二个参数是一个参数数组。当第一个参数为null、undefined的时候，默认指向window。如：getColor.apply(obj, ['yellow', 'blue', 'red'])
``` js
let arr = [2,3,4,5,6];
let other = [10,89];
arr.push.apply(arr, other); // or Array.prototype.apply(arr, other)

console.log(arr); // [2, 3, 4, 5, 6, 10, 89]
```
3. bind 方法和 call 方法很相似，第一个参数是this的指向，从第二个参数开始是接收的参数列表。区别在于bind方法返回值是函数以及bind接收的参数列表的使用。低版本浏览器没有该方法，需要自己手动实现
**例子：**
``` js
this.name="jack";
var demo={
name:"rose",
getName:function(){
	return this.name;
}
}

console.log(demo.getName());//输出rose  这里的this指向demo

var another=demo.getName;
console.log(another())//输出jack  这里的this指向全局对象
  
//运用bind方法更改this指向
var another2=another.bind(demo);
console.log(another2());//输出rose  这里this指向了demo对象了
```
## es6箭头函数中的this
+ 默认指向定义它时，所处上下文的对象的this指向。即ES6箭头函数里this的指向就是上下文里对象this指向，偶尔没有上下文对象，this就指向window
+ 即使是call，apply，bind等方法也不能改变箭头函数this的指向
**例子：**
diameter是普通函数，里面的this指向直接调用它的对象obj。perimeter是箭头函数，this应该指向上下文函数this的指向，这里上下文没有函数对象，就默认为window，而window里面没有radius这个属性，就返回为NaN。
``` js

const obj = {
  radius: 10,  
  diameter() {    
      return this.radius * 2
  },  
  perimeter: () => 2 * Math.PI * this.radius
}
console.log(obj.diameter())    // 20
console.log(obj.perimeter())    // NaN
```
> 注意：箭头函数中的this，指向与一般function定义的函数不同，比较容易绕晕，箭头函数this的定义：箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定即：this是继承自父执行上下文！！中的this！
{.is-warning}

``` js
var b=11;
var obj={
 b:22,
 say:()=>{
 console.log(this.b);
}
}
obj.say();//输出的值为11
```
这里的箭头函数中的this.b，箭头函数本身与say平级以key:value的形式，也就是箭头函数本身所在的对象为obj，而obj的父执行上下文就是window，因此这里的this.b实际上表示的是window.b，因此输出的是11。(this只有在函数被调用，或者通过构造函数new Object()的形式才会有this)
