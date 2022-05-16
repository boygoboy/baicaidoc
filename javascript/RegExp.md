---
title: js中的正则表达式
description: 正则表达式
published: 1
date: 2022-05-16T04:00:48.365Z
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
>  * 代表0个或多个
{.is-info}

# 原子组
**描述**
+ 如果一次要匹配多个元子，可以通过元子组完成
+ 原子组与原子表的差别在于原子组一次匹配多个元子，而原子表则是匹配任意一个字符
+ 元字符组用 () 包裹
## 基本使用
没有添加 g 模式修正符时只匹配到第一个，匹配到的信息包含以下数据
![yzz.png](/js-regexp/yzz.png)
在match中使用原子组匹配，会将每个组数据返回到结果中
+ 0 为匹配到的完成内容
+ 1/2 等 为原子级内容
+ index 匹配的开始位置
+ input 原始数据
+ groups 组别名
``` js
let hd = "houdunren.com";
console.log(hd.match(/houdun(ren)\.(com)/)); 
//["houdunren.com", "ren", "com", index: 0, input: "houdunren.com", groups: undefined]
```
下面使用原子组匹配标题元素
``` js
let hd = `
  <h1>houdunren</h1>
  <span>后盾人</span>
  <h2>hdcms</h2>
`;
console.table(hd.match(/<(h[1-6])[\s\S]*<\/\1>/g));
```
检测 0~100 的数值，使用 parseInt 将数值转为10进制
``` js
console.log(/^(\d{1,2}|100)$/.test(parseInt(09, 10)));
```
## 邮箱匹配
下面使用原子组匹配邮箱
``` js
let hd = "2300071698@qq.com";
let reg = /^[\w\-]+@[\w\-]+\.(com|org|cn|cc|net)$/i;
console.dir(hd.match(reg));
```
如果邮箱是以下格式 houdunren@hd.com.cn 上面规则将无效，需要定义以下方式
``` js
let hd = `admin@houdunren.com.cn`;
let reg = /^[\w-]+@([\w-]+\.)+(org|com|cc|cn)$/;
console.log(hd.match(reg));
```
## 引用分组
+ 描述
原子组在正则中可以用\1,\2,\3......按原子组顺序代替，在替换等操作中可以用$1,$2,$3.......等按原子组顺序替换
+ 例子
\n 在匹配时引用原子组， $n 指在替换时使用匹配的组数据。下面将标签替换为p标签
``` js
let hd = `
  <h1>houdunren</h1>
  <span>后盾人</span>
  <h2>hdcms</h2>
`;

let reg = /<(h[1-6])>([\s\S]*)<\/\1>/gi;
console.log(hd.replace(reg, `<p>$2</p>`));
```
> 当替换操做第二个参数使用函数时，函数中的参数为返回的原子组
{.is-info}

## 嵌套分组不记录组
如果只希望组参与匹配，便不希望返回到结果中使用 (?: 处理。下面是获取所有域名的示例
``` js
let hd = `
  https://www.houdunren.com
  http://houdunwang.com
  https://hdcms.com
`;

let reg = /https?:\/\/((?:\w+\.)?\w+\.(?:com|org|cn))/gi;
while ((v = reg.exec(hd))) {
  console.dir(v);
}
```
> 这里https后面的？指s不确定有没有，使用(?:将对应原子组排除在外，执行reg.exec时只能得到最外层包裹域名的原子组
{.is-info}

## 分组别名
如果希望返回的组数据更清晰，可以为原子组编号，结果将保存在返回的 groups字段中
``` js
let hd = "<h1>houdunren.com</h1>";
console.dir(hd.match(/<(?<tag>h[1-6])[\s\S]*<\/\1>/));
````
组别名使用 ?<> 形式定义，下面将标签替换为p标签
``` js
let hd = `
  <h1>houdunren</h1>
  <span>后盾人</span>
  <h2>hdcms</h2>
`;
let reg = /<(?<tag>h[1-6])>(?<con>[\s\S]*)<\/\1>/gi;
console.log(hd.replace(reg, `<p>$<con></p>`));
```
获取链接与网站名称组成数组集合
``` js
<body>
  <a href="https://www.houdunren.com">后盾人</a>
  <a href="https://www.hdcms.com">hdcms</a>
  <a href="https://www.sina.com.cn">新浪</a>
</body>

<script>
  let body = document.body.innerHTML;
  let reg = /<a\s*.+?(?<link>https?:\/\/(\w+\.)+(com|org|cc|cn)).*>(?<title>.+)<\/a>/gi;
  const links = [];
  for (const iterator of body.matchAll(reg)) {
    links.push(iterator["groups"]);
  }
  console.log(links);
</script>
```
# 重复匹配
+ 基本使用
如果要重复匹配一些内容时我们要使用重复匹配修饰符，包括以下几种。
![zfpp.png](/js-regexp/zfpp.png)
> 因为正则最小单位是元字符，而我们很少只匹配一个元字符如a、b所以基本上重复匹配在每条正则语句中都是必用到的内容。
{.is-warning}

默认情况下重复选项对单个字符进行重复匹配，即不是贪婪匹配
``` js
let hd = "hdddd";
console.log(hd.match(/hd+/i)); //hddd
```
使用原子组后则对整个组重复匹配
``` js
let hd = "hdddd";
console.log(hd.match(/(hd)+/i)); //hd
```
下面是验证坐机号的正则
``` js
let hd = "010-12345678";
console.log(/0\d{2,3}-\d{7,8}/.exec(hd));
```
验证用户名只能为3~8位的字母或数字下划线，并以字母开始
``` js
<body>
  <input type="text" name="username" />
</body>
<script>
  let input = document.querySelector(`[name="username"]`);
  input.addEventListener("keyup", e => {
    const value = e.target.value;
    let state = /^[a-z][\w]{2,7}$/i.test(value);
    console.log(
      state ? "正确！" : "用户名只能为3~8位的字母或数字，并以字母开始"
    );
  });
</script>
```
验证密码必须包含大写字母并在5~10位之间
``` js
<body>
<input type="text" name="password" />
</body>
<script>
let input = document.querySelector(`[name="password"]`);
input.addEventListener("keyup", e => {
  const value = e.target.value.trim();
  const regs = [/^[a-zA-Z0-9]{5,10}$/, /[A-Z]/];
  let state = regs.every(v => v.test(value));
  console.log(state ? "正确！" : "密码必须包含大写字母并在5~10位之间");
});
</script>
```
# 禁止贪婪
+ 描述
正则表达式在进行重复匹配时，默认是贪婪匹配模式，也就是说会尽量匹配更多内容，但是有的时候我们并不希望他匹配更多内容，这时可以通过?进行修饰来禁止重复匹配
![jztl.png](/js-regexp/jztl.png)
``` js
let str = "aaa";
console.log(str.match(/a+/)); //aaa
console.log(str.match(/a+?/)); //a
console.log(str.match(/a{2,3}?/)); //aa
console.log(str.match(/a{2,}?/)); //aa
```
+ 例子
将所有span更换为h4 并描红，并在内容前加上 后盾人-
``` js
<body>
  <main>
    <span>houdunwang</span>
    <span>hdcms.com</span>
    <span>houdunren.com</span>
  </main>
</body>
<script>
  const main = document.querySelector("main");
  const reg = /<span>([\s\S]+?)<\/span>/gi;
  main.innerHTML = main.innerHTML.replace(reg, (v, p1) => {
    console.log(p1);
    return `<h4 style="color:red">后盾人-${p1}</h4>`;
  });
</script>
```
使用禁止贪婪查找页面中的标题元素
``` js
<body>
  <h1>
    houdunren.com
  </h1>
  <h2>hdcms.com</h2>
  <h3></H3>
  <H1></H1>
</body>

<script>
  let body = document.body.innerHTML;
  let reg = /<(h[1-6])>[\s\S]*?<\/\1>/gi;
  console.table(body.match(reg));
</script>
```
# 全局匹配
下面是使用match 全局获取页面中标签内容，但并不会返回匹配细节
``` js
<body>
  <h1>houdunren.com</h1>
  <h2>hdcms.com</h2>
  <h1>后盾人</h1>
</body>

<script>
  function elem(tag) {
    const reg = new RegExp("<(" + tag + ")>.+?<\.\\1>", "g");
    return document.body.innerHTML.match(reg);
  }
  console.table(elem("h1"));
</script>
```
## matchAll
需要添加g修饰符
在新浏览器中支持使用 matchAll 操作，并返回迭代对象
``` js
let str = "houdunren";
let reg = /[a-z]/ig;
for (const iterator of str.matchAll(reg)) {
  console.log(iterator);
}
```
在原型定义 matchAll方法，用于在旧浏览器中工作，不需要添加g 模式运行
``` js
String.prototype.matchAll = function(reg) {
  let res = this.match(reg);
  if (res) {
    let str = this.replace(res[0], "^".repeat(res[0].length));
    let match = str.matchAll(reg) || [];
    return [res, ...match];
  }
};
let str = "houdunren";
console.dir(str.matchAll(/(U)/i));
```
## exec
使用 g 模式修正符并结合 exec 循环操作可以获取结果和匹配细节
``` js
<body>
  <h1>houdunren.com</h1>
  <h2>hdcms.com</h2>
  <h1>后盾人</h1>
</body>
<script>
  function search(string, reg) {
    const matchs = [];
    while ((data = reg.exec( string))) {
      matchs.push(data);
    }
    return matchs;
  }
  console.log(search(document.body.innerHTML, /<(h[1-6])>[\s\S]+?<\/\1>/gi));
</script>
```
使用上面定义的函数来检索字符串中的网址
``` js
let hd = `https://hdcms.com  
https://www.sina.com.cn
https://www.houdunren.com`;

let res = search(hd, /https?:\/\/(\w+\.)?(\w+\.)+(com|cn)/gi);
console.dir(res);
```
# 字符方法
下面介绍的方法是 String 提供的支持正则表达式的方法
## search
+ 描述
search() 方法用于检索字符串中指定的子字符串，也可以使用正则表达式搜索，返回值为索引位置
+ 例子
``` js
let str = "houdunren.com";
console.log(str.search("com"));
//使用正则表达式
console.log(str.search(/\.com/i));
```
## match
直接使用字符串搜索
``` js
let str = "houdunren.com";
console.log(str.match("com"));
```
使用正则获取内容，下面是简单的搜索字符串
``` js
let hd = "houdunren";
let res = hd.match(/u/);
console.log(res);
console.log(res[0]); //匹配的结果
console.log(res[index]); //出现的位置
```
如果使用 g 修饰符时，就不会有结果的详细信息了（可以使用exec），下面是获取所有h1~6的标题元素
``` js
let body = document.body.innerHTML;
let result = body.match(/<(h[1-6])>[\s\S]+?<\/\1>/g);
console.table(result);
```
## matchAll
在新浏览器中支持使用 matchAll 操作，并返回迭代对象
``` js
let str = "houdunren";
let reg = /[a-z]/ig;
for (const iterator of str.matchAll(reg)) {
  console.log(iterator);
}
```
## split
用于使用字符串或正则表达式分隔字符串，下面是使用字符串分隔日期
``` js
let str = "2023-02-12";
console.log(str.split("-")); //["2023", "02", "12"]
```
如果日期的连接符不确定，那就要使用正则操作了
``` js
let str = "2023/02-12";
console.log(str.split(/-|\//));
console.log(str.split(/[-\/]/));
```
## replace
replace 方法不仅可以执行基本字符替换，也可以进行正则替换，下面替换日期连接符
``` js 
let str = "2023/02/12";
console.log(str.replace(/\//g, "-")); //2023-02-12
```
替换字符串可以插入下面的特殊变量名：
![thblm.png](/js-regexp/thblm.png)
在后盾人前后添加三个=
``` js
let hd = "=后盾人=";
console.log(hd.replace(/后盾人/g, "$`$`$&$'$'"));
```
把电话号用 - 连接
``` js
let hd = "(010)99999999 (020)8888888";
console.log(hd.replace(/\((\d{3,4})\)(\d{7,8})/g, "$1-$2"));
```
把所有教育汉字加上链接 https://www.houdunren.com
``` js
<body>
  在线教育是一种高效的学习方式，教育是一生的事业
</body>
<script>
  const body = document.body;
  body.innerHTML = body.innerHTML.replace(
    /教育/g,
    `<a href="https://www.houdunren.com">$&</a>`
  );
</script>
```
为链接添加上https ，并补全 www.
``` js
<body>
  <main>
    <a style="color:red" href="http://www.hdcms.com">
      开源系统
    </a>
    <a id="l1" href="http://houdunren.com">后盾人</a>
    <a href="http://yahoo.com">雅虎</a>
    <h4>http://www.hdcms.com</h4>
  </main>
</body>
<script>
  const main = document.querySelector("body main");
  const reg = /(<a.*href=['"])(http)(:\/\/)(www\.)?(hdcms|houdunren)/gi;
  main.innerHTML = main.innerHTML.replace(reg, (v, ...args) => {
    args[1] += "s";
    args[3] = args[3] || "www.";
    return args.splice(0, 5).join("");
  });
</script>
```
将标题标签全部替换为 p 标签
``` js
<body>
  <h1>houdunren.com</h1>
  <h2>hdcms.com</h2>
  <h1>后盾人</h1>
</body>

<script>
  const reg = /<(h[1-6])>(.*?)<\/\1>/g;
  const body = document.body.innerHTML;
  const html = body.replace(reg, function(str, tag, content) {
    return `<p>${content}</p>`;
  });
  document.body.innerHTML = html;
</script>
```
删除页面中的 h1~h6 标签
``` js
<body>
  <h1>houdunren.com</h1>
  <h2>hdcms.com</h2>
  <h1>后盾人</h1>
</body>
<script>
  const reg = /<(h[1-6])>(.*?)<\/\1>/g;
  const body = document.body.innerHTML;
  const html = body.replace(reg, "");
  document.body.innerHTML = html;
</script>
```
> 回调函数：replace 支持回调函数操作，用于处理复杂的替换逻辑
{.is-info}

![hdhs.png](/js-regexp/hdhs.png)
使用回调函数将 后盾人 添加上链接
``` js
<body>
  <div class="content">
    后盾人不断更新优质视频教程
  </div>
</body>

<script>
  let content = document.querySelector(".content");
  content.innerHTML = content.innerHTML.replace("后盾人", function(
    search,
    pos,
    source
  ) {
    return `<a href="https://www.houdunren.com">${search}</a>`;
  });
</script>
```
为所有标题添加上 hot 类
``` js
<body>
  <div class="content">
    <h1>后盾人</h1>
    <h2>houdunren.com</h2>
    <h1>后盾人</h1>
  </div>
</body>
<script>
  let content = document.querySelector(".content");
  let reg = /<(h[1-6])>([\s\S]*?)<\/\1>/gi;
  content.innerHTML = content.innerHTML.replace(
    reg,
    (
      search, //匹配到的字符
      p1, //第一个原子组
      p2, //第二个原子组
      index, //索引位置
      source //原字符
    ) => {
      return `
    <${p1} class="hot">${p2}</${p1}>
    `;
    }
  );
</script>
```
提取链接link和title
核心代码
``` js
let reg=/<a.*?href=(['"])(?<link>.*?)\1>(?<title>.*?)<\/a>/ig
for(const iterator of str.matchAll(reg)){
    links.push(iterator["groups"])
}
```
# 正则方法
下面是 RegExp 正则对象提供的操作方法
## test
检测输入的邮箱是否合法
``` js
<body>
  <input type="text" name="email" />
</body>

<script>
  let email = document.querySelector(`[name="email"]`);
  email.addEventListener("keyup", e => {
    console.log(/^\w+@\w+\.\w+$/.test(e.target.value));
  });
</script>
```
## exec
不使用 g 修饰符时与 match 方法使用相似，使用 g 修饰符后可以循环调用直到全部匹配完。
+ 使用 g 修饰符多次操作时使用同一个正则，即把正则定义为变量使用
+ 使用 g 修饰符最后匹配不到时返回 null
计算内容中后盾人出现的次数
``` js
<body>
  <div class="content">
    后盾人不断分享视频教程，后盾人网址是 houdunren.com
  </div>
</body>

<script>
  let content = document.querySelector(".content");
  let reg = /(?<tag>后盾)人/g;
  let num = 0;
  while ((result = reg.exec(content.innerHTML))) {
    num++;
  }
  console.log(`后盾人共出现${num}次`);
</script>
```
# 断言匹配
断言虽然写在扩号中但它不是组，所以不会在匹配结果中保存，可以将断言理解为正则中的条件。
## (?=exp)
+ 描述
?=exp 匹配后面为 exp 的内容
+ 例子
把后面是教程 的后盾人汉字加上链接
``` js
<body>
  <main>
    后盾人不断分享视频教程，学习后盾人教程提升编程能力。
  </main>
</body>

<script>
  const main = document.querySelector("main");
  const reg = /后盾人(?=教程)/gi;
  main.innerHTML = main.innerHTML.replace(
    reg,
    v => `<a href="https://houdunren.com">${v}</a>`
  );
</script>
```
下面是将价格后面 添加上 .00
``` js
<script>
  let lessons = `
    js,200元,300次
    php,300.00元,100次
    node.js,180元,260次
  `;
  let reg = /(\d+)(.00)?(?=元)/gi;
  lessons = lessons.replace(reg, (v, ...args) => {
    args[1] = args[1] || ".00";
    return args.splice(0, 2).join("");
  });
  console.log(lessons);
</script>
```
使用断言验证用户名必须为五位，下面正则体现断言是不是组，并且不在匹配结果中记录
``` js
<body>
  <input type="text" name="username" />
</body>

<script>
  document
    .querySelector(`[name="username"]`)
    .addEventListener("keyup", function() {
      let reg = /^(?=[a-z]{5}$)/i;
      console.log(reg.test(this.value));
    });
</script>
```
## (?<=exp)
+ 描述
?<=exp 匹配前面为 exp 的内容
+ 例子
匹配前面是houdunren 的数字
``` js
let hd = "houdunren789hdcms666";
let reg = /(?<=houdunren)\d+/i;
console.log(hd.match(reg)); //789
```
匹配前后都是数字的内容
``` js
let hd = "houdunren789hdcms666";
let reg = /(?<=\d)[a-z]+(?=\d{3})/i;
console.log(hd.match(reg));
```
所有超链接替换为houdunren.com
``` js
<body>
  <a href="https://baidu.com">百度</a>
  <a href="https://yahoo.com">雅虎</a>
</body>
<script>
  const body = document.body;
  let reg = /(?<=<a.*href=(['"])).+?(?=\1)/gi;
  // console.log(body.innerHTML.match(reg));
  body.innerHTML = body.innerHTML.replace(reg, "https://houdunren.com");
</script>
```
下例中将 后盾人 后面的视频添加上链接
``` js
<body>
  <h1>后盾人视频不断录制案例丰富的视频教程</h1>
</body>

<script>
  let h1 = document.querySelector("h1");
  let reg = /(?<=后盾人)视频/;
  h1.innerHTML = h1.innerHTML.replace(reg, str => {
    return `<a href="https://www.houdunren.com">${str}</a>`;
  });
</script>
```
将电话的后四位模糊处理
``` js
let users = `
  向军电话: 12345678901
  后盾人电话: 98745675603
`;

let reg = /(?<=\d{7})\d+\s*/g;
users = users.replace(reg, str => {
  return "*".repeat(4);
});
console.log(users); //向军电话: 1234567****后盾人电话: 9874567****
```
获取标题中的内容
``` js
let hd = `<h1>后盾人视频不断录制案例丰富的视频教程</h1>`;
let reg = /(?<=<h1>).*(?=<\/h1>)/g;
console.log(hd.match(reg));
```
## (?!exp)
+ 描述
后面不能出现 exp 指定的内容
+ 例子
使用 (?!exp)字母后面不能为两位数字
``` js
let hd = "houdunren12";
let reg = /[a-z]+(?!\d{2})$/i;
console.table(reg.exec(hd));
```
下例为用户名中不能出现向军
``` js
<body>
  <main>
    <input type="text" name="username" />
  </main>
</body>
<script>
  const input = document.querySelector(`[name="username"]`);
  input.addEventListener("keyup", function() {
    const reg = /^(?!.*向军.*)[a-z]{5,6}$/i;
    console.log(this.value.match(reg));
  });
</script>
```
## (?<!exp)
+ 描述
前面不能出现exp指定的内容
+ 例子
获取前面不是数字的字符
``` js
let hd = "hdcms99houdunren";
let reg = /(?<!\d+)[a-z]+/i;
console.log(reg.exec(hd)); //hdcms
```
把所有不是以 https://oss.houdunren.com 开始的静态资源替换为新网址
``` js
<body>
  <main>
    <a href="https://www.houdunren.com/1.jpg">1.jpg</a>
    <a href="https://oss.houdunren.com/2.jpg">2.jpg</a>
    <a href="https://cdn.houdunren.com/2.jpg">3.jpg</a>
    <a href="https://houdunren.com/2.jpg">3.jpg</a>
  </main>
</body>
<script>
  const main = document.querySelector("main");
  const reg = /https:\/\/(\w+)?(?<!oss)\..+?(?=\/)/gi;
  main.innerHTML = main.innerHTML.replace(reg, v => {
    console.log(v);
    return "https://oss.houdunren.com";
  });
</script>
```
