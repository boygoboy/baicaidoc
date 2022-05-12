---
title: js中的正则表达式
description: 正则表达式
published: 1
date: 2022-05-12T07:59:15.943Z
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
# .元字符
+ 描述
.表示匹配除换行符以外的任意字符
+ 例子
**如果要匹配点则需要转义**
``` js
let hd = `houdunren@com`;
console.log(/houdunren.com/i.test(hd)); //true
console.log(/houdunren\.com/i.test(hd)); //false
```
**使用.匹配除换行符外任意字符，下面匹配不到hdcms.com 因为有换行符**
``` js
const url = `
  https://www.houdunren.com
  hdcms.com
`;
console.log(url.match(/.+/)[0]);
```
**使用/s视为单行模式（忽略换行）时，. 可以匹配所有**
``` js
let hd = `
  <span>
    houdunren
    hdcms
  </span>
`;
let res = hd.match(/<span>.*<\/span>/s);
console.log(res[0]);
```
**正则中空格会按普通字符对待**
``` js
let tel = `010 - 999999`;
console.log(/\d+-\d+/.test(tel)); //false
console.log(/\d+ - \d+/.test(tel)); //true
```
# 模式修饰符
+ 描述
正则表达式在执行时会按他们的默认执行方式进行，但有时候默认的处理方式总不能满足我们的需求，所以可以使用模式修正符更改默认方式。
![msxsf.png](/msxsf.png)
## i
**将所有houdunren.com 统一为小写**
``` js
let hd = "houdunren.com HOUDUNREN.COM";
hd = hd.replace(/houdunren\.com/gi, "houdunren.com");
console.log(hd);
```
## g
**使用 g 修饰符可以全局操作内容**
``` js
let hd = "houdunren";
hd = hd.replace(/u/, "@");
console.log(hd); //没有使用 g 修饰符是，只替换了第一个

let hd = "houdunren";
hd = hd.replace(/u/g, "@");
console.log(hd); //使用全局修饰符后替换了全部的 u
```
## m
**用于将内容视为多行匹配，主要是对 ^和 $ 的修饰将下面是将以 #数字开始的课程解析为对象结构，学习过后面讲到的原子组可以让代码简单些**
``` js
let hd = `
  #1 js,200元 #
  #2 php,300元 #
  #9 houdunren.com # 后盾人
  #3 node.js,180元 #
`;
// [{name:'js',price:'200元'}]
let lessons = hd.match(/^\s*#\d+\s+.+\s+#$/gm).map(v => {
  v = v.replace(/\s*#\d+\s*/, "").replace(/\s+#/, "");
  [name, price] = v.split(",");
  return { name, price };
});
console.log(JSON.stringify(lessons, null, 2));
```
## u
**每个字符都有属性，如L属性表示是字母，P 表示标点符号，需要结合 u 模式才有效。其他属性简写可以访问[属性的别名](https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt) 网站查看。**
``` js
//使用\p{L}属性匹配字母
let hd = "houdunren2010.不断发布教程，加油！";
console.log(hd.match(/\p{L}+/u));
//使用\p{P}属性匹配标点
console.log(hd.match(/\p{P}+/gu));
```
**字符也有unicode文字系统属性 Script=文字系统，下面是使用 \p{sc=Han} 获取中文字符 han为中文系统，其他语言请查看 [文字语言表](http://www.unicode.org/standard/supported.html)**
``` js
let hd = `
张三:010-99999999,李四:020-88888888`;
let res = hd.match(/\p{sc=Han}+/gu);
console.log(res);
```
**使用 u 模式可以正确处理四个字符的 UTF-16 字节编码**
``` js
let str = "𝒳𝒴";
console.table(str.match(/[𝒳𝒴]/)); //结果为乱字符"�"

console.table(str.match(/[𝒳𝒴]/u)); //结果正确 "𝒳"
```
## lastIndex
+ 描述
RegExp对象lastIndex 属性可以返回或者设置正则表达式开始匹配的位置
1. 必须结合 g 修饰符使用
2. 对 exec 方法有效
3. 匹配完成时，lastIndex 会被重置为0
``` js
let hd = `后盾人不断分享视频教程，后盾人网址是 houdunren.com`;
let reg = /后盾人(.{2})/g;
reg.lastIndex = 10; //从索引10开始搜索
console.log(reg.exec(hd));
console.log(reg.lastIndex);

reg = /\p{sc=Han}/gu;
while ((res = reg.exec(hd))) {
  console.log(res[0]);
}
```
> 这里while循环中会一直匹配符号条件的字符直到匹配不到跳出循环
{.is-info}

## y
**我们来对比使用 y 与g 模式，使用 g 模式会一直匹配字符串**
``` js
let hd = "udunren";
let reg = /u/g;
console.log(reg.exec(hd));
console.log(reg.lastIndex); //3
console.log(reg.exec(hd));
console.log(reg.lastIndex); //3
console.log(reg.exec(hd)); //null
console.log(reg.lastIndex); //0
```
**但使用y 模式后如果从 lastIndex 开始匹配不成功就不继续匹配了,同时重置lastIndex**
``` js
let hd = "udunren";
let reg = /u/y;
console.log(reg.exec(hd));
console.log(reg.lastIndex); //1
console.log(reg.exec(hd)); //null
console.log(reg.lastIndex); //0
```
**因为使用 y 模式可以在匹配不到时停止匹配，在匹配下面字符中的qq时可以提高匹配效率**
``` js
let hd = `后盾人QQ群:11111111,999999999,88888888
后盾人不断分享视频教程，后盾人网址是 houdunren.com`;

let reg = /(\d+),?/y;
reg.lastIndex = 7;
while ((res = reg.exec(hd))) console.log(res[1]);
```
# 原子表
+ 描述
在一组字符中匹配某个元字符，在正则表达式中通过元字符表来完成，就是放到[] (方括号)中。
+ 语法
![yzb.png](/yzb.png)
+ 例子
**使用[]匹配其中任意字符即成功，下例中匹配ue任何一个字符，而不会当成一个整体来对待**
``` js
const url = "houdunren.com";
console.log(/ue/.test(url)); //false
console.log(/[ue]/.test(url)); //true
```
**日期的匹配**
``` js
let tel = "2022-02-23";
console.log(tel.match(/\d{4}([-\/])\d{2}\1\d{2}/));
```
> 这里\1表示使用[-\/]
{.is-info}

**获取0~3间的任意数字**
``` js
const num = "2";
console.log(/[0-3]/.test(num)); //true
```
**匹配a~f间的任意字符**
``` js
const hd = "e";
console.log(/[a-f]/.test(hd)); //true
```
**顺序为升序否则将报错**
``` js
const num = "2";
console.log(/[3-0]/.test(num)); //SyntaxError
```
**字母也要升序否则也报错**
``` js
const hd = "houdunren.com";
console.log(/[f-a]/.test(hd)); //SyntaxError
```
**原子表区间匹配：匹配以字母开头的字母数字下划线组成的长度在4-7位的字符**
``` js
let str='agf_ff'
console.log(str.magtch(/^[a-z]\w{3,6}$/i))
```
**排除匹配使用^**
``` js
let str='张三:010-99999999,李四:020-88888888;'
console.log(str.match(/[^:\d\-,]+/g))
```
**原子表中有些正则字符不需要转义，如果转义也是没问题的，可以理解为在原子表中. 就是小数点**
``` js
let str = "(houdunren.com)+";
console.table(str.match(/[().+]/g));
//使用转义也没有问题
console.table(str.match(/[\(\)\.\+]/g));
```
**可以使用 [\s\S] 或 [\d\D]匹配到所有字符包括换行符**
```js
let reg=/[\s\S]+/g
let reg1=/[\d\D]+/g
```
**下面是使用原子表知识删除所有标题**
``` js
<body>
  <p>后盾人</p>
  <h1>houdunren.com</h1>
  <h2>hdcms.com</h2>
</body>
<script>
  const body = document.body;
  const reg = /<(h[1-6])>[\s\S]*<\/\1>*/g;
  let content = body.innerHTML.replace(reg, "");
  document.body.innerHTML = content;
</script>
```
> * 代表0个或多个
{.is-info}
