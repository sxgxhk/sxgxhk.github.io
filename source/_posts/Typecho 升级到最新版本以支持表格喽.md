---
title: Typecho 升级到最新版本以支持表格喽
date: 2016-12-08 21:55
tags: typecho
categories: 网站建设
permalink: 2402
---

前言

今天群里有同学提到 typecho 最新版本已经有 1.1-beta 版本可用，而且 typecho 最新版本支持更丰富的 markdown 语法（泪流满面的发现支持了表格！）。
那么问题来了，如何升级到 typecho 的最新版本代码？如果有同学改动了源码又如何处理？
升级到最新版本


<!--more-->


官方网站已经有详细的说明，这里稍微总结如下：
>下载

首先，下载最新的版本代码，那么这里就有两个地方可以下载：
第一个是官方的打包版本[下载地址][1]
第二个是一般开源的东西会在 [github][2] 上有一份源码，在那里往往可以找到最新的版本也就是 [typecho 的 beta][3] 版本或者 [typecho 的 pre-release][4]。
如果后者存在则建议下载后者，不过想尝鲜的同学不妨下载 beta 版本，比如刚刚放出的 [typecho-1.1-beta][5] 版本就有劲爆的 markdown 功能，估计可以俘获很多站长的心。
1、删除服务器上的旧文件，请在服务器上删除如下目录和文件:
```
/admin/
/var/
/index.php
```
<font color="red">注意：请千万不要删除 /usr/ 目录，因为这个目录包含了你的主题，插件和上传的文件，它无需被升级
完成升级后排错</font>

当你没有进行下面的步骤时，访问前台页面可能回出现错误提示，请不要管他们，直接访问你的 admin 页面，按提示完成升级即可恢复正常。
用一个具有管理员权限的用户登录后台，系统会提示检测到新版本需要升级，点击完成升级按钮即可完成升级。
<font color="red">注意：如果在升级完成后，进入首页出现 500 或其他错误，请进入 admin 页面禁用所有的插件，并启用默认模板。如果正常，请逐步排查插件或模板存在的问题。</font>

>如何合并源码？

有的同学由于需要定制一些特殊的功能，需要改动源码，那么这个时候就不能按照官方的文档进行简单覆盖了，那么怎么处理呢？
如果没有使用 git 或者 svn 等版本管理软件，则将代码 down 到本地，使用 file compare 等文件比较工具比较目录，需要比较的目录清单：
```
/admin/
/var/
/index.php
```
比较完以后，将代码直接上传覆盖，或者按照第一部分的步骤删除文件目录后再上传，之后按照官方给出的办法进行排错。
如果使用 git 或者 svn，其实本质上没有多大的区别，只是利用 git 或者 svn 的文件比较特性而已。对于 coder 来说是一件很 easy 的事情。
>写在最后

如果您在使用 typecho 过程或者升级过程，可以把问题提交到邮件列表，你的参与对改善 typecho 很有帮助。
一个表格实例

显示效果:
| 友链 |  描述 | 备注 |
| :------------- | :-----| :-----|
|[Leonn](https://i-ebs.com/)|ERP学习|工作博客|
Markdown 内容:
```
| 友链 |  描述 | 备注 |
| :------------- | :-----| :-----|
|[Leonn](https://i-ebs.com/)|ERP学习|工作博客|
```
文章主要内容来自 [TypechoDev][6]


  [1]: https://liyuans.com/t/aHR0cDovL3R5cGVjaG8ub3JnLw==
  [2]: https://liyuans.com/t/aHR0cHM6Ly9naXRodWIuY29tL3R5cGVjaG8vdHlwZWNobw==
  [3]: https://liyuans.com/t/aHR0cHM6Ly9naXRodWIuY29tL3R5cGVjaG8vdHlwZWNoby9hcmNoaXZlL21hc3Rlci56aXA=
  [4]: https://liyuans.com/t/aHR0cHM6Ly9naXRodWIuY29tL3R5cGVjaG8vdHlwZWNoby9hcmNoaXZlL3YxLjEtMTUuNS4xMi1iZXRhLnppcA==
  [5]: https://liyuans.com/t/aHR0cHM6Ly9naXRodWIuY29tL3R5cGVjaG8vdHlwZWNoby9hcmNoaXZlL3YxLjEtMTUuNS4xMi1iZXRhLnppcA==
  [6]: https://liyuans.com/t/aHR0cHM6Ly93d3cudHlwZWNob2Rldi5jb20vaW5kZXgucGhwL2FyY2hpdmVzLzc0NS8=