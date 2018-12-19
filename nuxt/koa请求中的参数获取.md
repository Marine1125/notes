**Koa请求中的参数获取**
==========

## **GET请求**<br/>

>1. 是从上下文中直接获取：<br/>
> * 请求对象ctx.query，返回如 { a:1, b:2 } <br/>
> * 请求字符串 ctx.querystring，返回如 a=1&b=2 <br/>
>2. 是从上下文的request对象中获取：<br/>
> * 请求对象ctx.request.query，返回如 { a:1, b:2 } <br/>
> * 请求字符串 ctx.request.querystring，返回如 a=1&b=2 <br/>

## **POST请求**<br/>

> 1. 是从body中直接获取：<br/>
> * 请求对象ctx.body，返回如 { a:1, b:2 }<br/>