---
title: Typecho修改后台路径留下的漏洞
date: 2017-03-26 00:20
tags: typecho
categories: 网站建设
permalink: 2448
---

在溜网络发现了一篇关于Typecho的重大消息——大漏洞，赶紧照着检测了一下，虽然目前没问题跳空白或404了，但看到不少网友在评论说出现了博主说的问题，那还是等重视一下，赶紧收录吧：
使用Typecho几年了，之前也是看到网上一些教程，说是修改后台路径可以增加一定的安全系数，从理论上来说是对的，我也使用修改后的后台路径好多年。通过修改配置文件` config.inc.php `的后台目录:


<!--more-->

```php
/** 后台路径(相对路径) */
define('__TYPECHO_ADMIN_DIR__', '/admin/');
```
将admin修改为其他目录，然后通过FTP将admin目录做相应的名称修改，这就完事。

如果以为这真的完事那就真的是太相信Typecho了。刚才在某群里说到wp为何不修改后台地址进行提升安全系数，有人说没多大用处。我在想，这没理由啊，我自己修改后台路径这么多年，都没见什么破解，原来是我图样图森破。通过浏览器在后台登录界面那里打开个F12，可以看到首先是提交到一个action/login，然后302重定向到正确的登录界面。

而这个地址是暴露到大众面前，完整地址是：http://domain.com/index.php/action/login ,刚才我拿这个地址去试了几个Typecho博客，发现都修改了后台路径，但用这个会跳转到正确的登录界面，相当于你的修改是无效的，无效的！

我再也不淡定了，立马使用google搜索下修改方法，终于找到一篇N年前的文章，作者写到0.9之后的版本不需要修改，本想留言交流一下，发现评论不了，故作罢。但是不修改的话这个漏洞一直会存在，我现在使用的版本是1.0，依旧可以通过通用地址进入后台登录界面，进而进行密码猜测。

参考该作者的文章，进行修改2个文件：` typecho\var\Widget\Do.php (line26) `
![typecho-bug03.jpg][1]
接着在` typecho\var\Widget\Options.php (line208) `，继续修改：
![typecho_bug01.jpg][2]
只要将以上两个地方（红框内的）修改为一样的参数，然后再去试一试index.php/action/login，发现已经不存在该问题。

使用Typecho的你，还不修复下吗？

原文链接：[https://itlu.org/articles/2425.html][3]
参考文章：[https://www.bstaint.net/archives/224/][4]


  


  [1]: https://cdn.uu126.cn/usr/uploads/2017/03/281805458.jpg
  [2]: https://cdn.uu126.cn/usr/uploads/2017/03/4034124924.jpg
  [3]: https://itlu.org/articles/2425.html
  [4]: https://www.bstaint.net/archives/224/