#Ubuntu Server 连接Wi-Fi
##一、安装工具
```
$sudo apt-get install wpasupplicant
```

##二、生成无线路由密钥
```
$mkdir ~/wifiKey
$cd wifiKey
$sudo ifconfig wlan0 up /搜索wifi热点前需要启动网卡
$sudo iwlist wlan0 scan //搜索wifi热点
$wpa_passphrase ESSID PWD > essid.conf
```

##三、手动连接Wi-Fi
```
$sudo wpa_supplicant -B -i wlan0 -Dwext -c  ~/wifiKey/xxx.conf
$sudo iwconfig wlan0
$sudo dhclient wlan0 //DHCP获取IP

```

##四、配置开机自动连接
```
$sudo vi /etc/network/interfaces
```

加入以下内容：
```
auto wlan0
iface wlan0 inet dhcp
wpa-conf ~/wifiKey/xxx.conf
# address 10.10.5.13
# netmask 255.255.248.0
# gateway 10.10.5.1
```

