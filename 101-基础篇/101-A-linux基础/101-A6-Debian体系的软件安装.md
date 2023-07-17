# 101-A6-Debian体系的软件安装

在前文中，我们了解到linux系统的发行版主要分为debian和redhat两个体系。

两个体系最直观的不同是软件安装命令的不同。这一节，我们要来学习debian体系的软件安装。

前文我们使用的Kali系统，就以本节所介绍的命令来管理软件的安装与卸载。



## 1. dpkg 命令

`dpkg` 是为"Debian"操作系统专门开发的套件管理系统，用于软件的安装，更新和移除。能被`dpkg`命令安装的软件包一般以`.deb`为文件后缀。

```
dpkg -i     # 安装软件包
dpkg -r     # 移除软件（保留配置）
dpkg -P     # 移除软件（不保留配置）
dpkg -c     # 列出deb包的内容
dpkg -l     # 配合|grep，查找主机包
dpkg -s     # 查找包的详细信息
dpkg -L     # 查看已安装的软件包，都存在系统哪有文件
dpkg -S     # 显示指定包的状态信息
```



## 2. apt 命令

`apt`是一个在Debian中的Shell前端软件包管理器。

`apt`命令提供了查找、安装、升级、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

`apt`命令执行需要超级管理员权限(root)

### 2.1 源的配置

`apt`命令更新和安装软件包是从软件安装源中请求的。

kali系统的源文件在` /etc/apt/sources.list`。

常见kali源：

```
#官方源
deb http://http.kali.org/kali kali-rolling main contrib non-free
#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#阿里云
deb http://mirrors.a/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
```

可以编辑`/etc/apt/sources.list`文件，将源切换成需要的源。

修改之后，需要通过`apt update`命令来更新源。

### 2.2  apt 命令

```
apt-get install      #安装软件包                
apt-get remove       #仅卸载软件，但是并不卸载配置文件
apt-get purge        #卸载指令，同时卸载相应的配置文件
apt-get update       #将所有包的来源更新（更新源）
apt-get upgrade      #将系统中旧版本的包升级成最新的
apt-cache search     #用关键字搜索包
apt-cache show       #显示特定包的基本信息
apt-cache depends    #列出包的依赖
apt-get clean        #清理本地包占用的磁盘空间(/var/cache/apt/archives)
apt-get autoremove   #卸载软件的时候同时卸载那些当初作为依赖但是现在并不需要的包
```









