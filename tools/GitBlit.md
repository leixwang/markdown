GitBlit服务器搭建
================

环境准备
--------
1.<a href="http://gitblit.com/" target="_blank">GitBlit</a>
2.<a href="http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html" target="_blank">JDK</a>

安装
-------
>1.将下载好的JDK解压到/opt文件夹下，并将jdk目录加到系统环境变量中
>>在/etc/profile文件最后加入：
>> export JAVA_HOME=/opt/jdk1.8.0_45
>> export PATH=$PATH:$JAVA_HOME/bin
>>然后在shell命令行中运行以上文本，使新加入的环境变量生效。
>>最后运行以下命令，将java链接到/usr/bin/java
>> $ ln -s /opt/jdk1.8.0_45/bin/java /usr/bin/java
>
>2.将下载好的GitBlit也解压到/opt文件夹下
>3.使用vim打开配置文件 data/gitblit.properties
>>更改以下配置：
>> git.repositoriesFolder = /data/git   更改仓库位置，保证系统与源码分离，防止系统崩溃危及到源码数据
>> server.httpPort = 8080               更改http访问端口，防止与80端口冲突，如果本地服务器80端口未启用，可以选择使用80端口
>> server.httpsPort = 0                 更改https访问端口，也可不改，改为0是禁用SSL访问方式
>
>4.使用vim打开gitblit根目录下的service-ubuntu.sh文件
>>更改以下配置：
>> GITBLIT_USER="root" 更改gitblit用户
>>复制到/etc/init.d/gitblit
>> $ sudo cp service-ubuntu.sh /etc/init.d/gitblit
>> $ sudo chmod 755 /etc/init.d/gitblit
>> $ sudo /etc/init.d/gitblit start
>
>5.访问<a href="http://localhost:8080/" target="_blank">GitBlit</a>即可使用
>

