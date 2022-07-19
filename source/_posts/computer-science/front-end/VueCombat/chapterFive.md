---
title: 第五章：内置指令
date: 2022-02-19 14:19:09
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 内置指令

## 5.1 基本指令
- v-cloak: 不需要表达式，在Vue实例结束编译时从绑定的HTML元素上移除，经常和CSS的display:none 配合使用：
```html
<div id = "app" v-cloak>
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: '这是一段文本'
        }
    })
</script>
```
v-cloak是一个解决初始化慢导致页面闪动的最佳实践，对于简单的项目很有用，但是在具有工程化的项目里，项目的HTML结构只有一个空的div元素，剩余的内容都是由路由去挂载不同组件完成的，所以不在需要v-cloak。

- v-once：不需要表达式，作用是定义它的元素或组件只渲染一次，包括元素或组件的所有子节点。首次渲染后，不再随数据的变化重新渲染，将被视为静态内容。
在业务中很少使用，当需要进一步优化性能时，可能会用到。

## 条件渲染指令
- v-if、v-else-if、v-else：
例：
```html
<div id = "app">
    <p v-if = "status === 1">当status为1时显示该行</p>
    <p v-else-if = "status === 2">当status为2时显示该行</p>
    <p v-else>否则显示该行</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            status: 1
        }
    })
</script>
```
Vue在渲染元素时，出于效率考虑，会尽可能地复用已有的元素而非重新渲染，如果不希望这样做，可以使用Vue.js提供的key属性，可以让你自己决定是否要复用，key的值必须是唯一的，
```html
<div id = "app">
    <template v-if = "type === 'name'">
        <label>用户名：</label>
        <input placeholder = "输入用户名" key = "name-input">
    </template>
    <template v-else>
        <label>邮箱: </label>
        <input placeholder = "输入邮箱" key = "mail-input">
    </template>
    <button @click = "handleToggleClick">切换输入类型</button>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            type: 'name'
        },
        methods: {
            handleToggleClick: function(){
                this.type = this.type === 'name' ? 'mail' : 'name';
            }
        }
    })
</script>
```

- v-show: 与v-if基本一致，只不过v-show是改变元素的CSS属性display。

```html
<div id = "app">
    <p v-show = "status === 1">当status为1时显示该行</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            status: 2
        }
    })
</script>
```
`tips:v-show不能在<template>上使用`

v-if和v-show具有类似的功能，不过v-if才是真正的条件渲染，会根据表达式适当的销毁或重建元素及绑定的事件或子组件，若表达式初始值为false，则一开始元素/组件并不会渲染，只有当条件第一次变为真时才开始编译。
而v-show只是简单的CSS属性切换，无论条件阵雨否，都会被编译。想不之下，v-if更适合条件不经常改变的场景，因为它切换开销相对较大，而v-show适用于频繁切换调减。

## 5.3 列表渲染指令v-for
### 5.3.1 基本用法
- v-for：当需要将一个数组遍历或枚举一个对象循环显示时，就会用到列表渲染指令v-for。它的表达式需结合in来使用，类似item in items的形式。
例：
```html
<div id = "#app">
    <ul>
        <li v-for = "book in books">{{book.name}}</li>
    <ul>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            books: {
                {name: '《Vue.js实战》'},
                {name: '《JavaScript语言精粹》'},
                {name: '《JavaScript高级程序设计》'}
            }
        }
    })
</script>
```

### 5.3.2 数组更新
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
使用以上方法会改变被这些方法调用的原始数组，有些方法不会改变原数组，例：
- filter()
- concat()
- slice()
他们返回一个新数组，在使用这些非变异方法时，可以用新数组来替换原数组。

### 5.3.3 过滤与排序
当不想改变原数组，想通过一个数组的副本来做过滤或排序的显示时，可以使用计算属性来返回过滤或排序后的数组。
```html
<div id = "app">
    <ul>
        <template v-for = "book in filterBooks">
            <li>书名：{{book.name}}</li>
            <li>作者：{{book.author}}</li>
        </template>
    </ul>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            books: [
                {
                    name: '《Vue.js实战》',
                    author: '梁'
                },
                {
                    name: '《JavaScript语言精粹》',
                    author: 'Douglas Crockford'
                },
                {
                    name: '《JavaScript高级程序设计》',
                    author: 'Nicholas C.Zakas'
                }
            ]
        },
        computed: {
            filterBooks: function(){
                return this.books.filter(function(book){
                    return book.name.match(/JavaScript/);
                })
            }
        }
    })
</script>
```

## 5.4 方法与事件

### 5.4.1 基本用法
例：计数器
```html
<div id = "app">
    点击次数：{{counter}}
    <button @click="counter++">+1</button>
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            counter: 0
        }
    })
</script>
```
### 5.4.2 修饰符
- .stop
- .prevent
- .capture
- .self
- .once
具体用法：
```html
<!--阻止单击事件冒泡-->
<a @click.stop = "handle"></a>
<!--提交事件不再重载页面-->
<form @submit.prevent = "handle"></a>
<!--修饰符可以串联-->
<a @click.stop.prevent = "handle"></a>
<!--只有修饰符-->
<form @submit.prevent></form>
<!--添加事件侦听器时使用事件捕获模式-->
<div @click.capture = "handle">...</div>
<!--只当事件在该元素本身（而非子元素）触发时触发回调-->
<div @click.self = "handle">...</div>
<!--只触发一次，组件同样适用-->
<div @click.once = "handle">...</div>
```
在表单元素上监听键盘事件时，还可以使用按键修饰符。
```html
<!--只有在keyCode是13时调用vm.submit()-->
<input @keyup.13 = "submit">
```
也可以自己配置具体按键
```javascript
Vue.confit.keyCodes.f1 = 112;
//全局定义后，就可以使用@keyup.f1
```
快捷别名：
- .enter
- .tab
- .delete(捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

- .ctrl
- .alt
- .shift
- .meta(Mac下是Command键，Windows下是窗口键)
例：
```html
<!-- Shift + S-->
<input @keyup.shift.83="handleSave">
<!--Ctrl+Click-->
<div @click.ctrl="doSomething">Do something</div>
```
