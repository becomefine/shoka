---
title: 初次运行Git前的配置
date: 2022-01-18 22:51:16
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
---
初次运行Git前的配置
=================
`Git配置` `git config` `git命令`

在新的系统上，我们一般都需要先配置下自己的 Git 工作环境。配置工作只需一次，以后升级时还会沿用现在的配置。当然，如果需要，你随时可以用相同的命令修改已有的配置。

Git提供了一个叫做`git config`的工具（译注：实际上是git-config命令，只不过可以通过git加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了Git在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

:::default no-icon
- /etc/gitconfig文件：系统中对所有用户都普遍使用的配置。若使用git confit时用--system选项，读写的就是这个文件。
- ~/.gitconfig文件：用户目录下的配置文件只适用于该用户。若使用git config时用--global选项，读写的就是这个文件。
- 当前仓库的Git目录中的配置文件（也就是工作目录中的.git/config）：这里的配置仅仅针对当前仓库有效。每一个级别的配置都会覆盖上层的相同配置，所以.git/config里的配置会覆盖/etc/gitconfig中的同名变量。
:::
**在windows系统上，Git会找寻用户主目录下的.gitconfig文件。主目录即$HOME变量指定的目录，一般都是`C:\Documents and Settings\$USER`。此外，Git还会尝试找寻/etc/gitconfig文件，只不过看当初Git装在什么目录，就以此作为根目录来定位**

用户信息配置
-----------
第一个要配置的是你个人的用户名称和电子邮件地址，这两条配置很重要，每次Git提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

```bash
$git config --global user.name "John Doe"
$git config --global user.email johndoe@example.com
```
如果用了--global选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有仓库都会默认使用这里配置的用户信息。如果要在某个特定的仓库中使用其他名字或者电邮，只要去掉--global选项重新配置即可，新的设定保存在当前仓库的.git/config文件里。

如果你是使用`https`进行仓库的推拉，你可能需要配置客户端记住密码，避免每次都输入密码
```bash
$git config --global credential.helper store
```
文本编辑器配置
------------
接下来要设置的是默认使用的文本编辑器。Git需要你输入一些额外消息的时候，会自动调用一个外部文本编辑器给你用。默认会使用操作系统制定的默认编辑器，一般可能会Vi或者Vim。如果你有其他偏好，比如Emacs的话，可以重新设置：
```bash
$git config --global core.editor emacs
```
差异分析工具
还有一个比较常用的是，在解决合并冲突时使用哪种差异分析工具。比如要改用vimdiff的话：
```bash
$git config --global merge.tool vimdiff
```
Git可以理解kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge, 和opendiff等合并工具的输出信息。当然，你也可以指定使用自己开发的工具。

查看配置信息
-----------
要检查已有的配置信息，可以使用git config --list命令：
```bash
$ git confit --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
···
```
有时候会看到重复的变量名，那就说明他们来自不同的配置文件（比如/etc/gitconfig和~/.gitconfig），不过最终Git实际采用的是最后一个。

也可以直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可，像这样：
```bash
$ git config user.name
scott Chacon
```
