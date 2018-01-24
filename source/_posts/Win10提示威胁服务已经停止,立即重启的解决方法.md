---
title: Win10提示威胁服务已经停止,立即重启的解决方法
date: 2018-01-15 21:23
tags: windows
categories: 我的地盘
permalink: 2529
---

昨天手动将win10升级到1709，内部版本为16299.125。自带的杀毒软件Windows defender安全中心提示威胁服务已经停止,立即重启。病毒防护也无法启用，如图所示，点击立即重新启动，显示“发生未知错误。”。虽然平常整个电脑管家啥的，但就稳定性还是系统自带的好，加上微软这几年的不断努力，系统自带的杀毒软件也越来越完善了，虽然这些年没有查出什么威胁，只是不能运行，万一哪天遇到了，心里还是怕怕的。


<!--more-->

![win10_wx01.jpg][1]
![win10_wx02.jpg][2]

于是找度娘度了一番，发现很多法子都是修改组策略的方法来启动，但我从未更改过相关设置，按提示点进去看了，却发现都是“未配置”状态。另外还有命令提示符的方法，试了无效。无奈之下，之好翻墙请来谷哥来帮忙，终于在微软社区找到了个解决办法：

> `Windows+r`，输入：`regedit`，定位路径：` HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows     Defender `，查看是否存在` DisableAntiSpyware `这个键值，若有请删除。

按步骤修改好注册表，并重启，果然解决了，看来关键时候还是要找谷哥帮忙呀。

![win10_wx03.png][3]


  [1]: https://api.uu126.cn/usr/uploads/2018/01/2779409585.jpg
  [2]: https://api.uu126.cn/usr/uploads/2018/01/2825320544.jpg
  [3]: https://api.uu126.cn/usr/uploads/2018/01/2986629762.png