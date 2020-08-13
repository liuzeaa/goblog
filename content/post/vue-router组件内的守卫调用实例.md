---
layout: vue router
title: vue-router组件内的守卫调用实例
date: 2020-08-13 09:06:50
tags:
---
# vue-router组件内的守卫调用实例
<!--more-->

### 前言
我在实际开发过程中，问卷调查页面是由后端生成的html页面，只有在添加、编辑的时候才会调取后端生成html页面的方法。涉及到审核时并没有调用。我是通过组件内的守卫实现的，当离开该组件是就调取后端生成html页面的方法。具体代码如下：

```
export default{
    data(){
        return{}
    },
    methods:{},
    mounted(){},
    //关键代码
    beforeRouteLeave(to, from, next){
        generatorAllForm().then(res=>{
            if(res.success){
                next();
            }
        })
    }
}
```
