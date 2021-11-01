


# Redis install

In this section you’ll add the EPEL repository, and then use it to install Redis.

Add the EPEL repository, and update YUM to confirm your change:

```sh
sudo yum install epel-release
sudo yum update
```

Install Redis:

```sh
sudo yum install redis
```

Start Redis:

```sh
sudo systemctl start redis
```

Show Redis status:

```sh
sudo systemctl status redis
```

Optional: To automatically start Redis on boot:

```sh
sudo systemctl enable redis
```

Verify the Installation
Verify that Redis is running with redis-cli:

```sh
redis-cli -h 127.0.0.1 -p 6379
```



##### 构建Redis服务器 [512M]

配置`redis`文件如下:

```
sudo vim /etc/redis.conf
```

##### redis.conf

```sh
参
# 设置最大使用内存 512MB
maxmemory 512mb

# 设置客户端最多连接数量
maxclients 128

# bind
bind 0.0.0.0

# 首先将 daemonize 由 no 改成 yes，表示允许 Redis 在后台启动
daemonize yes

# 设置密码
requirepass b840fc02d52404

# protected-mode 设置为 no 关闭保护模式
protected-mode yes

```



##### 使用客户端连接redis.

```shell
$ redis-cli -h redis.edms.site -a b840fc02d52404

redis.edms.site:6379> info
redis.edms.site:6379> exit
```





# Java install

### Installing OpenJDK

```
// install jdk 8
# yum install java-1.8.0-openjdk-devel

// install jdk 11
# yum install java-11-openjdk-devel

// show jdk version
# java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-b08)
OpenJDK 64-Bit Server VM (build 25.242-b08, mixed mode)
```



# Maven

### 1. Download Apache Maven

At the time of writing this article, the latest version of Apache Maven is `3.6.3`. Check the [Maven download page](https://maven.apache.org/download.cgi) to see if a newer version is available.

Start by downloading the Apache Maven archive in the `/tmp` directory with [`wget`](https://linuxize.com/post/wget-command-examples/) command:

```
wget https://www-us.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz -P /tmp
```

Once the download is complete, [extract the archive](https://linuxize.com/post/how-to-create-and-extract-archives-using-the-tar-command-in-linux/) in the `/opt` directory:

```
sudo tar xf /tmp/apache-maven-3.6.3-bin.tar.gz -C /opt
```

To have more control over Maven versions and updates, we will [create a symbolic link](https://linuxize.com/post/how-to-create-symbolic-links-in-linux-using-the-ln-command/) `maven` that will point to the Maven installation directory:

```
sudo ln -s /opt/apache-maven-3.6.3 /opt/maven
```



### 2. Setup environment variables

Next, we’ll need to set up the environment variables. Open your text editor and create a new file named `mavenenv.sh` in the `/etc/profile.d/` directory.

```
sudo nano /etc/profile.d/maven.sh
```

Paste the following code:

```sh
export JAVA_HOME=/usr/lib/jvm/jre-openjdk
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

Save and close the file. The script will be sourced at shell startup.

Make the script executable by running the following [`chmod`](https://linuxize.com/post/chmod-command-in-linux/) command:

```
sudo chmod +x /etc/profile.d/maven.sh
```



### 3. Verify the installation

To verify that Maven is installed, use the `mvn -version` command which will print the Maven version:

```
mvn -version
```

You should see something like the following:

```output
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /opt/maven
Java version: 11.0.5, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-11-openjdk-11.0.5.10-0.el8_0.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.18.0-80.7.1.el8_0.x86_64", arch: "amd64", family: "unix"
```

That’s it. The latest version of Maven has been installed on your CentOS system.





# Nodejs install



[nvm-git](https://github.com/nvm-sh/nvm)   

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```



# python install

## Installing Python 3 on CentOS 8

To install Python 3 on CentOS 8 run the following command as root or [sudo user](https://linuxize.com/post/how-to-add-user-to-sudoers-in-centos/) in your terminal:

```shell
sudo dnf install python3
```

To verify the installation, check the [Python version](https://linuxize.com/post/how-to-check-python-version/) by typing:

```
python3 --version
```

At the time of writing this article, the latest version of Python 3 available in the CentOS repositories is “3.6.x”:

```output
Python 3.6.8
```



## Set Default Python Version (Unversioned Python Command)

If you have applications that expect to find the `python` command in the [system’s path](https://linuxize.com/post/how-to-add-directory-to-path-in-linux/), you’ll need to create the unversioned python command and set the default version.



To set Python 3 as the system-wide unversioned python command, use the `alternatives` utility:

```shell
sudo alternatives --set python /usr/bin/python3
```





# 配置SSl证书



## certbot 自动更新 HTTPS 证书

安装好了 Certbot，给网站安装好了 SSL 证书.

### Install SSL Letsencrypt Certificates

安装 letsencrypt 软件

```sh
sudo apt install letsencrypt -y
```

如果启动 nginx 服务, 需要将它关掉, 再去生成新的证书.

```sh
sudo sytemctl stop nginx
```

dms.pub 生成新的证书.

```sh
certbot certonly --rsa-key-size 2048 --standalone --agree-tos --no-eff-email --email admin@dms.pub -d dms.pub
or
certbot certonly --nginx -d dms.pub
```

生成新的证书放在 '/etc/letsencrypt/live/' 目录下.

### nginx 配置文件

```sh
server {
	server_name www.abc.com;
	listen 80;
	return 301 https://$host$request_uri;
}
server {
	listen 443;
	ssl on;
	server_name www.abc.com;
    ssl_certificate /etc/letsencrypt/live/www.abc.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/www.abc.com/privkey.pem;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        proxy_pass http://127.0.0.1:8080;
	}
}
```

### 创建 Cron 文件

输入以下命令：

1. 查看crontab定时执行任务列表

```sh
crontab -l
```

2. 添加crontab定时执行任务

```sh
crontab -e
```

### 添加编辑 Certbot 的自动续期命令

在 root cron 文件中，复制以下代码，粘贴，保存，上传。

```sh
0 1 1 * * /usr/bin/certbot renew --renew-hook "/etc/init.d/nginx reload"
```

以上含义是：每隔 7 天，夜里 3 点整自动执行检查续期命令一次。续期完成后，重启 nginx 服务。

### 重启 Cron 服务，使之生效

```sh
systemctl restart cron.service
```

重启之后，一切搞定！

### 想手动尝试 Certbot 证书更新？

一般是直接使用 renew 命令，即：

```sh
/usr/bin/certbot renew
```

但是现在 Certbot 也会自己判断了，没有快到期之前，它也觉得没必要频繁续期。所以看看我们手动去续期的结果：

## 工具

[工具 acme.sh github](https://github.com/acmesh-official/acme.sh)

