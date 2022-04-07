---
title: fs模块
description: 文件模块
published: 1
date: 2022-04-07T12:37:51.194Z
tags: node
editor: markdown
dateCreated: 2022-04-07T12:32:58.439Z
---

# fs模块

# fs.readFile()的使用

* 描述

fs.readFile(url,\[.\],callback(err,data))方法接收三个参数：url：为要读取文件的路径。\[\]是可选择的，例如编码格式。callback为回调函数其中有两个参数：err：读取文件失败时的回调，data为读取文件成功时返回的数据，当读取文件成功时err为null，当读取文件失败时err为失败对象，data为undefined。

* fs.readFile()使用步骤

1. 引入fs模块

```javascript
const fs=require('fs')
```

2. 调用fs.readFile()方法来读取文件

```javascript
fs.readFile('./01.txt','utf-8',function(err,data){
if(err){
//文件读取失败
console.log(err)
}else{
console.log(data)
}
})
```

# fs.writeFile()方法的使用

* 描述

fs.writeFile(url,content,\[.\],callback(err))方法接收是个参数：url为写入文件的路径。content为写入文件的内容。\[\]为可选参数例如编码格式的设置。callback为回调函数，其中err为错误对象，当写入文件成功时err为null。

* fs.writeFile()使用步骤

1. 引入fs模块

```javascript
const fs=require('fs')
```

2. 调用fs.writeFile()方法写入文件

```javascript
fs.writeFile('./write.txt','admin/123456','utf-8',function(err){
if(err){
console.log('文件写入失败！')
}else{
console.log('文件写入成功')
}
})
```

# 读写成绩单案例

* 描述

将grade01.txt文件读取出来变成grade02.txt的格式再写入grade02.txt中。

grade01.txt

```Plain Text
小工=99 小红=66 小军=80
```

grade02.txt

```Plain Text
小工：99
小红：66
小军：80
```

* 实现思路

1. 使用readFile()方法读取成绩单
2. 将读取到的内容进行格式化处理
3. 使用writeFile()方法将格式化处理后的成绩写入新文件

```javascript
const fs=require('fs')

fs.readFile('./grade.txt','utf-8',(err,data)=>{
    let writeData=[]
if(err){
    console.log('读取文件失败！')
}else{
    let writeData=[]
    data.split(' ').forEach(item=>{
      writeData.push(item.replace('=',':'))
    })
    console.log(writeData)
    fs.writeFile('./formatgrade.txt',writeData.join('\r\n'),'utf-8',(err)=>{
     if(err){
         console.log(err)
     }else{
         console.log('文件写入成功！')
     }
    })
}
})
```

**注意：fs.readeFile()和fs.writeFile()的回调函数都是异步的**

# 拓展

## fs模块路径问题

* 描述

fs.readFile()方法和fs.writeFile()方法中的url写相对路径会涉及到路径动态拼接问题，url=执行node命令所在的路径+相对路径，这样要动态的拼接路径。

* 解决

使用\_\_dirname参数可有效解决动态拼接路径的问题

例如：

```javascript
fs.readFile(__dirname+'./01.txt','utf-8',(err,data)=>{}){}
```

# demo案例

[时钟案例](https://pan.baicai.blog/%E7%94%B2%E9%AA%A8%E6%96%87%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8/node%E5%AD%A6%E4%B9%A0%E6%A1%88%E4%BE%8B)

