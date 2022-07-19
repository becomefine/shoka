---
title: 第四章：v-bind及class与style绑定
date: 2022-02-19 12:37:55
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 第四章
DOM元素经常会动态绑定一些class类名或style样式。

## 4.1 了解v-bind指令
作用：动态更新HTML元素上的属性。
举个例子：
```html 
<div id = "app">
    <a v-bind:href = "url">链接</a>
    <img v-bind:src = "imgUrl">
    <!--缩写为-->
    <a :href = "url">链接</a>
    <img :src = "imgUrl">
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            url: 'https://www.github.com',
            imgUrl: 'https://xxx.xxx.xx/img.png'
        }
    })
</script>
```
在数据绑定中，最常见的两个需求是元素的样式名称class和内联样式style的动态绑定，他们也是HTML的属性，因此可以使用v-bind指令。我们只需要用v-bind计算出表达式最终的字符就可以，不过有时候表达式的逻辑较复杂，使用字符串拼接方法较难阅读和维护，所以Vue.js增加了对class好style的绑定

## 4.2 绑定class的几种方式
### 4.2.1 对象语法
给v-bind:class设置一个对象，可以动态地切换class，例如
```html
<div id = "app">
    <div :class = "{ 'active': isActive, 'error': isError}"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,
            isError: false
        }
    })
</script>
```
当:class的表达式过长或逻辑复杂时，还可以绑定一个计算属性，这是一种很友好和常见的用法，一般当条件多于两个时，都可以使用data或computed，例如使用计算属性：
```html
<div id = "app">
    <div :calss = "calssed"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,
            error: null
        },
        computed: {
            calsses: function(){
                return {
                    active: this.isActive && !this.error,
                    'text-fail': this.error && this.error.type === 'fail'
                }
            }
        }
    })
</script>
```
### 4.2.2 数组语法
当需要应用多个class时，可以使用数组语法，给:class绑定一个数组，应用一个class列表：
```html
<div id = "app">
    <div :class = "[activeCls,errorCls]"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            activeCls: 'active',
            errorCls: 'error'
        }
    })
</script>
```
渲染后结果：`<div class = "active error"></div>`

也可以使用三元表达式来根据条件切换class，例如下面的示例：
```html
<div id = "app">
    <div :class = "[isActive ? activeCls : '', errorCls]"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,
            activeCls: 'active',
            errorCls: 'error'
        }
    })
</script>
```
样式error会始终应用，当数据isActive为真时，样式active才会被应用。class有多个条件时，这样写较为繁琐，可以在数组语法中使用对象语法：
```html
<div id = "app">
    <div :class = "[{'active': isActive}, errorCls]"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,
            errorCls: 'error'
        }
    })
</script>
```

### 4.2.3 在组件上使用

## 4.3 绑定内联样式

```html
<div id = "app">
    <div :style="{'color':color,'fontSize':fontSize+'px' }">文本</div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        fontSize: 14
    })
</script>
```

使用数组语法：
```html
<div id = "app">
    <div :style = "styles">文本</div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            styles: {
                color: 'red',
                fontSize: 14+'px'
            }
        }
    })
</script>
```