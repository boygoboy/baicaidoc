---
title: web开发身份认证机制
description: web开发身份认证的机制
published: 1
date: 2022-04-24T07:09:48.350Z
tags: session
editor: markdown
dateCreated: 2022-04-24T03:46:40.870Z
---

# session认证机制
## 介绍
1. http协议的无状态性
客户端的每次http请求都是独立的，连续多个请求之间没有直接的关系，服务器不会主动保留每次的http请求的状态，这里可以通过cookie机制突破http协议的无状态性。
2. 什么是cookie
cookie是存储在用户浏览器中的一段不超过4kb的字符串，由一个名称、一个值、和其它几个用于控制cookie有效期、安全性、使用范围的可选属性组成。
不同域名下的cookie各自独立，当客户端发起请求时，会自动把当前域名下所有未过期的cookie一同发送到服务器。
cookie特点：
+ 自动发送
+ 域名独立
+ 过期限制
+ 4kb限制
3. cookie在身份认证中的作用
+ 第一次登陆请求时服务器通过响应头发送cookie给浏览器。
+ 此时会自动将身份认证的cookie保存到浏览器中。
+ 当客户端再次发起请求，会通过请求头自动将cookie发送给服务器。
+ 服务器根据请求头中的cookie验明用户的身份。
4. cookie不具有安全性
因为cookie是存储在浏览器中，并且浏览器提供了读写cookie的api，所以cookie很容易伪造不具有安全性，不建议服务器将隐私数据通过cookie形式发送给浏览器。
5. session认证过程
为解决cookie的不安全性，通过session认证可解决过程如下：
+ 浏览器：客户端登录提交账号密码
+ 服务端：验证账号密码，将登录成功后的用户信息存储在服务器的内存中，同时生成对应的cookie字符串
+ 服务端：服务器响应将生成的cookie响应给客户端
+ 浏览器：客户端再次发起请求时，通过请求头自动把当前域名下所有可用的cookie发送给服务器
+ 服务端：服务器根据请求头中携带的cookie从内存中查找对应的用户信息，用户的身份认证成功后，服务器针对当前用户生产特定响应内容
+ 服务端：服务器响应把当前用户对应的页面内容响应给浏览器
## 在express中使用session认证
1. 安装express-session中间件
在express项目中，只需要安装express-session中间件，即可在项目中使用session认证：
``` js
npm install express-session
```
2. 配置express-session
``` js
const express=require('express')
const app=express()
//导入session中间件
const session=require('express-session')
//配置session中间件
app.use(session({
   secret:'cwj', //自定义字符串
   resave:false, //固定写法
   saveUninitialized:true //固定写法
}))
```
3. 向session中存储数据
当express-session中间件配置成功后，可通过req.session来访问和使用session对象，来存储用户关键信息：
``` js
app.post('/api/login',(req,res)=>{
if(req.body.username!=='admin'||req.body.password!=='000'){
 return res.send({status:1,msg:'登录失败！'})
  req.session.user=req.body
  req.session.isLogin=true
  res.send({status:0,msg:'登录成功'})
}
})
```
4. 从session中取数据
可以直接从req.session对象上获取之前存储的数据，示例代码如下：
``` js
api.get('/api/username',(req,res)=>{
 if(!req.session.isLogin){
   return res.send({status:1,msg:'fail'})
 }
  res.send({status:0,msg:'success',useaname:req.session.user.username})
})
```
5. 清空session
调用req.session.destory()函数即可清空服务器保存的session信息
``` js
app.post('/api/logout',(req,res)=>{
req.session.destroy()
  res.send({
     status:200,
     msg:'退出成功！'
  })
})
```
> 注意：并不好把所有session清除掉只会清除对应客户端session
{.is-warning}

# jwt认证机制
## session认证的局限性
session认证机制需要配合cookie才能实现，由于cookie默认不支持跨域访问，所以当遇到跨域问题时需要做很多额外的配置才能实现跨域session认证
> 注意：1. 当前端请求后端接口不存在跨域问题的时候，推荐使用session身份认证机制。2. 当前端需要跨域请求后端接口的时候，不推荐使用session身份认证机制，推荐使用jwt认证机制。
{.is-warning}

## jwt认证机制
1. 什么是jwt
目前最流行的跨域认证解决方案
2. jwt的工作原理
+ 浏览器：客户端登录：提交账号与密码
+ 服务端：验证账号和密码，验证通过后将用户的信息对象经过加密之后生产token字符串
+ 服务端：服务器响应将生成的token发送给客户端
+ 浏览器：将token存储到localStorage或者sessionStorage中
+ 浏览器：客户端再次发起请求时，通过请求头的Authorization字段将token发送给服务器
+ 服务端：服务器把token字符串还原成用户的信息对象
+ 服务端：用户的身份认证成功后，服务器针对当前用户生成特定的响应内容
+ 服务端：服务器响应把当前用户对应的页面内容响应给浏览器
总结：用户的信息通过token字符串的形式，保存在客户端浏览器中。服务器通过还原token字符串的形式来认证用户的身份
3. jwt的组成部分
由三部分组成：Header（头部）、Payload（有效荷载）、Signature（签名）三者之间使用英文'.'分隔，格式如下：
``` js
Header.Payload.Signature
```
4. 三个组成部分含义
+ Payload部分才是真正的用户信息，它是用户信息经过加密之后生成的字符串
+ Header和Signature是安全性相关的部分，只为保证token的安全性
5. jwt的使用方式
客户端收到服务器返回的jwt之后将其存储到localStorage或sessionStorage中，之后客户端每次与服务器通信，要带上JWT的字符串进行身份认证，推荐做法是把jwt放在http请求头的Authorization字段中，格式如下：
``` js
Authorization: Bearer <token>
```
## 在express中使用jwt
1. 安装jwt相关的包
``` shell
npm install jsonwebtoken express-jwt
```
+ jsonwebtoken用于生产jwt字符串
+ express-jwt用于将jwt字符串解析还原成JSON对象
2. 导入jwt相关的包
``` js
//导入用于生成jwt字符串的包
const jwt=require('jsonwebtoken')
//导入用于将客户端发送过来的jwt字符串解析还原成JSON对象的包
const expressJWT=require('express-jwt')
```
3. 定义secret秘钥
为了保证jwt字符串的安全性，防止jwt字符串在网络传输过程中被破解需要定义一个用于加密和解密的secret秘钥：
+ 当生成jwt字符串的时候需要使用secret密钥对用户的信息进行加密，最终得到加密好的jwt字符串
+ 当把jwt字符串解析还原成JSON对象的时候，需要使用secret密钥进行解密
+ secret密钥的本质就是一个字符串
4. 在登录成功后生成jwt字符串
调用jsonwebtoken包提供的sign()方法，将用户的信息加密成jwt字符串，响应给客户端：
``` js
app.post('/api/login',(req,res)=>{
   res.send({
       status:200,
       message:'登录成功',
       token:jwt.sign({username:userinfo.username},secretKey,{expiresIn:'30s'})
   })
})
```
5. 将jwt字符串还原为JSON对象
通过express-jwt中间件自动将客户端发送过来的token解析还原成JSON对象：
``` js
app.use(expressJWT({secret:secretKey}).unless({path:[/^\/api\//]}))
```
> .unless()方法用于指定哪些接口不需要访问权限
{.is-info}

6. 使用req.user获取用户信息
``` js
app.get('/admin/getinfo',(req,res)=>{
console.log(req.user)
  res.send({
      status:200,
      message:'获取用户信息成功！',
      data:req.user
  })
})
```
> 注意：这里通过express-jwt中间件将token解析还原成JSON对象后会自动将user对象挂载到req中
{.is-warning}

7. 捕获解析jwt失败后产生的错误
当使用express-jwt解析token字符串时，如果客户端发送过来的token字符串过期或不合法，会产生一个解析失败的错误，影响项目的正常运行，使用express错误中间件可捕获错误并处理。
``` js
app.use((err,req,res,next)=>{
      if(err.name=='UnauthorizedError'){
      return res.send({status:401,message:'无效token'})
      }
      res.send({status:500,message:'未知错误'})
})
```
