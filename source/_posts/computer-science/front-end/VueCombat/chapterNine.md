---
title: '第九章: Render函数'
date: 2022-03-07 14:18:51
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 第九章: Render函数
Vue.js 2.x使用了Virtual Dom(虚拟DOM)来更新DOM节点，提升渲染性能。

## Virtual Dom

React和Vue 2都是用了Virtual Dom技术，Virtual Dom并不是真正意义上的DOM，而是一个轻量级的JavaScript对象，在状态发生变化时，Virtual Dom会进行Diff运算，来更新只需要被替换的DOM，而不是全部重绘。
与DOM操作相比，Virtual Dom是基于JavaScript计算的，所以开销会小很多。
正常的DOM节点在HTML中是这样的：
```html
<div id = "main">
    <p>文本内容</p>
    <p>文本内容</p>
</div>
```
![virtualDOM](/assets/vue/virtualDom.png)
用Virtual Dom创建的JavaScript对象一般会是这样的：
```javascript
var vNonde = {
    tag: 'div',
    attributes: {
        id: 'main'
    },
    children: {
        //p节点
    }
}
```
vNode对象通过一些特定的选项描述了真实的DOM结构。
在Vue.js中，Virtual Dom就是通过一种VNode类表达的，每个DOM元素或组件都对应一个VNode对象，在Vue.js源码中是这样定义的：
```javascript
export interface VNode{
    tag?: string;
    data?: VNodeDate;
    children?: VNode[];
    text?: string;
    elm?: Node;
    ns?: string;
    context?: Vue;
    key?: string|number;
    componentOption?: VNodeComponentOptions;
    componentInstance?: Vue;
    parent?: VNode;
    raw?: boolean;
    isStatic?: boolean;
    isRootInsert: boolean;
    isComment: boolean;
}
```
具体含义:
- tag 当前节点的标签名
- data 当前节点的数据对象
```javascript VNodeData 
export interface VNodeData{
    key?: string|number;
    slot?: string;
    scopeSlots?: { [key: string]: ScopedSlot };
    ref?: string;
    tag?: string;
    staticClass?: string;
    class?: any;
    staticStyle?: { [key: string]: any };
    style?: Object[] | Object;
    props?: { [key: string]: any };
    attrs?: { [key: string]: any };
    domProps?: { [key: string]: any };
    hook?: { [key: string]: Function };
    on?: { [key: string]: Function | Function[] };
    nativeOn?: { [key: string]: Function | Function[] }
    transition?: Object;
    show?: boolean;
    inlineTemplate?: {
        render: Function;
        staticRenderFns: Function[];
    };
    directives?: VNodeDirective[];
    keepAlive?: boolean;
}
```
- children 子节点，数组，也是VNode类型
- text 当前节点的文本，一般文本节点或注释节点会有该属性。
- elm 当前虚拟节点对应的真实的DOM节点。
- ns 节点的namespace。
- content 编译作用域。
- functionContext 函数化组件的作用域。
- key 节点的key属性，用于作为节点的标识，有利于patch的优化。
- componentOptions 创建组件实例时会用到的选项信息。
- child 当前节点对应的组件类型。
- parent 组件的占位节点。
- raw 原始html。
- isStatic 静态节点的标识。
- isRootInsert 是否作为根节点插入，被<transition>包裹的节点，该属性的值为false。
- isComment 当前节点是否是注释节点。
- isCloned 当前节点是否是克隆节点。
- isOnce 当前节点是否有v-once指令。
VNode 主要可以分为如下积累。
![VNode](/assets/vue/VNode.png)
- TextVNode 文本节点。
- ElementVNode 普通元素节点
- ComponentVNode 组件节点。
- EmptyVNode 没有内容的注释节点
- CloneVNode 克隆节点，可以是以上任意类型的节点，唯一的区别在于isCloned属性为true
::: default no-icon
使用Virtual Dom就可以完全发挥JavaScript的编程能力。在多数场景中，我们使用template就足够了，但在一些特定的场景下，使用Virtual Dom会更简单。
:::

## Render函数简介
先来看这样一个场景：在很多文章类型的网站中(比如文档、博客)都有区分一级标题、二级标题、三级标题······为方便分享url，他们都做成了锚点，点击一下，会将内容加在网址后面，以“#”分隔。
prop: level具备heading级别的含义，使用render函数改写组件
```html
<div id = "app">
    <anchor :level = "2" title = "特性">特性</anchor>
</div>
<script>
    Vue.component('anchor',{
        props: {
            level: {
                type: Number,
                required: true
            },
            title: {
                type: String,
                default: ''
            }
        },
        render: function(createElement) {
            return createElement(
                'h' + this.level,
                [
                    createElement(
                        'a',
                        {
                            domProps:{
                                href: '#' + this.title
                            }
                        },
                        this.$slots.default
                    )
                ]
            )
        }          
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
Render函数通过createElement参数来创建VirtualDom，结构精简了很多。

## createElement用法
### 基本参数
createElement构成了Vue Virtual Dom的模板，有三个参数：
```javascript
createElement(
    //{String | Object | Function}
    //一个HTML标签，组件选项，或一个函数
    //必须Return上述其中一个
    'div',
    //{Object}
    //一个对应属性的数据对象，可选
    //您可以在template中使用
    {
        //稍后详细介绍
    },
    //{String | Array}
    //子节点(VNodes), 可选
    [
        createElement('h1', 'hello world'),
        createElement(MyComponent,{
            props: {
                someProp: 'foo'
            }
        }),
        'bar'
    ]
)
```
第一个参数必选，可以是一个HTML标签，也可以是一个组件或函数：第二个是可选参数，数据对象，在template中使用。第三个是子节点，也是可选参数，用法一致。
对于第二个参数“数据对象”，具体的选项如下：
```javascript
{
    //和v-bind:class 一样的API
    'class': {
        foo: true,
        bar: false
    },
    //和v-bind:style一样的API
    style: {
        color: 'red',
        fontSize: '14px'
    },
    //正常的HTML特性
    attrs: {
        id: 'foo'
    },
    //组件props
    props: {
        myProp: 'bar'
    },
    //DOM属性
    domProps: {
        innerHTML: 'baz'
    },
    //自定义事件监听器“on”
    //不支持v-on: keyup.enter的修饰器
    //需要手动匹配keyCode
    on: {
        click: this.clickHandler
    },
    //仅对于组件，用于监听原生事件
    //而不是组件使用vm.$emit触发的自定义事件
    nativeOn: {
        click: this.nativeClickHandler
    },
    //自定义指令
    directives: [
        {
            name: 'my-custom-directive',
            value: '2'
            expression: '1 + 1',
            arg: 'foo',
            modifiers: {
                bar: true
            }
        }
    ],
    //作用域slot
    //{name: props => VNode | Array<VNode>}
    scopedSlots: {
        default: props = > h('span', props.text)
    },
    //如果子组件有定义slot的名称
    slot: 'name-of-slot'
    //其他特殊顶层属性
    key: 'myKey',
    ref: 'myRef'
}
```
以往在template里，我们都是在组件的标签上使用形如 v-bind:class、v-bind:style、v-on:click这样的指令，在Render函数都将其写进在了数据对象里。
```html 传统组件template写法
<div id = "app">
    <ele></ele>
</div>
<script>
    Vue.component('ele',{
        template: '\
            <div id = "element"\
            :class = "{show: show}"\
            @click = "handleClick">文本内容</div>',
        data: function(){
            return {
                show: true
            }
        },
        methods: {
            handleClick: function(){
                console.log('clicked!');
            }
        }
    })
    
    var app = new Vue({
        el: '#app'
    })
</script>
```
使用Render改写：
```html 
<div id = "app">
    <ele></ele>
</div>
<script>
    Vue.component('ele',{
        render: function(createElement){
            return createElement(
                'div',
                {
                    //动态绑定class，同:class
                    class: {
                        'show': this.show
                    },
                    //普通html特性
                    attrs: {
                        id: 'element'
                    },
                    //给div绑定click事件
                    on: {
                        click: this.handleClick
                    }
                },
                '文本内容'
            )
        },
        data: function(){
            return {
                show: true
            }
        },
        methods: {
            handleClick: function(){
                console.log('clicked!');
            }
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```

### 约束
所有的组件树中，如果VNode是组件或含有组件的slot，那么VNode必须唯一。
错误示例：
```html 重复使用组件
<div id = "app">
    <ele><ele>
</div>
<script>
    //局部声明组件
    var child = {
        render: function(createElement){
            return createElement('p','text');
        }
    };
    Vue.component('ele',{
        render: function(createElement){
            //创建一个节点，使用组件Child
            var ChildNode = createElement(Child);
            return createElement('div',[
                ChildNode,
                childNode;
            ]);
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
```html 重复使用含有组件的slot
<div id = "app">
    <ele>
        <div>
            <Child></Child>
        </div>
    </ele>
</div>
<script>
    //全局注册组件
    Vue.component('Child',{
        render: function(createElement){
            return createElement('p', 'text');
        }
    });

    Vue.component('ele',{
        render: function(createElement){
            return createElement('div',[
                this.$slots.default;
                this.$slots.default;
            ]);
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
这两个示例都期望在子节点内渲染出两个Child，也就是两个<p>text</p>节点，实际预览时只渲染出一个，因为在这种情况下，VNode受到了约束。
```html 重复渲染多个组件
<div id = "#app">
    <ele></ele>
</div>
<script>
    //局部声明组件
    var Child = {
        render: function(createElement){
            return createElement('p','text');
        }
    };
    Vue.component('ele',{
        render: function(createElement){
            return createElement('div',
                Array.apply(null,{
                    length: 5
                }).map(function(){
                    return createElement(Child);
                })
            );
        }
    });

    var app = new Vue({
        el: '#app'
    })
<script>
```
通过一个循环和工厂函数就可以渲染5个重复的子组件Child。对于含有组件的slot，复用就要稍微复杂一点了，需要将slot的每个子节点都克隆一份。
```html 复用含有组件的slot
<div id = "app">
    <ele>
        <div>
            <Child></Child>
        </div>
    </ele>
</div>
<script>
    //全局注册组件
    Vue.component('Child',{
        render: function(createElement){
            return createElement('p','text');
        }
    });
    Vue.component('ele',{
        render: function(createElement){
            //克隆slot节点的方法
            function cloneVNode(vnode){
                //递归遍历所有子节点，并克隆
                const cloneChildren = vnode.children && vnode.children.map(function(vnode){
                    return cloneVNode(vnode);
                });
                const cloned = createElement(
                    vnode.tag,
                    vnode.data,
                    cloneChildren
                );
                cloned.text = vnode.text;
                cloned.isComment = vnode.isComment;
                cloned.componentOptions = vnode.componentOptions;
                cloned.elm = vnode.elm;
                cloned.context = vnode.context;
                cloned.ns = vnode.ns;
                cloned.isStatic = vnode.isStatic;
                cloned.key = vnode.key;

                return cloned;
            }
            const vNodes = this.$slots.default;
            const clonedVNodes = vNodes.map(function(vnode){
                return cloneVNode(vnode);
            });

            return createElement('div',[
                vNodes,
                clonedVNodes
            ]);
        }
    });

    var app = new Vue({
        el: '#app
    })
```
在Render函数里创建了一个cloneVNode的工厂函数，通过递归将slot所有子节点都克隆了一份，并对VNode的关键属性也进行了复制。
深度克隆slot的做法有点偏黑科技，不过在一般业务中几乎不会遇到这样的需求，主要还是运用在独立组件中。

### 使用JavaScript代替模板功能
在Render函数中，不再需要Vue内置的指令，如v-if、v-for，当然，也没方法使用他们，无论实现什么功能，都可以使用原生JavaScript。
```html v-if&v-else
<div id = "app">
    <ele :show = "show"></ele>
    <button @click = "show = !show">切换</button>
</div>
<script>
    Vue.component('ele',{
        render: function(createElement){
            if(this.show){
                return createElement('p', 'show的值为true');
            }else {
                return createElement('p', 'show的值为false');
            }
        },
        props: {
            show: {
                type: Boolean,
                default: false
            }
        }
    });

    var app = new Vue({
        el: '#app',
        data: {
            show: false
        }
    })
</script>
```
使用for循环实现
```html
<div id = "app">
    <ele :list = "list"></ele>
</div>
<script>
    Vue.component('ele', {
        render: function(createElement){
            var nodes = [];
            for(var i = 0; i < this.list.length; i++){
                nodes.push(createElement('p', this.list[i]));
            }
            return createElement('div', nodes);
        },
        props: (
            list: {
                type: Array
            }
        )
    });

    var app = new Vue({
        el: '#app',
        data: {
            list: [
                '《Vue.js实战》',
                '《JavaScript高级程序设计》',
                '《JavaScript语言精粹》'
            ]
        }
    })
</script>
```
:::info 
map()方法是快速改变数组结构，返回一个新数组，如果你不熟悉数组的这种链式操作(map常和filter、sort等方法一起使用，因为他们返回的都是新数组)，可以使用简单的for循环，这样更容易理解。
:::

Render函数里也没有与v-model对应的API，需要自己来实现逻辑。
```html
<div id = "app">
    <ele></ele>
</div>
<script>
    Vue.component('ele',{
        render: function(createElement){
            var _this = this;
            return createElement('div',[
                createElement('input',{
                    domProps:{
                        value: this.value
                    },
                    on: {
                        input: function(event){
                            _this.value = event.target.value;
                        }
                    }
                });
                createElement('p', 'value: ' + this.value)
            ])
        },
        data: function(){
            return {
                value: ''
            }
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
事实上，v-model就是prop: value和event: input组合使用的一个语法糖，虽然在Render里写起来比较复杂，但是可以自由控制，深入到最底层。
上例的Render函数对应的template写法如下：
```html
<div>
    <input v-model = "value">
    <p>value:{{value}}</p>
</div>
```
对于事件修饰符和按键修饰符，基本也需要自己实现，修饰符对应的实现方案：
|修饰符|对应的句柄|
|:----|:---------|
|.stop|event.stopPropagation()|
|.prevent|event.preventDefault()|
|.self|if(event.target!==event.currentTarget)return|
|.enter、.13|if(event.keyCode!==13)return 替换13位需要的keyCode|
|.ctrl、.alt、.shift、.meta|if(!event.ctrlKey)return 根据需要替换ctrlKey为altKey、shiftKey或metaKey|
对于事件修饰符.capture和.once，Vue提供了特殊的前缀，可以直接写在on的配置项里。


|修饰符|前缀|
|:----|:----|
|.capture|!|
|.once|~|
|.capture.once或.once.capture|~!|

写法如下：
```javascript
on: {
    '!click': this.doThisInCapturingMode,
    '~keyup': this.doThisOnce,
    '~!mouseover': this.doThisOnceInCapturingMode
}
```


## 函数化组件
Vue.js 提供了一个functional的布尔值选项，设置为true可以使组件无状态和无实例，也就是没有data和this上下文。这样用render函数返回虚拟节点可以更容易渲染，因为函数化组件只是一个函数，渲染开销要小很多。
使用函数化组件时，Render函数提供了第二个参数context来提供临时上下文。组件需要的data、props、slots、children、parent都是通过这个上下文来传递的，比如this.level要改写为context.props，this.$slots.default改写为context.children。

```html 函数化组件
<div id = "app">
    <smart-item :data = "data"></smart-item>
    <button @click = "change('img')">切换为图片组件</button>
    <button @click = "change('video')">切换为视频组件</button>
    <button @click = "change('text')">切换为文本组件</button>
</div>
<script>
    //图片组件选项
    var ImgItem = {
        props: ['data'],
        render: function(createElement){
            return createElement('div',[
                createElement('p','图片组件'),
                createElement('img',{
                    attrs: {
                        src: this.data.url
                    }
                })
            ]);
        }
    };
    //视频组件选项
    var VideoItem = {
        props: ['data'],
        render: function(createElement){
            return createElement('div',[
                createElement('p','视频组件'),
                createElement('video',{
                    attrs: {
                        src: this.data.url,
                        controls: 'controls',
                        autoplay: 'autoplay'
                    }
                })
            ]);
        }
    };
    //纯文本组件选项
    var TextItem = {
        props: ['data'],
        render: function(createElement){
            return createElement('div',[
                createElement('p','纯文本组件'),
                createElement('p',this.data.text)
            ]);
        }
    };
    Vue.component('smart-item',{
        //函数化组件
        function: true,
        render: function(createElement, context){
            //根据传入的数据，智能判断显示哪种组件
            function getComponent(){
                var data = context.props.data;
                //判断prop: data的type字段是哪种组件
                if(data.type === 'img') return ImgItem;
                if(data.type === 'video') return VideoItem;
                return TextItem;
            }
            return createElement(
                getComponent(),
                {
                    props: {
                        data: context.props.data
                    }
                },
                context.children
            )
        },
        props: {
            data: {
                type: Object,
                required: true
            }
        }
    })

    var app = new Vue({
        el: '#app',
        data: {
            data: {}
        },
        methods: {
            //切换不同类型组件的数据
            change: function(type){
                if(type === 'img'){
                    this.data = {
                        type: 'img',
                        url: 'https://raw.githubusercontent.com/iview/iview/master/assets/logo.png'
                    }
                }else if(type === 'video'){
                    this.data = {
                        type: 'video',
                        url: 'http://vjs.zencdn.net/v/oceans.mp4'
                    }
                }else if(type === 'text'){
                    this.data = {
                        type: 'text',
                        content: '这是一段纯文本'
                    }
                }
            }
        },
        created: function(){
            //初始化时，默认设置图片组件的数据
            this.change('img');
        }
    })
</script>
```
分析: ImgItem、VideoItem、TextItem这3个对象分别是图片组件、视频组件和纯文本组件的选项，他们都接收一个prop: data。在函数化组件smart-item里，也有props: data，通过getComponent函数来判断其字段type的值，选择这条数据适合渲染的组件。通过createElement把getComponent()返回的对象设置为第一个参数，然后通过第二个参数把smart-item的data传递到选择的组件里的prop: data, 组件许渲染出不同的内容。
根实例app中的方法change用来生成不同的数据，通过3个button来切换。
:::info 
函数化组件主要适用于以下两个场景：
- 程序化的在多个组件中选择一个。
- 在将children, prop, data传递给子组件之前操作他们。

## JSX
使用Render函数最不友好的地方就是模板比较简单时，写起来比较复杂，而且难以阅读出DOM结构，尤其当子节点嵌套较多时，嵌套的createElement就像盖楼一样一层层延伸下去。
为了让Render函数更好的书写和阅读，Vue.js提供了插件babel-plugin-transform-vue-jsx来支持JSX语法。
JSX是一种看起来像HTML，但实际上是JavaScript的语法扩展，它用更接近于DOM结构的形式来描述一个组件的UI和状态信息，最早在React.js里大量应用。
```javascript JSX
new Vue({
    el: '#app',
    render(h) {
        return (
            <Anchor level = {1}>
                <span>一级</span>标题
            </Anchor>
        )
    }
})
```
上面的代码无法直接运行，需要在webpack里配置插件babel-plugin-transform-vue-jsx编译后才可以。这里render使用了ES2015的语法缩写了函数。
常用的配置实例如下：
```javascript
render(creatElement){
    return createElement('div',{
        props: {
            text: 'some text'
        },
        attrs: {
            id: 'myDiv'
        },
        domProps: {
            innerHTML: 'content'
        },
        on: {
            change: this.changeHandler
        },
        nativeOn: {
            click: this.clickHandler
        },
        class: {
            show: true,
            on: false
        },
        style: {
            color: '#fff',
            background: '#f50'
        },
        key: 'key',
        ref: 'element',
        refInFor: true,
        slot: 'slot'
    })
}
```
上面的示例使用了JSX后等同于下面的代码：
```javascript
render(h){
    return (
        <div 
            id = "myDiv"
            domPropsInnerHTML = "content"
            onChange = {this.changeHandler}
            nativeOnClick = {this.clickHandler}
            class = {{show: true, on: false}}
            style = {{color: '#fff', background: '#f50'}}
            key = "key"
            ref = "element"
            refInFor
            slot = "slot">
        </div>
    )
}
```
:::warning 
JSX仍然四JavaScript而不是DOM，如果你的团队不是JSX强驱动的，建议还是以模板template的方式为主，特殊场景(比如锚点标题)使用Render的createElement辅助完成。
