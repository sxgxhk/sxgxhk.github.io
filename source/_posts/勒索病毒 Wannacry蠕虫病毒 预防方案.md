---
title: 勒索病毒 Wannacry蠕虫病毒 预防方案
date: 2017-05-14 15:50
tags: 随笔
categories: 我的地盘
permalink: 2468
---

#### 事件背景

5月12日晚， WannaCry 蠕虫病毒在全球大肆爆发。据BBC、CNN等媒体报道，恶意攻击者利用 NSA（美国国家安全局）泄露的 Windows 0day 利用工具对99个国家实施了超过75000次攻击，主要影响SMB和RDP服务，主要影响了137、138、139、445端口。
目前已知已知的Windows版本包括但不限于一下都受影响：


<!--more-->

 - Windows NT
 - Windows 2000
 - Windows XP
 - Windows 2003
 - Windows Vista
 - Windows 7
 - Windows 8
 - Windows 10
 - Windows 2008
 - Windows 2008 R2
 - Windows Server 2012 SP0

#### 2017-5-16补充：

勒索病毒各系统的SMB补丁下载地址：链接: [http://pan.baidu.com/s/1c1Tx6zY][1] 密码: 3jvg


勒索者源头来自暗网，攻击具备兼容性、多语言支持，多个行业受到影响，国内高校网络系统沦为感染重灾区。据有关机构统计，目前国内每天有数万台机器遭到该蠕虫病毒袭击，国内的ATM机、火车站、自助终端、邮政、医院、政府办事终端、视频监控都可能遭受攻击。据报道，今日全国多地的中石油加油站无法进行网络支付，只能进行现金支付。中石油有关负责人表示，怀疑受到病毒攻击，具体情况还在核查。而截至目前，一些公安系统已经遭到入侵。
![3d0jo.png][2]
如果你也遇到了这样的问题，请不要担心，我在下面给出了如何修复这个漏洞的建议。
![9y7cc.jpg][3]

#### 什么是比特币蠕虫病毒？

这次攻击的始作俑者是一款名为“WannaCry”（中文名：想哭）的勒索病毒，带有加密功能，它利用 Windows 在 445 端口的安全漏洞潜入电脑并对多种文件类型加密并添加后缀(.onion)使用户无法打开，用户电脑存在文档被加密的情况，攻击者称需支付比特币解锁。(比特币是一种全球通用的互联网加密货币)

#### 漏洞验证

使用` Win+R `按键打开运行窗口，输入` cmd `，进入命令行工具，然后输入` netstat -an `查看是否开放了对应的端口。
![9y7cc.jpg][4]

上图中的服务器就是开放了445端口，这有很大的风险可能会WannaCry 蠕虫病毒被攻击到，所以我们应该关掉对应端口，并修复漏洞。

#### 漏洞修复

 - 目前微软已发布补丁MS17-010修复了“永恒之蓝”攻击的系统漏洞，请尽快为电脑安装此补丁，网址为[https://technet.microsoft.com/zh-cn/library/security/MS17-010][5]
 - 对于XP、2003等微软已不再提供安全更新的机器，推荐使用360“NSA武器库免疫工具”检测系统是否存在漏洞，并关闭受到漏洞影响的端口，可以避免遭到勒索软件等病毒的侵害，可以在360电脑安全管家中找到。
 - 开启系统防火墙，利用系统防火墙的“高级设置”阻止外部对 445 端口进的访问（存在一定影响，该操作会影响使用 445 端口的服务）。

#### 修复脚本

如果以上方式都不能修复漏洞，大家可以使用我以下的批处理脚本文件来尝试关闭端口及服务，批处理禁用该漏洞可能利用到的端口，全版本通用，右键管理员启动即可，注意这需要打开Windows的防火墙。
```python
net stop SCardSvr
net stop SCPolicySvc
sc config SCardSvr start=disabled
sc config SCPolicySvc start=disabled
net start MpsSvc
sc config MpsSvc start=auto
netsh advfirewall set allprofiles state on
netsh advfirewall firewall add rule name="deny udp 137" dir=in protocol=udp localport=137 action=block
netsh advfirewall firewall add rule name="deny tcp 137" dir=in protocol=tcp localport=137 action=block
netsh advfirewall firewall add rule name="deny udp 138" dir=in protocol=udp localport=138 action=block
netsh advfirewall firewall add rule name="deny tcp 138" dir=in protocol=tcp localport=138 action=block
netsh advfirewall firewall add rule name="deny udp 139" dir=in protocol=udp localport=139 action=block
netsh advfirewall firewall add rule name="deny tcp 139" dir=in protocol=tcp localport=139 action=block
netsh advfirewall firewall add rule name="deny udp 445" dir=in protocol=udp localport=445 action=block
netsh advfirewall firewall add rule name="deny tcp 445" dir=in protocol=tcp localport=445 action=block
pause
```


本文转自：[https://samzong.me/2017/05/13/fix-Wannacry/][6]


  [1]: http://pan.baidu.com/s/1c1Tx6zY
  [2]: https://cdn.uu126.cn/usr/uploads/2017/05/2221204239.png
  [3]: https://cdn.uu126.cn/usr/uploads/2017/05/3149424871.jpg
  [4]: https://cdn.uu126.cn/usr/uploads/2017/05/2761870806.jpg
  [5]: https://technet.microsoft.com/zh-cn/library/security/MS17-010
  [6]: https://samzong.me/2017/05/13/fix-Wannacry/