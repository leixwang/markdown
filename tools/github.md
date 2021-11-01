## clone 整个项目
```
git clone https://github.com/limingth/microduino.git
cd microduino
git branch -a
git log
```

## 切换分支
```
git checkout dev/limingth
git checkout development
git checkout master
```

## 每次切换分支后，确认所在分支的相关信息
```
git branch 
git log
```

## 拉取development最新代码到自己的分支下
```
git checkout dev/limingth
git reset --hard origin/development
git log
git branch
```

## 出现冲突
```
git rebase --skip  （可能会用到）
```

## 提交自己的新修改
```
git commit -a -m "[Maodou]: Shipping: #123, update something"
git push origin dev/limingth
```


# [GIT] 第二阶段GIT操作指南

## 说明
由于各种原因，安全性跟CI的结合问题，我们觉得还是应该回归到Github传统的流程。也就是：
* 开发者各自fork项目的repo到自己Github账户下
* 每次开发同步到项目的repo然后开发
* push自己的开发分支到自己Github账户下面的fork的项目repo
* 发送pull request给项目管理员
* 等待review或者merge

## 分支规划
采用git remote add命令给自己本地的开发repo添加分支，我们用一下约定来处理分支的名字
* origin - 直接指向项目的repo
* dev_name - 指向自己fork出来的repo例如我的叫cooloney
* reaction - 指向reaction 原版的repo

## 具体操作
### Fork 项目repo到自己Github账户（只需要setup一次）
* 用自己账户登录Github
* 进入Microduino项目repo[主页](https://github.com/limingth/microduino)
* 点击右上角的[*fork*按钮](https://github.com/limingth/microduino#fork-destination-box)
* 1分钟不到，就会在自己的Github项目下面建立一个私有的项目

### 本地开发的配置（以我的账户示例，也只要setup一次）
* 从李明老师的项目repo clone最新的代码
```
git clone --recursive https://github.com/limingth/microduino.git
```
* 添加自己fork的repo用来发布代码和发送pull request
```
cd microduino
git remote add cooloney https://github.com/cooloney/microduino.git
```
* 添加reaction的repo用来跟reaction upstream同步
```
git remote add reaction https://github.com/reactioncommerse/reaction.git
```

### 开发流程（每次开发都要运行）
```
git fetch origin
git checkout -b development origin/development (create a new branch for development)
git reset --hard origin/development (reset the local branch to latest origin development branch)
git rebase origin/development (rebase local change onto origin development branch)
do some work ...
git commit changes
git push --force cooloney development
```

### 运行测试（第一次运行时需要下载相关的package）
```
git clone --recursive https://github.com/cooloney/microduino.git
./microduino
```

### 发送pull request（每次push都需要）
* 登录自己repo的Github主页
* 点击pull request
* 按照下图配置好pull request

<img width="947" alt="screen shot 2015-10-06 at 12 23 08 am" src="https://cloud.githubusercontent.com/assets/758488/10302377/b2d14b5e-6bc0-11e5-9395-b72e0b98616e.png">

## Package Git使用指南
### 说明
* 每一个Package也是一个单独的repo
* 我们只是加入了那些我们需要修改的package，没有修改的package我们用reaction原版的代码。
* 请登陆自己的Github账户fork相应的package repo到自己的Github账户

### 配置开发分支
```
cd packages/core
git remote add cooloney git@github.com:cooloney/reaction-core.git
git remote add reaction git@github.com:reactioncommerce/reaction-core.git
$ git remote -v
cooloney	git@github.com:cooloney/reaction-core.git (fetch)
cooloney	git@github.com:cooloney/reaction-core.git (push)
origin	git@github.com:limingth/reaction-core.git (fetch)
origin	git@github.com:limingth/reaction-core.git (push)
reaction	git@github.com:reactioncommerce/reaction-core.git (fetch)
reaction	git@github.com:reactioncommerce/reaction-core.git (push)
```

### 取出最新的代码开发
```
git fetch origin
git checkout -b development origin/development (create a new branch for development)
git reset --hard origin/development (reset the local branch to latest origin development branch)
git rebase origin/development (rebase local change onto origin development branch)
do some work ...
git commit changes
git push --force cooloney development
```

### 发送pull request（每次push都需要）
* 登录自己repo的Github主页
* 点击pull request





## 标准PR流程

以我的账号dev1为例，首先需要在master下Fork到自己账户下

#### 首次运行，只需要运行一次
```
git clone https://github.com/dev1/h5.git
git submodule init
git remote add upstream https://github.com/master/h5.git
git checkout -b dev upstream/dev
```
#### 每次都需要运行
```
git submodule update
git fetch upstream
git reset --hard upstream/dev
```
#### ... work
```
git rebase upstream/dev
git push -f origin master
```





# git init 操作



Git 全局设置

```shell
git config --global user.name "wanglei" 
git config --global user.email "wanglei@micous.com"
```

创建一个新仓库

```sh
git clone https://gitlab.int.nbt.ren/gsc/activity.git 
cd activity touch README.md 
git add README.md 
git commit -m "add README" 
git push -u origin master
```

推送现有文件夹

```shell
cd existing_folder 
git init 
git remote add origin https://gitlab.int.nbt.ren/gsc/activity.git 
git add . 
git commit -m "Initial commit" 
git push -u origin master
```

推送现有的 Git 仓库

```shell
cd existing_repo 
git remote rename origin old-origin 
git remote add origin https://gitlab.int.nbt.ren/gsc/activity.git 
git push -u origin --all 
git push -u origin --tags
```

