# Meteor 命令的操作


## 安装meteor
```
$ curl https://install.meteor.com/ | sh
```

## 创建一个项目
```
$ meteor create myapp
```

## 向meteor服务提交
```
$ meteor deploy *.meteor.com
username: smartx
password: ********
```
## 显示所有部署到服务器的域名
```bash
$  meteor list-sites
```

## 部署服务器
所有设置都在客户端这边完成, 然后通这命令去运程到服务器上进行自动安装软件和配置环境.


## 创建本地包
```bash
$ meteor create --package 包名
$ meteor add 包名
```

### 部署自己服务器上.
```bash
// ubuntu 64bit v14.04 LTS, 记得 update

$ npm -g install git+https://github.com/RockaLabs/meteor-up.git#muprockanew

$ muprockanew setup --config mupx.json

$ muprockanew deploy --config mupx.json
```

附上配置文件`mupx.json`
```text
{
  "servers": [
    {
      "host": "server_ip",
      "username": "root",
      //"pem": "~/.ssh/id_rsa",
    "password": "root user passwd"
    }
  ],

  "setupMongo": true,
  "appName": "mobileweb",
  "app": "~/mobile-web",          //网站文件存放的目录
  "env": {
    "PORT": 80,
    "ROOT_URL": "http://demo.plw.io"
  },

  "deployCheckWaitTime": 60,
  "enableUploadProgressBar": true
}
```

## pem的配置
### 生成密 pem
客户端（本地主机 ）生成验证没有密码密钥对

```sh
 $ ssh-keygen -t rsa -b 2048 -v
```

执行上述命令首先会让你输入生成密钥的文件名：我这里输入的 `mykey` ，之后一路回车。

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/anonymouse/.ssh/id_rsa): mykey
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in mykey.
Your public key has been saved in mykey.pub.
The key fingerprint is:
bb:c6:9c:ee:6b:c0:67:58:b2:bb:4b:44:72:d3:cc:a5 localhost@localhost
The key's randomart image is:
+---[RSA 2048]----+
|++o +    OO0     |
|ooo+ O   yt      |
|EB+ =.+  o       |
|+=ooo.o.. +      |
|. o..+ oS. .     |
|.  .o  . ..      |
|.  oo o o  .     |
| . .== .... .    |
|  ...+o... .     |
+----[SHA256]-----+
```
在执行命令的当前目录下会生成一个 `mykey.pub`、`mykey` 两个文件。并将这两个文件移到 `~/.ssh` 目录下.
```sh
 $ mv mykey* ~/.ssh
```

### 将公匙推放到服务器上
把生成的 `mykey.pub` 通过本地命令推送到服务器端，使服务器自动添加认证这个证书.
```sh
 $ ssh-copy-id -i ~/.ssh/mykey.pub root@12.34.56.78
```

测试连接:
使用`pem`连拉服务器, 如果设置成功, 不需要输入密码, 就果以进入到服务器上.
```sh
 $ ssh -i ~/.ssh/mykey root@12.34.56.78
```

# 部署服务器的配置
-----------------

## 安装软件
系统使用: ubuntu14.04 aliyun

### 安装docker


* [Docker 官方基于ubuntu安装](https://docs.docker.com/engine/installation/linux/ubuntulinux/)





## 安装 mup
```sh
 $ sudo npm install mup -g -f
```

初始化配置文件:
```sh
 $ mup init
```

配置文件如下: mup.json
```
module.exports = {
  servers: {
    one: {
      host: '120.77.44.150',
      username: 'root',
      // password:
      // or leave blank for authenticate from ssh-agent
    }
  },

  meteor: {
    name: 'zhangbowen',
    path: '../todo-react',
    servers: {
      one: {}
    },
    buildOptions: {
      serverOnly: true,
    },
    env: {
      PORT: ****,
      ROOT_URL: 'http://app.com',
      MONGO_URL: 'mongodb://localhost/meteor'
    },

    dockerImage: 'abernix/meteord:base',

    deployCheckWaitTime: 60
  },
  mongo: { // (optional)
    oplog: true,
    port: 27017,
    servers: {
      one: {},
    },
  },
};
```


### 配置命令
```sh
 $ mup setup
```

### 部署
```sh
 $ mup deploy
```

