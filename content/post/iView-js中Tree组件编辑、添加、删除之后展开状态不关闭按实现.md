---
layout: iview.js
title: iView.js中Tree组件编辑、添加、删除之后展开状态不关闭按实现
date: 2020-08-13 09:06:04
tags:
---
# iView.js中Tree组件编辑、添加、删除之后展开状态不关闭按实现
<!--more-->
#### 实际开发过程中，添加、编辑、删除节点之后重新请求init方法，之前展开的节点都会关闭，这么做非常不友好，我在项目中具体是通过以下方式实现的。
data中的含有children的数据进行扁平化处理
```
flatFun(data){
   data.forEach(v=>{
      this.flatData.push(v);
      if(v.hasOwnProperty('children')){
          if(v.children.length>0){
              this.flatFun(v.children)
          }
      }
   })
}
```
添加、编辑、删除之后对扁平化之后的数据进行递归，Tree组件data识别的数据
```
dataFun(){
    this.data = this.flatData.filter(v=>v.pid==0);
    this.reDataFun(this.data);
},
reDataFun(data){
    data.forEach(v=>{
        if (v.parent) {
            v.loading = false;
            v.children = []
        }
        const list = this.flatData.filter(a=>a.pid==v.id);
        if(list.length>0){
            this.$set(v,'children',list);
            this.reDataFun(list);
        }
    })
}
```
添加方法
```
this.flatFun([...this.data,res.result]);
this.flatData = this.flatData.map(v=>{
    this.$delete(v,'children');
    this.$delete(v,'loading');
    return v
});//扁平化之后的数据进行删除children，防止添加、编辑、删除数据为变化
this.dataFun(this.flatData);
this.flatData=[];//成功之后清空扁平化数据
```
编辑方法
```
this.flatFun(this.data);
this.flatData = this.flatData.map(v=>{
    this.$delete(v,'children');
    this.$delete(v,'loading');
    return v
});//扁平化之后的数据进行删除children，防止添加、编辑、删除数据为变化
const index = this.flatData.findIndex(v=>v.id==res.result.id);
this.flatData[index] = {...this.flatData[index],...res.result};
this.dataFun(this.flatData);
this.flatData = []//成功之后清空扁平化数据
```
删除方法
```
this.flatFun(this.data);
this.flatData = this.flatData.map(v=>{
    this.$delete(v,'children');
    this.$delete(v,'loading');
    return v
});//扁平化之后的数据进行删除children，防止添加、编辑、删除数据为变化
this.selectList.forEach(obj=>{
    this.flatData = this.flatData.filter(v=>v.id!=obj.id)
})
this.dataFun(this.flatData);
this.flatData = []//成功之后清空扁平化数据
```
