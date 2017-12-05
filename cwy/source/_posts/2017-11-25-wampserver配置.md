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

** wamp = window + apache + mysql + php **

1. wamp的默认端口是80,容易和已安装的ISS等其他服务冲突，导致wampserver无法启动，修改httpd.conf文件如下：
搜索'80'找到`Listen 80`和`ServerName localhost:80`，修改'80'为想要设置的端口重启即可。详细参见[这篇文章](http://blog.csdn.net/paranoidyang/article/details/54293732)

2. 仅仅修改httpd.conf文件可能还不够，还需要修改wamp目录下wampmanager.tpl文件，找到`Parameters: “http://localhost/”; Glyph: 5`修改为`Parameters: “http://localhost:8080/”; Glyph: 5`（比如新端口为8080）,详细参见[这篇文章](http://blog.csdn.net/paranoidyang/article/details/54293732)

3. 修改phpmyadmin端口号：方法同2，打开wamp目录下wampmanager.tpl文件，找到`Parameters: “http://localhost/phpmyadmin”; Glyph: 5`修改为`Parameters: “http://localhost:8080/phpmyadmin”; Glyph: 5`（比如新端口为8080）,详细参见[这篇文章](http://blog.csdn.net/paranoidyang/article/details/54293732)

4. 如何修改mysql数据库的用户名密码呢？a. 通过WAMP打开mysql控制台；b. 输入`use mysql;`，意思是使用mysql这个数据库，提示“Database changed”就行；c. 输入要修改的密码的sql语句`update user set password=PASSWORD('hooray') where user='root';`; d. 输入`flush privileges;`; e. 输入`quit;`退出。详细参见[这篇文章](http://blog.csdn.net/nczb007/article/details/52593958)。做完了这些关闭wampserver,重新打开可能你还是不能进入phpmyadmin,因为你还需要把新密码手动加入到phpmyadmin配置文件中，位置在wamp/apps/phpadmin/config.inc.php,打开配置文件把密码写进去重启wamp就可以了。详细参见[这篇文章](http://blog.csdn.net/sonnylight/article/details/51900050)

5. wamp安装完成后，等图标变绿后，在浏览器输入`127.0.0.1`即可进入默认页面，输入localhost呢，如果不可以需要配置`httpd.conf`文件，如下图：
![允许all](/images/httpd.png)
修改完后，重启wamp，在浏览器中输入localhost应该可以正常访问。访问`localhost/phpmyadmin/`时依然无法访问，但是访问`127.0.0.1/phpmyadmin`可以，需要配置wamp目录下的alias目录`phpmyadmin.conf`文件：
![允许all](/images/phpmyadmin.png)

6. 一般情况下，我们把代码和程序分开，wamp默认的www目录在安装程序目录下，所以我们需要自定义www目录。还是修改`httpd.conf`目录，如下图：
![自定义www目录](/images/httpd2.png)
![自定义www目录](/images/httpd3.png)
我把目录改为d盘下的www目录，重启后输入localhost，应该可以进入d:/www这个目录的`index.php`文件。但是点击wamp图标的www还是显示默认wamp下面的www目录，因此还需要修改两个文件`wamp/wampmanager.ini`和`wampmanager.tpl` ：
![自定义www目录](/images/httpd4.png)
![自定义www目录](/images/httpd5.png)
关闭wamp，重新打开，在点击www目录，应该就是修改后的目录了。在浏览器地址栏输入localhost也会进入修改后的目录的`index.php`