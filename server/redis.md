# Redis简介


## ubuntu安装Redis
更新安装源, 安装编程工具和 `tcl`.

```shell
$ sudo apt-get update
$ sudo apt-get install build-essential tcl
```

### 下载 redis 源码包
通过 `curl` 下载最新的 `redis` 源码压缩文件. 如果没有安装 `curl` 命令工具, 先安装它.

```shell
$ sudo apt-get install curl -y
$ curl -O http://download.redis.io/redis-stable.tar.gz
```

### 解压 redis 
解压刚刚下载的 `redis` 安装包.

```shell
$ tar xzvf redis-stable.tar.gz
```


### 编译 redis
进到解压目录中, 使用 `make` 命令编译 `redis`

```shell
$ cd redis-stable
$ make
```

### 进行测试

```shell
$ make test
```


### redis 安装

```shell
$ sudo make install
```


### 配置 redis
需要要 `/etc` 目录下创建一个 `redis` 目录, 用来存储配置文件的.

```shell
$ sudo mkdir /etc/redis
```

### 复制配置文件
将 `redis-stable` 目录下中的 `redis.conf` 默认的配置文件复制到 `/etc/redis` 目录中


```shell
$ sudo cp redis-stable/redis.conf /etc/redis
```

### 修改编辑 redis.conf 文件
需要打开 `supervised` 和设置存储临时数据的目录.


需要将 `supervised` 设置为 `systemd`

```shell
$ sudo vim /etc/redis.conf
. . .

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

. . .
```


下面我们来设置一下临时数据的存储目录为 `/var/lib/redis`, `/var/lib/redis` 这是目录需要我们自己去创建. 

```shell
. . .

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir /var/lib/redis

. . .
```
### 创建 systemd 配置文件
需要创建 `/etc/systemd/system/redis.service` 配置文件.

```shell
$ sudo vim /etc/systemd/system/redis.service
```
编辑内容如下:
```conf
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```


### 创建 redis 用户, 组和目录
创建 `redis` 用户, 这个用户不需要创建用户目录.

```shell
$ sudo adduser --system --group --no-create-home redis
```

创建刚才我们在配置文件中, 使用的 `/var/lib/redis` 目录.

```shell
$ sudo mkdir /var/lib/redis
```

将 `redis` 用户和组分配到 `/var/lib/redis` 目录.

```shell
$ sudo chown redis:redis /var/lib/redis
$ sudo chmod 770 /var/lib/redis
```

### 启动测试 redis 服务

启用 `redis` 服务

```shell
$ sudo systemctl start redis
```

查看 `redis` 服务启动是否成功.

```shell
$ sudo systemctl status redis
```

运行下面的命令, 将会有下面的输出信息:

```log
Output
● redis.service - Redis Server
   Loaded: loaded (/etc/systemd/system/redis.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2016-05-11 14:38:08 EDT; 1min 43s ago
  Process: 3115 ExecStop=/usr/local/bin/redis-cli shutdown (code=exited, status=0/SUCCESS)
 Main PID: 3124 (redis-server)
    Tasks: 3 (limit: 512)
   Memory: 864.0K
      CPU: 179ms
   CGroup: /system.slice/redis.service
           └─3124 /usr/local/bin/redis-server 127.0.0.1:6379       

. . .
```

### redis 命令行工具

可以使用命令行工具, 连接 `redis`

```shell
$ redis-cli
```

```
127.0.0.1:6379> info
```


## nodejs 使用 redis
先要使用 `express -e` 创建项目, 并使用 `npm` 命令安装`nodejs`模块.

```sh
$ express -e redis-node
$ cd redis-node
$ npm install
```

需要安装 `nodejs` 的 `redis` 库文件.

```sh
$ npm i redis --save
```

在当前项目下创建 `redis` 目录, 并在目录下创建 `index.js` 文件. (当前目录在 `redis` 项目目录.)

```sh
$ mkdir redis
$ cd redis
$ touch index.js
```

我们现在需要编写代码, 连接 `redis` 服务器.

下面是 `redis/index.js` 文件内容.

```js
const redis = require('redis');
const client= redis.createClient({
  host: 'redis-13302.c14.us-east-1-2.ec2.cloud.redislabs.com',
  port: 13302,
  password:'asdfgh'
});

client.on('connect',(err)=>{
  if(err){
    console.log('Redis connect error');
    console.log(err);
  }else{
    console.log('Redis connect OK');
  }
});

module.exports = client;
```


## 参考文档
* [ubuntu1604安装配置redis](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)
