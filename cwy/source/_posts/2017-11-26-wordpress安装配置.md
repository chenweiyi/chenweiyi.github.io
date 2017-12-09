---
title: wordpress安装配置
date: 2017-11-26 17:23:04
categories:
- 技术贴
tags:
- wordpress
- wampserver
---

今天记一下我自己使用wordpress建站的过程中遇到的一些问题。
1. 首先下载wordpress，然后解压到wamp的www目录中，在浏览器中输入localhost/wordpress/进入wordpress安装
2. 首先填写需要的数据库名称（事先在phpmyadmin中建一个数据库），数据库登陆名（默认root），数据库密码（默认为空），等等。
3. 成功后，填写网站名称，管理员名称，密码，电子邮箱
4. 安装成功后，在localhost/wordpress/wp-admin.php地址下登陆管理员用户名，密码，进入前后端预览界面。
5. 设置头像。这里我们使用一款插件:wp-user-avatar。就要用到wordpress下载插件功能了。如果下载插件报错No working transports found，
是因为php没有开启curl功能，修改方法：`将php.ini中的;extension=php_curl.dll前的分号去掉`，修改完成后，重启wamp。
6. 插件下载完毕后启用插件，进行需要的设置即可。
7. wordpress默认没有开启用户登录的功能，在设置中勾选`允许任何人注册`选项。
8. 用户注册时，需要填写电子邮箱，然后wordpress会给该用户发送电子邮件来设置密码。所以，作为管理员，需要开启发送电子邮件的功能。这时我们需要下载一款插件：`easy-wp-smtp`。启用，进入设置。
9. 首先我们得启用php的ssl功能：`将php.ini中的;extension=php_openssl.dll前的分号去掉`
10. 设置插件填写用户名密码时要注意密码不是你邮箱的登陆密码，而是开启smtp的功能的授权码。不然会报错：`535 authentication failed`
11. 填写设置成功后可以测试下，测试成功表明开启成功。
12. 用户注册的时候，发送邮件给用户，用户点击邮件后提示“您的密码重设链接无效”，这其实不是wordpress的问题，邮箱收到邮件后，会将密码重置链接地址及其前后的“<>”一起当成链接地址生成超链接，点击此超链接后，由于传给wordpress的参数不对（多了个>），例如把鼠标移到下图的红色框的连接上，并看到浏览器左下角的URL提示连接，会发现多了一个“>”，所以wordpress提示密码重设链接无效。
解决这个问题，打开wp根目录下的wp-login.php,找到如下代码:
```javascript
$message .= '<' . network_site_url( "wp-login.php?action=rp&key=$key&login=" . rawurlencode( $user_login ), 'login' ) . ">\r\n";
```
修改为：

	$message .= network_site_url( "wp-login.php?action=rp&key=$key&login=" . rawurlencode( $user_login ), 'login' ) . "\r\n";

就是把`<`,`>`去掉。还要修改wp/wp-includes/pluggable.php文件：

	$message .= '<' . network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user->user_login), 'login') . ">\r\n\r\n";

修改为：

	$message .= network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user->user_login), 'login') . "\r\n\r\n";

也是把`<`,`>`




