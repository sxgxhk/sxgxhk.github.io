---
title: Typecho下使用多说显示UA信息
date: 2016-11-27 20:21
tags: typecho
categories: 网站建设
permalink: 2372
---

记得在使用Wordpress时，给评论加过UA（访问者信息，如系统、浏览器等），比较简单只需要使用相应的插件即可，随着自己的博客改用Typecho后，原来的这些插件也就用不了，但也不能阻挡我那股折腾的心呀，现在博客使用的是多说的评论系统，网上看了一下教程还是蛮多的，但大部分是针对Wordpress的，虽然都是PHP架构的，还是要亲身体验过才知道好不好用。试了很多位博主的办法都不行（可能是俺功力还不够吧），不过也还是让俺给找到了，并且测试过好用，目前本站已经用上了，需要看效果的可以自己评论一下就可是看到了，好了，开始分享工作：
<!--more-->
1、先下载多说的embed.js文件，多说现在对Https的支持已经有所改观了，可以下载：https://static.duoshuo.com/embed.js

2、下载好后，打开编辑（本人不建议使用记事本，可以使用Notepad等软件，也方便），在最前面加入以下代码：

```java
//移动客户端判断开始，作用：在移动客户端显示不同样式
function sskcheckMobile(){  
    var isiPad = navigator.userAgent.match(/iPad/i) != null;  
    if(isiPad){  
        return false;  
    }  
    var isMobile=navigator.userAgent.match(/iphone|android|phone|mobile|wap|netfront|x11|java|opera mobi|opera mini|ucweb|windows ce|symbian|symbianos|series|webos|sony|blackberry|dopod|nokia|samsung|palmsource|xda|pieplus|meizu|midp|cldc|motorola|foma|docomo|up.browser|up.link|blazer|helio|hosin|huawei|novarra|coolpad|webos|techfaith|palmsource|alcatel|amoi|ktouch|nexian|ericsson|philips|sagem|wellcom|bunjalloo|maui|smartphone|iemobile|spice|bird|zte-|longcos|pantech|gionee|portalmmm|jig browser|hiptop|benq|haier|^lct|320x320|240x320|176x220/i)!= null;  
    if(isMobile){  
        return true;  
    }  
    return false;  
}  
//移动客户端判断结束
//显UA开始
function sskua(e) {
 var r = new Array;
 var outputer = '';
 if (r = e.match(/FireFox\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_firefox"><i class="fa fa-globe"></i> Mozilla FireFox' + '|' + r1[1]
 } else if (r = e.match(/Maxthon([\d]*)\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_maxthon"><i class="fa fa-globe"></i> Maxthon'
 } else if (r = e.match(/BIDUBrowser([\d]*)\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_ucweb"><i class="fa fa-globe"></i> 百度浏览器' + '|' + r1[1]
 } else if (r = e.match(/UBrowser([\d]*)\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_ucweb"><i class="fa fa-globe"></i> UCBrowser' + '|' + r1[1]
 } else if (r = e.match(/UCBrowser([\d]*)\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_ucweb"><i class="fa fa-globe"></i> UCBrowser' + '|' + r1[1]
 } else if (r = e.match(/MetaSr/ig)) {
 outputer = '<span class="ua_sogou"><i class="fa fa-globe"></i> 搜狗浏览器'
 } else if (r = e.match(/2345Explorer/ig)) {
 outputer = '<span class="ua_2345explorer"><a href="http://ssk.91txh.com/2345download.php?id=2" target="_blank" style="color:#FFFFFF!important;"><i class="fa fa-globe"></i> 2345王牌浏览器</a>'
 } else if (r = e.match(/2345chrome/ig)) {
 outputer = '<span class="ua_2345chrome"><a href="http://ssk.91txh.com/2345download.php?id=3" target="_blank" style="color:#FFFFFF!important;"><i class="fa fa-globe"></i> 2345加速浏览器</a>'
 } else if (r = e.match(/LBBROWSER/ig)) {
 outputer = '<span class="ua_lbbrowser"><i class="fa fa-globe"></i> 猎豹安全浏览器'
 } else if (r = e.match(/MicroMessenger\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_qq"><i class="fa fa-weixin"></i> 微信' + '|' + r1[1]/*.split('/')[0]*/
 } else if (r = e.match(/QQBrowser\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_qq"><i class="fa fa-globe"></i> QQ浏览器' + '|' + r1[1]/*.split('/')[0]*/
 } else if (r = e.match(/QQ\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_qq"><i class="fa fa-globe"></i> QQ浏览器' + '|' + r1[1]/*.split('/')[0]*/
 } else if (r = e.match(/MiuiBrowser\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_mi"><i class="fa fa-globe"></i> Miui浏览器' + '|' + r1[1]/*.split('/')[0]*/
 } else if (r = e.match(/Chrome([\d]*)\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_chrome"><i class="fa fa-globe"></i> Chrome' + '|' + r1[1]/*.split('.')[0]*/
 } else if (r = e.match(/safari\/([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_apple"><i class="fa fa-globe"></i> Apple Safari' + '|' + r1[1]
 } else if (r = e.match(/Opera[\s|\/]([^\s]+)/ig)) {
 var r1 = r[0].split("/");
 outputer = '<span class="ua_opera"><i class="fa fa-globe"></i> Opera' + '|' + r[1]
 } else if (r = e.match(/Trident\/7.0/gi)) {
 outputer = '<span class="ua_ie"><i class="fa fa-globe"></i> Internet Explorer 11'
 } else if (r = e.match(/MSIE\s([^\s|;]+)/gi)) {
 outputer = '<span class="ua_ie"><i class="fa fa-globe"></i> Internet Explorer' + '|' + r[0]/*.replace('MSIE', '').split('.')[0]*/
 } else {
 outputer = '<span class="ua_other"><i class="fa fa-globe"></i> 其它浏览器'
 }
 if(sskcheckMobile()){
 Mobile='<br><br>';
 }else{
 Mobile='';
 }
 return outputer+"</span>"+Mobile ;
 }
 function sskos(e) {
 var os = '';
 if (e.match(/win/ig)) {
 if (e.match(/nt 5.1/ig)) {
 os = '<span class="os_xp"><i class="fa fa-desktop"></i> Windows XP'
 } else if (e.match(/nt 6.1/ig)) {
 os = '<span class="os_7"><i class="fa fa-desktop"></i> Windows 7'
 } else if (e.match(/nt 6.2/ig)) {
 os = '<span class="os_8"><i class="fa fa-desktop"></i> Windows 8'
 } else if (e.match(/nt 6.3/ig)) {
 os = '<span class="os_8_1"><i class="fa fa-desktop"></i> Windows 8.1'
 } else if (e.match(/nt 10.0/ig)) {
 os = '<span class="os_8_1"><i class="fa fa-desktop"></i> Windows 10'
 } else if (e.match(/nt 6.0/ig)) {
 os = '<span class="os_vista"><i class="fa fa-desktop"></i> Windows Vista'
 } else if (e.match(/nt 5/ig)) {
 os = '<span class="os_2000"><i class="fa fa-desktop"></i> Windows 2000'
 } else {
 os = '<span class="os_windows"><i class="fa fa-desktop"></i> Windows'
 }
 } else if (e.match(/android/ig)) {
 os = '<span class="os_android"><i class="fa fa-android"></i> Android'
 } else if (e.match(/ubuntu/ig)) {
 os = '<span class="os_ubuntu"><i class="fa fa-desktop"></i> Ubuntu'
 } else if (e.match(/linux/ig)) {
 os = '<span class="os_linux"><i class="fa fa-linux"></i> Linux'
 } else if (e.match(/mac/ig)) {
 os = '<span class="os_mac"><i class="fa fa-desktop"></i> Mac OS X'
 } else if (e.match(/unix/ig)) {
 os = '<span class="os_unix"><i class="fa fa-desktop"></i> Unix'
 } else if (e.match(/symbian/ig)) {
 os = '<span class="os_nokia"><i class="fa fa-mobile"></i> Nokia SymbianOS'
 } else {
 os = '<span class="os_other"><i class="fa fa-desktop"></i> 其它操作系统'
 }
 return os+"</span>" ;
 }
//显UA结束
```
3、接着搜索
```java
data-qqt-account="' + (r.qqt_account || "") + '">' + u(r.name) + "</span>"),
```
如果查不到，可以尝试缩短关键词查询，在后面增加：
```java
t += "<span class=\"ua\">" + sskua(s.agent) + "</span><span class=\"ua\">" + sskos(s.agent) + "</span>",
```
注意是增加，不是覆盖哦。


4、增加美观度，可以在主题的style.css或多说设置中的自定义css（本人用的是后者，为什么？换主题不影响呀），代码如下：
```css
    /*多说UA开始*/
span.ua{
 margin: 0 1px!important;
 color:#FFFFFF!important;
 /*text-transform: Capitalize!important;
 float: right!important;
 line-height: 18px!important;*/
}
.ua_other.os_other{
 background-color: #ccc!important;
 color: #fff;
 border: 1px solid #BBB!important;
 border-radius: 4px;
}
.ua_ie{
 background-color: #428bca!important;
 border-color: #357ebd!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_firefox{
 background-color: #f0ad4e!important;
 border-color: #eea236!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_maxthon{
 background-color: #7373B9!important;
 border-color: #7373B9!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_ucweb{
 background-color: #FF740F!important;
 border-color: #d43f3a!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_sogou{
 background-color: #78ACE9!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_2345explorer{
 background-color: #2478B8!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_2345chrome{
 background-color: #F9D024!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_mi{
 background-color: #FF4A00!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_lbbrowser{
 background-color: #FC9D2E!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_chrome{
 background-color: #EE6252!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_qq{
 background-color: #3D88A8!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_apple{
 background-color: #E95620!important;
 border-color: #4cae4c!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.ua_opera{
 background-color: #d9534f!important;
 border-color: #d43f3a!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
 
 
.os_vista,.os_2000,.os_windows,.os_xp,.os_7,.os_8,.os_8_1 {
 background-color: #39b3d7!important;
 border-color: #46b8da!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
 
.os_android {
 background-color: #98C13D!important;
 border-color: #01B171!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.os_ubuntu{
 background-color: #DD4814!important;
 border-color: #01B171!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.os_linux {
 background-color: #3A3A3A!important;
 border-color: #1F1F1F!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.os_mac{
 background-color: #666666!important;
 border-color: #1F1F1F!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.os_unix{
 background-color: #006600!important;
 border-color: #1F1F1F!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
.os_nokia{
 background-color: #014485!important;
 border-color: #1F1F1F!important;
 border-radius: 4px;
 padding: 0 5px!important;
}
/*多说UA结束*/
```
最后，上一个本站的效果图，大伙品尝一下：
![20161127202015.png][2]

　


  


  


 
  [2]: https://cdn.uu126.cn/usr/uploads/2016/11/3041626217.png