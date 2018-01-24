---
title: Nginx强制带www到www后跳404的解决方法
date: 2016-12-06 16:07
tags: typecho
categories: 网站建设
permalink: 2401
---

使用Oneinstack搭建了Typecho博客，因为博客一直用的都是不带www访问的，带www的也可以访问，这不看到其它小伙伴们都是强制带www到www的形式访问的，自己也想整个，可是一直都不行，主机配置文件和伪静态都是用的Oneinstack提供的，主机配置里也开启了强制Https且跳转到不带www上，结果就是跳404，具体为地址栏上显示的是“https://uu126.cn//index.php"，另外打开的页面如果加www了也是直接跳首页，弄来弄去还是不行，头都大了！后来在Oneinstack的讨论群里救教，虽然答案没有，但是有人点拨了一下：伪静态规则故障？那就动手试试看，结果…………OK了！

<!--more-->

<del>把主机配置文件中（在/usr/local/nginx/conf/vhost目录下），把下面这条删除掉：</del>
2016-12-09修正后，可以在达到效果的同时也能正常登录后台了，这里要感谢一下在Segmentfault给我回复的南小鸟，下面提供一下需要修改的配件文件代码：
```php
include /usr/local/nginx/conf/rewrite/typecho.conf;#typecho重写规则（自带）
if ($ssl_protocol = "") {
    return 301 https://$server_name$request_uri; 
} 
if ($host != 'uu126.cn' ) {
    return 301 https://uu126.cn$request_uri; 
} 
```
主要修改了OneinStack提供的301跳转代码，建议OneinStack官方是不是看着修改一下，呵呵。
伸手党的可以看这个全部代码：
```php
server {
  listen 80;
  listen 443 ssl http2;
  ssl_certificate /usr/local/nginx/ssl/uu126.pem;
  ssl_certificate_key /usr/local/nginx/ssl/uu126.key;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
  ssl_prefer_server_ciphers on;
  ssl_session_timeout 10m;
  ssl_session_cache builtin:1000 shared:SSL:10m;
  ssl_buffer_size 1400;
  add_header Strict-Transport-Security max-age=15768000;
  ssl_stapling on;
  ssl_stapling_verify on;
  server_name uu126.cn www.uu126.cn;
  access_log off;
  index index.html index.htm index.php;
  include /usr/local/nginx/conf/rewrite/typecho.conf;#typecho重写规则（自带）
  if ($ssl_protocol = "") {
    return 301 https://$server_name$request_uri; 
  } 
  if ($host != 'uu126.cn' ) {
    return 301 https://uu126.cn$request_uri; 
  } 
  root /data/wwwroot/uu126;
  #error_page 404 = /404.html;
  #error_page 502 = /502.html;
  location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
  }
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
  }
  location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
  }
  location ~ /\.ht {
    deny all;
  }
}
```
请党员们根据自己的实际情况进行修改，直接扔进去可是木用的哦，^-^^-^^-^
仅供小伙伴们参考使用，这是强制Https，并且将带www的访问跳转到不带www上，希望可以帮到各位，有问题请留言。