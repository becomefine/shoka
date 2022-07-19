---
title: 第二章：数据绑定
date: 2022-02-15 18:32:09
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 数据绑定和第一个Vue应用
实例：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset = "utf-8">
        <title>Vue 示例</title>
    </head>
<body>
    <div id = "app">
        <input type = "text" v-model = "name" placeholder = "你的名字">
        <h1>你好，{{name}}</h1>
    </div>
    <script src = "https://unpkg.com/vue/dist/vue.min.js"></script>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                name: ''
            }
        })
    </script>
</body>
</html>
```

## 2.1 Vue实例与数据绑定
### 2.1.1 实例与数据

el: 指定一个页面中已存在的DOM元素来挂载Vue实例，可以是HTMLElement或CSS选择器。
- 挂载成功后，可以通过app.$el来访问该元素。
- 访问方法：
```javascript
var app = new Vue({
    el: '#app',
    data: {
        a: 2
    }
})
console.log(app.2);
```

或：
```javascript
var myData = {
    a: 1
}
var app = new Vue({
    el: '#app',
    data: myData
})

console.log(app.a); //1
//修改属性，原数据也会随之修改
app.a = 2
console.log(myData.a);//2
//反之，修改原数据，Vue属性也会修改
myData.a = 3;
console.log(app.a); //3
```

### 2.1.2 生命周期

- `created` 实例创建完成后调用，此阶段完成了数据的观测等，但尚未挂载，$el还不可用。
- `mounted` el挂载到实例上后调用，一般我们的第一个业务逻辑会在这里开始。
- `beforeDestroy` 实例销毁之前调用，主要解绑一些使用addEventListener监听的事件等。

### 2.1.3 插值与表达式
基本文本插值方法：`{{ }}`
例：
```html
<div id = "app">
    {{book}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            book: ''《Vue.js实战》'
        }
    })
</script>
```
每秒更新时间
```html
<div id = "app">
    <!-- {{data}} -->
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            data: new Date()
        },
        mounted: function(){
            var _this = this; //声明一个变量指向Vue实例this，保证作用域一致
            this.timer = setInterval(function(){
                _this.timer = new Date(); //修改数据data
            },1000);
        },
        beforeDestroy: function(){
            if(this.timer){
                clearInterval(this.timer);//在Vue实例销毁前，清除我们的定时器。
            }
        }
    })
    </script>
```

### 2.1.4 过滤器
例子：对事件进行格式化处理
```html
<div id = "app">
    <!-- {{date | formatDate}} -->
</div>
<script>
    //在月份、日期、小时等小于10前面补0
    var padDate = function(value){
        return vale < 10? '0' + value : value;
    };

    var app = new Vue({
        el: '#app',
        data: {
            date: new Date()
        },
        filters: {
            formatDate: function(value){ //value为需要过滤的数据
                var date = new Date(value);
                var year = date.getFullYear();
                var month = padDate(date.getMonth()+1);
                var day = padDate(date.getDate());
                var hours = padDate(date.getHours());
                var minutes = padDate(date.getMinutes());
                var seconds = padDate(date.getSeconds());//将整理好的数据返回
                return year+'-'+month+'-'+day+''+hours+':'+minutes+'-'+seconds;
            }
        },
        mounted: function(){
            var _this = this;
            this.timer = setInterval(function(){
                _this.date = new Date();
            },1000);
        },
        beforeDestroy: function(){
            if(this.timer){
                clearInterval(this.timer);
            }
        }
    })
</script>
```

## 2.2 指令与事件
作用：档期表达式的值改变时，相应的将某些行为应用到DOM上。
- v-if: 条件判断
- v-bind: 动态更新HTML元素上的属性
- v-on: 绑定事件监听器 

## 2.3 语法糖
- v-bind - ":"
- v-on - "@"


