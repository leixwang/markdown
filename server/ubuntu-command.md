# kvmla 配置
web : meteor user
ritter : build user

### 创建一个sudo用户
```bash
# adduser 'ritter'
# adduser ritter sudo 
# adduser ritter root
# adduser ritter ssh
```

### 重新指定语言
```bash
$ sudo apt-get install localepurge
$ sudo locale-gen en_US.UTF-8 zh_CN.UTF-8
$ locale    //查看本地语言
```

### 自动补齐
```bash
$ sudo apt-get install bash-completion
$ sudo apt-get install --reinstall bash-completion
$ cp /etc/skel/.bashrc ~/
$ . ~/.bashrc
```

### 安装fish
```bash
$ sudo install fish
$ fish
```
### update
```
$ sudo apt-get update
E: The method driver /usr/lib/apt/methods/http could not be found.
```
Fixed:
```bash
$ sudo apt-get install apt-transport-https
$ sudo apt-get update
```

### 将程序放到后台运行
```bash
$ nohup meteor &
```


## 安装ShadowSocks
简介`Shadowsocks`安装配置过程。

### ubuntu 安装方法

```
 $ sudo apt-get update 
 $ sudo apt-get install python-pip
 $ sudo apt-get shadowsocks
```
### 配置文件
需要在`/etc/`目录下创建一个`shadowsocks.json`的文件, 配置文件内容如下:
```
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"rc4-md5"
}
```
各字段的含义：

|name	|info|
| :------ | :--------------------------------: |
|server	|服务器 IP (IPv4/IPv6)，注意这也将是服务端监听的 IP 地址|
|server_port|	服务器端口|
|local_port|	本地端端口|
|password|用来加密的密码|
|timeout|	超时时间（秒）|
|method|加密方法，"rc4-md5"|

### 运行方法
如果想直接在命令行下运行, 如果要停止运行，将命令中的start改成stop。

```
$ sudo ssserver -c /etc/shadowsocks.json -d start
```

### 配置开机自启动

```
$ sudo chmod 755 /etc/shadowsocks.json
$ sudo apt–get install python–m2crypto   //安装支持算法的库
```
需要将启动项增加到, 启动文件中.
```
$ sudo vi /etc/rc.local

/usr/local/bin/ssserver –c /etc/shadowsocks.json
```


# Ubuntu config

## 语言设置
如果没有设置好语言启动不了`meteor`, 会显示`mongoDB`服务启动失败.

解决方法如下:
```
$ sudo apt-get install locales
$ sudo locale-gen en_US.UTF-8
$ sudo localedef -i en_GB -f UTF-8 en_US.UTF-8 
```


# http 转 https 服务器配置

## 开发机配置

修改/etc/hosts，加上：

```
127.0.0.1 dev.badui.com
```
#### 推荐一个频繁切换hosts的小工具[[SwitchHosts](http://oldj.github.io/SwitchHosts/)]
![](https://cloud.githubusercontent.com/assets/4111893/26032565/0e388dd2-38c9-11e7-8230-c595534dec95.png)

---
在本地安装nginx，配置文件加上：

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}
server {
	server_name dev.badui.com;
	listen 80;
	return 301 https://$host$request_uri;
}
server {
	server_name dev.badui.com;
	listen 443 ssl;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
	ssl_prefer_server_ciphers on;
	ssl_session_timeout 5m;
	ssl_session_cache shared:SSL:50m;
	ssl_session_tickets off;
	ssl_certificate cert/xxx.pem;
	ssl_certificate_key cert/xxx.key;
	add_header Strict-Transport-Security "max-age=31536000";
	location / {
		proxy_pass http://127.0.0.1:3000;
        	proxy_http_version 1.1;
        	proxy_set_header   Upgrade          $http_upgrade;
        	proxy_set_header   Connection       $connection_upgrade;

        	proxy_set_header   Host             $host;
        	proxy_set_header   X-Real-IP        $remote_addr;
        	proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        	proxy_set_header   X-Forwarded-Proto $scheme;

        	proxy_buffering off;
        	proxy_buffer_size  128k;
        	proxy_buffers   32 32k;
        	proxy_busy_buffers_size 128k;
        	proxy_connect_timeout 1200s;
        	proxy_read_timeout 1200s;
        	proxy_send_timeout 1200s;
        	proxy_redirect off;
        	proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
        	expires 1d;
	}
}
```


如果你发现证书过期了，去服务器上取一个最新的证书，顺便帮忙更新一下本文。

用devlive的配置，在默认的3000端口启动meteor:

```
ROOT_URL=https://dev.badui.com meteor --settings .deploy/dev/settings-dev.json
```

在微信web开发者工具中可以通过公众号网页授权登录前台页面 https://dev.badui.com

在Chrome中可以通过微信扫码登录前台 https://dev.badui.com 或后台 https://dev.badui.com/admin

## 在手机上调试

手机要改hosts需要越狱，要拿开发机做proxy也经常不灵。所以最省事的办法是配置一台无线路由器，让开发机和手机都通过这个无线路由器上网。在路由器上改域名解析，比如极路由可以装一个插件叫“自定义hosts”，在里面加一条“192.168.199.101    dev.badui.com”即可。

## 调试微信支付

除了设置上述手机调试步骤之外，微信支付的统一下单接口有个notify_url参数，是异步接收微信支付结果通知的回调地址，由于回调地址必须为外网可访问的url，可以用ngrok做一个tunnel，notify_url参数填ngrok生成的url，这样本地程序就能接收到回调的通知了。注意在提交代码时把notify_url改回服务器的地址，比如 https://dev.badui.com/pay-cb 。

## 调试公众号消息接口

这个就复杂了。公众号消息高度依赖回调，而且回调地址是在公众号后台填写的，必须是已备案的域名，而ngrok生成的域名没法备案，所以就行不通了。有一个办法是在服务器上自己做这个tunnel，这样就可以用服务器的已备案域名来接收回调并tunnel到本地（参考 
 https://blog.rodneyrehm.de/archives/38-You-may-not-need-localtunnel-or-ngrok.html ）。但这样调试必然影响公众号的正常服务，所以只能在测试用的公众号上做。





Ubuntu 16.04下Shadowsocks服务器端安装及优化
 2017-06-13   47716   其他   shadowsocks, bbr, systemd   84
前言
本教程旨在提供简明的Ubuntu 16.04下安装服务器端Shadowsocks。不同于Ubuntu 16.04之前的教程，本文抛弃initd，转而使用Ubuntu 16.04支持的Systemd管理Shadowsocks的启动与停止，显得更为便捷。优化部分包括BBR、TCP Fast Open以及吞吐量优化。

本教程仅适用于Ubuntu 16.04及之后的版本，基于Python 3，支持IPv6。

安装pip
本教程使用Python 3为载体，因Python 3对应的包管理器pip3并未预装，首先安装pip3：

Bash
sudo apt install python3-pip
安装Shadowsocks
因Shadowsocks作者不再维护pip中的Shadowsocks（定格在了2.8.2），我们使用下面的命令来安装最新版的Shadowsocks：

Bash
pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip
安装完成后可以使用下面这个命令查看Shadowsocks版本：

Bash
sudo ssserver --version
目前会显示“Shadowsocks 3.0.0”。

创建配置文件
创建Shadowsocks配置文件所在文件夹：

Bash
sudo mkdir /etc/shadowsocks
然后创建配置文件：

Bash
sudo nano /etc/shadowsocks/config.json
复制粘贴如下内容（注意修改密码“password”）：

JSON
{
    "server":"::",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
然后按Ctrl + O保存文件，Ctrl + X退出。

测试Shadowsocks配置
首先记录下服务器的IP地址

Bash
ifconfig
找到IPv4地址（和IPv6地址），如我的ifconfig输出为

eth0      Link encap:Ethernet  HWaddr 46:91:89:4e:c1:52
          inet addr:138.68.51.55  Bcast:138.68.63.255  Mask:255.255.240.0
          inet6 addr: fe80::4491:89ff:fe4e:c152/64 Scope:Link
          inet6 addr: 2604:a880:2:d0::3727:7001/64 Scope:Global
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:102667 errors:0 dropped:0 overruns:0 frame:0
          TX packets:7869 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:151166937 (151.1 MB)  TX bytes:1151476 (1.1 MB)
所以我的IPv4地址是138.68.51.55，IPv6地址是2604:a880:2:d0::3727:7001。

然后来测试下Shadowsocks能不能正常工作了：

Bash
ssserver -c /etc/shadowsocks/config.json
在Shadowsocks客户端添加服务器，如果你使用的是我提供的那个配置文件的话，地址填写你的IPv4地址或IPv6地址，端口号为8388，加密方法为aes-256-cfb，密码为你设置的密码。然后设置客户端使用全局模式，浏览器登录Google试试应该能直接打开了。

这时浏览器登录http://ip138.com/就会显示Shadowsocks服务器的IP啦！

测试完毕，按Ctrl + C关闭Shadowsocks。

配置Systemd管理Shadowsocks
新建Shadowsocks管理文件

Bash
sudo nano /etc/systemd/system/shadowsocks-server.service
复制粘贴：

[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/config.json
Restart=on-abort

[Install]
WantedBy=multi-user.target
Ctrl + O保存文件，Ctrl + X退出。

启动Shadowsocks：

Bash
sudo systemctl start shadowsocks-server
设置开机启动Shadowsocks：

Bash
sudo systemctl enable shadowsocks-server
至此，Shadowsock服务器端的基本配置已经全部完成了！

优化
这部分属于进阶操作，在你使用Shadowsocks时感觉到延迟较大，或吞吐量较低时，可以考虑对服务器端进行优化。

开启BBR
BBR系Google最新开发的TCP拥塞控制算法，目前有着较好的带宽提升效果，甚至不比老牌的锐速差。

升级Linux内核
BBR在Linux kernel 4.9引入。首先检查服务器kernel版本：

Bash
uname -r
如果其显示版本在4.9.0之下，则需要升级Linux内核，否则请忽略下文。

更新包管理器：

Bash
sudo apt update
查看可用的Linux内核版本：

Bash
sudo apt-cache showpkg linux-image
找到一个你想要升级的Linux内核版本，如“linux-image-4.10.0-22-generic”：

Bash
sudo apt install linux-image-4.10.0-22-generic
等待安装完成后重启服务器：

Bash
sudo reboot
删除老的Linux内核：

Bash
sudo purge-old-kernels
开启BBR
运行lsmod | grep bbr，如果结果中没有tcp_bbr，则先运行：

Bash
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
运行：

Bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
运行：

Bash
sysctl -p
保存生效。运行：

Bash
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
若均有bbr，则开启BBR成功。

优化吞吐量
新建配置文件：

Bash
sudo nano /etc/sysctl.d/local.conf
复制粘贴：

# max open files
fs.file-max = 51200
# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
net.ipv4.tcp_max_tw_buckets = 5000
# turn on TCP Fast Open on both client and server side
net.ipv4.tcp_fastopen = 3
# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1

net.ipv4.tcp_congestion_control = bbr
运行：

Bash
sysctl --system
编辑之前的shadowsocks-server.service文件：

Bash
sudo nano /etc/systemd/system/shadowsocks-server.service
在ExecStart前插入一行，内容为：

ExecStartPre=/bin/sh -c 'ulimit -n 51200'
即修改后的shadowsocks-server.service内容为：

[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStartPre=/bin/sh -c 'ulimit -n 51200'
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/config.json
Restart=on-abort

[Install]
WantedBy=multi-user.target
Ctrl + O保存文件，Ctrl + X退出。

重载shadowsocks-server.service：

Bash
sudo systemctl daemon-reload
重启Shadowsocks：

Bash
sudo systemctl restart shadowsocks-server
开启TCP Fast Open
TCP Fast Open可以降低Shadowsocks服务器和客户端的延迟。实际上在上一步已经开启了TCP Fast Open，现在只需要在Shadowsocks配置中启用TCP Fast Open。

编辑config.json：

Bash
sudo nano /etc/shadowsocks/config.json
将fast_open的值由false修改为true。Ctrl + O保存文件，Ctrl + X退出。

重启Shadowsocks：

Bash
sudo systemctl restart shadowsocks-server
注意：TCP Fast Open同时需要客户端的支持，即客户端Linux内核版本为3.7.1及以上；你可以在Shadowsocks客户端中启用TCP Fast Open。

至此，Shadowsock服务器端的优化已经全部完成了！

