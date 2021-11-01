# vscode 使用





## 命令行

在 `mac osx` 下用命令启动 vscode 编辑器.

bash，写入 ~/.bash_profile
zsh, 写入 ~/.zshrc

在里面以上相应文件中写入

```sh
code () { VSCODE_CWD="$PWD" open -n -b "com.microsoft.VSCode" --args $* ;}

```
