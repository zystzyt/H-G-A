---
title: Linux下操作ln -s指令出现File exists的解决方法
date: '2020/8/6 11:08:33'
categories:
  - Linux
tags:
  - Linux
abbrlink: 3982553368
---

# 问题
操作ln -s给文件创建软链接时会出现以下情况，xxx文件以存在。
```shell
ln: failed to create symbolic link '/usr/bin/xxx': File exists
```
# 解决方法
```shell
rm -rf /usr/bin/xxx
```
再进行ln -s操作就解决了。