---
title: 第八章 自定义指令
date: 2022-03-06 13:18:36
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 自定义指令

## 基本用法
自定义指令的注册方法和组件很像，也分全局注册和局部注册，比如注册一个v-focus的指令，用于在<input>、<textarea>元素初始化时自动获得焦点。
```javascript v-focus
//全局注册
Vue.directive('focus',{
    //指令选项
});
//局部注册
var app = new Vue({
    el: '#app',
    directives: {
        focus: {
            //指令选项
        }
    }
})
```
自定义指令的各个选项
- bind: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
- inserted: 被绑定元素插入父节点时调用(父节点存在即可调用，不必存在于document中)。
- update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
- componentUpdated: 被绑定元素所在模板完成一次更新周期时调用。
- unbind: 只调用一次，指令与元素解绑时调用。

可以根据需求在不同的钩子函数内完成逻辑代码，例如上面的v-focus，我们希望在元素插入父节点时就调用，那用到的最好是inserted。
```html
<div id = "app">
    <input type = "text" v-focus>
</div>
<script>
    Vue.directive('focus',{
        inserted: function(el){
            el.focus();
        }
    });

    var app = new Vue({
        el: '#app'
    })
</script>
```
打开这个页面，input输入框就自动获得了焦点，成为可输入状态。每个钩子函数都有几个参数可用。
- el 指令所绑定的元素，可以用来直接操作DOM。
- binding 一个对象，包含以下属性：
    + name 指令名，不包括v-前缀
    + value 指令的绑定值，例如v-my-directive = “1+1”，value的值是2.
    + oldValue 指令绑定的前一个值，仅在update和componentUpdated钩子中可用，无论值是否改变都可用。
    + expression 绑定值的字符串形式。例如：v-my-directive = "1 + 1"，expression的值是"1+1"。
    + arg 传给指令的参数。例如 v-my-directive:foo，arg的值是foo。
    + modifiers 一个包含修饰符的对象。例如v-my-directive:foo.bar, 修饰符对象modifiers的值是{foo: true, bar: true}。
- vnode Vue编译生成的虚拟节点，在进阶篇介绍。
- oldVnode 上一个虚拟节点仅在update和componentUpdated钩子中可用。

```html 
<div id = "app">
    <div v-test:msg.a.b="message"></div>
</div>
<script>
    Vue.directive('test,{
        bind: function(el, binding, vnode){
            var keys = [];
            for (var i in vnode){
                keys.push(i);
            } 
            el.innerHTML = 
                'name: ' + binding.name +'<br>' +
                'value: ' +binding.value+'<br>' +
                'expression: '+binding.expression +'<br>' +
                'argument: '+binding.arg + '<br>' +
                'modifiers: ' + JSON.stringify(binding.modifiers) + '<br>' +
                'vonde keys: ' +keys.join(', ')
        }
    });
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'some text'
        }
    })
</script>
```

