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