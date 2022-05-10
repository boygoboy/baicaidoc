---
title: js中的this
description: 介绍this相关问题
published: 1
date: 2022-05-10T05:04:02.121Z
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
2. apply 方法接受两个参数，第一个参数是this的指向，第二个参数是一个参数数组。当第一个参数为null、undefined的时候，默认指向window。如：getColor.apply(obj, ['yellow', 'blue', 'red'])
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