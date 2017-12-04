---
title: wampserver配置
date: 2017-11-25 20:43:16
categories:
- 技术贴
tags:
- wampserver
- php
- mysql
- apache
---

# wamp配置

** wamp = window + apache + mysql + php **

wamp的默认端口是80,容易和已安装的ISS等其他服务冲突，导致wampserver无法启动，修改httpd.conf文件如下：
搜索'80'找到`Listen 80`和`ServerName localhost:80`，修改'80'为想要设置的端口重启即可。

wamp安装完成后，等图标变绿后，在浏览器输入`127.0.0.1`即可进入默认页面，输入localhost呢，如果不可以需要配置`httpd.conf`文件，如下图：
![允许all](/images/httpd.png)
修改完后，重启wamp，在浏览器中输入localhost应该可以正常访问。访问`localhost/phpmyadmin/`时依然无法访问，但是访问`127.0.0.1/phpmyadmin`可以，需要配置wamp目录下的alias目录`phpmyadmin.conf`文件：
![允许all](/images/phpmyadmin.png)
一般情况下，我们把代码和程序分开，wamp默认的www目录在安装程序目录下，所以我们需要自定义www目录。还是修改`httpd.conf`目录，如下图：
![自定义www目录](/images/httpd2.png)
![自定义www目录](/images/httpd3.png)
我把目录改为d盘下的www目录，重启后输入localhost，应该可以进入d:/www这个目录的`index.php`文件。但是点击wamp图标的www还是显示默认wamp下面的www目录，因此还需要修改两个文件`wamp/wampmanager.ini`和`wampmanager.tpl` ：
![自定义www目录](/images/httpd4.png)
![自定义www目录](/images/httpd5.png)
关闭wamp，重新打开，在点击www目录，应该就是修改后的目录了。在浏览器地址栏输入localhost也会进入修改后的目录的`index.php`