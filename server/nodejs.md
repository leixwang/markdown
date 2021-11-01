# nodejs安装


## ubuntu install 
安装nodejs的方法.

```bash
 $ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - 
 $ sudo apt-get install -y nodejs
```

开发环境的安装:
```bash
 $ sudo apt-get install -y build-essential
```


### node.js升级


node有一个模块叫 n，是专门用来管理 node.js 的版本安装并将当前的nodejs版本升级为stable最新版本

```bash
$ npm install -g n        //安装nodejs
$ sudo n stable           //升级nodejs到stable版本
```
显示当前nodejs版本信息.

```bash
 $ node -v
```

### nvm使用
总的来说，nvm有点类似于 Python 的 virtualenv 或者 Ruby 的 rvm，每个node版本的模块都会被安装在各自版本的沙箱里面（因此切换版本后模块需重新安装），因此考虑到需要时常对node版本进行切换测试兼容性和一些模块对node版本的限制，我选择了使用nvm作为管理工具，下面就来说说nvm的安装和使用过程。

#### 安装nvm

通下面的网址可以找到 nvm 最新的版本. [nvm最新的版本](https://github.com/creationix/nvm)

```bash
 $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash
```
完成后nvm就被安装在了 ~/.nvm 下啦，接下来就需要配一下环境变量了，如果你也使用了 zsh 的话，就需要在 ~/.zshrc 这个配置文件中配置，否则就找找看 ~/.bash_profile 或者 ~/.profile 吧。

打开 ~/.zshrc ，在最后一行加上：
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```
这一步的作用是每次新打开一个bash，nvm都会被自动添加到环境变量中了。

完成后输入 source ~/.zshrc 重新启动一下配置。

输入 nvm 可以看到如下信息：
```
➜  ~  nvm

Node Version Manager

Note: <version> refers to any version-like string nvm understands. This includes:
  - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
  - default (built-in) aliases: node, stable, unstable, iojs, system
  - custom aliases you define with `nvm alias foo`

Usage:
  nvm help                                  Show this message
  nvm --version                             Print out the latest released version of nvm
  nvm install [-s] <version>                Download and install a <version>, [-s] from source. Uses .nvmrc if available
    --reinstall-packages-from=<version>     When installing, reinstall packages installed in <node|iojs|node version number>
  nvm uninstall <version>                   Uninstall a version
  nvm use [--silent] <version>              Modify PATH to use <version>. Uses .nvmrc if available
  nvm exec [--silent] <version> [<command>] Run <command> on <version>. Uses .nvmrc if available
  nvm run [--silent] <version> [<args>]     Run `node` on <version> with <args> as arguments. Uses .nvmrc if available
  nvm current                               Display currently activated version
  nvm ls                                    List installed versions
  nvm ls <version>                          List versions matching a given description
  nvm ls-remote                             List remote versions available for install
  nvm version <version>                     Resolve the given description to a single local version
  nvm version-remote <version>              Resolve the given description to a single remote version
  nvm deactivate                            Undo effects of `nvm` on current shell
  nvm alias [<pattern>]                     Show all aliases beginning with <pattern>
  nvm alias <name> <version>                Set an alias named <name> pointing to <version>
  nvm unalias <name>                        Deletes the alias named <name>
  nvm reinstall-packages <version>          Reinstall global `npm` packages contained in <version> to current version
  nvm unload                                Unload `nvm` from shell
  nvm which [<version>]                     Display path to installed node version. Uses .nvmrc if available

Example:
  nvm install v0.10.32                  Install a specific version number
  nvm use 0.10                          Use the latest available 0.10.x release
  nvm run 0.10.32 app.js                Run app.js using node v0.10.32
  nvm exec 0.10.32 node app.js          Run `node app.js` with the PATH pointing to node v0.10.32
  nvm alias default 0.10.32             Set default node version on a shell

Note:
  to remove, delete, or uninstall nvm - just remove the `$NVM_DIR` folder (usually `~/.nvm`)

```


### meteor 1.30部署
需要用到nodejs 5.3.0版本, 我们先需要安装这个版本的nodejs.

安装node.js 5.3.0版本:
```bash
 $ nvm install v5.3.0
```

切换当前的nodejs版本.
```bash
 $ nvm use 5.3.0
```
