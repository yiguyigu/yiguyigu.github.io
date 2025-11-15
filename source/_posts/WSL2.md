---
title: How to use WSL2
date: 2025-04-20
tags: WSL 
category: Technical stack
---

# WSL2

## 一、前置条件

- 开启CPU虚拟化
- 开启两个Windows功能
  - 子系统
  - 虚拟机平台（Windows11家庭版没有）

tip：在win11家庭版开启子系统后虚拟机无法使用，wsl和vm二选一

## 二、命令

```bash
wsl --install 			#默认下载ubuntu，后面添加支持版本名称下载对应的
wsl --list --online		#显示支持的Linux版本
wsl --list -v			#显示已经安装的Linux
wsl --set-defaule		#设置wsl默认打开的系统
wsl --unregister <system name>	#卸载
wsl --export <system name> <name.tar>	#备份系统（导出系统）
wsl --import <name>	<target_path> <.tar_path>	#导入
```

tip：开启VPN下载



# 使用Windows下的WSL2搭建编译与开发环境

[适用于 Linux 的 Windows 子系统文档 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/)



## 1. 安装 WSL 命令


现在，可以使用单个命令安装运行 WSL 所需的一切内容。 在管理员模式下打开 PowerShell 或 Windows 命令提示符，方法是右键单击并选择“以管理员身份运行”，输入 wsl --install 命令，然后重启计算机。



```
wsl --install
```


此命令将启用运行 WSL 并安装 Linux 的 Ubuntu 发行版所需的功能。 （[可以更改此默认发行版](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands#install)）。
如果你运行的是旧版，或只是不想使用 install 命令并希望获得分步指引，请参阅[旧版 WSL 手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)。
首次启动新安装的 Linux 发行版时，将打开一个控制台窗口，要求你等待将文件解压缩并存储到计算机上。 未来的所有启动时间应不到一秒。

## 2. 如果担心C盘空间不够，可以迁移Ubuntu18.04到D盘

**1） 停止正在运行的wsl**
wsl --shutdown


**2）将需要迁移的Linux，进行导出(ps: 其实就是相当于备份)**

```
#查看安装的Linux的name,如下图所示 wsl -l -v
```

```
#导出到D盘下的一个文件中 wsl --export Ubuntu-18.04 D:\ubuntu18.04\export.tar
```


**3）导出完成之后，将原有的Linux卸载(ps: 也可以不用卸载)**

```
#卸载 wsl --unregister Ubuntu-18.04
```



**4） 然后将导出的文件放到需要保存的地方，进行导入即可**

```
wsl --import Ubuntu-miivii D:\ubuntu18.04\ D:\ubuntu18.04\export.tar --version 2
```


**5) 使用命令进入复制好的系统中**

```
wsl --distribution Ubuntu-miivii
```


这个时候会发现一个问题就是默认进入的是root用户，而不是我们自己创建的用户，如果你在之前有创建过用户的话，并且想继续使用它的话，那么这里你需要使用下面的方式指定默认的用户

```
vim /etc/wsl.conf #复制下面的内容到文件里面去 [user] default = [your username]
```

保存退出即可

## 3. 在 适用于 Linux 的 Windows 子系统 上运行 Linux GUI 应用


运行 Linux GUI 应用
可从 Linux 终端运行以下命令，下载并安装这些常用的 Linux 应用程序。 如果使用的是不同于 Ubuntu 的发行版，则它可能使用与 apt 不同的包管理器。 安装 Linux 应用程序后，可在“开始”菜单中的发行版名称下找到它。 例如：
 备注
更新发行版中的包

```
sudo apt update
```



**安装 Gedit**
Gedit 是 GNOME 桌面环境的默认文本编辑器。

```
sudo apt install gedit -y
```



若要在编辑器中启动 bashrc 文件，请输入：gedit ~/.bashrc
**安装 GIMP**
GIMP 是一种免费的开源光栅图形编辑器，用于图像操作和图像编辑、自由形态绘图、不同图像文件格式之间的转码，以及更专业的任务。

```
sudo apt install gimp -y
```

若要启动，请输入：gimp
**安装 Nautilus**
Nautilus 也称为 GNOME Files，是 GNOME 桌面的文件管理器。 （类似于 Windows 文件资源管理器）。

```
sudo apt install nautilus -y
```


若要启动，请输入：nautilus


**安装 VLC**
VLC 是一种免费的开源跨平台多媒体播放器和框架，可播放大多数多媒体文件。

```bash
sudo apt install vlc -y
```


若要启动，请输入：vlc


**安装 X11 应用**
X11 是 Linux 窗口管理系统，这是随它一起提供的各种应用和工具的集合，例如 xclock、xcalc 计算器、用于剪切和粘贴的 xclipboard、用于事件测试的 xev 等。有关详细信息，请参阅 [x.org 文档](https://www.x.org/wiki/UserDocumentation/GettingStarted/)。

```bash
sudo apt install x11-apps -y
```



**安装SDKManager**

```bash
#下载好sdkmanager的deb包 
sudo apt update sudo dpkg -i xxx.deb 
sudo apt-get -f install 
sudo dpkg -i xxx.deb #启动sdkmanager sdkmanager
```



具体使用流程可以参考文档：[Windows Subsystem for Linux :: NVIDIA SDK Manager Documentation](https://docs.nvidia.com/sdk-manager/wsl-systems/index.html)

## 4. 使用USB设备,其实就是刷机时候需要使用

参考下面的链接

[连接 USB 设备 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/connect-usb)



## 5. 搭建开发和编译环境

以搭建APEX Xavier II PLUS开发环境为例

1. #### 刷机环境搭建

   1. [按照文档步骤进行刷机环境搭建](http://wiki.miivii.com/x/SyCWAQ)

2. #### Linux kernel 编译环境搭建

```bash
#安装依赖环境和依赖包 
sudo apt -y install build-essential bc 
sudo apt -y install flex 
sudo apt -y install bison 
sudo apt-get -y install qemu-user-static 
sudo apt-get -y install libxml2-utils 
sudo apt-get -y install libssl-dev 
sudo apt-get -y install python
```

http://wiki.miivii.com/x/oYmBAw

具体环境搭建可以参考上面的链接，已经写的很详细了，这里附加一些需要注意的地方

1. **在执行完env.sh脚本之后，请在相同的目录下去执行install-environment.sh脚本**

```
#请按照提示进行用户名和密码的输入，它将去配置你的刷机环境以及交叉编译环境工具 ./patches/build-scripts/sop/install-environment.sh 
```

   

2. **在切换pathchs分支的时候，需要注意使用下面的命令**

   ```bash
   #使用下面的git checkout 切换方式，env脚本才能正常执行，其中本地分支名需要去掉远程分支名的origin即可 
   git checkout -b 本地分支名 远程分支名 
   #以APEX II PLUS为例 
   git checkout -b apex2plus/jetpack4.6.1-kernel origin/apex2plus/jetpack4.6.1-kernel
   ```
   
   







