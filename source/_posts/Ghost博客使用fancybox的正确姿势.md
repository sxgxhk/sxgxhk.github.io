---
title: Ghost博客使用fancybox的正确姿势
date: 2017-10-05 17:49
tags: Ghost
categories: 网站建设
permalink: 2500
---

在Ghost博客后台，使用Markdown编辑器。

` Ctrl + Shift + I `，图片快捷键，生成代码：` ![](https://) `

`Ctrl + K `，链接快捷键，生成代码：` [](https://) `

正确格式

<!--more-->

fancybox实现后实际上是个图片链接，So，把图片放入链接里。

格式：` [![](https://)](https://) `

例如：` [![风景 - 1](http://xlbd.me/content/images/2016/05/vies1.jpg)](http://xlbd.me/content/images/2016/05/vies1.jpg) `

[![风景 - 1](http://xlbd.me/content/images/2016/05/vies1.jpg)](http://xlbd.me/content/images/2016/05/vies1.jpg)

[![风景 - 2](http://xlbd.me/content/images/2016/05/view2.jpg)](http://xlbd.me/content/images/2016/05/view2.jpg)

[![风景 - 3](http://xlbd.me/content/images/2016/05/view3.jpg)](http://xlbd.me/content/images/2016/05/view3.jpg)

方式可能不一定适用于所有Ghost主题，反正本站目前在用的主题是这样子的。

本博文转载自：小蘿蔔丁 [http://xlbd.me/how-to-use-fancybox-in-ghost-blog/][1]


  [1]: http://xlbd.me/how-to-use-fancybox-in-ghost-blog/