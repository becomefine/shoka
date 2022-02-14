---
title: Git仓库基本操作
date: 2022-01-20 14:50:42
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
 - git clone
---
# Git仓库基础操作
`git命令`
## 仓库基本管理
### 初始化一个Git仓库（以`/home/gitee/test`为例）
```bash
$ cd /home/gitee/test #进入git文件夹
$ git init            #初始化一个Git仓库
```
### 将文件添加到Git的暂存区
```bash
$ git add "readme.txt"
```
注：使用`git add -A`或`git add .`可以提交当前仓库的所有改动。
### 查看仓库当前文件提交状态（A：提交成功；AM：文件在添加到缓存之后又有改动）
```bash
$ git status -s
```
### 从Git的暂存区提交版本到仓库，参数`-m`后为档次提交的备注信息
```bash
$ git commit -m "1.0.0"
```
### 将本地的Git仓库信息推送上传到服务器
```bash
$ git push https://gitee.com/***/test.git
```
### 查看git提交的日志
```bash
$ git log
```
## 远程仓库管理
### 修改仓库名
一般来讲，默认情况下，在执行clone或者其他操作时，仓库名都是origin如果说我们想给他改改名字，比如我不喜欢origin这个名字，想改为oschina那么就要在仓库目录下执行命令：
```bash
git remote rename origin oschina
```
这样你的远程仓库名字就改成了oschina，同样，以后推送时执行的命令就不再是git push origin master而是git push oschina master，拉取也是一样的。
### 添加一个仓库
在不执行克隆操作时，如果想将一个远程仓库添加到本地的仓库中，可以执行
```bash
git remote add origin 仓库地址
```
[注意]{.label .warning}1.origin是你的仓库的别名，可以随便改，但请务必不要与已有的仓库别名冲突 2.仓库地址一般来讲支持http/https/ssh/git协议，其他协议地址请勿添加
### 查看当前仓库对应的远程仓库地址
```bash
git remote -v
```
这条命令能显示你当前仓库中已经添加了的仓库名和对应的仓库地址，通常来讲，会有两条一模一样的记录，分别是fetch和push，其中fetch是用来从远程同步 push是用来推送到远程。
### 修改仓库对应的远程仓库地址
```bash
git remote set -url origin 仓库地址
```