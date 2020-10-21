---
title: Linux执行.sh文件出现cd xxx\r ：No such file or directory
date: '2020/9/8 23:00:00'
categories:
  - Linux
tags:
  - Linux
  - shell
  - vim
abbrlink: 2343020999
---

# Linux执行.sh文件出现cd xxx\r ：No such file or directory
为了方便通常会在windows系统下创建好shell文件后上传到linux服务器里运行。运行后报错：No such file or directory（无此文件或目录）。大多是因为2个不同系统直接所用的文件格式不对应导致的。

来到Linux服务器端，输入以下指令查看文件格式。
```shell
cd File Directory   #“File Directory”为.sh文件所在的目录
vi File name.sh     #“File name”为报错的.sh文件名
```

在命令模式下，也就是刚进去时的界面输入：
```shell
:set ff   #查看文件格式
```
如返回值为 fileformat=dos ，输入以下指令把 "dos" 设置为 "unix" :
```shell
:set ff=unix   #把.sh文件格式设置为unix
：wq           #保存并退出vim编辑器
```
这时候在执行以下.sh文件就不会出问题了。
