## install nodejs

```sh
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - 
$ sudo apt-get install -y nodejs
```



## install npm

```sh
$ sudo apt-get install npm 
```


```sh
$ sudo npm install hexo-cli -g 
```


## 创建 blog

```sh
 $ hexo init blog
 $ cd blog
 $ npm install 
 $ hexo server
```

可以通过浏览器访问网址.

## 修改配置文件

```sh
$ vim _config.yml  (在最后一行增加）
----
deploy:
    type: git
    repo: https://github.com/github-name/github-name.github.io.git
    branch: master
```


## 网站进行布置

部署前需要生成网站， 每次修改后都需要重新生成网站。

```sh
$ hexo g
$ hexo d
```

## 常见问题
1.  ERROR Deployer not found: git

需要安装部署时使用的git包。

```sh
$ npm install hexo-deployer-git --save
```

## 参考

```sh
$ git clone https://github.com/wangleihd/leix.wang.git
$ cd leix.wang
```

