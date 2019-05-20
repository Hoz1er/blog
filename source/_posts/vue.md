---
title: Vue笔记
tags:
    - Vue
    - 随笔
---

## [监听事件](https://cn.vuejs.org/v2/guide/events.html#监听事件)
    ```html
    <a v-on:click="doTheThing"></a>
    <!--简写：-->
    <a @click="doTheThing"></a>
    <!--修饰符 (可只使用修饰符 e.g.,<a v-on:click.stop></a>) 
    .stop [仅限原生DOM事件] 阻止传播 
    .prevent [仅限原生DOM事件] 不重载页面  
    .capture [仅限原生DOM事件] 捕获  即内部元素触发的事件先在此处处理，然后才交由内部元素自身进行处理
    .self [仅限原生DOM事件] 只当在 event.target 是当前元素自身时触发处理函数  即事件不是从内部元素触发的  
    .once 点击事件将只会触发一次-->

    
    ```
### [监听组件的原生事件](https://cn.vuejs.org/v2/guide/components.html#给组件绑定原生事件)
    ```html
    <!--使用.native修饰-->
    <my-component v-on:click.native="doTheThing"></my-component>
    <!--e.g.-->
    <router-link to="/" v-on:click.native="doTheThing">X</router-link>
    ```
<!--More-->
## [方法放在DOM更新之后执行](https://cn.vuejs.org/v2/api/#vm-nextTick)
    ```js
    new Vue({
        // ...
        methods: {
            // ...
            example: function () {
                // 修改数据
                this.message = 'changed'
                // DOM 还没有更新
                this.$nextTick(function () {
                    // DOM 现在更新了
                    // `this` 绑定到当前实例
                    this.doSomethingElse()
                })
            }
        }
    });
    //e.g.
    methods:{
        //重置Canvas
        RefreshCanvas:function(){
            //修改Canvas宽度高度
            this.canvasWidth=NEW_WIDTH;
            this.canvasHeight=NEW_HEIGHT;
            //数据已改变 DOM尚未更新

            this.$nextTick(function(){
                //DOM已更新
                this.$refs.eleImgCanvas.getContext("2d").clearRect(0, 0, this.canvasWidth,this.canvasHeight);
            });
        }
    }
    
    ```

## 使用深度选择器来修改第三方组件的样式  
```css
.my-class >>> .third-party-class{
    margin:10px;
}
.my-class /deep/ .third-party-class{
    margin:10px;
}
/*编译后：*/
.my-class[data-v-54c1ae8e] .third-party-class{
    margin:10px;
}
```