---
title: 补充：组件高级用法
date: 2022-03-04 16:03:45
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - Vue核心功能
 - Vue
 - 编程技术
---

# 组件高级用法
`这些用法在实际业务中不常用，但在独立组件开发时可能会用到`

## 递归组件
组件在它的模板内可以递归地调用自己，只要给组件设置name的选项就可以了。
```html
<div id = "app">
    <child-component :count="1"></child-component>
</div>
<script>
    Vue.component('child-component',{
        name: 'child-component',
        props: {
            count: {
                type: Number,
                default: 1
            }
        },
        template: '\
            <div class = "child">\
                :count = "count + 1"\
                v-if = "count < 3"></child-component>\
            </div>',
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
设置name后，在组件模板内就可以递归地使用了，不过需要注意的是，必须给一个条件来限制递归数量，否则会抛出错误：max stack size exceeded。
组件递归使用可以用来开发一些具有未知层级关系的独立组件，比如级联选择器和树形控件等。

## 内联模板
组件的模板一般都是在template选项内定义的，Vue提供了一个内联模板的功能，在使用组件时，给组件标签使用inline-template特性，组件就会把他的内容当做模板，而不是把它当内容分发，这让模板更灵活。
```html 内联模板
<div id = "app">
    <child-component inline-template>
        <div>
            <h2>在父组件中定义子组件的模板</h2>
            <p>{{message}}</p>
            <p>{{msg}}</p>
        </div>
    </child-component>
</div>
<script>
    Vue.component('child-component',{
        data: function(){
            return {
                msg: '在子组件声明的数据'
            }
        }
    });

    var app = new Vue({
        el: '#app',
        data: {
            message: '在父组件声明的数据’
        }
    })
</script>
```
渲染后的结果为:
```html
<div id = "app">
    <div>
        <h2>在父组件中定义子组件的模板</h2>
        <p>{{message}}</p>
        <p>{{msg}}</p>
    </div>
</div>
```
在父组件中声明的数据message和子组件中声明的数据msg，两个都可以渲染（如果同名，优先使用子组件的数据）。这是内联模板的缺点，就是作用域比较难理解，如果不是非常特殊的场景，建议不要轻易使用内联模板。

## 动态组件
Vue.js提供了一个特殊的元素<component>用来动态地挂载不同的组件，使用is特性来选择要挂载的组件。
```html
<div id = "app">
    <component :"is = "currentView"></componet>
    <button @click = "handleChangeView('A')">切换到A</button>
    <button @click = "handleChangeView('B')">切换到B</button>
    <button @click = "handleChangeView('C')">切换到C</button>
</div>
<script>
    var app = new Vue({
        el: '#app',
        components: {
            comA: {
                template: '<div>组件A</div>'
            },
            comB: {
                template: '<div>组件B</div>'
            },
            comC: {
                template: '<div>组件C</div>'
            }
        },
        data: {
            currentView: 'comA'
        },
        methods: {
            handleChangeView: function(component){
                this.currentView = 'com' + component;
            }
        }
    })
</script>
```
动态的改变currentView的值就可以动态地挂载组建了。也可以直接绑定在组件对象上：
```html 
<div id = "app">
    <component :is = "currentView"></component>
</div>
<script>
    var Home = {
        template: '<p>Welcome home!</p>'
    };
    var app = new Vue({
        el: '#app',
        data: {
            currentView: Home
        }
    })
</script>
```

## 异步组件
当工程足够大，使用的组件足够多时，需要考虑下性能问题，因为一开始把所有的组件都加载是没必要的一笔开销。Vue.js允许将组件定义为一个工程函数，动态地解析组件。Vue.js只在组件需要渲染时触发工厂函数，并且把结果缓冲起来，用于后面的再次渲染。
```html 异步组件
<div id = "app">
    <child-component></child-component>
</div>
<script>
    Vue.component('child-component',function(resolve, reject){
        window.setTimeout(function(){
            resolve({
                template: '<div>我是异步渲染的</div>'
            });
        },2000);
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
工厂函数接收一个resolve回调，在收到从服务器下载的组件定义时调用，也可以调用reject(reason)指示加载失败。这里setTimeout只是为了演示异步，具体的下载逻辑可以自己决定，比如把组件配置写成一个对象配置，通过Ajax来请求，然后调用resolve传入配置选项。

## 其他

### $nextTick
先来看这样一个场景：有一个div，默认用v-if将它隐藏，点击一个按钮后，改变v-if的值，让它显示出来，同时拿到这个div的文本内容。如果v-if的值是false，直接去获取div的内容是获取不到的，因为此时div还没有被创建出来，那么应该在点击按钮后，改变v-if的值为true，div才会被创建，此时再去获取。
Vue在观察到数据变化时并不是直接更新DOM，而是开启一个队列，并缓冲在同一个事件循环中发生的所有数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和DOM操作。然后，在下一个事件循环tick中，Vue刷新队列并执行实际(已去重的)工作。所以如果用一个for循环来动态改变数据100次，其实他只会应用最后一次改变，如果没有这种机制，DOM就要重绘100次，这是一个很大的开销。
在执行this.showDiv = true; 时，div仍然还是没有被创建出来，直到下一个Vue事件循环时，才开始创建。$nextTick就是用来知道什么时候DOM更新完成的。
```html nextTick
<div id = "app">
    <div id = "div" v-if = "showDiv">这是一段文本</div>
    <button @click = "getText">获取div内容</button>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            showDiv: false
        },
        methods: {
            getText: function(){
                this.showDiv = true;
                this.$nextTick(function(){
                    var text = document.getElementById('div').innerHTML;
                    console.log(text);
                });
            }
        }
    })
</script>
```
### X-Templates
如果你没有使用webpack、gulp等工具，试想一下你的组件template的内容很冗长、复杂、如果都在JavaScript里拼接字符串，效率是很低的，因为不能像写HTML那样舒服。Vue提供了另一种定义模板的方式, 在`<script>`标签里使用text/x-template类型，并且指定一个id，将这个id赋给template。
```html X-Templates
<div id = "app">
    <my-component></my-component>
    <script type = "text/x-template" id = "my-component">
        <div>这是组件的内容</div>
    </script>
</div>
<script>
    Vue.component('my-component',{
        template: '#my-component'
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
在`<script>`标签里，可以写HTML代码，不用考虑换行问题。

### 手动挂载实例
我们现在所创建的实例都是通过new Vue()的形式创建出来的。在一些非常特殊的情况下，我们需要动态地创建Vue实例，Vue提供了Vue.extend和$mount两个方法来手动挂载一个实例。
Vue.extend是基础Vue构造器，创建一个“子类”，参数是一个包含组件选项的对象。
如果Vue实例在实例化时没有收到el选项，他就处于“未挂载”状态，没有关联的DOM元素，可以使用$mount()手动地挂载一个未挂载的实例。这个方法返回实例自身，因而可以链式调用其他实例方法。
```html
<div id = "mount-div">

</div>
<script>
    var MyComponent = Vue.extend({
        template: '<div>Hello: {{name}}</div>',
        data: function(){
            return {
                name: 'Aresn'
            }
        }
    });

    new MyComponent().$mount('#mount-div');
</script>
```
运行后，id为mount-div的div元素会被替换为组件MyComponent的template的内容：
`<div>Hello:Aresn</div>`
除了这种写法外，以下两种写法也是可以的：
```javascript
new MyComponent().$mount('#mount-div');
//同上
new MyComponent({
    el: '#mount-div'
});
//或者，在文档之外渲染并且随后挂载
var component = new MyComponent().$mount();
document.getElementById('mount-div').appendChild(component.$el);
```
手动挂载实例（组件）是一种比较极端的高级用法,在业务中几乎用不到，只在开发一些复杂的独立组件时可能会使用。


