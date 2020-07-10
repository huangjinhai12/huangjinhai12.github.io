---
layout:     post
title:      "如何上传内核源码到git"
subtitle:   ""
author:     "hjh"
header-img: "img/bg-1.png"
header-mask:  0.5
catalog: true
tags:
    - linux
---

1. 首先我们在用户主目录下看是否存在.ssh目录。
- 以笔者为例，笔者使用的是Ubuntu系统，当前用户的主目录是/home/hjh，所以此时我们需要查找路径/home/hjh/.ssh是否存在。
如果存在，查看是否存在id_rsa与id_rsa.pub两个文件是否存在。如果也存在，就可以调到下一步；
如果不存在，便打开终端，输入自己的邮箱地址，创建SSH Key。
```
git config --global user.name "huangjinhai12"
git config --global user.email "2538082724@qq.com"
ssh-keygen -t rsa -C "youremail@example.com"
```
- 在笔者的主目录下就会生成/home/grq/.ssh文件夹，里面也会生成文件id_rsa与id_rsa.pub，它们是SSH Key的秘钥对。其中id_rsa是私钥，不能泄露，id_rsa.pub是公钥.
2. 在GitHub端设置SSH Key
- 登录GitHub，点击右上角头像，Settings -> Personal settings -> SSH and GPG keys。在SSH Keys标签右方点击New SSH Key。
弹出两个文本框。其中的Title，可以随意命名。笔者此处随便命名为grq-Ubuntu。
另一个Key文本框，需要输入刚刚生成的id_rsa.pub文件中的内容。粘贴后点击Add SSH Key，即可生成SSH Key.
3. 上传项目
- 有网友得出总结，可以将git分为四部分：一部分是自己的本机文件，一部分是缓存区，一个是本地仓库，一个是服务器仓库。==当用户在本机修改了文件后，就应该使用git add xx指令将修改保存到缓存区，然后再用git commit yy指令将推送从缓存区修改到本地仓库中，最后使用git push将本地仓库中的修改推送到服务器仓库中。==
    1. 准备上传
    ```
    这个命令可以把当前目录变成git可以管理的仓库
    git init
    ```
    2. 添加需要上传的文件
    ```
    git add file
    file是我们想要添加的文件。这里笔者想要将整个文件夹内容都添加进去，所以此处笔者输入的指令如下
    git add ./
    ```
    3. 检查当前git状态
    ```
    git status
    ```
    4. commit推送
    ```
    git commit -m "Update Readme Files(Version of Chinese & English)"
    ```
    5. 如果邮箱出错
    ```
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```
    6. 添加文件到远程库
    ```
    git remote add origin git@github.com:huangjinhai12/linux.git
    ```
    - 出现错误 fatal: remote origin already exists
    ```
    git remote rm origin
    ```
    - 第一次push
    ```
    git push origin master
    ```
    - 更新
    ```
    git push
    ```