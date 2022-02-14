---
title: 如何在众多提交中保留个别提交
date: 2022-02-10 13:22:16
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
 - git commit
 - 合并提交
---

# 如何从众多提交中保留个别提交
`合并提交`
如果说在众多提交中,已某个提交为基准,只保留上游众多提交中的某个或者某几个,可以使用 cherry-pick命令,具体是:
```bash
git cherry-pick <commit id>
```
如果没有冲突，则回显示如下：
```bash
Finished one cherry-pick.
# On branch dev
# Your branch is ahead of 'origin/dev' by 3 commits.
```
如果存在冲突，则需要解决冲突然后继续，关于如何冲突，请查看如何处理代码冲突小节。