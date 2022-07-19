---
title: 第六章：表单与v-model
date: 2022-02-20 02:21:50
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# 第六章：表单与v-model
表单类控件承载了一个网页数据的录入与交互。

## 基本用法
表单控件包括单选、多选、下拉选择、输入框等，Vue.js提供了v-model指令，用于在表单类元素上双向绑定数据。
```html 表单
<div id = "app">
    <input type = "text" v-model = "message" placeholder = "输入...">
    <p>输入内容是: message </p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        }
    })
<script>
```
:::info 
使用v-model后，表单控件显示的值只依赖所绑定的数据，不再关心初始化时的value属性，对于在<textarea></textarea>之间插入的值，也不会生效。
使用v-model时，如果是用中文输入法输入中文，一般在没有选定词组前，也就是在拼音阶段，Vue是不会更新数据的，当敲下汉字时才会触发更新。如果希望总是实时更新，可以用@input来代替v-model。事实上，v-model也是一个特殊的语法糖，只不过他会在不同的表单上智能处理。
:::

`1、单选按钮`:
单选按钮在单独使用时，不需要v-model，直接使用v-bind绑定一个布尔类型的值，为真时选中，为否时不选。
```html
<div id = "app">
    <input type = "radio" :checked = "picked">
    <label>单选按钮</label>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            picked: true
        }
    })
</script>
```

`2、复选框`:
复选框也分单独使用和组合使用，不过用法稍与单选不同。复选框单独使用时，也是用v-model来绑定一个布尔值，例如：
```html 复选框
<div id = "app">
    <input type = "checkbox" v-model = "check" id = "checked">
    <label for = "checked">选择状态: {{checked}} </label>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            checked: false
        }
    })
</script>
```

在勾选时，数据checked的值变为了true，label中渲染的内容也会更新。
组合使用时，也是v-model与value一起，多个勾选框都绑定到同一个数组类型的数据，value的值在数组当中，就会选中这一项。这一过程也是双向的，在勾选时，value的值也会自动push到这个数组中。
```html 示例
<div id = "#app">
    <input type = "checkbox" v-model = "checked" value = "html" id = "html" >
    <label for = "html">HTML</label>
    <br>
    <input type = "checkbox" v-model = "checked" value = "js" id = "js">
    <label for = "js">JavaScript</label>
    <br>
    <input type = "checkbox" v-model = "checked" value = "css" id = "css">
    <label for = "css">CSS</label>
    <br>
    <p>选择的项是: checked</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            checked: ['html','css']
        }
    })
</script>
```
`3、下拉框`:
```html 下拉框
<div id = "app">
    <select v-model = "selected">
        <option>html</option>
        <option value = "js">JavaScript</option>
        <option>css</option>
    </select>
    <p>选择的项是: selected</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            selected: 'html'
        }
    })
</script>
```
:::primary
<option>是备选项，如果含有value属性，v-model就会优先匹配value的值：如果没有，就会直接匹配<option>的text，比如选中第二项时，selected的值是js，而不是JavsScript。
给<select>添加属性multiple就可以多选了，此时v-model绑定的是一个数组，与复选框用法类似。
:::
```html 示例
<div id = "app">
    <select v-model = "select" multiple>
        <option>html</option>
        <option value = "js">JavaScript</option>
        <option>css</option>
    </select>
    <p>选择的项是: selected</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            selected: ['html','js']
        }
    })
</script>
```
:::info 
在业务中, <option>经常使用v-for动态输出，value和text也是用v-bind来动态输出的。
:::

```html 示例
<div id = "app">
    <select v-model = "selected">
        <option
            v-for = "option in options"
            :value = "option.value">{{option.text}}</option>
    </select>
    <p>选择的项是: selected</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            selected: 'html',
            option: [
                {
                    text: 'HTML',
                    value: 'html'
                },
                {
                    text: 'JavaScript',
                    value: 'js'
                },
                {
                    text: 'CSS',
                    value: 'css'
                }
            ]
        }
    })
</script>
```
:::warning 
虽然用选择列表<select>控件可以很简单的完成下拉选择的需求，但是在实际业务中反而不常用，因为它的样式依赖平台和浏览器，无法统一，也不太美观，功能也受限，不如不支持搜索，所以常见的解决方案是用div模拟一个类似的控件。
:::

## 绑定值
`v-model绑定的值是一个静态字符串或布尔值，但在业务中，有时需要绑定一个动态的数据，这时可以用v-bind实现`

`单选按钮`
```html 示例
<div id = "app">
    <input type = "radio" v-model = "picked" :value = "value">
    <label>单选按钮</label>
    <p>picked</p>
    <p>value</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            picked: false,
            value: 123
        }
    })
</script>
<!-- 在选中时，app.picked === app.value -->
```

`复选框`
```html 示例
<div id = "app">
    <input 
        type = "checkbox"
        v-model = "toggle"
        :true-value = value1"
        :false-value = value2">
    <label>复选框</label>
    <p>{{toggle}}</p>
    <p>{{value1}}</p>
    <p>{{value2}}</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            toggle: false,
            value1: 'a',
            value2: 'b'
        }
    })
</script>
<!-- 勾选时，app.toggle === app.value1; 未勾选时, app.toggle === app.value2 -->
```

`选中列表`
```html 示例
<div id = "app">
    <select v-model = "selected">
        <option :value = "{number: 123}">123 </option>
    </select>
    {{selected.number}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            selected: ''
        }
    })
</script>
<!-- 当选中时，app.selected是一个Object，所以app.selected.number === 123 -->
```

## 修饰符

- .laze:
在输入框中，v-model默认是在input事件中同步输入框的数据(除了提示中介绍的中文输入法情况外)，使用修饰符.lazy会转变为在change事件中同步。
- .number:
使用修饰符.number可以将输入转换为Nubmer类型，否则虽然你输入的是数字，但它的类型其实是String，比如在数字输入框时会比较有用。
- .trim:
修饰符.trim可以自动过滤输入的首尾空格。

