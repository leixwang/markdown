# U盘安装ubuntu教程

### Universal USB Installer下载并安装

![](img/inub001.jpg)
![](img/inub002.jpg)


### 下载 ubuntu 桌面的镜像

* [下载ubuntu镜像](http://www.ubuntu.com/desktop)

![](img/inub003.jpg)


### 打开Universal USB Installer 在setp 1:select... 下选择ubuntu

![](img/inub004.jpg)

### 在setp 2:select ...下选择你下载的ubuntu的iso镜像位置

![](img/inub005.jpg)

### 在setp 3:select...下选择你U盘所在的盘符(也可以勾选format进行格式化，但是U盘中的内容记得备份一下)

![](img/inub006.jpg)

### 完成配置后 点击下面的creat 进行制作

![](img/inub007.jpg)

![](img/inub008.jpg)

![](img/inub009.jpg)

### 安装ubuntu系统

* 重新启动计算机，选择USB盘启动，就可以进行ubuntu的安装.


## 配置网卡ip地址

* 图形界面:


* 命令方式:
```bash
 $ sudo ifconfig eth0 192.168.1.101
```


## 安装必须软件

### 首先更新软件源
```bash
 $ sudo apt-get update
 $ sudo apt-get upgrade
```
### 安装 git 下载运行需要安装软件的sh

```sh
 $ sudo apt-get install git-all -y
 $ git clone https://github.com/wangleihd/software.git
 $ cd software
 $ chmod 755 software.sh
 $ ./software.sh
 ```

### 配置vim

```sh
 $ chmod 755 install.sh
 $ ./install.sh
```

### 安装atom

访问官方网站下载atom for ubuntu 64位安装软件包.

* [atom 官方网站](http://www.atom.io)
```sh
 $ sudo dpkg - i atom-amd64.deb
```
### 安装docker


```bash
```


## install teamview

```bash
$ sudo dpkg --add-architecture i386
$ sudo apt-get update
$ sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
```


## 安装node.js 开发环境

系统为ubuntu14.04 node.js + express + mongodb + redis

### 安装Node.js

安装node.js, npm
```bash
 $ sudo apt-get install node npm
```

### 安装express

### 安装mongodb

### 安装redis
