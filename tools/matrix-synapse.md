# How to Install Matrix Synapse Chat on Ubuntu 18.04 LTS

## Step 0 - ubuntu configure

```sh
sudo apt-get install build-essential python3-dev libffi-dev \
  python-pip python-setuptools sqlite3 \
  libssl-dev python-virtualenv libjpeg-dev libxslt1-dev
```



## Step 1 - Update and Upgrade System

Login to your Ubuntu server, update the repository and upgrade all packages using the apt command below.

```sh
sudo apt update
sudo apt upgrade
```
And all ubuntu packages have been upgraded.

## Step 2 - Install Matrix Synapse

In this step, we will install the matrix synapse software using the Debian packages from the official matrix repository.

Add the matrix key and repository by running all commands below.

```sh
wget -qO - https://matrix.org/packages/debian/repo-key.asc | sudo apt-key add -
sudo add-apt-repository https://matrix.org/packages/debian/
```

The command will automatically update the repository.

Install Matrix Synapse

Now install matrix synapse using the apt command as below.

```sh
sudo apt install matrix-synapse -y
```

During the installation, it will ask you about the matrix server name - type the matrix domain name 'matrix.hakase-labs.io'.

Matrix synapse apt installer - part 1

And for the anonymous data report, choose 'No'.

Matrix synapse apt installer - part 1

When the matrix synapse installation is complete, start the service and enable it to launch everytime at system boot.

```sh
sudo systemctl start matrix-synapse
sudo systemctl enable matrix-synapse
```

The matrix synapse is now up and running using the default configuration on port '8008' and '8448'. Check using netstat command.

```sh
netstat -plntu
```

Check open ports



## Step 3 - Configure Matrix Synapse

After the matrix synapse installation, we will configure it to run under the local IP address, disable matrix synapse registration, and enable the registration-shared-secret.

Before editing the home server configuration, we need to generate the shared secret key.

Run the command below.

```sh
cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1
```

And you will get the generated key. Copy the result key.

Now we need to edit the home server configuration file 'homeserver.yaml' on the '/etc/matrix-synapse/' directory. Change the current directory to '/etc/matrix-synapse' and edit the configuration file using vim.

```sh
cd /etc/matrix-synapse/
vim homeserver.yaml
Change the HTTP and HTTPS Listener port '8008' and '8448' to the local IP address '127.0.0.1'.

    port: 8448
    bind_addresses:
      - '127.0.0.1'

    - port: 8008
    bind_addresses: ['127.0.0.1']
```

Matrix Synapse configuration

Matrix Synapse port config

Disable the matrix synapse registration, uncomment the 'registration_shared_secret' configuration and paste the secret key generated.

```sh
enable_registration: False
registration_shared_secret: "MtkF9JOkNHsRRISyR5L91KAQlrrPhyWX"
```

Save and exit.


Note:

registration_shared_secret: If set allows registration by anyone who also has the shared secret, even if registration is disabled.

Now restart the matrix synapse services.

```sh
sudo systemctl restart matrix-synapse
```

restart matrix-synapse

Check the homeserver service using the command below.

```sh
netstat -plntu
```

You will get the matrix synapse service is now on the local IP address.

Check Matrix Synapse Ports

And we have completed the matrix synapse installation and configuration.



## Step 4 - Generate SSL Letsencrypt Certificates

In this tutorial, we will enable HTTPS for the Nginx reverse proxy, and we will generate the SSL certificate files from Letsencrypt.

Install the letsencrypt tool using the apt command below.

```sh
sudo apt install letsencrypt -y
```

The Letsencrypt tool is installed on the system, now generate the SSL certificate files for the matrix domain name 'matrix.hakase-labs.io' using the certbot command as shown below.

```sh
certbot certonly --rsa-key-size 2048 --standalone --agree-tos --no-eff-email --email hakaselabs@gmail.com -d matrix.hakase-labs.io
or
certbot certonly --nginx -d matrix.hakase-labs.io
```

The Letsencrypt tool will generate SSL certificate files by running the 'standalone' temporary web server for verification.

And when it's complete, you will get the result as shown below.

Generate SSL Letsencrypt Certificates

SSL certificate files for the matrix synapse domain name 'matrix.hakase-labs.io' is generated inside the '/etc/letsencrypt/live/' directory.



## Step 5 - Install and Configure Nginx as a Reverse Proxy


In this step, we will install the Nginx web server and configure it as a reverse proxy for home server that is running on the port '8008'.

Install the Nginx web server using the apt command below.

```sh
sudo apt install nginx -y
```

After the installation is complete, start the service and enable it to launch everytime at system boot

```sh
sudo systemctl start nginx
sudo systemctl enable nginx
```

Next, we will create a new virtual host configuration for the matrix domain name 'matrix.hakase-labs.io'.

Go to the '/etc/nginx' configuration directory and create a new virtual host file 'matrix'.

```sh
cd /etc/nginx/
vim sites-available/matrix
Paste the following configuration there.

server {
       listen 80;
       server_name matrix.hakase-labs.io;
       return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name matrix.hakase-labs.io;

    ssl_certificate /etc/letsencrypt/live/matrix.hakase-labs.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/matrix.hakase-labs.io/privkey.pem;

    # If you don't wanna serve a site, comment this out
    root /var/www/html;
    index index.html index.htm;

    location /_matrix {
      proxy_pass http://127.0.0.1:8008;
      proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```

or

```sh
server {
    listen 80;
	listen [::]:80;
    server_name matrix.example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name matrix.example.com;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/matrix.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/matrix.example.com/privkey.pem;

    location / {
        proxy_pass http://localhost:8008;
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}

server {
    listen 8448 ssl default_server;
    listen [::]:8448 ssl default_server;
    server_name matrix.example.com;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/matrix.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/matrix.example.com/privkey.pem;
    location / {
        proxy_pass http://localhost:8008;
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```

Save and exit.

Activate the virtual host file and test the configuration.

```sh
ln -s /etc/nginx/sites-available/matrix /etc/nginx/sites-enabled/
nginx -t
```

Make sure there is no error, then restart the Nginx services.

```sh
sudo systemctl restart nginx
```

Nginx installation and configuration as a reverse proxy for the Matrix Synapse homeserver has been completed.





## Step 6 - Create a New Matrix User
At this stage, the matrix synapse homeserver installation and configuration is complete. And in this step, we will show you how to add a new matrix user from the command line server.

To create a new matrix user, run the command below.

```sh
register_new_matrix_user -c /etc/matrix-synapse/homeserver.yaml https://127.0.0.1:8448
```

Now you need to input the user name, password, and decide whether the user will have the admin privileges or not.

Below is the result on my system.


## create homeserver.yaml config file

```sh
python -m synapse.app.homeserver \
  --server-name matrix.example.com \
  --config-path homeserver.yaml \
  --generate-config \
  --report-stats=yes|no
 ```

==============================================================================

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
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name dms.pub;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/dms.pub/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/dms.pub/privkey.pem;

    location / {
        proxy_pass http://localhost:8008;
        proxy_set_header X-Forwarded-For $remote_addr;
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
