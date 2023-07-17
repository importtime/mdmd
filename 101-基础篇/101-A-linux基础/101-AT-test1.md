# 101-AT-test1

如果你已经熟练掌握101-A单元的内容，那么你可以挑战以下的问答和操作。

本测试需要准备好一台centos虚拟机。

运用所学知识完成以下题目，可以对照你自己的学习笔记，答题时间为2个小时。



```bash
1.kali源的配置文件全路径
2.后缀为deb以及rpm的安装包安装命令分别为？
3.在kali安装virtualbox时，需要首先查看本机内核，命令为？查看最新内核的命令为？
4.使用root用户身份远程连接服务器（Centos虚拟机，此后操作在centos下完成）。
5.查看selinux状态
6.确认防火墙状态，并且设置为“开机自启”
7.建立用户farmsec，并指定用户uid为0，且用户组为farmsec1
8.退出root用户，使用farmsec身份远程连接服务器，并确定用户身份
9.在家目录建立名为farma的文件夹
10.查看新建立的文件夹的权限及属主属组
11.将/etc/shadow文件拷贝到farm文件夹中，并改名为test
12.使用绝对路径的方式进入到farm文件夹中
13.使用相对路径进入到farm文件夹中
14.查看test文件内容
15.查看test文件的前3行
16.查看test文件的后5行
17.查看test文件的第7到10行
18.查看系统的全部进程并将结果保存到/var/farm目录中，命名为jc.txt
19.在jc.txt中，文件尾部添加一行内容：coding by farmsec! (至少2种方式)
20.查看系统的进程，并找出与ssh相关的内容，结束除/usr/sbin/sshd外所有ssh进程，并确认是通过服务器端断开的访问连接.（使用一行命令）
21.在系统中另建立一个名为farm1的用户,使其拥有sudo权限
22.切换至farm1用户
23.将farm文件夹中所有文件的属主改为farm1，属组改为farmsec1
24.查找系统中名字以pass字段开头的文件(多种方式)
25.查找/var目录中的文件，且过去一天内被修改过权限
26.查找/etc目录中，文件包含password内容的文件 (多种方式)
27.使用一行命令提取主机中的所有可登录用户，并按照字母正常顺序排序，结果保存至家目录中的user.txt中
28.查找主机中所有uid为0的用户
29.将test文件中的所有:替换为+
30.切割test文件保存为每5行为一个文件
31.安装名为nginx的服务，并启动该服务
32.配置一个计划任务，用户root，每隔5小时的第46分钟执行命令echo hello >/root/hello.txt
33.在文件/etc/passwd中查找所有包含字符串oo或者ai的行。将找出的行按照原文的先后顺序拷贝到/root/cc文件中。
34.删除掉/var/log/audit/audit.log,然后数据恢复到/opt中
35.查看服务名为Nginx所对应的进程id，再通过查询到的该id查找该进程打开的文件
36.查看网络连接状态，并提取出来源IP以及PID号（一行命令）
37.你现在有个一个文本文件，它的内容如下：

http://www.sina.com
https://www.sohu.com
http://www.163.com

通过Linux的命令，将文本处理成如下格式（可尝试多种写法）：

www.sina.com/login.action
www.sohu.com/login.action
www.163.com/login.action
```

