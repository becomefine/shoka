---
title: Git是什么
date: 2022-01-17 23:42:54
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
---

# Git是什么
`git的来源` 

:::primary
[**:rocket:git简介**](/computer-science/software-technology/git/what-is-git) - [:love_letter:git的安装](/computer-science/software-technology/git/git-install)
:::

`Git`是`Linus Torvalds`为了帮助管理`Linux`内核开发而开发的一个开放源码的分布式版本控制系统

与常用的版本控制工具`CVS`,`Subversion`等不同，它采用了分布式版本库的方式，不必服务器端软件支持（注：这得分是用什么样的服务端，使用`http`协议或者`git`协议等不太一样。并且`push`和`pull`的时候和服务器端还是有交互的），使源代码的发布和交流及其方便。`Git`的速度很快，这对于诸如`Linux kernel`这样的大项目来说自然很重要。`Git`最为出色的是他的合并追踪（`merge tracing`）能力。

同生活中的许多伟大事件一样，`Git`诞生于一个极富纷争大举创新的年代。`Linux`内核开源项目有着为数众多众广的参与者。绝大多数`Linux`内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991-2002年间）。到2002年，整个项目组开始启用分布式版本控制系统`BitKeeper`来管理和维护代码。

到了2005年，开发`BitKeeper`的商业公司同`Linux`内核开源社区的合作关系结束，他们收回了免费使用`BitKeeper`的权力。这就迫使`Linux`开源社区（特别是`Linux`的缔造者`Linux Torvalds`）不得不吸取教训，只有开发一套属于自己的版本控制系统才不至于重蹈覆辙。他们对新的系统制定了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许上千个并行开发的分支）
- 完全分布式
- 有能力高效管理类似`Linux`内核一样的超大规模项目（速度和数据量）

自诞生于2005年以来，`Git`日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标。它的速度飞快，及其适合管理大项目，它还有着令人难以置信的非线性分支管理系统，可以应付各种复杂的项目开发需求。尽管最初`Git`的开发是为了辅助`Linux`内核开发的过程，但是我们已经发现在很多其他自由软件项目中也使用了`Git`。