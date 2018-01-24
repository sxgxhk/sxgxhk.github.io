---
title: VPS性能等一键测试脚本
date: 2016-11-29 12:38
tags: vps
categories: 网站建设
permalink: 2379
---

大家新买了VPS，免不了需要测试一下新VPS的性能，以前测试比较繁琐，显示结果也不直观。今天发现秋大新的一键测试脚本bench.sh，特别转发过来。经过几个版本的演化，一键测试脚本 bench.sh 已经几乎全面适用于各种 Linux 发行版的网络（下行）和 IO 测试。并将测试结果以较为美观的方式显示出来。
总结一下 bench.sh 特点：
1、显示当前测试的各种系统信息；
2、取自世界多处的知名数据中心的测试点，下载测试比较全面；
3、支持 IPv6 下载测速；
4、IO 测试三次，并显示平均值。
再配合 unixbench.sh 脚本测试，即可全面测试 VPS 的性能。


<!--more-->


使用方法：
命令1：
```
wget -qO- bench.sh | bash
```
或者
```
curl -Lso- bench.sh | bash
```
命令2：
```
wget -qO- 86.re/bench.sh | bash
```
或者
```
curl -so- 86.re/bench.sh | bash
```
备注：
bench.sh 既是脚本名，同时又是域名。所以不要怀疑我写错了或者你看错了,直接按命令输入即可，本人亲测可用的！当然也可以自己下载好脚本丢到自己的VPS上测试，下载地址：
[https://github.com/teddysun/across/blob/master/bench.sh][1]
最后放几张测试图片：
1、BandwagonHost Los Angel（搬瓦工，洛杉矶）
![speedtest_bwg.png][2]
2、DigitalOcean Singapore（新加坡）
![speedtest_do_sg.png][3]
3、Ramnode Seattle（西雅图）
![speedtest_ramnode.png][4]
4、Xvmlabs Los Angel（洛杉矶）
![speedtest_xvmlabs.png][5]
5、CloudAtCost（传说中一次付费永久使用的VPS？加州）
![speedtest_cloudatcost.png][6]


  [1]: https://github.com/teddysun/across/blob/master/bench.sh
  [2]: https://cdn.uu126.cn/usr/uploads/2016/11/3899324618.png
  [3]: https://cdn.uu126.cn/usr/uploads/2016/11/3776800208.png
  [4]: https://cdn.uu126.cn/usr/uploads/2016/11/3469839796.png
  [5]: https://cdn.uu126.cn/usr/uploads/2016/11/1052655045.png
  [6]: https://cdn.uu126.cn/usr/uploads/2016/11/448338830.png