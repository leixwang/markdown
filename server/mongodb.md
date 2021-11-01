## 1.本地数据库备份与恢复
##### 1.1 本地docker数据库

- 备份

```
mongodump -d testdb -o testdb
```


- 恢复

```
mongorestore -d testdb --drop testdb
```


##### 1.2 本地meteor运行数据库
meteor运行时，默认在3000端口，可以使用meteor mongo查看数据库

- 备份

```
mongodump -h 127.0.0.1:3001 -d meteor -o testdb
```

- 恢复

```
mongorestore -h 127.0.0.1:81 -d testdb --drop testdb
```

**需要注意，restore的数据库collecton文件夹名和collection名要一直**

## 2.远程数据库备份与恢复
##### 2.1 compose.io数据库

- 备份

```
mongodump -h lighthouse.5.mongolayer.com:10367 -u root -p Password -d testdb -o testdb
```

- 恢复

```
mongorestore --host lighthouse.5.mongolayer.com --port 10367 -u root -p Password -d testdb --drop testdb
```


##### 2.2 aliyun mongo数据库
**注意，阿里云的mongo只能阿里云主机才能连接**

- 备份

```
mongodump -h dds-2ze04b800265f5b42.mongodb.rds.aliyuncs.com:3717 -u maodou -p password -d testdb -o testdb
```

- 恢复

```
mongorestore -h dds-2ze021cc656a02141.mongodb.rds.aliyuncs.com:3717 --authenticationDatabase admin -u root -p password -d testdb --drop testdb
```

## Faq
1. 出现错误
 
```
terminate called after throwing an instance of 'std::runtime_error'
  what():  locale::facet::_S_create_c_locale name not valid
```

解决办法

```
export LC_ALL="C"
```



```bash
#!/bin/sh
#title         :mongod
#author        :Bertrand Martel
#date          :15/08/2015
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

        echo -e "\E[31;33m[ OK ]\E[0m"
        ;;
    stop)
        echo "Stopping mongodb service..."

        start-stop-daemon -K -q -p $PID_FILE

        echo -e "\E[31;33m[ OK ]\E[0m"
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
自动在ubuntu16.04系统启动mongodb
```sh
cp mongod /etc/init.d/

update-rc.d mongod defaults
```

# 数据更新
```
1. db.getCollection('allrecords').updateMany({}, {$rename: {'bet':'slotBet'}})
2. db.getCollection('allrecords').updateMany({}, {$rename: {'type':'coinType'}})
3. db.getCollection('allrecords').updateMany({}, {$set: {'gameType':'Slot'}})
4. db.getCollection('allrecords').updateMany({}, {$set: {'dealId':''}})
5. db.getCollection('allrecords').updateMany({}, {$set: {'payoff': 0}})
6. db.getCollection('allrecords').updateMany({'isrobot': {$exists : false}}, {$set: {'isrobot': false}})
7. db.getCollection('allrecords').updateMany({}, {$rename: {'isrobot': 'isRobot'}})
8. db.getCollection('allrecords').updateMany({'upaddress': {$exists : false}}, {$set: {'upaddress': null}})
9. db.getCollection('allrecords').updateMany({}, {$rename: {'upaddress': 'inviterAddress'}})
```
