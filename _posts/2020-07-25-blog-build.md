---
layout:     post
title:      "virtualbox虚拟机连接"
subtitle:   ""
author:     "wml"
header-img: "img/bg-1.png"
header-mask:  0.5
catalog: true
tags:
    - 笔记
---

#### 安装前提
- 前提是已经安装好可用ubuntu虚拟机
#### 安装步骤
1. Ubuntu下安装ssh服务
- 输入 `ps -e |grep ssh `如果服务已经启动，则可以同时看到 ssh-agent 和 sshd ，否则表示没有安装服务，或没有开机启动
![8ee4a4028a289ea3d9fb4db283152c72.png](evernotecid://922780E7-E43B-404E-8E29-693B4DD855A0/appyinxiangcom/26648766/ENResource/p90)
- 安装ssh服务，输入命令：`sudo apt-get install openssh-server`
- 启动服务: /etc/init.d/ssh start
- 本机测试是否能够成功登录：ssh -p 1113 hjh@localhost
注： 1113是本机转发虚拟机22端口
2. virtualbox配置端口转发
![49658292990cd524e46b48ebb7e66aa7.png](evernotecid://922780E7-E43B-404E-8E29-693B4DD855A0/appyinxiangcom/26648766/ENResource/p91)