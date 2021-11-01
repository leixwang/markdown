# python ip 池



### 1. 安装软件包:

安装软件包.

```shell
pip install pymongo
pip install requests
```



### 2. IP 池 代码:

IP代理池.

```python
import requests
import time
import datetime
from pymongo import MongoClient

client = MongoClient('localhost', 27017)["proxy_save"]
db = client["proxy"]


spiderId=''
api_url = 'http://api.xdaili.cn/xdaili-api//privateProxy/applyStaticProxy?spiderId={}&returnType=2&count=1'


def del_proxies():
    mins_age = (datetime.datetime.now() - datetime.timedelta(minutes=3)).strftime("%Y-%m-%d %H:%M:%S")
    # print(mins_age)
    myquery = {"record_time": {"$lte": mins_age}}
    x = db.delete_many(myquery)
    print(x.deleted_count, "个文档已删除")


def write_url():
    proxies = requests.get(api_url).json()
    if ('频繁' or '未知') in str(proxies):
        sleep_time = 3
        print(proxies, '睡眠%s秒后重试' % sleep_time)
        time.sleep(sleep_time)
        return write_url()
    try:
        proxies_list = proxies['RESULT']
    except:
        return
    # print(proxies_list)
    if 'port' not in str(proxies_list):
        return print(proxies)

    address_list = []
    record_time = time.time()
    for ip_info in proxies_list:
        ip = ip_info['ip']
        port = ip_info['port']
        address = ip + ':' + port
        # print(address)
        mongo_doc = {'ip': address, 'record_time': record_time}
        address_list.append(mongo_doc)

    print('成功插入代理条数： ' + str(len(address_list)))
    try:
        db.insert_many(address_list)
    except Exception as e:
        print(e)


if __name__ == "__main__":
    try:
        db.create_index([('ip', 1)], unique=True, background=True)
    except:
        pass
    while True:
        del_proxies()
        result = write_url()
        time.sleep(10)

```



### 3. 代理商



http://www.xdaili.cn/

