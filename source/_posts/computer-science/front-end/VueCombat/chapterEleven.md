---
title: '第十一章: 插件'
date: 2022-03-28 22:46:05
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 插件

插件可以在全局添加一些功能，他们可以简单到几个方法、属性，也可以很复杂，比如一整套组件库。
注册插件需要一个公开的方法install，第一个参数是Vue构造器，第二个参数是一个可选的选项对象。
```javascript 注册插件
MyPlugin.install = function(Vue, option){
    //全局注册插件(指令等功能资源类似)
    Vue.component('component-name', {
        //组件内容
    })
    //添加实例方法
    Vue.prototype.$Notice = function(){
        //逻辑...
    }
    //添加全局混合
    Vue.mixin({
        mounted: function(){
            //逻辑...
        }
    })
}
```
```javascript 使用插件
Vue.use(MyPlugin)
//或
Vue.use(MyPlugin, {
    //参数
})
```
:::info 
绝大多数情况下，开发插件主要是通过NPM发布后给别人使用的，在自己的项目中可以直接在入口调用以上的方法，无需多一步注册和使用的步骤。
:::

### 前端路由与vue-router

#### 前端路由

单页面富应用(SPA)的核心就是前端路由。路由通俗来讲就是网址，专业来讲，就是每次GET或者POST等请求在服务端有一个专门的正则配置列表，然后匹配到具体的一条路径后，分发到不同的Controller，进行各种操作，最终将html或数据返回到前端，这就完成了一次IO。
目前绝大多数的网站都是这种后端路由，也就是多页面的，好处是服务端渲染好后直接返回给浏览器，不用等待前端加载任何js和css就可以直接显示网页内容，且对SEO的友好。缺点是模板是由后端来维护或改写，前端开发者需要安装整套的后端服务，必要时还得学习向PHP或Java这些非前端语言来改写html结构，所以html和数据、逻辑混为一谈，维护起来麻烦。
SPA就是在前后端分离的基础上加一层前端路由。
前端路由由前端维护一个路由规则，实现方式由两种，一种是利用url和hash，就是锚点(#), Javascript通过hashChange事件来监听url的改变；另一种就是HTML5的History模式，它使url看起来像普通网站一样，以"/"分割，没有#，但页面并没有跳转，不过使用这种模式需要服务端支持，服务端在接收到所有请求后，都指向同一个html文件，不然会出现404，因此，SPA只有一个html，整个网站所有的内容都在这一个html里，通过JavaScript来处理。
**前端路由的优点**:
- 页面持久性，像大部分音乐网站，可以在播放歌曲的同时跳转到别的页面，而音乐没有中断。
- 前后端彻底分离。
前端路由的框架通用的由`Director`, Angular的`ngRouter`, React的`ReactRouter`, 以及Vue的`vue-router`。

### vue-router基本用法

新建一个目录router, 通过NPM安装vue-router
```bash
npm install --save vue-router
```
在main.js里使用Vue.use()加载插件:
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';
import App from './app.vue';

Vue.use(VueRouter);
```

每个页面对应一个组件，也就是对应一个.vue组件。在router目录下创建views目录，用于存放所有的页面，然后在views里创建index.vue和about.vue两个文件；
```vue index.vue
<template>
    <div>首页</div>
</template>
<script>
    export default {

    }
</script>
```
```vue index.vue
<template>
    <div>介绍页</div>
</template>
<script>
    export default {

    }
</script>
```
在`main.js`里，完成路由的剩余配置。创建一个数组来指定路由匹配列表，每一个路由映射一个组件:
```javascript
const Routers = [
    {
        path: '/index',
        component: (resolve) => require(['./views/index.vue'], resolve)
    },
    {
        path: '/about',
        component: (resolve) => require(['./views/about.vue'],resolve)
    }
];
```

`Routers`里每一项的path属性就是指定当前匹配的路径，`component`是映射的组件。`webpack`会把每一个路由都打包为一个js文件，在请求到该页面时，才去加载这个页面的js，也就是异步实现的懒加载(按需加载)。
:::info no-icon
使用了异步路由后，编译出的每个页面的js都叫做chunk(块)，默认命名是0.main.js、1.main.js······可以在webpack配置的出口output里通过设置chunkFilename字段修改chunk命名，
```javascript
output: {
    publicPath: '/dist/',
    filename: '[name].js',
    chunkFilename: '[name].chunk.js'
}
```
有了chunk后，在每个页面(.vue文件)里写的样式也需要配置后才会打包进main.css，否则仍然会通过JavaScript动态创建`<style>`标签的形式写入。配置插件:
```javascript
plugins: [
    new ExtractTextPlugin({
        filename: '[name].css',
        allChunks: true
    })
]
```
:::

在main.js里完成配置和路由:
```javascript
const RouterConfig = {
    mode: 'history',
    routes: Routers
};
const router = new VueRouter(RouterConfig);

new Vue({
    el: '#app',
    router: router,
    render: h => {
        return h(App)
    }
});
```

:::primary no-icon
在RouterConfig里，设置mode为history会开启HTML5的Histroy路由模式，通过"/"设置路径，如果不配置mode，就会使用"#"来设置路径。开启History路由，在生产环境时服务端必须进行配置，将所有路由都指向同一个html，或设置404页面为该html，否则刷新时页面会出现404.
:::

webpack-dev-server也要配置下来支持History路由，在package.json中修改dev命令:
```json
"script" {
    "dev": "webpack-dev-server --open --history-api-fallback --config webpack.config.js"
}
```
增加了`--history-api-fallback`, 所有的路由都会指向`index.html`。

最后在根实例`app.vue`里添加一个路由视图`<router-view>`来挂载所有的路由组件:
```vue
<template>
    <div>
        <router-view></router-view>
    </div>
</template>
<script>
    export default{

    }
</script>
```
运行网页时，`<router-view>`会根据当前路由动态渲染不同的页面组件。网页中一些公共部分，比如顶部的导航栏、侧边导航栏、底部的版权信息，这些也可以直接写在`app.vue`里，与`<router-view>`同级。路由切换时，切换的是`<router-view>`挂载的组件，其他的内容不会变化。

### 跳转

`vue-router`有两种跳转页面的方法，第一种是使用内置的`<router-link>`组件，它会被渲染成为一个`<a>`标签:
```vue index.vue
<template>
    <div>
        <h1>首页</h1>
        <router-link to="/about">跳转到 about</router-link>
    </div>
</template>
```

它的用法与一般的组件一样，`to`是一个`prop`，指定需要跳转的路径，当然也可以用`v-bind`动态设置。使用`<router-link>`，在HTML5的History模式下会拦截点击，避免浏览器重新加载页面。
`<router-link>`的prop：
- tag: tag可以指定渲染成什么标签，比如<router-link to="about" tag="li">渲染的结果就是<li>而不是<a>。
- replace: 使用replace不会留下History记录，所以导航后不能用后退键返回上一个页面，如<router-link to="/about" replace>
- active-class: 当<router-link>对应的路由匹配成功时，会自动给当前元素设置一个名为`router-link-active`的class，设置prop: active-class可以修改默认的名称。在做类似导航栏时，可以使用该功能高亮显示当前页面的导航菜单栏，但是一般不会修改`active-class`，直接使用默认值router-link-active就可以。

有时候，跳转页面可能需要在JavaScript里进行，类似于window.location.href。这时可以用第二种跳转方法，使用router实例的方法。比如在about.vue里，通过点击时间跳转:
```vue
<template>
    <div>
        <h1>介绍页<h1>
        <button @click="handleRouter">跳转到user</button>
    </div>
</template>
<script>
    export default {
        methods: {
            handleRouter(){
                this.$router.push('/user/123');
            }
        }
    }
</script>
```

$router还有其他一些方法:
- replace 
类似于<router-link>的replace功能，他不会向history添加新纪录，而是替换掉当前的history记录，如this.$router.replace('/user/123');。
- go
类似于window.history.go()。在history记录中向前或者后退多少布，参数是整数，例如:
```javascript
this.$router.go(-1);
this.$router.go(2);
```

### 高级用法

可以通过JavaScript修改<title>的内容：
```javascript
window.document.title = '要修改的网页标题';
```

在Vue工程里，当网页发生路由改变时，统一设置。`vue-router`提供了导航钩子`beforeEach`和`afterEach`，他们会在路由即将改变前和改变后触发，所以设置标题可以在beforeEach钩子完成。

```javascript main.js
const Routers = [
    {
        path: '/index',
        meta: {
            title: '首页'
        },
        component: (resolve) => require(['./views/index.vue'], resolve)
    },
    {
        path: '/about',
        meta: {
            title: '关于'
        },
        component: (resolve) => require(['./views/about.vue'], resolve)
    },
    {
        path: '/user/:id',
        meta: {
            title: '个人主页'
        },
        component: (resolve) => require(['./views/user.vue'], resolve)
    },
    {
        path: '*',
        redirect: '/index'
    }
];

const router = new VueRouter(RouterConfig);
router.beforeEach((to, from, next) => {
    window.document.title = to.meta.title;
    next();
});
```

导航钩子有3个参数:
- to 即将要进入的目标的路由对象。
- from 当前导航即将要离开的路由对象。
- next 调用该方法后，才能进入下一个钩子。

路由列表的meta字段可以自定义一些信息，比如我们将每个页面的title写入了meta来统一维护，beforeEach钩子可以从路由对象to里获取meta信息，从而改变标题。

当一个页面较长，滚动到某个位置，在跳转到另一个页面，滚动条默认是在上一个页面停留的位置，而好的体验是返回顶端。
通过钩子afterEach可以实现:
```javascript main.js
router.afterEach((to, form, next) => {
    window.scrollTo(0, 0);
})
```
登录访问，否则跳转到登录页:
```javascript
router.beforeEach((to, from, next) => {
    if(window.localStorage.getItem('token')){
        next();
    }else{
        next('/login');
    }
});
```

next()的参数设置为false时，可以取消导航，设置为具体的路径可以导航到指定的页面。


## 状态管理与Vuex

### 状态管理与使用场景

vuex解决非父子组件(跨级组件和兄弟组件)通信的问题，作为一个插件来更好的管理和维护整个项目的组件状态。

一个组件可以分为数据(model)和视图(view)，数据更新时，视图也会自动更新。在视图中又可以绑定一些事件，他们触发methods里指定的方法，从而可以改变数据，更新视图，这是一个组件基本的运行模式。
```vue message.vue
<template>
    <div>
        {{message}}
    <button @click="handleClick">Change word</button>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                message: 'Hello World,'
            }
        },
        methods: {
            handleClick(){
                this.messsage = 'Hello Vue.';
            }
        }
    };
</script>
```
这里的数据message和方法handleClick只有在message.vue组件里可以访问和使用，其他的组件无法读取和修改message。
因此Vues的设计就是用来统一管理状态的，定义了一系列规范来使用和操作数据，使组件应用更加高效。
Vues的主要使用场景是大型单页应用，更适合多人协同开发。

### Vuex基本用法

