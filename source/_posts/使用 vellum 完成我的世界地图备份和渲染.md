---
title: 使用 vellum 完成我的世界地图备份和渲染网页地图
date: '2020/9/12 13:13:00'
categories:
  - Minecraft
tags:
  - Minecraft
  - vellum
abbrlink: 2091446885
---

# 使用 vellum 完成我的世界地图备份和渲染
## 安装 vellum
### 下载
浏览器打开vellum仓库
```
https://github.com/clarkx86/vellum/releases
```
找到 *vellum_linux-x64-bundled_v目前版本.zip* 鼠标右键复制链接地址，回到服务器输入以下指令：
 ```shell
mkdir vellum; \
cd vellum; \
wget https://github.com/clarkx86/vellum/releases/download/v1.2.3/vellum_linux-x64-bundled_v目前版本.zip; \
unzip vellum_linux-x64-bundled_v目前版本.zip; \
chmod +x ./vellum; \
./vellum
 ```
 
 ### 配置
 
vellum目录会生成 *configuration.json* 的文件，这是我们要配置的。下面是每一项的描述：
```
KEY               VALUE               ABOUT
----------------------------------------------------------
必要设置
-----------------
BdsBinPath         String  (!)        基岩服务器启动文件路径，比如（
									  Linux的"/../../bedrock_server"
									  Windows的 "/../../bedrock_server.exe"）

WorldName          String  (!)        要备份的世界名字，在 /worlds/里
                                      填写的是世界的名字不是路径
---------------
备份设置
---------------
EnableBackups      Boolean (!)        备份文件是否保存为 .zip 格式。

BackupInterval     Double             距离上次备份的间隔时间，以“分钟”为单位。

ArchivePath        String             创建备份的保存路径。

BackupsToKeep      Integer            保留在“ArchivePath”也就是备份路径里的备份数量，
									  超过这个数量会把旧备份删除掉.
									  
OnActiviyOnly      Boolean            如果设置为“true”，vellum只会在上次备份后至少有
									  一个玩家连接的情况下执行备份，以便只存档实际修改过的世界。

StopBeforeBackup   Boolean            是否停止服务器进行备份，备份完成后自动重启服务器
                                      而不是进行热备份。

NotifyBeforeStop   Integer            停止服务器进行备份之前，在游戏聊天窗口广播服务器停
									  止倒计时，以秒为单位。

BackupOnStartup    Boolean            是否在服务器启动之前进行备份，强烈建议将此设置
                                      保留为“ true”。

PreExec            String             每次备份开始前执行的指令，可选。

PostExec           String             每次备份结束后执行的指令，可选。
---------------
渲染世界地图设置
---------------
EnableRenders      Boolean (!)        是否使用 “PapyrusCS” 渲染世界地图。

PapyrusBinPath     String             PapyrusCS所在文件目录，比如（
                                      Linux的"/../../PapyrusCs" 
                                      Windows的"/../../PapyrusCs.exe"）。

PapyrusOutputPath  String             渲染后世界地图文件输出路径。

RenderInterval     Double             距离上次渲染的间隔世界，以分钟为单位。

PapyrusGlobalArgs  String             执行渲染的全局参数，请勿更改已经提供
                                      的--world和--ouput参数                                      
                                      详细参数详解参考以下链接：
                                      https://github.com/mjungnickel18/papyruscs
                                      
PapyrusTasks       String [Array]     papyrus 的附加参数数组，其中每个数组
									  条目在前一个完成后会执行另一个PapyrusCS过程
									  （例如，用于渲染多个尺寸）
-------------------
其他设置
-------------------
QuietMode          Boolean (!)        禁止通知游戏中的玩家*vellum*正在创建备份和渲染

HideStdout         Boolean (!)        是否隐藏PapyrusCS渲染过程生成的控制台输出，
									  将其设置为“ true”可能有助于调试您的配置。

BusyCommands       Boolean (!)        在vellum进行备份时是否允许执行BDS命令

CheckForUpdates    Boolean (!)        是否在启动时检查更新

StopBdsOnException Boolean (!)        如果* vellum *运行异常而意外崩溃，
									  是否向BDS进程发送“stop”命令，
									  以防止其继续以分离模式运行。
----------------------------------------------------------
*标有（！）的值为必填项
```
#### 配置示例
```json
{
  "BdsBinPath": "/root/mc/bedrock_server",
  "WorldName": "Bedrock level",
  "Backups": {
    "EnableBackups": true,
    "BackupInterval": 60.0,
    "ArchivePath": "./backupsmc/",
    "BackupsToKeep": 10,
    "OnActivityOnly": false,
    "StopBeforeBackup": false,
    "NotifyBeforeStop": 60,
    "BackupOnStartup": true,
    "PreExec": "",
    "PostExec": ""
  },
  "Renders": {
    "EnableRenders": true,
    "PapyrusBinPath": "/root/papyruscs/PapyrusCs",
    "PapyrusOutputPath": "constructmc",
    "RenderInterval": 60.0,
    "PapyrusGlobalArgs": "-w $WORLD_PATH -o $OUTPUT_PATH --htmlfile index.html -f png -q 100 --deleteexistingupdatefolder",
    "PapyrusTasks": [
      "--dim 0"
    ]
  },
  "QuietMode": false,
  "HideStdout": true,
  "BusyCommands": true,
  "CheckForUpdates": true,
  "StopBdsOnException": true
}
```
### 运行
configuration.json配置完成后运行vellum
 ```shell
 ./vellum
 ```
 ## 参数与命令
 ### 参数
 可用启动参数概述：
```
PARAMETER                             ABOUT
----------------------------------------------------------
-c | --configuration                  指定自定义配置文件。 
										 （默认：configuration.json）

-h | --help                           打印可用参数的概述。
```
参数是可选的，如果未指定，则默认为默认值
 ### 指令
 vellum还提供了一些新功能，并重载了一些现有命令，您可以执行这些命令来强制调用备份或渲染任务并计划服务器关闭。
```
 COMMAND                               ABOUT
----------------------------------------------------------
force start backup                    强制执行备份

force start render                    强制PapyrusCS渲染世界

stop <time in seconds>                安排服务器关闭并在游戏中通知玩家

reload vellum                         重新加载先前指定的（或默认的）配置文件

updatecheck                           获取最新的BDS和vellum版本并将其显示在控制台中
```