---
title: 第三章：计算属性
date: 2022-02-19 11:38:04
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 计算属性
模板内的表达式常用于简单的运算，当其过长或逻辑复杂时，会难以维护，本章的计算属性就是用于解决该问题的。常用于动态的设置元素的样式名称class和内联样式style，也经常用来动态传递props。
`tips: 计算属性可以依赖其他计算属性，计算属性不仅可以依赖当前Vue实例的数据，还可以依赖其它实例的数据`

## 3.1 什么是计算属性
模板表达式：
```html
<div>
    <!-- {{text.split(',').reverse().join(',')}} -->
</div>
```
计算属性：
```html
<div id = "app">
    {{reveredText}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            text: '123,456'
        },
        computed: {
            reverseText: function(){
                return this.text.split(',').reverse().join(',');
            }
        }
    })
</script>
```

## 3.2 计算属性用法
每一个计算属性都包含一个getter和一个setter，在需要时可以提供一个setter函数，当手动修改计算属性的值就像修改一个普通数据那样时，就会触发setter函数，执行一些自定义操作：
```html
<div id = "app">
    姓名：{{fullName}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            firstName: 'Jack',
            lastName: 'Green'
        },
        computed:{
            fullName: {
                //getter,用于读取
                get: function(){
                    return this.firstName+' '+this.lastName;
                },
                //setter,写入时触发
                set: function(newValue){
                    var names = newValue.split(' ');
                    this.firstName = names[0];
                    this.lastName = names[names.length-1];
                }
            }
        }
    });
</script>
```
## 3.3 计算属性缓存
computed：基于它的依赖缓存的。一个计算属性所依赖的数据发生变化时，他才会重新取值。
methods：只要重新渲染，它就会被调用，因此函数也会被执行。

使用计算属性还是methods取决于你是否需要缓存，当遍历大数组和做大量计算时，应当使用计算属性，除非你不希望得到缓存。

