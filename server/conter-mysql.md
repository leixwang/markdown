# CentOS install mysql 8.0

## 获取源

```sh
yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
```

## 安装命令

安装mysql8.0

```sh
yum install mysql-community-server
```


## 启动mysql
启动mysql和加入系统自动启动mysql

```sh
systemctl start mysqld.service       ## use restart after update
systemctl enable mysqld.service
```


## 查看临时密码

```sh
grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |tail -1
```



## config mysql

```sh
/usr/bin/mysql_secure_installation
```

output:

```mysql
Securing the MySQL server deployment.

Enter password for user root: 

The existing password for the user account root has expired. Please set a new password.

New password: 

Re-enter new password: 

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
Using existing password for root.

Estimated strength of the password: 100 
Change the password for root ? ((Press y|Y for Yes, any other key for No) : y

New password: 

Re-enter new password: 

Estimated strength of the password: 50 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done! 
```


## 本地连接数据库

```shell
$ mysql -u root -p

## OR ##
# mysql -h localhost -u root -p
```


## 创建数据库和用户

* DB_NAME = webdb
* USER_NAME = webdb_user
* REMOTE_IP = 10.0.15.25
* PASSWORD = password123
* PERMISSIONS = ALL

```mysql
## CREATE DATABASE ##
mysql> CREATE DATABASE webdb;

## CREATE USER ##
mysql> CREATE USER 'webdb_user'@'%' IDENTIFIED BY 'password123';

## GRANT PERMISSIONS ##
mysql> GRANT ALL ON webdb.* TO webdb_user@'%';
mysql> grant all on webdb.* to webdb_user@'%';

##  FLUSH PRIVILEGES, Tell the server to reload the grant tables  ##
mysql> FLUSH PRIVILEGES;
```

注: 更新用户可以访问的限制,  `%` 是表示任何IP地址可以访问的.

```sh
mysql> update user set host='%' where user='root' 
```



## 修密码等级

如果设置出现 `ERROR 1819 (HY000): Your password does not satisfy the current policy requirements` 错误信息, 是因为密码的等级设置过高, 需要将安全等降低或是重新用随机软件生成密码.

```mysql

## 查看密码安全等级
mysql> set global validate_password.policy=0;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password.check_user_name    | ON    |
| validate_password.dictionary_file    |       |
| validate_password.length             | 8     |
| validate_password.mixed_case_count   | 1     |
| validate_password.number_count       | 1     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 1     |
+--------------------------------------+-------+
14 rows in set (0.00 sec)

## 创建新用户
mysql> create user 'user'@'%' identified by 'abcd1234';
Query OK, 0 rows affected (0.08 sec)

````


## 开启远程连数据库

### 1. Fedora 28/27/26 and CentOS/Red Hat (RHEL) 7.5

1.1 Add New Rule to Firewalld
```shell
firewall-cmd --permanent --zone=public --add-service=mysql
## OR ##
firewall-cmd --permanent --zone=public --add-port=3306/tcp
````

1.2 Restart firewalld.service

```sh
systemctl restart firewalld.service
```

### 2. CentOS/Red Hat (RHEL) 6.9

2.1 Edit /etc/sysconfig/iptables file:
```shell
nano -w /etc/sysconfig/iptables
```

2.2 Add following INPUT rule:

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
```

2.3 Restart Iptables Firewall:
```shell
service iptables restart
## OR ##
/etc/init.d/iptables restart
```

### 2. Ubuntu 20.04

2.1 Edit /etc/sysconfig/iptables file:

```shell
$ sudo vim /etc/iptables/rules.v4
```

2.2 Add following INPUT rule:

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
```

2.3 Restart Iptables Firewall:

```shell
sudo systemctl restart iptables.service
## OR ##
service iptables restart
## OR ##
/etc/init.d/iptables restart
```



### 3. Test remote connection

```mysql
mysql -h 10.0.15.25 -u myusername -p
````

Posted in Featured, Linux, Servers, SQL
Tagged CentOS, Fedora, MySQL, Oracle, Red Hat, RHEL, Scientific Linux







### 4.配置

```sh
$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

修改为:

```mysq
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address          = 127.0.0.1
bind-address            = 0.0.0.0
```

重启 `mysql` 服务

```sh
$ sudo systemctl restart mysql.service
```





## 优化数据库配置



## 重置root密码

如果root密码忘了, 需要重置一下密码.


### 1. 修改 `my.cnf` 文件
找到 `my.cnf` 文件,  `ubuntn mysql my.cnf` 文件位置如下:

```sh
sudo vim /etc/mysql/my.cnf
```

在文件中, 增加下面2行. 

```mysq
[mysqld]
skip-grant-tables
```

### 2. 重新启动 mysql 服务

修改完 `my.cnf` 文件, 需要重新启动 `mysql` 服务

```sh
$ sudo systemctl restart mysql.service   (重启服务)
$ sudo systemctl status mysql.service   (查看服务状态)
```



### 3. 查看系统默认管理员

使用下面的命令可以查看默认的管理员账号和密码

```sh
sudo cat /etc/mysql/debian.cnf
```

下面是文件内容:

```mysql
# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = FU3sZfPdY8Wt1m6N
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = FU3sZfPdY8Wt1m6N
socket   = /var/run/mysqld/mysqld.sock
```



### 4. 使用默认管理员登录

使用默认的管理员账号登录 `debian-sys-maint` 密码 `FU3sZfPdY8Wt1m6N`

```sh
$ sudo mysql -u debian-sys-maint -p
```

```mysql
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.25-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```



### 5. 将 root 用户密码清空

先查看 root 用户状态

切换到 mysql 库上: 

 `use mysql;`

```my
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

查看命令:

`select host, user, authentication_string from user;`

```mysq
mysql> select host, user, authentication_string from user;
+-----------+------------------+------------------------------------------------------------------------+
| host      | user             | authentication_string                                                  |
+-----------+------------------+------------------------------------------------------------------------+
| %         | root             | *C0961D81E1BF229EA454AC7FA97F6CC6388DFCDC                              |
| localhost | debian-sys-maint | $A$005$JLr8W
KKmoD0bUP!.Bmf.hea/KBZCLYLutujZEjqqtrUxxrXENn40qve030 |
| localhost | mysql.infoschema | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | mysql.session    | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | mysql.sys        | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
| localhost | phpmyadmin       | $A$005$mB9jb</
Bbmzi\vipjsNhUZexEwx7V6yYKvubT48LBnx/tMKKsa8N2YL3HC |
+-----------+------------------+------------------------------------------------------------------------+
6 rows in set (0.00 sec)
```

修改 root 密码

`mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'mysql2021';`

```msyql
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'VQnK7Kz@dTr#pu2s7';
Query OK, 0 rows affected (0.00 sec)
```



刷新权限, 修改后的权限生效.

`mysql> flush privileges;`

```mys
mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye
```







### 6. 遇到的错误

#### 1  错误

```my
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1234';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

这个错误是设置的密码过于简单, 密码要求是:

1. 最少8个字符,

2. 必须包含最少1个大写字母

3. 必须包含最少1个数字

4. 必须包含最少1个特殊字符. 如: `~!@#$%^&*`

   密码如下: `A54$%DGdd@#434`



#### 2 错误

```mysq
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'A54$%DGdd@#434';
ERROR 1396 (HY000): Operation ALTER USER failed for 'root'@'localhost'
```

需要将 localhost 修改成 %

```my
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'VQnK7Kz@dTr#pu2s7';
Query OK, 0 rows affected (0.00 sec)
```

