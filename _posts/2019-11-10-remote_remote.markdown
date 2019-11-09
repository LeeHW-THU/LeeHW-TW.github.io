---
layout:     post
title:      "远程访问内网服务器"
subtitle:   " 记录打码。 "
date:       2019-11-10 
author:     "LeeHW"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - remote
---

## 前提

| 机器号        |      IP       |   用户名   | 备注                                          |
| ------------- | :-----------: | :--------: | --------------------------------------------- |
| In_server     |   <host_In>   | <name_In>  | 处于内网，可访问Public_server                 |
| Public_server | <host_Public> | <name_Out> | 处于公网，不可访问server                      |
| computer      |      --       |     --     | 处于公网，不可访问server，可访问Public_server |

**需要一个公网server（相信很多科学上网的小伙伴都有）**

## 实现

### 	Server为访问目标，用ssh -R 反向端口转发

​		在Server上运行命令：

```shell
#Step_1 把本地的22端口映射到<host_Public>的20001端口，-o后的参数作用是保证连接(心跳)
ssh -o ServerAliveInterval=60 -o ServerAliveCountMax=60 -fCNR 20001:localhost:22 <name_Out>@<host_Public>
```



### 	Public_server为跳板，用ssh -L 本地端口映射

​		在Public_server上运行命令：

```shell
#将本地的20001端口映射到20002上
ssh -fCNL *:20002:localhost:20001 localhost
```



### 	computer为当前使用机，ssh -p 指定端口访问

```shell
#访问Public_server的20002端口
#注意 要使用<name_In>去访问<host_Public>，因为20002端口已经被转发到<host_In>了
ssh -p 20002 <name_In>@<host_Public>
```



------

注意事项：

​	1) Public_server不能重启（好像）

​	2) 请确保Server与public之间的访问权限，使用**ssh-copy-id/ssh-keygen**解决

​	3) 

```shell
#查看端口使用情况
lsof -i:20002
```

