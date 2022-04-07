---
title: node模块化
description: 模块化
published: 1
date: 2022-04-07T12:46:19.768Z
tags: module
editor: markdown
dateCreated: 2022-04-07T12:46:19.768Z
---

# require的作用
+ 描述
使用require()方法可以加载需要的内置模块、用户自定义模块、第三方模块。
1. 引入内置模块
``` javascript
const fs=require('fs')
```
2. 引入自定义模块
``` javascript
const custom=require(./custom) //.js后缀可以省略
```
3. 引入第三方模块
``` javascript
const moment=require('moment')
```
注：reuire引入模块中的代码会被立即执行
# nodejs中的模块作用域
+ 描述
nodejs模块中的变量只有在模块内部有效，且不可被外界访问，如需访问需导出。
+ 优点
防止全局变量的污染
## 向外共享模块作用域中的成员
+ module对象
该对象为模块的对象，里面有相关模块的信息其中exports对象可导出模块中的变量，向外共享作用域中的成员。
+ module.exports对象
1. 作用：module.exports可以向外界共享成员
2. 如何向外界共享成员
``` javascript
// fun.js文件
module.exports.age=12
module.exports.sayname=function(){
console.log('hello!')
}
```
3. module.exports以指向的对象为准
``` javascript
module.exports.age=12
module.exports={
age:12,
sayname(){
}
}
```
最终module.exports为最后指向的对象为准
+ exports导出对象
1. 作用
通过exports对象也可导出共享成员，其与module.exports指向同一个对象，最终共享结果以module.exports指向的对象为准
2. 例子
``` javascript
exports.name='111'
exports.age=12
```
3.  exports和module.exports的使用误区
使用require()模块时得到的永远是module.exports指向的对象
误区1：
``` javascript
exports.name='12'
module.exports={
age:16
}
//最后require()获得的模块为 {age:16}
```
误区2：
``` javascript
module.exports.name='jack'
exports={
name:'tim',
age:18
}
//最终获得的成员为：{name:'jack'}
```

