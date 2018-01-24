---
title: 让Typecho中的多说头像支持HTTPS
date: 2016-11-13 22:54
tags: typecho
categories: 网站建设
permalink: 2352
---

多说其实是个还不错的社会化评论插件，支持多个网站的账号快捷登陆，虽然某些行为有些流氓，但仍旧算是十分流行的一个系统，可是多说在HTTPS下的兼容性十分糟糕，头像仍旧调用HTTP，导致浏览器报出不安全的警告，可以通过下面的办法解决这个问题：
首先介绍一下实现原理，我们制作一个php，实时从多说服务器获取最新的js文件，然后实时将js中头像的URL部分修改为自己服务器上的地址，由于新浪的头像服务器并不支持HTTPS，我们必须在自己的服务器上建立一个图片代理php，来解决问题，最后再修改多说插件，让它调用我们的php即可，这可能会消耗一些服务器资源和流量，当然，由于头像图片一般都很小，几乎可以忽略不计。<!--more-->那就开始下载吧，[https://pan.baidu.com/s/1i5NTZSx][1]，密码是：p48m
下载之后解压，编辑embed.php和embed.js，通过查找将"https://uu126.cn“修改为自己对应的网址：
1、embed.js中的修改![duoshuo01.jpg][2]
2、embed.php中的修改![duoshuo02.jpg][3]
保存好之后将整个duoshuo目录上传到网站根目录即可。再将主题目录下的footer.php（这里就以Typecho为例了）中的多说地址修改为自己的地址，如图：![duoshuo03.png][4]
全部更好完成后，刷新一下（有CDN或缓存的请清除一下）就可以看见地址栏上的小绿锁了，当然多说的头像也出来了。


  [1]: https://pan.baidu.com/s/1i5NTZSx
  [2]: https://cdn.uu126.cn/usr/uploads/2016/11/2908937494.jpg
  [3]: https://cdn.uu126.cn/usr/uploads/2016/11/526624769.jpg
  [4]: https://cdn.uu126.cn/usr/uploads/2016/11/375246450.png