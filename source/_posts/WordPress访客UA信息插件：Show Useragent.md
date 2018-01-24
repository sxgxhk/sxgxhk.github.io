---
title: WordPress访客UA信息插件：Show Useragent
date: 2016-07-05 22:08
tags: WordPress
categories: 网站建设
permalink: 2105
---

一直想给自己的博客加个访客UA信息（如下图），可……，不能再拖了，赶紧动手添加吧，网上逛了逛，就选Show Useragent吧，因为本人不是太喜欢插件的形式，所以能代码化就……（其实代码多了也会影响程序的执行率的，多少的问题）。


<!--more-->


![请输入图片描述][1]
先下载张戈提供的汉化包，然后将解压后的`show-useragent`文件夹上传到正在使用的主题目录下，接着在主题`functions.php`里添加如下代码,###注意就添加在`<?php`和最后`?>`之间：
```php
//显示访客信息
include("show-useragent/show-useragent.php");
```
接着在主题目录下的`comments.php`里查找`wp_list_comments`，如：本站使用的是知更鸟的begin主题 ，返回的是：
```php
?php wp_list_comments( 'type=comment&amp;callback=mytheme_comment' ); 
```
如果发现`callback=XXXXX`回调函数时，说明主题是自定义了评论列表，那么直接去查找后面回调函数位置，然后在合适的位置加入以下代码：
```php
?php CID_print_comment_flag(); echo ' ';CID_print_comment_browser(); 
```
本站（知更鸟的begin主题）的位置是在主题目录的`inc/function`里的`comment-template.php`里，改好之后的位置如下：
```php
span class="comment-meta commentmetadata" php CID_print_comment_flag(); echo ' ';CID_print_comment_browser(); 
```
全部弄好之后刷新一下，就可以在评论里看到相关访客的UA信息了。补充一下，里面有一个IP文件是需要更新的，详细的更新办法可参考这个：[http://www.speedfly.cn/9488.html][2]


  [1]: https://cdn.uu126.cn/wp-content/uploads/2016/07/woredpress-ua.png
  [2]: http://www.speedfly.cn/9488.html