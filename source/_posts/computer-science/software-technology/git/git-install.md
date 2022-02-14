---
title: Git的安装
date: 2022-01-18 13:27:42
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
---

# Git的安装
最早Git是在Linux上开发的，很长一段时间内，Git只能在Linux/Unix系统上运行。随着Git的使用逐渐普及，一些开发者也慢慢将Git移植到了Windows平台上。目前Git已经发展为可以在Windows/macOS/Linux/Unix上运行的扩平台工具。

## 下载
你可以在[https://git-scm.com/](https://git-scm.com/)获得Git在Windows/macOS/Linux三个操作系统相关的安装包。也可以通过以下方式安装。

## `Windows`下的安装
从[http://git-scm.com/download](http://git-scm.com/download)上下载window版的客户端，以管理员身份运行后，一直选择下一步安装即可，请注意，如果你不熟悉每个选项的意思，请保持默认的选项。

## `Ubuntu`下的安装
在终端执行
```bash
apt-get install git
```

## `Centos/Redhat`安装
在终端下执行
```bash
yum install git
```

## `Fedora23`安装
在终端执行
```bash
dnf install git 或 yum install git
```

## `Fedora22/21`安装
在终端执行
```bash
yum install git
```

## `SUSE/OPENSUSE`安装
在终端执行
```bash
sudo zypper install git
```

## `Mac OS X`安装
在终端执行
```bash
brew install git
```
## 编译安装（注：仅适合非`window`系统）
在[https://github.com/git/git/releases](https://github.com/git/git/releases)上选取一个版本下载，解压缩后进入到Git的目录然后一次执行一下代码：
```
make configure
./configure
make all
sudo make install
```
[注意]{.label .warning}：如果遇上无法编译的问题，请自行通过搜索引擎来查找Git所需的依赖

如果以上一切正常，打开终端（Window下打开安装git时一并安装的bash）输入`git --version`应该会显示如下类似的信息
`git version 2.5.0`



