---
title: OneinStack架设的Nginx配置SSL（https)
date: 2016-10-18 21:36
tags: Oneinstack
categories: 网站建设
permalink: 2227
---

我自从建立博客以来，这主题也换了好多（反正我是记不得了），一直想追求一个简洁又不失必要功能的主题（没啥的银子去找二师兄帮忙），这不这两天找到一个单栏主题，稍加工了一下（能力有限，还得慢慢的来），暂时用着还好。这主题比较小巧，加载起来也还算快，换主题的同时也扔了好多插件，验证也改用极验证了（不仅得滑还得点图，避免一些垃圾评论），缓存插件也不用了，反正能不用的都扔了，所谓的一身轻了。又开始扯远了，言归正传，说说配置SSL的事吧，本机用的是OneinStack架设的Nginx（估计像军哥的LNMP也差不多的），证书用的是腾讯的免费证书（推荐这个），wosign和startssl也可以采用下面的配置方法，同样可以成功，至于选择哪个看自己吧，免费的都差不多，本人是这样的感觉。下面以腾讯的免费证书申请安装为例：


<!--more-->


![请输入图片描述][1] 

腾讯云免费SSL证书的申请地址：<a href="https://console.qcloud.com/ssl">https://console.qcloud.com/ssl</a>，使用QQ帐号就可以直接登录申请，过程也比较简单，只是现在证书在申请时只能填写带WWW的（当然本人试过，即使只能填写带WWW的，如果访问时不带WWW时也是可以显示小绿锁的）。

![请输入图片描述][2] 

申请审核通过过后，就可以下载了，有两个文件（1_域名.crt和2_域名.key），然后在 /usr/local/nginx/conf 路径下新建一个 “ssl” 目录，把你的证书全部上传到这个ssl目录下，接着可以通过Winscp上传到这个目录，再进入 /usr/local/nginx/conf/vhost 找到你网站的配置文件，比如uu126.conf，编辑成如下格式：
```python
server
    {
        listen 80;
        #listen [::]:80;
	listen 443;
        ssl on;
        ssl_certificate /usr/local/nginx/conf/SSL/1_你的域名.crt;
        ssl_certificate_key /usr/local/nginx/conf/SSL/2_你的域名.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers   on;
```

其实就是在"listen 80;"后面加上：
```python
listen 443;
ssl on;
ssl_certificate /usr/local/nginx/conf/SSL/1_你的域名.crt;
ssl_certificate_key /usr/local/nginx/conf/SSL/2_你的域名.key;
ssl_session_timeout 5m;
ssl_protocols TLSv1;
ssl_ciphers  HIGH:!aNULL:!MD5;
ssl_prefer_server_ciphers   on;
```

其它的代码不要去管它，保存后上传，重启nginx，然后再用https://域名，就可以看到小绿锁了，如果加了https://域名要以访问但不显示小绿锁时，别急这说明页面里有非https的链接，去修改回来后刷新一下就可以了。


  [1]: https://cdn.uu126.cn/wp-content/uploads/2016/10/https.jpg
  [2]: https://cdn.uu126.cn/wp-content/uploads/2016/10/2464377-9740e0273f1e2c1a.png