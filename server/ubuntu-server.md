
# ubuntu 16.04 64位
需要新更新系统.
```sh
# apt update
# apt install -y build-essential
```


# install node.js 
install node.js for v6.11.3

1. 需要配置node.js源, 添加到服务器中.
```ssh
# curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - 
# apt install -y nodejs
```

# install mongodb

install mongodb enterprise

1. Import the public key used by the package management system.
```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

2. Create a /etc/apt/sources.list.d/mongodb-enterprise.list file for MongoDB.
```sh
echo "deb [ arch=amd64,arm64,ppc64el,s390x ] http://repo.mongodb.com/apt/ubuntu xenial/mongodb-enterprise/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
```
3. Reload local package database.
```sh
sudo apt-get update
```

4. Install the MongoDB Enterprise packages.
```sh
sudo apt-get install -y mongodb-enterprise
```


mongodb command:
1. start mongodb
```sh
service mongod start
```

mongodb config:
```sh
# vim /etc/mongodb.conf
```

### bootstart mongodb

```sh
root@iZrj978n2nwntexfoyz539Z:~# cat mongod.sh
#!/bin/sh
#title         :mongod.sh
#author        :Richard Wang
#date          :22/09/2017
#description   :start/stop/restart mongod
#########################################
### install   :  cp mongod /etc/init.d/
#                update-rc.d mongod defaults
### uninstall :  update-rc.d -f mongodb remove

PATH_TO_MONGO=/usr/bin/mongod

#file containing all mongodb pid
PID_FILE=/tmp/mongodb.pid

case "$1" in
	start)
		echo "Starting mongodb service..."

		COMMAND_TO_RUN=`start-stop-daemon -S -b -m -p $PID_FILE -x $PATH_TO_MONGO& :`
		setsid sh -c $COMMAND_TO_RUN> /dev/null 2>&1 < /dev/null

		echo -e "Start mongodb server [ OK ]"
		;;
	stop)
		echo "Stopping mongodb service..."

		start-stop-daemon -K -q -p $PID_FILE

		echo -e "Stop mongodb server [ OK ]"
		;;
	restart|reload)
		"$0" stop
		"$0" start
		;;
	*)
		echo $"Usage: $0 {start|stop|restart}"
		exit 1
esac

exit $?
```
install monod
```sh
cp mongod.sh /etc/init.d/mongod
chmod 755 /etc/init.d//mongod
update-rc.d mongod defaults
```


### Remove Mongodb 
```sh
apt-get purge mongodb-enterprise*
```
delete log file

```sh
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```


# nginx

1. install nginx
```sh
# apt install nginx
```

2. nginx config

http.conf 
```sh
vim /etc/nginx/sites-available/http.conf
```
http.conf:
```sh
server {
	server_name xxx.com www.xxx.com
	listen 80;
	location / {
		proxy_pass http://127.0.0.1:3000;
	}
}
```

ln
```sh
cd /etc/nginx/sites-enabled
ln -s /etc/nginx/sites-available/http.conf
```

**https configure**:
```sh
server {
	server_name xxx.com www.xxx.com;
	listen 80;
	return 301 https://$host$request_uri;
}
server {
	listen 443;
	ssl on;
	server_name xxx.com www.xxx.com;
	ssl_certificate   cert/xxx.pem;
	ssl_certificate_key  cert/xxx.key;
	ssl_session_timeout 5m;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;
	location / {
		proxy_pass http://127.0.0.1:3000;
	}
}
```


# 部署网站

install tools git

```sh
apt install git
```

instal node.js tools

```sh
npm i yarn -g
npm i pm2 -g 
```


download web project:

```sh
git clone url-for-project
cd project-name
```

pm2 start:

```sh
pm2 start www/bin
```

pm2 show information:

```sh
pm2 show id
```




# redis

# mysql


====================

# ubuntu 18.04.2

### update and upgrade

```sh
$ sudo apt-get updat
$ sudo apt-get upgrde
$ sudo apt install -y build-essential wget git vim

```



### Install bash-completion

```sh
$ sudo apt install bash-completion
$ cp /etc/skel/.bashrc ~/
```

### Install golang

```sh
$ wget https://dl.google.com/go/go1.13.3.linux-amd64.tar.gz
$ tar xvf go1.13.3.linux-amd64.tar.gz
$ mv go /usr/local
```

`vim .bashcr`, 在文件最后面增加下面的全局变量设置.

```sh
--------------------------------------------
export GOROOT=/usr/local/go
export GOPATH=$HOME/workspace
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```








