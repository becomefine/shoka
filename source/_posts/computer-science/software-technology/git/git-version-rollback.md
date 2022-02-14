---
title: 如何进行版本回退
date: 2022-01-24 00:01:45
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
---
# 如何进行版本回退
`版本回退`
## 版本回退由多种方式，下面一一演示：
### 回退到当前版本（放弃所有修改）
![git-version](/assets/git/git_version_rollback1.gif)
### 放弃某一个文件的修改
![git-version](/assets/git/git_version_rollback2.gif)
### 回退到某一版本号但保存自该版本起的修改
![git-version](/assets/git/git_version_rollback3.gif)
### 回退到某一版本并且放弃所有的修改
![git-version](/assets/git/git_version_rollback4.gif)
### 回退到远程仓库的版本
先在本地切换到远程仓库要回退的分支对应的本地分支，然后本地回退至你需要的版本，然后执行：
```bash
git push <仓库名> <分支名> -f
```
### 如何以当前版本为基础，回退指定个commit
首先，确认你当前的版本需要回退多少个版本，然后计算出你要回退的版本数量，执行如下命令
```bash
git reset HEAD~X //X代表你要回退的版本数量，是数字！！！！
```
需要注意的是，如果你是合并过分支，那么被合并分支带过来的commit并不会被记入回退数量中，而是只计算一个，所以如果需要一次回退多个commit，不建议使用这种方法
### 如何回退到和远程版本洋
有时候，当发生错误修改需要放弃全部修改时，可以以远程分支作为回退点回退到与远程分支一样的地方，执行的命令如下
```bash
git reset --hard origin/master //origin代表你远程仓库的名字，master代表分支名

