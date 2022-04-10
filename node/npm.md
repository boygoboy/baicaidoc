---
title: 自定义npm包
description: 开发自己的npm包
published: 1
date: 2022-04-10T09:18:14.359Z
tags: npm
editor: markdown
dateCreated: 2022-04-10T09:18:14.359Z
---

# 初始化npm包结构
1. 创建npm包的根目录（注意是英文不能为中文且英文间不能有空格）
2. 在根目录下创建三个文件分别为：
+ package.json:包管理配置文件
+ index.js：包的入口文件
+ README.md：包的说明文档
> package.json配置文件介绍
{.is-info}

``` javascript
{
  "name":"my-npm",
  "version":"1.0.0",
  "main":"index.js",
  "description":"描述信息",
  "keywords":["itheima","mynpm"],
  "license":"ISC"
}
```
+ name：为包的名称
+ version：包的版本号
+ main：包的入口文件
+ description：包的描述
+ keywords：搜索包的关键词
+ license：包的开源许可协议
# 封装需要的功能函数
> 例如这里需要一类格式化时间功能的函数和一类转译html标签的功能的npm包，实现思路分为以下几个步骤：
1. 分别创建formattime.js文件和babelhtml.js文件并在各自的js文件里面编写功能代码，最后将功能模块导出，大致如下：
**formattime.js**
``` javascript
function formatDate(datestr){
  //功能代码
......
}
module.exports={
formatDate
}
```
**babelhtml.js**
``` javascript
function htmlenCode(str){
//功能代码
 ......
}
 module.exports={
 htmlenCode
 }
```
2. 在index.js入口文件中聚合所有功能模块并导出，实现代码如下：
``` javascript
const format=require('./formattime.js')
const babel=require('./babelhtml.js')
model.exports={
...format,
...babel
}
```
> 这里导出模块需要使用拓展运算符把format和babel模块中对象展开
{.is-success}
# 包发布到npm库中
> 把包发布到npm库中需要在官方注册账号然后才能发布，下面介绍发布流程：
{.is-info}
1. 在已有账号的情况下，在终端登录账号（注意是终端中并非官网）
输入：```npm login``` 命令，然后根据提示信息输入账户信息
2. 使用 ```npm publish``` 将包发布到官方库中
发布之前需要到官方库中搜索包名是否存在
3. 删除npm包
+ 删除包的命令
``` npm unpublish 包名 --force```
> 注意：1. npm unpublish命令只能删除72小时内发布的包。 2. 删除的包在24小时内不允许重复发布。3. 发布的包需有意义。
{.is-warning}


