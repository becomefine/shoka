---
title: 如何处理代码冲突
date: 2022-01-22 00:29:29
categories:
 - [计算机科学,软件技术,git]
tags: 
 - git
 - 分布式
 - 软件技术
 - 代码冲突
---
# 如何处理代码冲突
`代码冲突`

冲突合并一般是因为自己的本地做的提交和服务器上的提交有差异，并且这些差异中的文件改动，Git不能自动合并，那么就需要用户手动进行合并

如我这边执行`git pull origin master`

**如果Git能够自动合并，那么过程看起来是这样的**
![git](/assets/git/git_code_clash.gif)
拉取得时候，Git自动合并，并产生了一次提交。

**如果Git不能够自动合并，那么会提示**
![git2](/assets/git/git_code_clash2.jpg)
这个时候我们就可以知道`README.MD`有冲突，需要我们手动解决，修改`README.MD`解决冲突
![git3](/assets/git/git_code_clash3.png)
可以看出来，在1+1=几的这行代码上产生了冲突，解决冲突的目标是保留期望存在的代码，这里保留1+1=2，然后保存退出。
![git4](/assets/git/git_code_clash4.png)
退出之后，确保所有的冲突都得以解决，然后就可以使用
```bash
git add .
git commit -m "fixed conflicts"
git push origin master
```
即可完成一次冲突的合并。

整个过程看起来是这样的
![git5](/assets/git/git_code_clash5.gif)
