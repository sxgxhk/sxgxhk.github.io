---
title: 博客计划由Typecho转Hexo
date: 2017-02-02 10:37
tags: [hexo,typecho]
categories: 我的地盘
permalink: 2415
---

我的博客唠唠叨叨的也写了好几年了，文章也有三百多篇了，从一开始用Wordpress到Typecho，其中以Wordpress的使用时间最长，占了博客生命的2/3了吧，空间也是一样从最早的虚拟空间，到VPS，一路也是坎坷而来，虽然博客没有带来什么收入（之前在用Wordpress时有打过小广告，后来就没有了），但却丝毫没影响我对它的投入、记录。博客的发展史也正是我的折腾史，不管程序还是空间，一直都没有停过，这不2017年了，又有了新的折腾方向——Hexo。


<!--more-->

Hexo—— 简单、快速、强大的Node.js静态博客框架，这是大部分网站给的评介，我也体验了一段时间，确实挺方便的（看的我Hexo博客：[https://blog.uu126.cn][1]），架设本地Hexo和Node、Git也比较容易上手，速度也快，不像在VPS里搭建环境包（如：LNMP、Oneinstack等）没个半小时（有时候慢的都不止）都弄不好，这点Hexo还是有优势的，另外Hexo架设的博客没有后台，妈妈再也不用担心我的博客被入侵了（哈哈），另外Hexo可以双备或多备，即同时发布在Github、Coding等上面，这样使的博客更加可靠了。从我目前使用来看，国内访问还是放在Coding（码市）上，国外的可以放在Github，原因嘛，百度一下就懂了，这里就不唠叨了。
![hexo_1.jpg][2]
![hexo_2.jpg][3]     
当然Hexo也不是没有缺点，首先就是文章的发布有点问题，虽然Coding里提供了一个`WebIDE`，但是感觉还是没有本地Windows搭建的来的方便，目前我的解决办法是：把Hexo上传环境搭建在家里，如果不在家里也想写文章的话，可以先写成MD文件，存至云盘，回家时再下载同步，虽然麻烦了一点，但可以保存文章，毕竟放在云盘里相对还是可靠（除非再来个360）。另外刚从其它博客程序过来的，还是要适应一下Hexo，设置、操作还是相差蛮大的。
目前我的Hexo博客还在测试期，一些功能还得去摸索，另外最大的问题是要把文章导过来，百度了一圈也没有什么好的办法，加之直接导到hexo容易出错，所以现在采用的是原始的办法，一篇一篇的手工新建、复制、粘贴到MD文件里，三百多篇呀（大部分还是Wordpress里的格式，还得转换成markdown语法），工作量还是有点的，慢慢来吧……………………
最后奉上直接同时同步到Github和Coding的代码，也不是什么新玩意，希望能帮到有需要的童鞋们吧
```python
deploy:
  type: git     
   repo: 
      github: git@github.com:用户名/用户名.github.io,master  #请将用户名改成你在Github注册的，后面的master可随便写
      coding: git@git.coding.net:用户名/用户名.git,master    #请将用户名改成你在Coding注册的，后面的master可随便写
```
代码后面的`master`可以随便写，只要在后台选择相应的即可。
#### 2017-2-18
博客目前使用的主题是nextd（是改版的next），感觉比原版的好，但是和原版一样有一个同样的问题，至少对我来说那是问题，因为博客是从Typecho迁徙而来的，用了nextd或next等主题后，多说的评价不能同步了，只能显示新的评论（经测试yilia主题是可以显示的，其带的多说模块是多说官方提供的，而nextd等多数Hexo主题都有多多少少的修改），找了好多个地方，也修改了好好几次，最终解决了！找到主题目录下的` comments.swig `，路径是：` \layout\_partials\ `，将其中：
```
<div class="ds-thread" data-thread-key="{{ page.path }}"
     data-title="{{ page.title }}" data-url="{{ page.permalink }}">
</div>
```
改成：
```
<div class="ds-thread" data-thread-key="{{ post.layout }}"
     data-title="{{ post.title }}" data-url="{{ page.permalink }}">
</div>
```
然后再保存，使用命令` hexo serve `来测试一下，可以发现之前没有显示（或同步）的评论可以显示了，赶紧` hexo deploy `上传了！
其它的修改还得慢慢来，Hexo的主题设置比Typecho或Wordpress还是需要熟悉一下的——看来还可以再好好折腾一下。



  [1]: https://blog.uu126.cn
  [2]: https://cdn.uu126.cn/usr/uploads/2017/02/1924333009.jpg
  [3]: https://cdn.uu126.cn/usr/uploads/2017/02/2990810344.jpg