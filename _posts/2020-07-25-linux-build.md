---
layout:     post
title:      "内核手动编译安装"
subtitle:   ""
author:     "wml"
header-img: "img/bg-1.png"
header-mask:  0.5
catalog: true
tags:
    - 笔记
---
- 有台破服务器，上面啥grub配置都没有，也不能直接make install，只能手动配置安装
- 给的教程也让人崩溃吧
### 编译 & 安装

```
1. 切换到源码目录下
2. zcat /proc/config.gz > .config
3. make menuconfig，修改LOCALVERSION(必须有deepin）
4. make -j16 deb-pkg
5. dpkg -i <xxxx>.deb
6. 切换到/boot，找到刚安装的vmlinux
7. objcopy -O binary vmlinux <original name>
8. copy <out file> /boot/<file> (file名字必须含有deepin）
9. vim /boot/grub/grub.cfg
```
```
3. make menuconfig，修改LOCALVERSION(必须有deepin）
注：后面追加-hjh
6. 切换到/boot，找到刚安装的vmlinuz，不是vmlinx
- vmlinuz-4.4.15-deepin-aere-hjh+为elf文件
7. objcopy -O binary vmlinux <original name>
- objcopy -O binary vmlinuz-4.4.15-deepin-aere-hjh+ vmlinuz
- 转换为二进制启动文件
8. copy <out file> /boot/<file> (file名字必须含有deepin）
- mv vmlinuz vmlinuz-4.4.15-deepin-aere-hjh+只是重命名而已
9. vim /boot/grub/grub.cfg
- 添加自己生成的内核启动项
menuentry "deepin server V15 hjh Build47 (6A)" --class gnu-linux --class gnu --class os{
        echo "装载中，请耐心等待……"
        set boot=(${root})/boot/
        linux.boot ${boot}/initrd.img-4.4.15-deepin-aere-hjh+
        echo "装载 initrd 成功"
        linux.console ${boot}/bootloader.bin
        echo "装载 bootloader.bin 成功"
        linux.vmlinux ${boot}/vmlinuz-4.4.15-deepin-aere-hjh+ quiet splash net.ifnames=0 audit=0 apparmor=0 root=UUID=30736336-d307-4197-ac9e-d994b34a18d4
        echo "装载 vmlinux 成功"
        boot
}
```
- 参考链接：非常感谢三位老哥
[debian9升级4.9.0内核到4.19.2内核过程]https://blog.csdn.net/weixin_39465823/article/details/84138135

[Debian 9 下载内核源码，手动编译安装，从4.9.0-9内核升级到5.1.0]https://blog.csdn.net/QHTSM/article/details/89954135

[How to Upgrade Kernel of Debian 9 Stretch from Source]https://linuxhint.com/how-to-upgrade-kernel-of-debian-9-stretch-from-source/