---
title: Linux修改文件权限
date: 2022-03-21 09:14:48
categories:
 - [计算机科学,OS,Linux]
tags: 
 - 操作系统
 - Linux
 - Linux文件权限
---

# Linux chmod

## 更改文件属性

### chgrp: 更改文件属组

语法:
:::default no-icon
chgrp [-R] 属性名 文件名
:::

参数选项
- -R: 递归更改文件数组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属性都会更改。

### chown: 更改文件属主，也可以同时更改文件属组

语法:
:::default no-icon
chown [-R] 属组名 文件名
chown [-R] 属主名: 属组名 文件名
:::

进入/root目录(~)将install.log的拥有者改为bin这个账号
```shell
[root@www ~] cd ~
[root@www ~]# chown bin install.log
[root@www ~]# ls -1
-rw-r--r-- 1 root root 68495 Jun 25 08:53 install.log
```

### chmod: 更改文件9个属性

Linux文件属性有两种设置方法，一种是数字，一种是符号。
Linux文件的基本权限就有九个，分别是`owner/group/others(拥有者/组/其他)`三种身份各有自己的`read/write/execute`权限。
文件的权限字符为: `-rwxrwxrwx`, 这九个权限是三个三个一组的! 其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下:
- r: 4
- w: 2
- x: 1

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为: `-rwxrwx---`分数则是:
- owner = rwx = 4+2+1 = 7
- group = rwx = 4+2+1 = 7
- others = --- = 0+0+0 = 0

所以等一下我们设定权限的变更时，该文件的权限数字就是770.变更权限的指令的指令chmod的语法是这样的:
:::default no-icon
chmod [-R] xyz 文件或目录
:::

选项与参数:
- xyz: 就是刚刚提到的数字类型的权限属性，为 **rwx**属性数值的相加。
- -R: 进行递归(recursive)的持续变更，以及连同此目录下的所有文件都会变更

举例: 如果要将 `.bashrc`这个文件所有的权限都设定启用:
```shell
[root@www ~]# ls -al .bashrc
-rw-r--r-- 1 root root 395 Jul 4 11:45 .bashrc
[root@www ~]# chmod 777 .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx 1 root root 395 Jul 4 11:45 .bashrc
```

如果要将权限变成-rwxr-xr--呢？那么权限的分数就变成了[4+2+1][4+0+1][4+0+0]=754.

### 符号类型修改文件权限

还有一个改变权限的方法，基本上都九个权限分别为:
- user: 用户
- group: 组
- others: 其他

我们可以用 **u, g, o**来代表三种身份的权限。
**a**表示**all**, 即全部的身份。读写的权限可以写成`r, w, x`, 也就是可以使用下表的方式进行查看
|指令|||||
|:--|:-|:-|:-|:-|
|chmod| u g o a|+(加入) -(除去) =(设定)| r w x| 文件或目录|

```shell 
#  touch test1    // 创建 test1 文件
# ls -al test1    // 查看 test1 默认权限
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
# chmod u=rwx,g=rx,o=r  test1    // 修改 test1 权限
# ls -al test1
-rwxr-xr-- 1 root root 0 Nov 15 10:32 test1
```

如果要将权限去掉而不改变其他已存在的权限？例如要拿掉全部人的可执行权限。
```shell
#  chmod  a-x test1
# ls -al test1
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
```
