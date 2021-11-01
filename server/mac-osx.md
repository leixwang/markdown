---
sidebar: auto
---


# Mac os X 系统中一些常用软件的配置

## Install Meteor for Sublime text3 
### 设置命令行快捷方式(有两种方法效果都一样)
* 如果是在默认shell下, 
```
$sudo ln -s "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl" /usr/bin/subl
```
* 放在bash_profile文件中使用以下命令
```
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"
alias nano="subl"
export EDITOR="subl"
```

### 配置Meteor
参考下面的链接.

[meteor](https://github.com/wangleihd/tern-meteor-sublime)


## GIT 自动补齐
```
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
$ brew install bash
$ brew install bash-completion
$ echo 'if [ -f $(brew --prefix)/etc/bash_completion ]; then . $(brew --prefix)/etc/bash_completion; fi' >> ~/.bash_profile
$ sudo sh -c 'echo "/usr/local/bin/bash" >> /etc/shells'
$ chsh -s /usr/local/bin/bash
```
## .bash_profile 配置
```
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
alias ll='ls -al'
```

## vim 配色方案


```
$ vim .vimrc
---------------
syntax enable
set background=light
set nu
```

## install brew
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## tree命令支持中文
使用`tree`命令, 需要支持显示中文目录
```
$ brew install uni2ascii
$ tree | ascii2uni -a K
```


## Web server 
启动mac ox10.11 自带的apache服务.

```
$ sudo apachectl start
```
目录:
> cd Library/WebServer/Documents/

## 关闭自动生成.DS_store 文件

打开 “终端” ，复制粘贴下面的命令，回车执行，重启Mac即可生效。
禁止生成的命令：(不知道需不需要加sudo执行)
```bash
 $ defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

恢复.DS_store生成：
```bash
 $ defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```
## install gcc

```
$ brew tap homebrew/versions
$ brew install gcc
$ brew install binutils
