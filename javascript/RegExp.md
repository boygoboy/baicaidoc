---
title: js中的正则表达式
description: 正则表达式
published: 1
date: 2022-05-10T09:41:03.389Z
tags: regexp
editor: markdown
dateCreated: 2022-05-10T09:01:58.926Z
---

# 字面量创建正则表达式
``` js
let str='catafdkslujf'
let pattern=/u/
 console.log(pattern.test(str))
```
> 拓展：RegExp有两个方法exec()和test()方法
{.is-warning}

+ exec()方法
RegExp的exec()方法和String的match()方法很类似，它对一个指定的字符串执行一个正则表达式匹配，如果没有找到任何一个匹配，它 将返回null，否则返回一个数组，这个数组的第一个元素包含的是与正则表达式相匹配的字符串,余下的所有元素包含的是匹配的各个分组。而且，正则表达式 对象的index属性还包含了匹配发生的字符串的位置，属性input引用的则是被检索的字符串。 如果正则表达式具有g标志，它将把该对象的lastIndex属性设置到紧接着匹配字符串的位置开始检索，如果exec()没有发现任何匹配，它将把 lastIndex属性重置为0，这一特殊的行为可以使你可以反复调用exec()遍历一个字符串中所有的正则表达式匹配。
+ test()方法
RegExp对象的test()参数为一个字符串，如果这个字符串包含正则表达式的一个匹配，它就返回true，否则返回false 当一个具有g标志的正则表达式调用test()方法时，它的行为和exec()相同，既它从lastIndex处开始检索特定的字符串，如果它发现匹配，就将lastIndex设置为紧接在那个匹配之后的字符的位置，这样我们就可以使用方法test()来遍历字符串了。
# 使用对象创建正则表达式
+ 描述
使用new RegExp(正则，匹配模式)可创建正则表达式
+ 案例
搜索内容标红操作
``` js
let con=prompt('请输入要检测内容')
let reg=new RefExp(con,'g')
let div=document.querySelector('div')
div.innerHtml=div.innerHtml.replace(reg,search=>{
  return `<span style="color:red;">${search}</span>`
})
```
# 选择符的使用
+ 描述 
'|'选择符相当于或者
+ 案例
``` js
let str='fdjfklu'
console.log(/u|@/.test(str)) //true
```
# 初识原子组和原子表
+ 描述
用[]包裹的为原子表用()包裹的是原子组，[]和()的作用与'|'类似，[]和()区别：[]中的每个字符都是或者关系，()中'|'两边的整体字符串是或者的关系
``` js
let str=1245457
let reg=/[1234]/
console.log(str.match(reg)) //这里会查找出字符串中的1,2,3,4
let reg1=/(12|34)/ 
console.log(str.match(reg1)) //这里会查找配置'|'左面的12和右面的34
```
# 正则中的转义
+ 描述
对于一些特殊符号需要使用\进行转义，此外对字符串的转义还是字符串，比如```console.log('\d'=='d')```因此在字符串中的正则转义需要使用\\进行转义
+ 例子
``` js
let price='12.13'
//.除换行外任何字符
console.log(/\d+\.\d+/.test(price))
let reg=new RegExp("\\d+\\.\\d+")
console.log(reg.test(price))
```
# 字符边界约束
+ 描述
'^'表示匹配模式以什么开始，'$'表示匹配模式以什么结尾
+ 例子
``` js
let str=21kk33
console.log(/^\d$/.test(str)) //表示以数字开头以数字结尾的字符串
```
# 数值与空白元字符
+ 描述
d表示数值类型数据，D表示非数值类型数据，s表示空白数据、S表示非空白数据
+ 例子
``` js
let str='hong 2020'
console.log(str.match(/\d+/)) //2020
console.log(str.match(/\D+/))  //hong
```
# w与W元字符
+ 描述
w表示匹配字母数字下划线的字符，W表示匹配非字母数字下划线的字符
+ 例子
``` js
let str=cai23_
console.log(str.match(/\w+/)) //cai123_
//匹配邮箱
let email=2343fdssdf@qq.com
console.log(email.match(/^\w+@\w+\.\w+$/))
```
