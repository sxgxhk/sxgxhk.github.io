---
title: Apache用.htaccess来实现强制https访问
date: 2016-12-04 21:22
tags: typecho
categories: 网站建设
permalink: 2400
---

我们可以用Apache的.htaccess来实现http强制跳转到https访问网站。
代码如下：
```
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://uu126.cn/$1 [R,L]
```


<!--more-->


如果是在子目录，可以用:
```
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteCond %{REQUEST_URI} subfolder
RewriteRule ^(.*)$ https://uu126.cn/blog [R,L]
```