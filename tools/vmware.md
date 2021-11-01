# VMWare

### share

##### Install vmware tools


  
#### 设置共享文件夹目录
 1. 将Ubuntu关机（power off），否则不能添加共享文件夹
 2. 在VMware虚拟机窗口，选择VM->Settings->Options->Shared Folders
 3. 点右边的Add，点Next->选择Win7共享目录的路径，然后点Next->选中Enable this share->Finish
 4. 在VM->Settings->Options->Shared Folders窗口的右边，Folder sharing栏里选择Always enabled
 5. 点 OK 确定退出 (但在这里还没有完成，一点要进行第三步才可以完成文件共享。)

#### 在Ubuntu虚拟机下安装插件
 1. 执行 sudo apt-get install open-vm-dkms (注：如果安装过，以后就不用执行这一行)
 2. 执行 sudo mount -t vmhgfs .host:/ /mnt/hgfs
 3. cd /mnt/hgfs
