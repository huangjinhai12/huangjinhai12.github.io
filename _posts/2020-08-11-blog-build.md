---
layout:     post
title:      "虚拟机简单环境配置"
subtitle:   ""
author:     "hjh"
header-img: "img/bg-1.png"
header-mask:  0.5
catalog: true
tags:
    - linux
---

前言：每次弄虚拟机都要花很长一段时间，烦死
##### 环境
- 宿主机：macos
- 虚拟机软件：virtualbox
- 虚拟机操作系统：centos 7.5
- 内核：4.4.15
#### 配置源
- 虚拟机一定要有线连接（人傻）
1、安装wget
`yum install -y wget`
2、下载CentOS 7的repo文件
`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`
3. 因为最初可能是国外源，你需要的是先下载在本机然后共享文件
4. 更新镜像源
清除缓存：yum clean all
生成缓存：yum makecache
更新yum：yum update
#### 设置共享文件夹
![linux](/img/linux/1.png)
##### 安装 Centos 所需的增强功能包
- 这时启动 Centos ，输入 df 命令，并不能看到我们需要的共享文件夹，这是因为 Centos 还需要增强功能包以支持此需求。
##### 首先尝试直接安装增强功能包
- Centos 的功能包需要光驱支持，首先在设置里添加虚拟光驱
![linux](/img/linux/2.png)
具体怎么导入增强工具包忘了，以后有时间补上
- 其次需要 gcc 环境，在命令行输入以下代码安装 gcc 。
`yum install -y gcc gcc-devel gcc-c++ gcc-c++-devel make kernel kernel-devel`
#### 手动挂载 iso
创建 /media/drive
`mkdir -p /media/drive`
挂载 iso，可能会提示 sr0 只读，如果下面提示已挂载，也说明挂载成功
`sudo mount -t auto /dev/cdrom /media/drive/`
安装增强功能
```
cd /media/drive/
sudo sh VBoxLinuxAdditions.run
```
稍等片刻，重启 Centos，输入 df 指令，应该能看到共享文件夹，即设置成功。
##### 挂载共享文件夹
切换到root用户输入挂载命令：
```
sudo mount -t vboxsf shared_file /home/xingoo/shared
sudo mount -t vboxsf 共享文件夹名称（在设置页面设置的） 挂载的目录
```
#### 配置ssh
- 配置虚拟机ssh的原因就是虚拟机命令终端不好用
1. 首先，要确保CentOS7安装了  openssh-server，在终端中输入  `yum list installed | grep openssh-server`
2. 通过输入  `yum install openssh-server`
3. 找到了  /etc/ssh/  目录下的sshd服务配置文件 sshd_config，用Vim编辑器打开，将文件中，关于监听端口、监听地址前的 # 号去除
![linux](/img/linux/3.png)
然后开启允许远程登录
![linux](/img/linux/4.png)
最后，开启使用用户名密码来作为连接验证
![linux](/img/linux/5.png)
保存文件，退出