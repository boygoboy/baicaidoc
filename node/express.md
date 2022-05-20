---
title: express模块
description: express的使用
published: 1
date: 2022-05-20T07:47:37.744Z
tags: express
editor: markdown
dateCreated: 2022-04-10T11:04:52.890Z
---

# express的基本使用
+ express模块的介绍
Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。使用 Express 可以快速地搭建一个完整功能的网站。
+ express的引入使用
1. 安装express模块
``` shell
npm install express @4.17.1
```
2. 引入express模块并启动
``` javascript
const express=require('express')
//创建express实例
const app=express()
//启动express服务
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
# express的具体使用
## 监听get请求
1. 介绍
通过app.get()方法可以监听get请求，具体语法如下：
``` javascript
//url为get请求的路径
//回调函数req为请求对象，res为响应对象
app.get(url,function(req,res){

})
```
2. 使用
``` js
const express=require('express')
const app=express()
app.get('/get',(req,res)=>{
res.send({id:'1',name:'abc'})
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```

## 监听post请求
1. 介绍
通过app.post()方法可以监听post请求，具体语法如下：
``` js
//url为post请求路径
//req为请求对象
//res为响应对象
app.post(url,function(req,res){})
```
2. 使用
``` js
const express=require('express')
const app=express()
app.post('/post',(req,res)=>{
res.send('发送请求成功！')
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
## 响应对象send()方法
send()方法可以返回响应信息
## 获取url中携带的查询参数
例如：get请求的url为：http://127.0.0.1/?name=aa&age=18 那么可用通过响应对象中的query属性获取查询参数
``` js
const express=require('express')
const app=express()
app.get('/get',(req,res)=>{
console.log(req.query) //输出：{name:aa,age:18}
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
## 获取url中的动态参数
通过req.params对象可以获取到:匹配到的动态参数，使用案例如下：
1. 发送get请求url
url：http://127.0.0.1/33 
``` js
const express=require('express')
const app=express()
app.get('/get/:id(req,res)=>{
console.log(req.params) //输出：{id:33}
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
# 托管静态资源
## express.static()用法
+ 描述
express.static()方法可以创建一个静态资源服务器，将js、css、image等资源开放供外部访问,比如public目录下有index.html文件，使用```app.use(express.static('public'))```即可创建静态资源服务器。浏览器输入http://127.0.0.1/index.html即可访问该资源。
> 注意：public目录并不会出现在访问路径里面
{.is-warning}
+ 案例
将clock目录下的资源开放为静态资源，实现代码如下：
``` js
const express=require('express')
const app=express()
app.use(express.static('./clock'))
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
浏览器中输入：http://127.0.0.1/index.html即可访问资源
## 托管多个静态资源目录
+ 描述
通过多次调用express.static()方法可创建多个静态资源目录
``` js
app.use(express.static('public'))
app.use(express.static('clock'))
```
以上即可创建两个静态资源目录
> 注意：访问静态资源文件时，express.static()会根据添加资源目录的顺序查找所需文件，例如：public和clock目录下都有index.html文件，那么当浏览器中输入：http://127.0.0.1/index.html会访问public目录下的index.html
{.is-warning}
## 挂载路径前缀
+ 描述
如果想在访问的静态资源文件前挂载路径前缀可通过``` app.use('/public',express.static('public'))``` 方式实现
+ 案例
``` js
const express=require('express')
const app=express()
app.use('/public',express.static('./public'))
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
此时浏览器中需要输入：http://127.0.0.1/public/index.html才可以访问index.html网页
# nodemon工具的使用
+ 描述
该工具的作用是可以监听代码的修改然后自动重启node服务器。
+ 使用
1. 安装
``` shell
npm install -g nodemon
```
2. 使用
终端中输入nodemon+要执行node文件的路径
``` shell
nodemon ./index.js
```
# express路由
## 路由的概述
+ 描述
express路由是指客户端请求与服务器端对应服务的映射，express路由包括：url、方法、功能处理函数。
+ 简单使用
``` js
const express=require('express')
const app=express()
app.get('/',(req,res)=>{
res.send('success')
})
app.listen(80,()=>{console.log('server is started at http://127.0.0.1')})
```
## 路由的模块化
+ 描述
为方便路由进行模块化管理不推荐直接挂载到app上，可单独封装为单独模块导入挂载使用，步骤如下：
1. 创建路由模块对应的.js文件
2. 调用 express.Router()函数创建路由对象
3. 向路由对象上挂载具体的路由
4. 使用module.exports向外共享路由对象
5. 使用app.use()函数注册路由模块
+ 代码实现
**自定义文件：router.js**
``` js
const express=require('express')
// 创建路由对象
const router=express.Router()
// 挂载具体的路由
router.get('/get',(req,res)=>{console.log('get')})
router.post('/post',(req,res)=>{console.log('post')})
//向外导出路由
module.exports=router
```
**在index.js文件中使用**
``` js
const express=require('express')
const router=require('./router')
const app=express()
app.use(router)
app.listen(81,()=>{console.log('Server is started at http://127.0.0.1')})
```
> 这里的app.use()作用是注册全局中间件
{.is-info}

**拓展：**
当需要为路由路径前添加公共访问路径，可在app.use()中配置
``` js
app.use('/api',router)
```
# 中间件
+ 描述
 需要达到某种结果中间进行多个预处理的过程为中间件，上一个中间件的输出是下一个中间件的输入，express的中间件调用流程是：一个请求到达Express的服务器之后，连续调用多个中间件，对这次请求进行预处理最终返回响应结果。
+ express中间件的格式
express中间件的本质就是一个function处理函数，例如：```app.get('/get',(req,res,next)=>{...})```中回调函数为中间件。
> 注意：中间件和路由处理函数的区别是中间件包含next，next的作用：把中间件串联起来，把流转关系转交个下一个中间件或者路由。
{.is-warning}

## 全局中间件
+ 定义全局中间件
``` js
const mc=(req,res,next)=>{console.log('第一个中间件') next()}
```
+ 使用多个中间件
注册多个中间件使用app.use()多次注册即可
``` js
const mc=(req,res,next)=>{console.log('第一个中间件')}
app.use(mc)
//中间件可这样简写
app.use((req,res.next)=>{console.log('第二个中间件')})
```
完整代码
``` js
const express=require('express')
const router=require('./router')
const app=express()
const mc1=(req,res,next)=>{
    console.log('第一个中间件')
    next()
}
app.use(mc1)
app.use((req,res,next)=>{
    console.log('第二个中间件')
    next()
})
app.use(router)
app.listen(81,()=>{console.log('Server is started at http://127.0.0.1')})
```
这里客户端发起请求后服务端执行的顺序是：输出'第一个中间件'->'第二个中间件'->路由处理函数。
> 注意：这里路由模块的注册需要放在全局中间件注册代码后面，否则执行api请求时全局中间件并没有注册导致使用失败
{.is-warning}
+ 中间件的作用
多个中间件之间共享一份req和res。基于这样的特性在上游中间件中统一为req和res对象添加自定义属性和方法，供下游中间件或路由使用。
+ 例子
``` js
const express=require('express')
const router=require('./router')
const app=express()
const mc1=(req,res,next)=>{
    req.first='aa'
    console.log('第一个中间件')
    next()
}
app.use(mc1)
app.use((req,res,next)=>{
     console.log(req.first) //aa
    console.log('第二个中间件')
    next()
})
app.use(router)
app.listen(81,()=>{console.log('Server is started at http://127.0.0.1')})
```
上述例子体现了中间件的这一作用，在第一个中间件为req对象添加first属性，第二个中间件即可使用该属性。
# 局部中间件
+ 介绍
局部中间件即不使用app.use()创建的中间件叫局部中间件
+ 创建局部中间件
``` js
const express=require('express')
const app=express()
const mc1=(req,res,next)=>{
    req.first='aa'
    console.log('第一个中间件')
    next()
}
app.get('/',mc1,(req,res)=>{})
app.listen(81,()=>{console.log('Server is started at http://127.0.0.1')})
```
这里在app.get()中传入创建的mc1中间件即可注册局部中间件
+ 注册多个局部中间件
``` js
const express=require('express')
const app=express()
const mc1=(req,res,next)=>{
    req.first='aa'
    console.log('第一个中间件')
    next()
}
const mc2=(req,res,next)=>{
    req.first='aa'
    console.log('第二个中间件')
    next()
}
app.get('/',mc1,mc2,(req,res)=>{})
app.listen(81,()=>{console.log('Server is started at http://127.0.0.1')})
```
注册多个中间件在app.get()中传入多个中间件即可。
> 注册多个中间件还可简写为：app.get('/',[mc1,mc2],(req,res)=>{})
{.is-info}

> 中间件使用注意事项：
{.is-warning}

1. 一定要在路由之前注册中间件
2. 客户端发送过来的请求，可以连续调用多个中间件进行处理
3. 执行完中间件的业务代码之后，不要忘记调用next()函数
4. 为防止业务代码逻辑混乱，调用next()函数后不要再写额外的代码
5. 连续调用多个中间件，多个中间件共享req和res
# 中间件的分类
1. 应用级别中间件
通过app.use()或app.get()或app.post()，绑定到app实例上的中间件叫做应用级别的中间件，代码示例如下：
``` js
app.use((req,res,next)=>{next()})
app,get('/',mc1,(req,res)=>{})
```
2. 路由级别中间件
绑定到express.Router()实例上的中间件叫做路由级别的中间件，示例代码如下：
``` js
let app=express()
let router=express.Router()
router.use((req,res,next)=>{
next()
})
```
3. 错误级别的中间件
用于捕获项目中异常错误的中间件，其处理函数有四个形参：err、req、res、next，示例代码：
``` js
const express=require('express')
const app=express()
app.get('/',(req,res)=>{
throw new Error('error')
  res.send('success')
})
app.use((err,req,res,next)=>{res.send('err')})
app.listen(80,()=>{console.log(server is started at http://127.0.0.1)})
```
> 注意：错误级别的中间件要放在所有路由后面，否则会报错！
{.is-warning}

4. express内置的中间件
+ express.static快速托管静态资源的内置中间件，例如：html、css、js（无兼容性）
+ express.json()解析JSON格式的请求体数据（有兼容性，仅在4.16.0+ 版本中可用）
``` js
const express=require('express')
const app=express()
app.use(express.json())
app.post('/',(req,res)=>{console.log(req.body)})
app.listen(80,()=>{})
```
通过注册处理json的内置中间件即可获取客户端发送过来的json格式数据，通过req.body获取
+ express.urlencoded解析URL-encoded格式的请求体数据（有兼容性，仅在4.16.0+ 版本中可使用）
当客户端发送url-encoded格式的数据可以通过以下方式解析
``` js
const express=require('express')
const app=express()
app.use(express.urlencoded({extended:false}))
app.post('/',(req,res)=>{console.log(req.body)})
app.listen(80,()=>{})
```
通过req.body即可获取客户端发送过来的url-encoded格式的数据
5. 第三方中间件
非express官方内置，由第三方开发出来的中间件，比如express 4.16.0之前的版本使用body-parser第三方中间件解析url-encoded格式的数据，实例：
+ 安装依赖
``` shell
npm install body-parser
```
+ 使用
``` js
const express=require('express')
const app=express()
const parser=require('body-parser')
app.use(parser.urlencoded({extended:false}))
app.post('/',(req,res)=>{console.log(req.body)})
app.listen(80,()=>{})
```
# 自定义中间件
+ 实现步骤
1. 定义中间件
2. 监听req的data事件
3. 监听req的end事件
4. 使用querystring模块解析请求体数据
5. 将解析出来的数据对象挂载为req.body
6. 将自定义中间件封装为模块
+ 具体实现
这里以自定义解析客户端发送的urlencoded格式的数据为例：
1. 定义中间件
``` js
app.use((req,res,next)=>{})
```
2. 监听req的data事件
由于客户端发送的数据比较大的时候，需要将数据分段发送这样会多次触发data事件，因此需要手动将接收到的数据进行拼接。
``` js
 let str=''
    req.on('data',(chunk)=>{
        str+=chunk
    })
```
3. 监听req的end事件
当请求体发送数据完毕后会自动触发end事件，此时可以进行数据的处理
``` js
req.on('end',()=>{})
```
4. 使用querystring模块解析请求体数据
node.js内置了一个querystring模块，专门用来处理查询字符串。其parse()方法可以把查询字符串解析为对象格式，示例：
``` js
const qs=require('querystring')
    req.on('end',()=>{
        console.log(str)
        //处理客户端请求的数据
        const body=qs.parse(str)
        req.body=body
    })
```
5. 将解析出来的数据对象挂载为req.body
``` js
   req.on('end',()=>{
        console.log(str)
        //处理客户端请求的数据
        const body=qs.parse(str)
        req.body=body
        next()
    })
```
> 注意：处理完客户端请求的数据不要忘记调用next()交给下一级中间件或路由
{.is-warning}

**以上完整代码：**
``` js
const express=require('express')
const qs=require('querystring')
const app=express()
//定义注册中间件
app.use((req,res,next)=>{
    let str=''
    req.on('data',(chunk)=>{
        str+=chunk
    })

    req.on('end',()=>{
        console.log(str)
        //处理客户端请求的数据
        const body=qs.parse(str)
        req.body=body
        next()
    })
    
})

app.post('/',(req,res)=>{
    res.send('success')
})

app.listen(80,()=>{
    console.log('server is started at http://127.0.0.1')
})
```
6. 将自定义中间件封装为模块
**custommc.js文件中封装解析urlencoded的中间件功能函数**
``` js
const qs=require('querystring')
const babelencoded=function(req,res,next){
    let str=''
    req.on('data',(chunk)=>{
        str+=chunk
    })
    req.on('end',()=>{
        const body=qs.parse(str)
        req.body=body
        next()
    })
}
module.exports=babelencoded
```
**mc.js中注册使用中间件**
``` js
const qs=require('querystring')
const babelencoded=function(req,res,next){
    let str=''
    req.on('data',(chunk)=>{
        str+=chunk
    })
    req.on('end',()=>{
        const body=qs.parse(str)
        req.body=body
        next()
    })
}
module.exports=babelencoded
```
# 编写get接口
**apirouter.js文件**
``` js
const express=require('express')
const router=express.Router()

router.get('/get',(req,res)=>{
    const query=req.query
    res.send({
        query
    })
})
module.exports=router
```
**index.js文件**
``` js
const express=require('express')
const app=express()
const apirouter=require('./apirouter')
app.use('/api',apirouter)
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
# 编写post接口
**apirouter.js文件**
``` js
const express=require('express')
const router=express.Router()

router.post('/post',(req,res)=>{
    const body=req.body
    res.send({
        body
    })
})
module.exports=router
```
**index.js文件**
``` js
const express=require('express')
const app=express()
const apirouter=require('./apirouter')
app.use(express.urlencoded({extended:false}))
app.use('/api',apirouter)
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
# 接口跨域问题
## 使用cors中间件方案解决跨域问题
+ 使用流程
1. 运行 ```npm install cors``` 安装中间件
2. 使用 ```const cors=require('cors')导入中间件
3. 在路由之前调用app.use(cors())配置中间件
+ 代码
``` js
const express=require('express')
const app=express()
const apirouter=require('./apirouter')
const cors=require('cors')
app.use(cors())
app.use(express.urlencoded({extended:false}))
app.use('/api',apirouter)
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
> 注意：一定要在路由之前配置中间件
{.is-warning}

+ cors使用注意点
1. cors在服务端进行配置，客户端不需要做配置
2. cors具有兼容性，在低版本浏览器不兼容
# 跨域cors相关的三个响应头
## Access-Control-Allow-Origin响应头
+ 描述
响应头部可以携带一个Access-Control-Allow-Origin字段，语法如下：
``` js
Access-Control-Allow-Origin:<origin>|*
```
这里origin指允许访问资源的外域URL
例如：
1. 允许百度访问资源
``` js
res.setHeader('Access-Control-Allow-Origin','https://www.baidu.com')
````
2. 允许所有外域访问资源
``` js
res.setHeader('Access-Control-Allow-Origin','*')
```
## Access-Control-Allow-Headers响应头
+ 描述
默认情况CORS仅支持客户端发送如下9个请求头：
Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width、Content-Type（值仅限于text/plain、multipart/form-data、application/x-www-form-urlencoded三者之一）
如果客户端向服务器发送额外的请求头信息，需要在服务端通过Access-Control-Allow-Headers对额外的请求头进行声明，否则请求失败！
+ 使用
``` js
res.setHeader('Access-Control-Allow-Headers','Content-Type,X-Custom-Header')
```
这里自定义允许Content-Type和X-Custom-Header请求头访问
## Access-Control-Allow-Methods响应头
+ 描述
默认情况下仅支持客户端发起GET、POST、HEAD请求
+ 使用
1. 如果希望服务端支持客户端发送PUT、DELETE请求，需做以下设置：
``` js
res.setHeader('Access-Control-Allow-Methods','PUT,DELETE')
```
2. 如果希望服务端支持客户端发送所有请求，做以下设置：
``` js
res.setHeader('Access-Control-Allow-Methods','*')
```
# 跨域CORS简单请求与预检请求
## 简单请求
+ 描述
满足以下两个条件属于简单请求：
1. 请求方式：GET、POST、HEAD三者之一
2. 请求头为默认的9个请求头（见上述响应头）
## 预检请求
+ 描述
预检请求与简单请求对立，其满足以下条件：
1. 请求为GET、POST、HEAD之外的Methods类型
2. 请求头中包含自定义头部字段
3. 向服务器发送了application/json的字段
在浏览器与服务器正式通信之前，浏览器会发送OPTION请求进行预检，获取服务器是否允许该实际请求，这次的OPTION请求称为预检请求，服务器成功响应后才会发起真正的请求并携带数据。
## 简单请求和预检请求区别
+ 简单请求特点：客户端与服务器之间只会发生一次请求
+ 预检请求：客户端与服务器之间发生两次请求
# 编写JSONP接口
1. JSON的概念与特点
+ 概念
浏览器通过script标签的src属性，请求服务器上的数据，同时服务器返回一个函数的调用，这种请求数据的方式叫做JSONP。
+ 特点
（1） JSONP不属于真正的ajax请求，因为它没有使用XMLHttpRequest这个对象
（2）JSONP仅支持GET请求，不支持POST、PUT、DELETE等请求。
2. 创建JSONP接口
+ 创建JSONP接口注意点
如果项目中配置了CORS资源共享，必须在CORS中间件之前声明JSONP接口，否则JSONP接口会被处理成开启了CORS的接口
+ 创建JSONP接口
``` js
const express=require('express')
const app=express()
const apirouter=require('./apirouter')
const cors=require('cors')
app.use(express.urlencoded({extended:false}))
app.get('/api/jsonp',(req,res)=>{})
app.use(cors())
app.use('/api',apirouter)
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
3. 实现JSONP接口
+ 案例描述 
（1）获取客户端发送过来的回调函数的名字
（2）得到要通过JSONP形式发送给客户端的数据
（3）根据前两步得到的数据，拼接出一个函数调用的字符串
（4）把上一步调用的字符串响应给客户端的script标签进行解析执行
+ 具体代码
**js核心代码**
``` js
const express=require('express')
const app=express()
const apirouter=require('./apirouter')
const cors=require('cors')
app.use(express.urlencoded({extended:false}))
app.get('/api/jsonp',(req,res)=>{
const funcName=req.query.callback
const data={id:1,name:'cwj'}
const returnStr=`${funcName}(${JSON.stringify(data)})`
res.send(returnStr)
})
app.use(cors())
app.use('/api',apirouter)
app.listen(80,()=>{
console.log('server is started at http://127.0.0.1')
})
```
**客户端调用测试**
``` js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button id="btnget">GET</button>
    <button id="btnpost">POST</button>
    <button id="btnjsonp">JSONP</button>
    <script src="https://cdn.staticfile.org/jquery/1.10.0/jquery.min.js"></script>
    <script>
    $(function(){
      $('#btnget').on('click',function(){
        $.ajax({
            type:'GET',
            url:'http://127.0.0.1/api/get',
            data:{
                id:1,
                name:'cwj'
            },
            success:function(res){
                console.log(res)
            }
        })
      })
      $('#btnpost').on('click',function(){
        $.ajax({
            type:'POST',
            url:'http://127.0.0.1/api/post',
            data:{
                id:1,
                name:'cwj'
            },
            success:function(res){
                console.log(res)
            }
        })
      })
      $('#btnjsonp').on('click',function(){
          $.ajax({
              type:'GET',
              url:'http://127.0.0.1/api/jsonp',
              dataType:'jsonp',
              success:function(res){
                  console.log(res)
              }
          })
      })
    })
    </script>
</body>
</html>
```

