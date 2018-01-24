---
title: 一键检测Linux VPS服务器配置、IO、国内节点下载测速
date: 2017-10-05 17:48
tags: vps
categories: 网站建设
permalink: 2499
---

刚逛了一下老左博客，发现左哥又更文了《一键检测Linux VPS/服务器配置、IO、国内节点下载测速》，闲来无事，我也按照其提供的检测脚本对本站（阿里云香港主机）和另一台主机（华为云上海主机）分别进行了测试，两台都是1核1G40G的主机，直观上配置除了CPU不一样（阿里云是E5-2682 V4，华为云是E5-2680 V3，主频都是2.5GHz），其它的没怎么看出来，不过两者的IO差别还是有点的，具体见后文章。

<!--more-->

左哥的原文：

>继老左在前面有分享过oldking博主作者提供改良版的国内电信、移动、联通节点测速脚本（一键测试Linux VPS、服务器国内节点速度参数）之后，我们可以对国内、国外VPS主机采用一些小的工具文件进行简单的参数和速度的测试。这里这个兄台又将teddysun的一键检测IO、配置信息等问题检测脚本改良，丰富配置信息的展现、IO速度、以及下载速度检测点也是国内节点。
这里老左也简单的整理出来，对于我简单的测试手中的Linux VPS、服务器等信息还是有一点点参考比较价值用途的。

相比之前的脚本，这次提供的最大区别就是检测节点都是国内的，比较适合国内的朋友使用。本次测试的是晚上，估计相比白天结果要差一些，具体的如下：

 - 官方测试脚本（推荐使用这个，脚本更新快）：

```python
wget https://raw.githubusercontent.com/oooldking/script/master/superbench.sh
chmod +x superbench.sh
./superbench.sh
```

 - 备用测试脚本：

```python
wget http://soft.laozuo.org/scripts/superbench.sh
chmod +x superbench.sh
./superbench.sh
```

 - 阿里云（香港主机）测试结果：

[![阿里云（香港主机）](https://oft4n5tq6.qnssl.com/image/9/ec/fec7b1fe6fe32d1882f99e7103633.png)](https://oft4n5tq6.qnssl.com/image/9/ec/fec7b1fe6fe32d1882f99e7103633.png)

 - 华为云（上海主机）测试结果：

[![华为云（上海主机）](https://oft4n5tq6.qnssl.com/image/d/4a/d182dbc231a82f26e5259968a7c1a.png)](https://oft4n5tq6.qnssl.com/image/d/4a/d182dbc231a82f26e5259968a7c1a.png)

可以明显的看出两者还是有点差距的，从测试结果中也可以看出teddysun提供的丰富一些，可以测试到架构、IO分三个等级测试、以及测速下载节点是用的随机国内节点。有需要的亲，可以下载个试试，权当对自己的主机来个小体检吧233。