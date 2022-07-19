---
title: iview
date: 2022-04-03 11:08:12
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
---

# View UI

## 指南

### Layout布局

#### 设计规则

##### 尺寸

一级导航项偏左靠近 logo 放置，辅助菜单偏右放置。

- 顶部导航（大部分系统）：一级导航高度 `64px`，二级导航 `48px`。
- 顶部导航（展示类页面）：一级导航高度 `80px`，二级导航 `56px`。
- 顶部导航高度的范围计算公式为：`48+8n`。
- 侧边导航宽度的范围计算公式：`200+8n`。


##### 交互

- 一级导航和末级的导航需要在可视化的层面被强调出来；
- 当前项应该在呈现上优先级最高；
- 当导航收起的时候，当前项的样式自动赋予给它的上一个层级；
- 左侧导航栏的收放交互同时支持手风琴和全展开的样式，根据业务的要求进行适当的选择。

##### 视觉

导航样式上需要根据信息层级合理的选择样式：
- **大色块强调**
建议用于底色为深色系时，当前页面父级的导航项。
- **高亮火柴棍**
当导航栏底色为浅色系时使用，可用于当前页面对应导航项，建议尽量在导航路径的最终项使用。
- **字体高亮变色**
从可视化层面，字体高亮的视觉强化力度低于大色块，通常在当前的上一级使用。
- **字体放大**
`12px`、`14px`是导航的标准字号，14号字体用在一、二级导航中。字号可以考虑导航项的等级做相应选择。


#### 组件概述
- `Layout`: 布局容器，其下可嵌套`Header Sider Content Footer`或`Layout`本身，可以放在任何父容器中。
- `Header`: 顶部布局，自带默认样式，其下可嵌套任何元素，只能放在`Layout`中。
- `Sider`: 侧边栏，自带默认样式及基本功能，其下可嵌套任何元素，只能放在`Layout`中。
- `Content`: 内容部分，自带默认样式，其下可嵌套任何元素，只能放在`Layout`中。
- `Footer`: 底部布局，自带默认样式，其下可嵌套任何元素，只能放在`Layout`中。

## 组件

### 安装

### CDN引入

通过[unpkg.com/view-design](https://unpkg.com/view-design/)可以看到View UI的最新版本的资源，也可以切换版本选择需要的资源，在页面上引用js和css文件即可开始使用:
```html
<!-- import Vue.js -->
<script src="//unpkg.com/vue@2.6.14/dist/vue.min.js"></script>
<!-- import stylesheet -->
<link rel="stylesheet" href="//unpkg.com/view-design/dist/styles/iview.css">
<!-- import iView -->
<script src="//unpkg.com/view-design/dist/iview.min.js"></script>
```

示例:
```html
<!-- import Vue.js -->
<script src="//unpkg.com/vue@2.6.14/dist/vue.min.js"></script>
<!-- import stylesheet -->
<link rel="stylesheet" href="//unpkg.com/view-design/dist/styles/iview.css">
<!-- import iView -->
<script src="//unpkg.com/view-design/dist/iview.min.js"></script>
```

### 快速上手

- Vue组件