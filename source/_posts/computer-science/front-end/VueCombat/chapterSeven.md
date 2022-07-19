---
title: '第七章: 组件详解'
date: 2022-03-02 15:15:54
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - Vue核心功能
 - Vue
 - 编程技术
---

# 第七章：组件详解

##  组件与复用

### 组件用法

组件全局注册示例：
```javascript 组件注册
Vue.component('my-component',{
    //选项
})
```
`推荐使用小写加减号分割的形式命名`
要在父实例中使用这个组件，必须要在示例创建前注册，之后就可以用<my-component></my-component>的形式来使用组件了。
```html 组件使用方法
<div id = "app">
    <my-component></my-component>
</div>
<script>
    Vue.component('my-component',{
        //选项
        template: '<div>这里是组件的内容</div>'
    });

    var app = new Vue({
        el: '#app'
    })
</script>
<!-- 渲染后的结果 -->
<div id = "app">
    <div>这里是组件的内容</div>
</div>
```
:::primary no-icon
template的DOM结构必须被一个元素包含，如果之间写成"这里是组件的内容", 不带"<div></div>"是无法渲染的。
在Vue实例中，使用components选项可以局部注册组件，注册后的组件只有在该实例作用域下有效，组件中也可以使用components选项来注册组件, 是组件可以嵌套。
:::

```html 局部注册组件
<div id = "app">
    <my-component></my-component>
</div>
<script>
    var Child = {
        template: '<div>局部注册组件的内容</div>'
    }
    var app = new Vue({
        el: '#app',
        components: {
            'my-component': Child
        }
    })
</script>
```
:::warning 
Vue组件的模板在某些情况下会受到HTML的限制，比如在`<table>`内规定只允许是`<tr>`、`<td>`、`<th>`等这些表格元素，所以在`<table>`内直接使用这些组件是无效的。这种情况下，可以使用特殊的is属性来挂载组件。
:::

```html is属性
<div id = '#app'>
    <table>
        <tbody is = "my-component"></tbody>
    </table>
</div>
<script>
    Vue.component('my-component',{
        template: '<div>这里是组件的内容</div>'
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```
`tbody在渲染时，会被替换为组件的内容，常见的限制元素还有<ul>、<ol>、<select>`

:::info
如果使用的是字符串模板，是不受限制的，比如后面章节介绍的.vue单文件用法等
:::
除了`template`选项外，组件中还可以像Vue实例那样使用其他的选项，比如`data`、`computed`、`methods`等。但是在使用data是，和实例稍有区别，data必须是函数，然后将数据return出去。
```html 组件data
<div id = "app">
    <my-component></my-component>
</div>
<script>
    Vue.component('my-component',{
        template: '<div>{{message}}</div>,
        data: function(){
            return {
                message: '组件内容'
            }
        }
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```

```html 复用示例
<div id = "app">
    <my-component></my-component> 
    <my-component></my-component> 
    <my-component></my-component> 
</div>
<script>
    Vue.component('my-component',{
        template: '<button @click = "counter++">{{counter}}</button>',
        data: function(){
            return counter: 0
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```

## 使用props传递数据

### 基本用法
通常父组件的模板中包含子组件，父组件要正向地向子组件传递数据或参数，子组件接收后根据参数的不同来渲染不同的内容或执行操作。这个正向传递参数的过程就是通过`props`来实现的。
在组件中，使用选项`props`来声明需要从父级接收的数据，props的值可以是两种，一种是字符串数组，一种是对象。
示例：构造一个数组，接收一个来自父级的数据message, 并把它在组件模板中渲染：
```html props传递
<div id = "app">
    <my-component message = "来自父组件的数据"><my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['message'],
        template: '<div>{{message}}</div>'
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```
渲染后的结果为：
```html 
<div id = "app">
    <div>来自父组件的数据</div>
</div>
```
[由于HTML特性不区分大小写，当使用DOM模板时，驼峰命名(camelCase)的props名称要转为短横分隔命名(kebab-case)]{.warning .label}

```html camelCase
<div id = "app">
    <my-component warning-text = "提示信息"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['warningText'],
        template: '<div>{{warningText}}</div>
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```

:::info
如果使用的是字符串模板，仍然可以忽略这些限制
:::

使用v-bind动态绑定props的值，当父组件的数据变化时，也会传递给子组件。
```html
<div id = "app">
    <input type = "text" v-model = "parentMessage">
    <my-component :message = "parentMessage"><my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['message'],
        template: '<div>{{message}}</div>'
    });
    var app = new Vue({
        el: '#app',
        data: {
            parentMessage: ''
        }
    })
</script>
<!-- 这里使用v-model绑定了父级的数据parentMessage，当通过输入框任意输入时，子组件接收到的props “message” 也会实时响应，并更新组件模板 -->
```
:::info
注意，如果你要直接传递数字、布尔值、数组、对象，而且不使用v-bind，传递的仅仅是字符串，尝试下面的示例来对比：
```html
<div id = "app">
    <my-component message = "[1,2,3]"></my-component>
    <my-component :message = "[1,2,3]"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['message'],
        template: '<div>{{message.length}}</div>'
    });

    var app = new Vue({
        el: '#app'
    })
</script>
<!-- 同一个组件使用了两次，区别仅仅是第二个使用的是v-bind, 渲染后的结果，第一个是7，第二个才是数组的长度3 -->
```
:::

### 单向数据流
Vue.2x通过props传递数据是单向的，也就是父组件数据变化时会传递给子组件，但是反过来不行，业务中需要改变prop的两种情况：
- 父组件传递初始值进来，子组件将它作为初始值保存起来，在自己的作用域下可以随意使用和修改。这种情况可以在组件data内再声明一个数据，引用父组件的prop。
```html
<div id = "app">
    <my-component :init-count = "1"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['initCount'],
        template: '<div>{{count}}</div>',
        data: function(){
            return {
                count: this.initCount
            }
        }
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```

- prop作为需要被转变的原始值传入。这种情况用计算属性就可以了。
```html 
<div id = "app">
    <my-component :width = "100"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['width'],
        template: '<div :style = "style">组件内容</div>',
        computed: {
            style: function(){
                return {
                    width: this.width + 'px'
                }
            }
        }
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```
:::info 
注意，在JavaScript中对象和数组是引用类型，指向同一个内存空间，所以props是对象和数组时，在子组件内改变是会影响父组件的。
:::

### 数据验证

props选项的值可以是数组或者对象，当prop需要验证时，就需要对象写法。
```JavaScript 数据验证
Vue.component('my-component',{
    props: {
        propA: Number,
        propB: [String, Number],
        propC: {
            type: Boolean,
            default: true
        },
        propD: {
            type: Number,
            required: true
        },
        propE: {
            type: Array,
            default: function(){
                return [];
            }
        },
        propF: {
            validator: function(value){
                return value > 10;
            }
        }
    }
});
```
验证的type类型可以是：
- String
- Number
- Boolean
- Object
- Array
- Function
type也可以是一个自定义构造器，使用instanceof检测。
当prop验证失败时，在开发版本下会在控制台抛出一条警告。

## 组件通信

### 自定义事件
当子组件需要向父组件传递数据时，就要用到自定义事件。v-on除了监听DOM事件外，还可以用于组件之间的自定义事件。
父组件可以直接在子组件的自定义标签上使用v-on来监听子组件触发的自定义事件。
```html
<div id = "app">
    <p>总数：{{total}}</p>
    <my-component
        @increase = "handleGetTotal"
        @reduce = "handleGetTotal"></my-component>
</div>
<script>
    Vue.component('my-component',{
        template: '\
        <div>\
            <button @click = "handleIncrease"> +1 </button>\
            <button @click = "handleReduce"> -1 </button>\
        </div>',
        data: function(){
            return {
                counter: 0
            }
        },
        methods: {
            handleIncrease: function(){
                this.counter++;
                this.$emit('increase', this.counter);
            },
            handleReduce: function(){
                this.counter--;
                this.$emit('reduce', this.counter);
            }
        }
    });
    
    var app = new Vue({
        el: '#app',
        data: {
            total: 0
        },
        methods: {
            handleGetTotal: function(total){
                this.total = total;
            }
        }
    })
// 在改变组件的data "counter"后，通过$emit()再把它传递给父组件，父组件用v-on:increase和v-on:reduce。$emit()方法的第一个参数是自定义事件的名称，后面的参数都是要传递的数据。
```
### 使用v-model
v-model可以用来创建自定义的表单输入组件，进行数据双向绑定。
```html
<div id = "app">
    <p>总数: {{total}}</p>
    <my-component v-model = "total"></my-component>
    <button @click = "handleReduce"> -1 </button>
</div>
<script>
    Vue.component('my-component',{
        props: ['value'],
        template: '<input :value = "value" @input = "updateValue">'.
        methods: {
            updateValue: function(event){
                this.$emit('input', event.target.value);
            }
        }
    });
    var app = new Vue({
        el: '#app',
        data: {
            total: 0
        },
        methods: {
            handleReduce: function(){
                this.total--;
            }
        }
    })
</script>
```
实现这样一个具有双向绑定的v-model组件要满足下面两个要求：
- 接收一个value属性
- 在有新的value时触发input事件

### 非父子组件通信
在Vue.js 2.x中，推荐使用一个空的Vue实例作为中央事件总线(bus)，也就是一个中介。
```html  事件总线bus
<div id = "app">
    message
    <component-a></component-a>
</div>
<script>
    var bus = new Vue();

    Vue.component('component-a',{
        template: '<button @click = "handleEvent">传递事件</button>',
        methods: {
            handleEvent: function(){
                bus.$emit('on-message', '来自组件component-a的内容');
            }
        }
    });

    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        },
        mounted: function(){
            var _this = this;
            bus.$on('on-message', function(msg){
                _this.message = msg;
            });
        }
    })
</script>
```
首先创建了一个名为bus的空Vue实例，里面没有任何内容：然后全局定义了组件component-a；最后创建Vue实例app，在app初始化时，也就是在生命周期mounted钩子函数里监听了来自bus的事件on-message发出去，此时app就会接收到来自bus的事件，进而在回调里完成自己的业务逻辑。
当项目比较大，有更多的人员参与开发时，也可以选择更好的状态管理解决方案vuex。
除了中央事件总线bus外，还有两种方法可以实现组件间通信：父链和子组件索引。
- `父链`
在子组件中，使用`this.$parent`可以直接访问该组件的父实例或组件，父组件也可以通过this.$children访问它所有的子组件，而且可以递归的从上或乡下无线访问，直到根实例或最内层的组件
```html 父链通信
<div id = "app">
    message
    <component-a></component-a>
</div>
<script>
    Vue.component('component-a',{
        template: '<button @click = "handleEvent">通过父链直接修改数据</button>',
        methods: {
            handleEvent: function(){
                this.$parent.message = '来自组件component-a的内容';
            }
        }
    });

    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        }
    })
</script>
```
:::warning
尽管Vue允许这样操作，但在业务中，子组件应该尽可能的避免依赖父组件的数据，更不应该去主动修改它的数据，因为这样是的父子组件紧耦合，只看父组件，很难理解父组件的状态，因为他可能被任意组件修改，理想情况下，只有组件自己能修改它的状态，父子组件最好还是通过props和$emit来通信。
:::

- `子组件索引`
当子组件较多时，通过this.$children来一一遍历出我们需要的一个组件实例是比较困难的，尤其是组件动态渲染时，他们的序列是不固定的。Vue提供了子组件索引的方法，用特殊的属性ref来为子组件指定一个索引名称。
```html 子组件索引
<div id = "app">
    <button @click = "handleRef">通过ref获取子组件实例</button>
    <component-a ref = "comA"></component-a>
</div>
<script>
    Vue.component('component-a',{
        template: '<div>子组件</div>',
        data: function(){
            return {
                message: '子组件内容'
            }
        }
    })；
    
    var app = new Vue({
        el: '#app',
        methods: {
            handleRef: function(){
                var msg = this.$refs.comA.message;
                console.log(msg);
            }
        }
    })
</script>
```
:::info
在父组件中，子组件标签上使用ref指定一个名称，并在父组件内通过this.$refs来访问指定名称的子组件。
$refs只在组件渲染完成后才填充，并且它是非响应式的。它仅仅作为一个直接访问子组件的应急方案，应当避免在模板或计算属性中使用$refs。
:::

## 使用slot分发内容

当需要让组件组合使用，混合父组件的内容与子组件的模板时，就会用到slot，这个过程叫做内容分发(transclusion)。以<app>为例，有两个特点：
- <app>组件不知道它的挂载点会有什么内容。挂载点的内容分发就构成了Vue组件的3个<app>的父组件决定的。
- <app>组件很可能有它自己的模板。
props传递数据、events触发事件和slot内容分发就构成了Vue组件的3个API来源，再复杂的组件也是由这3部分构成的。

### 作用域
::: default no-icon
<child-component>
    {{message}}
</child-component>
:::

这里的message就是一个slot，但是它绑定的是父组件的数据，而不是组件`<child-component>`的数据。
父组件模板的内容是在父组件作用域内编译，子组件模板的内容是在子组件作用域内编译。
```html 作用域
<div id = "app">
    <child-component v-show = "showChild"></child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '<div>子组件</div>'
    });

    var app = new Vue({
        el: '#app',
        data: {
            showChild: true
        }
    })
</script>
<!-- 这里的状态showChild绑定的是父组件的数据 -->
```
```html
<div id = "app">
    <child-component></child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '<div v-show = "showChild">子组件</div>',
        data: function(){
            return {
                showChild: true
            }
        }
    })
</script>
<!-- slot分发的内容，作用域在父组件上 -->
```
### slot用法
- 单个Slot
在子组件内使用特殊的<slot>元素就可以为这个子组件开启一个slot(插槽)，在父组件模板里，插入在子组件标签内的所有内容将替代子组件的<slot>标签及它的内容。
```html  slot用法
<div id = "app">
    <child-component>
        <p>分发的内容</p>
        <p>更多分发的内容</p>
    </child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '\
        <div>\
            <slot>\
                <p>如果父组件没有插入内容, 我将作为默认出现</p>\
            </slot>\
        <div>'
    });

    var app = new Vue({
        el: '#app'
    })
```
渲染后的结果：
```html
<div id = "app">
    <div>
        <p>分发的内容</p>
        <p>更多分发的内容</p>
    </div>
</div>
```

- 具名slot
给<slot>元素指定一个name后可以分发多个内容，具名Slot可以与单个Slot共存。
```html
<div id = "app">
    <child-component>
        <h2 slot = "heander">标题</h2>
        <p>正文内容</p>
        <p>更多的正文内容</p>
        <div slot = "footer">底部信息</div>
    </child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '\
        <div class = "container">\
            <div calss = "header">\
                <slot name = "header"></slot>\
            </div>\
            <div class = "main">\
                 <slot></slot>\
            </div>\
            <div class = "footer">\
                <slot name = "footer"></slot>\
            </div>\
        </div>'
    })
</script>
```
### 作用域插槽
作用域插槽是一种特殊的slot，使用一个可以复用的模板替换已渲染元素。
```html 作用域插槽
<div id = "app">
    <child-component>
        <template scope = "props">
            <p>来自父组件的内容</p>
            <p>{{props.msg}}</p>
        </template>
    </child-component>
</div>
<script>
    Vue.component('component',{
        template: '\
        <div class = "container">\
            <slot msg = "来自子组件的内容"></slot>\
        </div>'
    });
    
    var app = new Vue({
        el: '#app'
    })
</script>
```
### 作用域插槽
作用域插槽是一种特殊的slot，使用一个可以服用的模板替换已渲染元素。
```html
<div id = "app">
    <child-component>
        <template scope = "props">
            <p>来自父组件的内容</p>
            <p>{{props.msg}}</p>
        </template>
    </child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '\
        <div class = "container">\
            <slot msg = "来自子组件的内容"></slot>\
        </div>'
    });

    var app = new Vue({
        el: '#app'
    })
    // 在<slot>元素上有一个类似props传递数据给组件的写法 msg = "xxx"，将数据传到了插槽。父组件使用了<template>元素，而且拥有一个scope = "props"的特性，这里的props只是一个临时变量，就像v-for="item in items"里面的item一样。template内可以通过临时变量props访问来自子组件插槽的数据msg。
</script>
```
渲染后的结果：
```html
<div id = "app">
    <div class = "container">
        <p>来自父组件的内容</p>
        <p>来自子组件的内容</p>
    </div>
</div>
```
作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项。
```html 列表示例
<div id = "app">
    <my-list :books = "books">
        <!-- 作用域插槽也可以是具名的Slot -->
        <template slot = "book" scope = "props">
            <li>{{props.bookName}}</li>
        </template>
    </my-list>
</div>
<script>
    Vue.component('my-list',{
        props: {
            books: {
                type: Array,
                default: function(){
                    return [];
                }
            }
        },
        template: '\
        <ul>\
            <slot name = "book"\
                v-for = "book in books"\
                :book-name = "book.name">\
                <!--这样也可以写默认的slot内容-->\
            </slot>\
        </ul>'
    });

    var app = new Vue({
        el: '#app',
        data: {
            books: [
                { name: '《Vue.js实战》'},
                { name: '《JavaScript语言精粹》'},
                { name: '《JavaScript高级程序设计》'},
            ]
        }
    })
</script>
```
子组件my-list接收一个来自父级的prop数组book，并且将它在name为book的slot上使用v-for指令循环，同时暴露一个变量bookName。

### 访问slot
Vue.js 2.x提供了用来访问被slot分发的内容的方法$slots。
```html 访问slot
<div id = "app">
    <child-component>
        <h2 slot = "header">标题</h2>
        <p>正文内容</p>
        <p>更多的正文内容</p>
        <div slot = "footer">底部信息</div>
    <child-component>
</div>
<script>
    Vue.component('child-component',{
        template: '\
        <div class = "container">\
            <div class = "header">\
                <slot name = "header"></slot>\
            </div>\
            <div class = "main">\
                <slot></slot>\
            </div>\
            <div calss = "footer">\
                <slot name = "footer"></slot>\
            </div>\
        </div>',
        mounted: function(){
            var header = this.$slots.header;
            var main = this.$slots.default;
            var footer = this.$slots.footer;
            console.log(footer);
            console.log(footer[0].elm.innerHTML);
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```

通过$slots可以访问某个具名slot, this.$slots.default包括了所有没有被包含在slot的节点。
[$slots在业务中几乎用不到，在用render函数创建组件时会比较有用，但还是主要用于独立组件开发中]{.warnging .label}。


