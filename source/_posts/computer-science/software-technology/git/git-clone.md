---
title: 如何通过git clone克隆仓库/项目
date: 2022-01-19 23:25:58
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
 - git clone
---

`仓库克隆`
# 如何通过git clone克隆仓库/项目
在前面我们介绍了Git支持多种数据传输协议，有`git://`协议、`http(s)://`和`user@server:/path.git`表示的SSH传输协议。我们可以通过这三种协议，对项目/仓库进行克隆操作。

下面，我们将以仓库`git@git.oschina.net:zxzllyj/sample-project.git`为例，对项目/仓库进行克隆。
## 通过HTTPS协议克隆
```bash
git clone https://gitee.com/zxzllyj/sample-project.git
```
## 通过SSH协议克隆
```bash
git clone git@gitee.com:zxzllyj/sample-project.git
```
以克隆仓库`git@gitee.com:zxzllyj/sample-project.git`为例（注：本处使用的是ssh地址，因为演示机已经配置好ssh公钥，故可以使用ssh地址，如果您没有配置公钥，请使用https地址）
![git-clone](/assets/git/git.gif)
注：上图的方法虽然将仓库完整的拉取了下来，但是仅仅只会显示默认分支，如果需要直接到指定的分支，可以在仓库地址后面加上分支名
