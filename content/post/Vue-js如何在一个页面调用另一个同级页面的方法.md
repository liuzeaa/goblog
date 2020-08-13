---
layout: vue.js
title: Vue.js如何在一个页面调用另一个同级页面的方法
date: 2020-08-13 09:05:27
tags:
---

# Vue.js如何在一个页面调用另一个同级页面的方法
<!--more-->

### 实际开发过程中，当前组件调用完方法之后，也许会调取同一级组件中的方法，本人暂时想到两种办法:
#### 1. vm.$on(event,callback)
1. 新建一个工具函数util.js，代码如下：
    ```
    import Vue from 'vue'
    export default new Vue;
    ```
2. 然后两个页面都引用它
    ```
    import util from './util'
    ```
    调用方：
    ```
    methonds:{
        functionA(){
            util.$emit('test','test')
        }
    }
    ```
    被调用方：
    ```
    methonds:{
        functionB(){
            
        }
    },
    mounted(){
        util.$on('test',(test)=>{
            console.log(test);
            this.functionB();
        })
    }
    ```
#### 2. vuex

1. 新建store.js
    ```
    import Vue from 'vue';
    import Vuex from 'vuex';
    Vue.use(Vuex);
    
    const store = new Vuex.Store({
        state: {
           list:[]
        },
        mutations: {
            setList(state,payload){
                state.list = payload
            }
        },
        actions: {
            getList({commit}){
                getList().then(res=>{
                    commit('setList',res.data)
                })
            }
        }
    });
    
    export default store;
    ```
2. 使用

    调用方：
    ```
    import {mapAction} from 'vuex'
    
    methonds:{
        ...mapAction([
            'getList'
        ]),
        functionA(){
            this.getList()
        }
    }
    ```
    被调用方：
    ```
    import {mapAction,mapState} from 'vuex'
    
    computed:{
      ...mapState([
        'list'
      ])  
    },
    methonds:{
        ...mapAction([
            'getList'
        ])
    },
    mounted(){
        this.getlist()
    }
    ```
