---
title: windows Terminal终端美化
date: 2022-04-12 00:01:54
categories:
 - [二进制杂谈]
tags: 
 - windows
 - terminal
 - 终端美化
---

# Windows Terminal终端美化

## 安装软件

微软商店搜索[termianl]{.purple}
![terminal](/assets/note/windows-terminal-beautify/windows_terminal1.jpg)

## 安装主题

:::info no-icon
[使用管理员身份]{.purple}打开Windows Terminal。安装[oh-my-posh]{.purple}和[posh-git]{.purple}
:::

- 第一条命令(绕过power shell执行策略, 使其可以执行脚本文件<后面会用到>)
```shell
Set-ExecutionPolicy Bypass
```

- 第二条命令(oh-my-posh)提供主题
```shell
Install-Module on-my-posh -Scope CurrentUser
```

- 第三条命令(posh-git将git信息添加到提示中)
```shell
Install-Module posh-git -Scope CurrentUser
```
:::info no-icon
如果中途有提示，直接Y就好了。
:::

## 编辑配置文件

:::info no-icon
在Windows Terminal中敲下面两行命令
:::

- 第一条(启动编辑power shell)配置文件的引擎
```shell
if(!(Test-Path -Path $PROFILE)) { New-Item -Type File -Path $PROFILE -Force }
```

- 第二条(使用记事本打开配置文件)
```shell
notepad $PROFILE
```

:::info no-icon
[在打开的记事本中写入如下内容]{.purple}(脚本文件), 并保存
:::

```shell
Import-Module posh-git
Import-Module on-my-posh
Set-PoshPrompt -Theme honukai
```

- 第一条命令表示导入posh-git
- 第二条命令表示导入oh-my-posh
- 第三条命令表示设置主题为honukai

:::info no-icon
如果想要更换主题，输入[Get-PoshThemes]{.purple}命令查看所有主题，并将上述配置文件中主题名替换即可。
:::

![terminal](/assets/note/windows-terminal-beautify/windows_terminal2.jpg)
:::warning
此时终端出现乱码很正常，我们还需要下一步配置合适的字体
:::

## 安装适配字体

:::info no-icon
进入下面网站下载字体，注意查看字体名字。(推荐[DejaVuSansMonoNerd Font]{.purple}或[Cousine Nerd Font]{.purple}, 这两套字体比较全，适配也还不错)。
:::
https://www.nerdfonts.com/

![terminal](/assets/note/windows-terminal-beautify/windows_terminal3.jpg)

![terminal](/assets/note/windows-terminal-beautify/windows_terminal4.jpg)

:::info no-icon
下载完成后解压，全选[右键安装]{.purple}字体
:::

![terminal](/assets/note/windows-terminal-beautify/windows_terminal5.jpg)

:::info no-icon
打开terminal, 并在上方[标签栏]{.purple}点击下拉按钮找到[设置]{.purple}, 并点击，然后在右侧最下方点击打开JSON文件。
:::

![terminal](/assets/note/windows-terminal-beautify/windows_terminal6.jpg)
![terminal](/assets/note/windows-terminal-beautify/windows_terminal7.jpg)

:::info no-icon
这个配置文件最开始几行表示的是[架构]{.purple}和[默认配置]{.purple}。下面几行有3个包含着字典的列表，分别表示[快捷键(keybindings)、配置(profiles)、配色方案(schemes)]{.purple}
。我们需要设置美化power shell，故找到字典中包含:
:::

```shell
"guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}"
```

:::info no-icon
找到后，将其中键为"fontFace"的键值对改为(如果没有fontFace就自己添加一下，放在guid一行，记得加逗号):
:::

![terminal](/assets/note/windows-terminal-beautify/windows_terminal8.jpg)

