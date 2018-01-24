---
title: 华硕笔记本预装WIN8系统改WIN7系统出现的问题及解决
date: 2015-12-27 22:32
tags: 维修
categories: IT综合技术
permalink: 1943
---

电脑型号：华硕Y582L笔记本
电脑故障描述：预装WIN8系统改WIN7系统，巧遇无无线网上问题，详解。
技术难度 ：2颗星
过程：
一个年青的小伙，想把笔记本预装WIN8系统改成WIN7系统，但自己搞了好久也没搞得，所以拿到我店里<!--more-->
![请输入图片描述][1]
我拿过机子后，首先开一次机子，正常开机。然后重启按F2进入BIOS。
![请输入图片描述][2]
然后左右键切换到` Security `，上下键找到` Secure BOOT menu `回车进入，将` Secure Boot Control `中的` Enabled `改为` Disabled `，这一步的作用是关闭微软的` Secure BOOT `，这个功能开启会导致不能识别U盘启动系统的安装 
![请输入图片描述][3]
![请输入图片描述][4]
按 Esc 键返回， 左右键切换到` Boot `，将` Launch CSM `中的` Disabled `改为` Enabled `（如果修改的时候弹出一个框改不了的话，先按F10保存退出再次进入就可以选择了），这个选项的作用是，将UEFI改为传统BIOS。
![请输入图片描述][5]
最后按 F10 保存并退出，重新启动。开机时按 ESC 键弹出选择开机启动项，就可以选择自己的U盘或许光盘启动了。
安装系统过程省略。。。
安装系统过程到进入系统后一切正常，然后问题来了——没有无线网卡，客户的需要连无线的。
我也没动其它哪里了啊，怎么会没有无线网卡呢，我确认客户之前是有使用过无线网卡的。
![请输入图片描述][6]
然后我又重启电脑按F2 进入BIOS，左右键切换到` Security `下的` I/O Interface Security `把` Wireless Network Interface `改成` UnLock `。按F10保存退出，重启。
![请输入图片描述][7]
哈哈，无线网卡出来了，装上驱动就OK了。这位年青小伙笑了，给了钱飞奔回去了。当时还下着雨。
![请输入图片描述][8]
### 总结：原本笔记本预装的WIN8系统改装WIN7系统是很平常的事了，但遇到问题还需要仔细思考和检查，步骤不能乱，心不能急。

希望帮助到有需要帮助的伙伴。

本文转载自2345论坛，http://wangpai.2345.cn/thread.php?fid=12&amp;pid=3463819


  [1]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L1.jpg
  [2]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L2.jpg
  [3]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L3.jpg
  [4]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L4.jpg
  [5]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L5.jpg
  [6]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L6.jpg
  [7]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L7.jpg
  [8]: https://cdn.uu126.cn/wp-content/uploads/2015/12/Y582L8.jpg