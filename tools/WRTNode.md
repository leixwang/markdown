WRTnode
=======
一、搭建编译环境
------------
###安装软件支持
Ubuntu 12.04LTS
```
$ sudo apt-get install build-essential subversion git-core libncurses5-dev zlib1g-dev gawk  flex quilt libssl-dev xsltproc libxml-parser-perl mercurial bzr ecj cvs unzip patch git-core g++
```
 注：多执行两次，可能会有软件未安装

###下载固件源码
```
 $ mkdir openWrt
 $ cd openWrt
 $ wget http://d.wrtnode.com/sdk/sdk.tar.bz2 
```
###编译固件
```
$ make menuconfig 
//进入编译配置界面，如果没什么需要就不要乱改，直接保存默认配置就好
$ make V=s  //开始编译，大约需要30分钟
```
 
经过一段漫长的编译之后生成的固件在 ./bin/ramips/openwrt-ramips-mt7620n-wrtnode-squashfs-sysupgrade.bin

###官方固件
http://d.wrtnode.com/old-firmware/openwrt-ramips-mt7620n-wrtnode-squashfs-sysupgrade.bin

