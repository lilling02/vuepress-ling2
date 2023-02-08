---
title: TypeORM的delete与remove的区别(delete不报错导致相应拦截不起作用)
date: 2023-01-13 22:17:17
permalink: /pages/a3a297/
categories:
  - nestjs学习
tags:
  - TypeORM
  - 遇坑
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---

typeorm 不知道说是坑还是新发现.

### delete :

在做crud的时候,如果删除的时候使用的使用的是delete方法,那么数据库中不管存不存在这个实体,删除成不成功都不会报错,导致异常拦截器捕获不到异常

举个例子:
```` js
    // xxx.service.ts

    //  这个方法不管要删除的实体数据库存不存在都不会抛出异常.也就导致了异常拦截器没办法成功的拦截异常但是这个方法很简单不在意有没有异常的时候可以使用

    async remove(id: number) {
    return this.levingWord.delete(id)
  }



/**
 * 我们看下前端返回的效果: (这里删除了一条数据库中不存在的实体)
 * {
    "data": {
        "raw": [],
        "affected": 0   // 这里也可以看到没有影响任何一行,说明删除是失败的
    },
    "status": 200,      但是因为delete不会在乎实体是否存在,所以没有报错,没走拦截器中的异常拦截
    "message": "成功",
    "success": true
}
 */
````

### remove :
remove()删除之前会看实体存不存在.不存在就报错. 缺点是他需要传入一个实体. 相对比较麻烦

```` js 
    async remove(id: number) {
    //  remove()删除之前会看实体存不存在.不存在就报错. 缺点是他需要传入一个实体. 相对比较麻烦
    let removeEntitie = await this.levingWord.findOneById(id);  // 先从数据库里获得实体
    return this.levingWord.remove(removeEntitie)                // 再删除掉
  }

````

