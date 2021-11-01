Ubuntu 16.04 x64 安装 React Native 开发环境
===================================

[TOC]
前言
----
NodeJs安装请参照
[https://reactnative.cn/docs/0.41/getting-started.html](https://reactnative.cn/docs/0.41/getting-started.html)
一、准备
------------
1、[jdk-8u121-linux-x64.tar.gz](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)<br/>
2、[android-studio-ide-145.3360264-linux.zip](http://www.android-studio.org)<br/>
3、如果是64位系统还需要安装32位运行库

```bash
$ sudo apt-get install lib32z1 lib32ncurses5  lib32stdc++6
```

二、安装
-------------
###1、解压
```bash
$ cp jdk-8u121-linux-x64.tar.gz /opt/.
$ cp android-studio-ide-145.3360264-linux.zip /opt/.
$ sudo tar -zxvf jdk-8u121-linux-x64.tar.gz
$ sudo unzip android-studio-ide-145.3360264-linux.zip
```

###2、赋权
```bash
$ sudo mkdir android-sdk
$ sudo chown -R $USER:$USER android-sdk
$ sudo chown -R $USER:$USER android-studio
$ /opt/android-studio/bin/studio.sh
```

###3、启动Android Studio
```bash
$ /opt/android-studio/bin/studio.sh
```

###4、配置Android Studio
***提示：***初次启动会弹出以下界面，点击“OK”
![enter image description here](http://www.2cto.com/uploadfile/Collfiles/20160502/2016050211205292.png)

***提示：***点击“Next”
![enter image description here](http://www.2cto.com/uploadfile/Collfiles/20160502/2016050211205294.png)

***提示：***
>1、选择“Custom”，点击“Next”。<br/>
>2、 .....<br/>
>3、选择SDK路径：/opt/android-sdk，点击“Next”

![enter image description here](http://www.2cto.com/uploadfile/Collfiles/20160502/2016050211205398.png)

***提示：***等待下载，最后点击“Finish”
![enter image description here](http://www.2cto.com/uploadfile/Collfiles/20160502/2016050211205399.png)

***提示：***关闭Android Studio
![enter image description here](http://www.2cto.com/uploadfile/Collfiles/20160502/20160502112054100.png)

###5、下载Android SDK
***注意***
>不要使用Android Studio自带的Android SDK Manager下载，可能会造成Android Studio无法启动的问题

```bash
$ /opt/android-sdk/tools/android
```

>***提示：***
>1、启动 Android SDK Manager ，打开主界面，依次选择「Tools」、「Options...」，弹出『Android SDK Manager - Settings』窗口；
>2、在『Android SDK Manager - Settings』窗口中，在「HTTP Proxy Server」和「HTTP Proxy Port」输入框内填入 mirrors.neusoft.edu.cn 和 80，并且选中「Force https://... sources to be fetched using http://...」复选框。设置完成后单击「Close」按钮关闭『Android SDK Manager - Settings』窗口返回到主界面；
>3、依次选择「Packages」、「Reload」
>4、在Tools中勾选Android SDK Build-tools 23.0.1
>5、在Android 6.0 (Marshmallow)中勾选Google APIs、Android SDK Platform 23、Intel x86 Atom System Image、Intel x86 Atom_64 System Image以及Google APIs Intel x86 Atom_64 System Image。
>6、下载完成后关闭Android SDK Manager

常见问题解决
----------
1、apt无法安装,提示错误
>E:Could not get lock /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)

```bash
$ sudo rm /var/cache/apt/archives/lock
 
$ sudo rm /var/lib/dpkg/lock
```

2、运行react-native run-android时，出现unzip错误,此问题一般是gradle下载不完整造成，删除~/.gradle/wrapper/dists/gradle-3.4-all/目录后再试，如果依然出现此错误，可使用方法二

方法一：

```bash
$ rm -rf .gradle/wrapper/dists/gradle-3.4-all/

```

方法二：

>下载gradle-3.4-all.zip文件到本地，并修改android/gradle/wrapper/gradle-wrapper.properties文件：

```bash
distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip
改为本地路径：
distributionUrl=file\:/Users/sandy/Downloads/gradle-3.4-all.zip

```

3、虚拟机无法启动Android模拟器
>将虚拟机CPU虚拟化模式设为SVM或VT-x,VirtualBox未解决。
