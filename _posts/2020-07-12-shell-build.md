---
layout:     post
title:      "shell处理git日志"
subtitle:   ""
author:     "hjh"
header-img: "img/bg-1.png"
header-mask:  0.5
catalog: true
tags:
    - shell
---
## 心情

此时的心情是崩溃的，知道它是这么个玩意

## 正文
- 目的：想将vkernel从linux-4.4.15迁移到linux-5.2上，vkernel被改得到处都是，没办法直接打补丁。
### 1. git的处理
```
git log > vkernel.log
```
### 2. shell命令
- 获取git commit id
```
cat vkernel.log | grep "commit" > commit.txt
```
- 还需要手动删除几行不符合要求的commit
### 3. git diff获取补丁
```
preline=""
num=1
while read line
do
    if["${preline:7}"]
        git diff ${line:7} ${preline:7} > ../vkernel-pat/$num-patch
        num=$(($num+1))
    preline=$line
done < commit.txt
``` 
### 4. 结果
```
git format-patch commit-id
可以直接生成commit-id后所有的补丁
内心崩溃
```
### 5. 反转
- 根据commit-id生成得补丁不能直接打在内核上，修改太乱了，都是冲突，于是决定把改过的文件提取出来然后根据文件生成补丁，最好打上。
- 添加一个很有用的命令
```
查看文件夹下包含某个字符串的文件
grep -rn "data_chushou_pay_info"  /home/hadoop/nisj/automationDemand/
```
-  文件列表是根据commit生成的patch文件中文件修改总结部分得出。
![shell](/img/shell/shell-build-2.png)
- 生成补丁的前提是有一个被改动文件的文件列表
![shell](/img/shell/shell-build-1.png)
```
read.sh
dir1="a"
dir2="b"
num=1
while read line
do
    diff -urN $dir1/$line $dir2/$line > vkernel-patch/$num-patch
    num=$(($num+1))
done < vkernel.txt
echo $num
```
### 6.流程总结
1. 准备
- 文件列表已经得到
- 在aufs补丁通过commit得到
2. 流程
- 解压linux-4.4.15文件
- git init
- git apply aufs.patch --reject
- 执行read.sh
- 将生成的补丁文件打到linux-4.4.15上
- 编译