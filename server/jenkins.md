Jenkins
=======
一、前提
--------
* Ubuntu 14.04
64位Ubuntu系统
```
$ sudo apt-get install lib32z1
$ sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
```
* Java JDK http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html
* Android SDK http://www.android.com/
    * Android SDK Build-tools
    * Android SDK Platform
    * Android Support Library - for eclipse build
    * Android Support Repository - for android studio build
* Gradle(此工具主要针对Android项目，C或C艹项目可以直接使用shell命令去编译) http://gradle.org/
* Apache Tomcat http://tomcat.apache.org
* Jenkins.war http://jenkins-ci.org
 注:使用官方安装包手动安装,不要apt-get install,否则会自动安装OpenJDK，OpenJDK在编译时会报缺少tools.jar错误

二、环境搭建
------------
###配置Java JDK环境
* 解压jdk-8u66-linux-x64.tar.gz
```
$ sudo cp jdk-8u66-linux-x64.tar.gz /opt/.
$ cd /opt
$ sudo tar -zxvf jdk-8u66-linux-x64.tar.gz
```
* 把JDK加入系统环境变量中
```
$ sudo echo "export JAVA_HOME=/opt/jdk1.8.0_66">>/etc/profile
$ sudo echo "export PATH=$PATH:$JAVA_HOME/bin">>/etc/profile
```
* 先不用着急把JDK配置到PATH中
```
$ source /etc/profile 
//使其系统环境变量生效,如果要在系统全局生效，需要重启系统
$ java -version
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)
//输出以上信息则证明JDK已安装完毕
```
###配置Android SDK环境