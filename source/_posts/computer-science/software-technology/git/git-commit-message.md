---
title: Git Commit msssage 编写指南
date: 2022-01-21 23:40:41
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
 - git commit
---
# Git Commit message编写指南
`git commit`
## 介绍
在Git中，每次提交代码，都要写Commit message（提交说明），否则就不允许提交。这个操作将通过git commit完成。
```bash
git commit -m "hello world"
```
:::default no-icon
上面的-m参数，就是用来指定commit message的。
:::
如果一行不够，可以只执行git commit，就会跳出文本编译器，让你写多行
```bash
git commit
```
## 格式
Commit message包括三个部分：Header,Body和Footer。可以用下方的格式表示它的结构。
```bash
<type>(<scope>):<subject>// 空一行<body>//空一行<footer>
```
:::default no-icon
其中，Header是必需的，Body和Footer可以省略（默认省略），一般我们在`git commit`提交时指定的`-m`参数，就相当于默认指定Header。
:::
:::default no-icon
不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。
:::
## Header
Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。
**type**
- feat: 新功能(feater)
- fix: 修补bug
- docs: 文档(documentation)
- style: 格式(不影响代码运行的变动)
- refactor: 重构(即不是新增功能，也不是修改bug的代码变动)
- test: 增加测试
- chore: 构建过程或辅助工具的变动
如果type为feat和fix，则该commit将肯定出现在Change log之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入Change log，建议是不要。
**scope**
scope用于说明commit影响的范围，比如数据层、控制层、视图层等等，视仓库不同而不同。
**subject**
subject是commit目的的简短描述，不超过50个字符。
- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号(.)
## **body**
Body部分是对本次commit的详细描述，可以分成多行。下面是一个范例。
:::default no-icom
More detailed explanatory text, if necessary. Wrap it to about 72 characters or so. Further paragraphs come after blank lines. Bullet points are okay, too- User a hanging indent
有两个注意点。
- 使用第一人称现在时，比如使用过change而不是changes。
- 应该说明代码变动的动机，以及与以前行为的对比。
## **Footer**
Footer部分只用于两种情况。
### 1、不兼容变动
如果当前代码与上一个版本不兼容，则Footer部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。
```bash
BREAKING CHANGE: isolate scope bindings definition has changed.
    To migrate the code follow the example below:

    Before:

    scope: {
        myAttr: 'attribute',
    }

    After:

    scope:{
        myAttr: '@',
    }

    The removed 'inject' wasn't generaly useful for directives so there should be no code using it.
```
### 2、**关闭Issue**
如果当前commit针对某个issue，那么可以在Footer部分关闭这个issue。
:::default no-icon
Closes #234
:::
也可以一次关闭多个issue。
:::default no-icon
Closes #123,#245,#992
:::
## Revert
还有一种特殊情况，如果当前commit用于撤销以前的commit，则必须以revert开头，后面跟着被撤销Commit的Header。
:::default no-icon
revert: feat(pencil): add 'graphiteWidth' option
This reverts commit 677ecc1654a317a13331b17617d973392f415f02.
:::
Body部分的格式是固定的，必须写成This reverts commit，其中的hash是被撤销commit的SHA标识符。

如果当前commit与被撤销的commit，在同一个发布(release)里面，那么他们都不会出现在Change log里面。如果两者在不同的发布，那么当前commit，会出现在Change log的Reverts小标题下面。

`本文摘自 阮一峰 博客 《Git Commit message编写指南》`
