# 101-B8-firewalld

## 一  Firewalld

### 1 firewalld简介

防火墙主要用于保护一个网络区域免受来自另一个网络区域的网络攻击和网络入侵行为。因其隔离、防守的属性，灵活应用于网络边界、子网隔离等位置，具体如企业网络出口、大型网络内部子网隔离、数据中心边界等等。

Firewalld提供了支持网络/防火墙区域(zone)定义网络链接以及接口安全等级的动态防火墙管理工具。



### 2 firewalld服务控制

firewalld在EL7之后系列已默认安装

firewalld服务的启动与关闭一般涉及到如下命令：

```
systemctl start firewalld            #启动firewalld服务
systemctl stop 	firewalld            #关闭firewalld服务
systemctl restart firewalld          #重启firewalld服务
systemctl status  firewalld          #查看firewalld服务状态
firewall-cmd --state                 #查看firewalld服务状态
systemctl disable firewalld          #firewalld开机自动关闭
systemctl enable  firewalld          #firewalld开机自动开启
```

注意：其中`systemctl status firewalld`和`firewall-cmd --state`都是查看firewalld的状态，两则区别在于`systemctl status firewalld`是查看服务启动详情，如出服务运行出现问题可以使用此命令进行查看，而`firewall-cmd --state`只是查看firewalld是否运行。



### 3 Firewalld 区域（zone）

firewalld加入了区域概念，区域可以理解为一个信赖等级，将一些配置好策略的模板划分为9种，每个模板的规则都不相同，同样每个模板中的过滤强度也不一样。每个区域除默认的规则，也可以添加或者修改删除实际环境所需要的规则。

firewalld区域默认有9种，以下对各个默认配置进行简单的解释以供参考：

| 区域     | 默认配置介绍                                                 |
| -------- | ------------------------------------------------------------ |
| trusted  | 允许所有的数据包进出                                         |
| home     | 专门用于家庭环境，仅接收ssh、mdns、ipp-client、samba-client与dhcpv6-client等服务流量 |
| Internal | 只有通过被允许的连接，和home区域一样。                       |
| work     | 定义内部网络，仅接收ssh、ipp-client与dhcpv6-client等服务流量。 |
| public   | 只接受那些被允许的连接，默认只允许 ssh 和 dhcpv6-client，这个也是默认区域。 |
| external | 相当于路由器的启用伪装（masquerading）选项。只有指定被允许的连接会被接受，默认只有SSH |
| dmz      | 仅接受ssh服务连接                                            |
| block    | 拒绝所有网络连接                                             |
| drop     | 拒接所有的网络连接，并且所有数据包都给丢弃，没有任何回复。   |

在Linux中，可通过如下的指令进行查看各个不同zone的区别

```
firewall-cmd --list-all-zones
```



### 4.Firewalld常用命令介绍及规则配置

4.1 查看firewalld服务版本以及查看帮助文档

```
firewall-cmd --version   #查看firewalld版本
firewall-cmd --help 	 #查看firewall-cmd用法
man firewall-cmd         #查看帮助手册
```

4.2 更新并加载firewalld的配置

```
firewall-cmd --reload           #重新载入防火墙配置，当前连接不中断
firewall-cmd --complete-reload 	#重新载入防火墙配置，当前连接中断
```

4.3 查看区域详情及指定默认区域等相关命令

```
firewall-cmd --get-zones                              #查看所有区域
firewall-cmd --list-all-zones                         #列出所有区域的所有配置
firewall-cmd --get-active-zones                       #查看默认区域,并显示分配给它们的接口列表
firewall-cmd --get-default-zone                       #查看默认是哪个区域
firewall-cmd --list-all                               #查看默认区域的所有配置
firewall-cmd --set-default-zone=public                #设定默认区域
firewall-cmd --zone=work --list-all                   #列出指定域的所有配置
firewall-cmd --get-zone-of-interface=enp0s3           #查看指定接口所属区域
```

4.4 拒绝及允许所有入站请求包，并查看是否拒绝

```
firewall-cmd --query-panic    #查看是否拒绝
firewall-cmd --panic-on       #拒绝所有包
firewall-cmd --panic-off      #取消拒绝状态
```

4.5 添加及删除服务，并且查看区域预设服务及某个区域加载的服务

```
firewall-cmd --get-services                             #查看所有区域预设服务
firewall-cmd --add-service=http                         #在默认区域下添加http服务
firewall-cmd --remove-service=http                      #在默认区域下删除http服务
firewall-cmd --list-services                            #查看默认区域加载了哪些服务
firewall-cmd --zone=work --add-service=http             #在指定的区域中添加了http服务
firewall-cmd --zone=work --remove-service=http          #在指定的区域中删除了https服务
firewall-cmd --zone=work --list-services                #查看指定区域中加载了哪些服务
```

4.6 添加及删除某个端口并且查看默认区域或者指定区域加载了哪些端口

```
firewall-cmd --add-port=445/tcp                     #在默认区域下添加445/tcp端口
firewall-cmd --remove-port=445/tcp                  #在默认区域下添加445/tcp端口
firewall-cmd --list-ports                           #查看默认区域加载的端口
firewall-cmd --zone=work --add-port=445/tcp         #在指定区域下添加445/tcp端口
firewall-cmd --zone=work --remove-port=445/tcp      #在指定去下删除445/tcp端口
firewall-cmd --zone=work --list-ports               #查看指定区域加载的端口
firewall-cmd --zone=work --add-port=3000-4000/tcp   #在指定区域中批量添加端口
```

4.7 禁止某ip或网段进行访问（黑名单）

```
firewall-cmd --zone=block --add-source=192.168.10.62            #禁止某个IP访问
firewall-cmd --zone=block --add-source=192.168.10.0/24          #禁止某个网段访问
firewall-cmd --zone=drop  --add-source=192.168.10.62            #禁止某个IP访问，并且丢弃所有数据包
firewall-cmd --zone=block --add-source=b0:6e:bf:c8:0b:bc        #禁止某个MAC地址访问
```

4.8 runtime-to-permanent属性

​	 firewalld的一大特性是就是规则可以动态更新，在输入所有更新规则的命令后都会及时生效，但是更新的规则只是临时生效并不会永久保存，只需要重启服务或者系统就可以恢复到原来的配置。

而runtime-to-permanent属性,它可以让在更新规则后永久保存，但是不会及时生效，需要执行`firewall-cmd --reload`命令手动加载，而`firewall-cmd --reload`命是动态加载也可以理解为在线更新，并不会断开连接重新启动。

runtime-to-permanent属性使用方法：

```
firewall-cmd --permanent <other options>
```

永久添加http服务到public区域：`firewall-cmd --permanent --zone=public --add-service=http`

此时http并未添加成功，需要进行手动加载：`firewall-cmd --reload`



### 5.Rich rule

#### 	5.1 rich rule简介

​	当基本firewalld语法规则不能满足要求时，可以使用rich-rules 富规则，功能强,表达性语言 rich规则比基本的firewalld语法实现更强的功能，不仅实现允许/拒绝，还可以实现日志syslog和auditd，也可以实现端口转发，伪装和限制速率。

​	基本语法：	

| 语法                      | 解释               |
| ------------------------- | ------------------ |
| --add-rich-rule=’rule’    | ##新建rich规则     |
| --remove-rich-rule=’rule’ | ##删除rich规则     |
| --query-rich-rule=’rule’  | ##查看单条rich规则 |
| --list-rich-rules         | ##查看rich规则列表 |

#### 	5.2 rich rule常用配置

5.2.1 利用rich rule设置禁止ping响应

```
firewall-cmd --add-rich-rule='rule protocol value=icmp drop'       #设置所有icmp协议请求都给丢弃
firewall-cmd --remove-rich-rule='rule protocol value=icmp drop'    #删除上条规则
```

5.2.2 利用rich rule设置拒绝或者允许某个IP、IP段访问某个服务或者端口

```
#拒绝个IP访问ssh
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 service name="ssh" reject'
#丢弃某个ip访问ssh的数据包
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 service name="ssh" dorp'
#允许某个IP访问ssh
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 service name="ssh" accept'

#拒绝某个IP访问22端口
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 port port=22 protocol=tcp reject'
#丢弃某个IP访问22端口的数据包
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 port port=22 protocol=tcp dorp'
#允许某个IP访问22端口
firewall-cmd --add-rich-rule='rule family="ipv4" source address=192.168.10.62 port port=22 protocol=tcp accept'

#如果要配置IP段，在address= 添加如：192.168.10.0/24
```

5.2.3 利用rich rule设置某条IP可以通过防火墙或者允许某个IP仅能通过指定端口访问到本机

```
#允许某条IP通过防火墙访问本机
firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.10.62" accept'
#允许某个IP通过（3000-4000）端口访问到本机
firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.10.62" destination address="127.0.0.1" port port="3000-4000" protocol="tcp" accept'
```

### 6. ipset

6.1 ipset简介

​	 当防火墙需要处理的IP、端口、MAC等比较复制且数量庞大时，可以使用ipset来进行处理。

​	 ipset可以存储多个IP地址或端口号，且在不影响性能的同时可以对IP或者端口等进行动态更新。

6.2 ipset使用

6.2.1 查看ipset都支持哪些类型，新建一个set并且指定类型。

创建的set在`/etc/firewalld/ipsets`目录生成了一个XML文件，这是对应set存储文件。

```
firewall-cmd --get-ipset-types                             #查看ipset支持类型
firewall-cmd --permanent --new-ipset=test1 --type=hash:ip  #新建一个set命名为test1，并且指定类型
firewall-cmd --permanent --delete-ipset=test1              #删除一个set
```

 6.2.2 在set中添加及删除ip

```
firewall-cmd --permanent --ipset=test1 --add-entry=192.168.10.62          #在set中添加IP
firewall-cmd --permanent --ipset=test1 --add-entry=192.168.10.10-100      #在set中批量添加IP
```

在set中添加ip，并且查看是否添加成功：`firewall-cmd --permanent --ipset=test1 --add-entry=192.168.10.16`；`more /etc/firewalld/ipsets/test1.xml`

删除set中的IP：`firewall-cmd --permanent --ipset=test1 --remove-entry=192.168.10.62`

6.2.3 打印一个set的路径以及set的内容

```
firewall-cmd --permanent --path-ipset=test1                     #打印set的路径
firewall-cmd --permanent --info-ipset=test1                     #打印set的内容
firewall-cmd --permanent --ipset=test1 --get-entries            #打印set中所有的entry
firewall-cmd --permanent --ipset=test1 --query-entry=8.8.8.8	#判断一个IP，set中是否存在
firewall-cmd --permanent --get-ipsets                           #列出所有set
```

6.2.4 结合脚本使用IPset

以登录失败的ip为例，把登录失败的IP存放至ipset中。

```
#!/bin/bash
for LINE in `grep -i 'Failed password' /var/log/secure | awk '{print $11}' | sort -n | uniq -c | sort -k1nr | awk '{if ($1>3) print $2}'`; do
    echo "${LINE}";
    firewall-cmd --permanent --ipset=test1 --add-entry="${LINE}";
done;
firewall-cmd --reload;
```

6.2.5 使ipset中的ip禁止访问

```
firewall-cmd --permanent --add-rich-rule 'rule family="ipv4" source ipset="test1" drop'
firewall-cmd --reload
```



### 7.利用firewalld进行端口转发及IP伪装

端口转发可以将指定地址访问指定的端口时，将流量转发至指定地址的指定端口。

转发的目的如果不指定ip的话就默认为本机。如果指定了ip却没指定端口，则默认使用来源端口。

```
# 将5555端口的流量转发至22端口。
firewall-cmd --add-forward-port=port=5555:proto=tcp:toport=22

# 将ssh的流量请求，直接转发给指定的ip地址。如果未指定端口，会默认访问这个ip地址的22端口。
firewall-cmd --add-forward-port=port=5555:proto=tcp:toaddr=192.168.10.84:toport=22
```

利用firewalld的端口转发功能，使C通过A的5555端口登录到B的ssh服务，需要使用到以下命令：

```
# 检查是否允许伪装IP
firewall-cmd --query-masquerade 

# 禁止防火墙伪装IP
firewall-cmd --remove-masquerad

# 允许防火墙伪装IP
firewall-cmd --add-masquerade

# 将A的5555端口流量转发到B的22端口上
firewall-cmd --add-forward-port=port=5555:proto=tcp:toaddr=192.168.10.84:toport=22
```

如果需要使用将本机的端口流量转发至指定IP的端口上，需要开启firewalld的ip伪装功能，即masquerade功能。

区域中的masquerade显示状态为yes，表示开启。

使用C通过A的5555端口登录SSH，查看登录后的IP发现为B的IP，表示端口转发成功。



