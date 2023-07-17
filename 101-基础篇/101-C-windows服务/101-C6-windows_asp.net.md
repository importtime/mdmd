# IIS+asp.net

**介绍**：

​	iis是Internet Information Services的缩写，意为互联网信息服务，是由微软公司提供的基于运行Microsoft Windows的互联网基本服务。最初是Windows NT版本的可选包，随后内置在Windows 2000、Windows XP Professional和Windows Server 2003一起发行，但在Windows XP Home版本上并没有IIS。IIS是一种Web（网页）服务组件，其中包括Web服务器、FTP服务器、NNTP服务器和SMTP服务器，分别用于网页浏览、文件传输、新闻服务和邮件发送等方面，它使得在网络（包括互联网和局域网）上发布信息成了一件很容易的事。

​	ASP.NET是由微软在·NET Framework框架中所提供，开发Web应用程序的类库，封装在System.Web.dll文件中，显露出System.Web命名空间，并提供ASP.NET网页处理、扩充以及HTTP通道的应用程序与通信处理等工作，以及Web Service的基础架构。ASP.NET是ASP技术的后继者，但它的发展性要比ASP技术强大许多!

## 准备工作

1.VirtualBox虚拟机，windows server 2019服务器

2.spacebuilder v5.3的网站源码

## 安装环境

1.进入服务器管理----仪表板---添加角色和功能

 ![image-20220331160401908](../../images/image-20220331160401908.png)

2.安装向导的前三步直接默认下一步即可

 ![image-20220331160705438](../../images/image-20220331160705438.png)

3.单击.NET Framework3.5和4.7功能,勾选ASP.NET 4.7,然后点击下一步

 ![image-20220331163916416](../../images/image-20220331163916416.png)

4.在此系统给我们解释了一下web服务器（iis）角色的一些注意事项，我们直接点击下一步即可

 ![image-20220331160843286](../../images/image-20220331160843286.png)

5.下滑到底部,点击应用程序,勾选前5个选项，其他的保持默认.然后点击下一步

 ![image-20220331160943000](../../images/image-20220331160943000.png)

6.这里确定我们所要安装的角色服务没有问题，勾选如果需要自动重新启动服务器,由于存在.Net FrameWork 3.5的一些配置，需要右击虚拟机右下角的光驱图标,选择windows server2019的镜像来挂载安装镜像

![image-20220411205018337](../../images/image-20220411205018337.png)

7.查看加载镜像的挂载路径,我这边为D

 ![image-20220331161637375](../../images/image-20220331161637375.png)

8.点击指定备用源路径、填写路径为: 系统盘挂载路径:\Sources\SxS,确认无误后点击确认,安装

![image-20220331170929827](../../images/image-20220331170929827.png)

9.安装完成,点击关闭

 ![image-20220331171139219](../../images/image-20220331171139219.png)

10.powershell命令行安装IIS+.net

(1)确认Windows server 2019镜像挂载的盘符

 ![image-20220331171244511](../../images/image-20220331171244511.png)

(2)安装IIS
Install-WindowsFeature web-server -Source D:\sources\sxs

 ![image-20220331171312645](../../images/image-20220331171312645.png)

(3)安装.net支持
Install-WindowsFeature web-asp,web-asp-net,web-asp-net45,Web-Net-Ext,Web-Net-Ext45 -Source D:\sources\sxs

 ![image-20220331171400080](../../images/image-20220331171400080.png)

(4)安装iis控制台
Install-WindowsFeature Web-Mgmt-Console -Source D:\sources\sxs

 ![image-20220331171726872](../../images/image-20220331171726872.png)

13.打开浏览器,输入127.0.0.1出现IIS默认的页面即为安装成功

 ![image-20220331201122837](../../images/image-20220331201122837.png)

## 了解IIS服务器

1.打开服务器管理器,点击右上角的工具,单击第一个iis管理器

 ![image-20220331201241039](../../images/image-20220331201241039.png)

2.先删除默认的网站来测试功能

 ![image-20220331201322703](../../images/image-20220331201322703.png)

3.在网站处右键,点击添加网站来添加一个新的网站，填写网站名称为farmsec,点击物理路径框后的按钮,选择网站的物理路径

 ![image-20220331201717175](../../images/image-20220331201717175.png)

4.点击测试设置,会报错为无法验证,这说明IIS没有对该文件的权限,这时候我们点击连接为来设置IIS使用的用户

 ![image-20220331201814832](../../images/image-20220331201814832.png)

5.选择特定用户,点击设置,在弹出的设置凭据中输入超级管理员的用户名和密码;在生产环境下这样设置是不严谨的,需要把文件夹授权给普通用户,然后设置普通用户为网站使用用户,这样可以权限最小化.实验环境无所谓,依次点击确定即可

 ![image-20220331202356432](../../images/image-20220331202356432.png)

6.再次点击测试设置已经没有了报错,点击关闭

 ![image-20220331202422969](../../images/image-20220331202422969.png)

7.在IP地址选项框内选择内网IP,点击确认就完成了网站的建设

 ![image-20220407180058891](../../images/image-20220407180058891.png)

8.打开c盘wwww文件夹,创建一个index.html,其内容为farmsec is ok!，打开浏览器,浏览http://ip,  

即可查看网站内容,由于第九步设置了ip所以不能使用127.0.0.1访问了,网站搭建成功

![image-20220407180048995](../../images/image-20220407180048995.png)

## 网站搭建的几种方式:

同IP不同端口

同IP同端口不同主机头

## 实验一:同IP不同端口(节省资源)

1.创建两个网站,使用同一个IP,填写不同的端口(80与81)

 ![image-20220407180332989](../../images/image-20220407180332989.png)

 ![image-20220407180058891](../../images/image-20220407180058891.png)

2.在两个目录下都新建index.html,分别写入内容为This is aaa!与This is bbb!

![image-20220405123504066](../../images/image-20220405123504066.png)

3.浏览器访问http://ip:80,   与http://ip:81,   显示文字与设置一致即为成功

 ![image-20220407180431990](../../images/image-20220407180431990.png)

 ![image-20220407180445158](../../images/image-20220407180445158.png)

## 实验二:同IP同端口不同主机头

1.创建两个网站(farmsec.com, farmsec.cn),使用同一个IP,填写相同的的端口(80),填写不同的主机名

 ![image-20220407180642432](../../images/image-20220407180642432.png)

 ![image-20220407180956314](../../images/image-20220407180956314.png)

2.在两个目录下都新建index.html,分别写入内容This is www!与ok! , 然后用浏览器访问http://IP/, 先测试一下,发现打不开,因为IIS设定填写主机名后就不可以使用IP进行访问了QWQ

 ![image-20220407180801541](../../images/image-20220407180801541.png)

3.修改c:\windows\system32\drivers\etc\hosts文件,添加解析记录

 ![image-20220407180929247](../../images/image-20220407180929247.png)

5.览器打开http://farmsec.com,  和http://farmsec.cn,  显示内容为设置内容即为成功

 ![image-20220407181104395](../../images/image-20220407181104395.png)![image-20220407181120449](../../images/image-20220407181120449.png)

二、测试asp解析
1.在farmsec.cn网站在目录中创建文件1.aspx修改其内容为:

```
<%@ Page Language="C#"%> <% Response.Write("hello,world"); %> (源码文件包中的1.aspx)
```

 ![image-20220331210656707](../../images/image-20220331210656707.png)

2.打开浏览器,输入http://farmsec.cn,  发现.net语言成功解析

 ![image-20220407181326658](../../images/image-20220407181326658.png)

三、安装asp.net站点
1.把asp源代码粘贴到网站根目录下

 ![image-20220331213252743](../../images/image-20220331213252743.png)

2.在浏览器访问页面：http://farmsec.cn

 ![image-20220407181553086](../../images/image-20220407181553086.png)

3.按照提示启动向导，点击下一步，进行安装,发现报错,修改文件属性

 ![image-20220331213900341](../../images/image-20220331213900341.png)

4.给网站根目录文件夹设置权限，在www文件夹处右键属性->安全->编辑->添加

 ![image-20220331214250148](../../images/image-20220331214250148.png)

5.添加NETWORK SERVICE用户

 ![image-20220331214422209](../../images/image-20220331214422209.png)

6.设置用户权限为读写或者完全控制，并设置web.config文件users用户完全控

 ![image-20220331214653564](../../images/image-20220331214653564.png)



7.点击重新检测发现没有报错了,点击下一步就好了

 ![image-20220331214743888](../../images/image-20220331214743888.png)

8.设置所连接的数据库信息（使用之前安装的sql server 2019即可 密码为当初设置的密码），点击安装

 ![image-20220331214815945](../../images/image-20220331214815945.png)

9.设置管理员账户，点击执行

 ![image-20220331214858994](../../images/image-20220331214858994.png)

10.完成安装，可以访问站点了~

 ![image-20220331215018798](../../images/image-20220331215018798.png)

11.前台能访问即为成功

 ![image-20220331215115756](../../images/image-20220331215115756.png)

