---
title: wordpress优化
date: 2017-12-09 18:31:42
categories:
- 技术贴
tags:
- wordpress
---

wordpress优化主要从下面几个方面着手：
1. wordpress头像。它自带的头像服务国内用户访问受限，因此需要一款安装插件解决这个问题：`wp-user-avatar`
2. wordpress字体。自带访问google字体，由于国内被墙因此会造成很大的阻塞，影响访问速度。下载一款插件解决吧：`Disable Google fonts`；也可以通过代码设置的方式解决，参见[这篇文章](https://www.xiaoz.me/archives/3335)
3. wordpress启用用户注册功能。 需要安装一款smtp的插件：`easy-wp-smtp`。安装完启用，设置配置项即可。需要注意的是：SMTP username是邮箱服务器的登陆名，SMTP password是**开启smtp的授权码而不是你邮箱的登录密码**
