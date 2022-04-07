---
title: path模块
description: path模块
published: 1
date: 2022-04-07T12:45:22.681Z
tags: path
editor: markdown
dateCreated: 2022-04-07T12:45:22.681Z
---

# path模块

# path.join()方法

* 描述

该方法的作用将多个路径拼接起来，比如之前的\_\_dirname路径的拼接。

* 使用方法

```javascript
//1.引入path模块
const path=require('path')
const fs=require('fs')
const pathStr=path.join('/a','/b','/c/d','../','/e')
console.log(pathStr) //\a\b\c\e  这里的\d路径被../抵消掉
fs.readFile(path.join(__dirname,'./01.txt'),'utf-8',function(err,data){})
```

# path.basename()方法

* 描述

path.basename(path,\[.\])该方法接收两个参数，第一个参数为路径url,第二个参数可选为文件拓展名，其作用为获取路径中最后一个文件名或者指定的拓展名。

* 使用

```javascript
const path=require('path')
let pathurl='/a/b/index.html'
let filename=path.basename(pathurl)
console.log(filename) //默认截取最后一个文件名
filename=path.basename(pathurl,'.html')
console.log(filename) //获取.html拓展名的文件名
```

# path.extname()方法

* 描述

path.extname(path)方法接收一个参数url路径，返回路径中的拓展名

* 使用

```javascript
const path=require('path')
let url='/a/index.html'
console.log(path.extname(url)) // .html
```