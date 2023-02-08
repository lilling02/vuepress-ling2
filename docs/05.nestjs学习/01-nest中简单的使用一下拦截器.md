---
title: nest中简单的使用一下拦截器
date: 2023-01-12 22:39:55
permalink: /pages/ed3c9a/
categories:
  - nestjs学习
tags:
  - 
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---
## 拦截器的作用

拦截器具有一系列有用的功能,这些功能受面向切面编程(AOP)技术的启发.它们可以:

- 在函数执行之前/之后绑定额外的逻辑
- 转换从函数返回的结果
- 转换从函数抛出的异常
- 扩展基本函数行为
- 根据所选条件完全重写函数 (例如, 缓存目的)


### 做一个请求拦截器:
我们现在没有给我们的Nestjs 规范返回给前端的格式现在比较乱
```` js
    前端收到的
    [
        {
            "id": 2,
            "fullName": "deng",
            "content": "1/12的一次尝试",
            "creattime": "2022-05-13T06:28:25.000Z"
        },
        {
            "id": 13,
            "fullName": "deng",
            "content": "1/12的一次尝试",
            "creattime": "2022-10-05T08:36:51.000Z"
        }
    ]
````
我们想给他返回一个标准的json 格式 就要给我们的数据做一个全局format

```` js
{
  data, //数据
  status:0,
  message:"成功",
  success:true
}
````

新建common 文件夹 创建 response.ts

```` js
    import { Injectable, NestInterceptor, CallHandler } from '@nestjs/common'
    import { map } from 'rxjs/operators'
    import {Observable} from 'rxjs'
    
    
    
    interface data<T>{
        data:T
    }
    
    @Injectable()
    export class Response<T = any> implements NestInterceptor {
        intercept(context, next: CallHandler):Observable<data<T>> {
            return next.handle().pipe(map(data => {
                return {
                data,
                status:200,
                success:true,
                message:"ling"
                }
            }))
        }
    }
````

在main.ts 注册

app.useGlobalInterceptors(new Response())

这样我们就成功注册了一个请求拦截器,目前功能还比较简单,如果有复杂业务需求再加

### 异常拦截器

common下面新建filter.ts

```` js
     
import { ExceptionFilter, Catch, ArgumentsHost,HttpException } from '@nestjs/common'
 
import {Request,Response} from 'express'
 
@Catch(HttpException)
export class HttpFilter implements ExceptionFilter {
    catch(exception:HttpException, host: ArgumentsHost) {
        const ctx = host.switchToHttp()
        const request = ctx.getRequest<Request>()
        const response = ctx.getResponse<Response>()
 
        const status = exception.getStatus()
 
        response.status(status).json({
           data:exception.message,
           time:new Date().getTime(),
           success:false,
           path:request.url,
           status
        })
    }
}
````

注册全局异常过滤器
```` js
// main.ts
app.useGlobalFilters(new HttpFilter())
```` 

这样我们就做好了一个请求拦截和异常拦截