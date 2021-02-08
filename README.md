# Doge-Persistence-Platform
本项目短期内不开源
## Intro

Doge-Persistence-Platform

DPP windows控制持久化辅助平台

在红蓝对抗/APT/高级渗透测试中，持久化控制已成为一个不可或缺的能力，

虽然，Cobalt Strike与MSF已经在一定程度上满足需求，

由于其具有较强特征，功能过剩，二开麻烦等原因，

从零开始完成了本项目

```
注: 本项目在完成过程中未参考任何项目源码

注: 本项目总用时为一周左右，仅为项目demo
```
## Arch
本项目采用

client----server----console的架构

client与console为golang编译的二进制程序，在windows下运行

server为python flask框架下的web api，在linux vps运行

## Build
server:
```
python3 -m pip install flask

export FLASK_APP=server.py

flask run --host=0.0.0.0 --port=9000

port可自定义
server.py内的key可自定义
heartbeat_raw为初始心跳时间可自定义

```

client:
```
将server端_url()函数内的url替换为启动url，不带最后一个/
例如：http://100.100.100.100:9000

将var init_key替换为你server中自定义的key

go build -gcflags=-trimpath=$GOPATH -asmflags=-trimpath=$GOPATH -ldflags "-w -s -H windowsgui" main.go

or

garble.exe build -trimpath -ldflags "-w -s -H windowsgui" main.go
```

console:
```
替换sKey为server中的自定义key
替换connUrl为server的url

go build console.go
```

## Use

### client
客户端支持两种上线方式：

#### 单次上线

单次上线为本项目的最初计划的方式，目的是配合系统持久化手段进行控制持久化，

例如：

	采用计划任务的方式，把客户端以单次运行方式执行，设定每1小时执行一次即可。

	采用服务自启动的方式，把客户端以单次运行方式执行，设定每次开机执行一次即可。

单次上线不常驻后台，每次执行即可与api对接一次，接收相关信息。

运行方式(以下三选一即可):
```
client.exe -o
client.exe --once
client.exe once
```
#### beacon上线
beacon上线为在开发过程中加入的上线方式，可以代替简单功能的C2。

常驻后台，心跳包定时请求api

运行方式:
```
client.exe -b
client.exe --beacon
client.exe beacon
```

### console
控制台采用命令行交互

运行方式：
```
在powershell中:
./console.exe
```

## Features
```
PS D:\> .\console.exe
       _            _            _              _
      /\ \         /\ \         /\ \           /\ \
     /  \ \____   /  \ \       /  \ \         /  \ \
    / /\ \_____\ / /\ \ \     / /\ \_\       / /\ \ \
   / / /\/___  // / /\ \ \   / / /\/_/      / / /\ \_\
  / / /   / / // / /  \ \_\ / / / ______   / /_/_ \/_/
 / / /   / / // / /   / / // / / /\_____\ / /____/\
/ / /   / / // / /   / / // / /  \/____ // /\____\/
\ \ \__/ / // / /___/ / // / /_____/ / // / /______
 \ \___\/ // / /____\/ // / /______\/ // / /_______\
  \/_____/ \/_________/ \/___________/ \/__________/


--------Welcome to Doge-Persistence-Platform--------

Input The Secret-Key:DogeTest


1.列出所有主机           | List all hosts                      | all

2.列出所有n天内上线主机  | List all hosts connected in n days  | days

3.列出所有心跳上线主机   | List all heartbeats hosts           | beacon

4.列出所有单次上线主机   | List all online once hosts          | once

Input(num or word): 1

1 | DESKTOP-DogeTest;8.8.8.8:192.168.1.1; heartbeat: 60 min ; 2021/02/08,17:01:29
2 | DESKTOP-DogeTest1;8.8.8.8:192.168.1.2; once ; 2021/02/07,11:05:41

选择目标主机(输入0回退)  | Target Num(input 0 back)

Input:1


1. 查看主机info          | Show Target Info                    | info

2. 命令执行              | Set CMD                             | cmd

3. 待执行命令或url       | List CMD or url                     | list

4. 下载url               | Set Download & exec Url             | download

5. 命令执行结果          | Show All CMD Results                | results

6. 删除历史数据          | Delete                              | del

7. 修改心跳时间(退出)    | Set Sleep (shutdown)                | sleep

Exploit(type 0 back):
```
