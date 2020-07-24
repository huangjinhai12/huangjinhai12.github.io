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
![blog](/img/blog/blog-build-19.png)
- 安装ssh服务，输入命令：`sudo apt-get install openssh-server`
- 启动服务: /etc/init.d/ssh start
- 本机测试是否能够成功登录：ssh -p 1113 hjh@localhost
注： 1113是本机转发虚拟机22端口
2. virtualbox配置端口转发
![blog](/img/blog/blog-build-20.png)