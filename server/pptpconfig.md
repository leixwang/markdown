#Ubunt Server 使用PPTP连接VPN
##一、安装PPTP工具
```
$ sudo apt-get install pptpclient 
```
##二、创建一个Tunnel
```
$ sudo pptpsetup --create vpn --server 106.184.2.94 --username x32349501 --password sandy360 --encrypt --start
$ ifconfig //查看连接后的信息
```

##三、手动连接
```
$sudo pon vpn //连接VPN
$sudo poff vpn //断开VPN
```

##四、添加默认网关
```
$sudo route -n //查看默认网关
$sudo route add -net 0.0.0.0 dev ppp0
```

