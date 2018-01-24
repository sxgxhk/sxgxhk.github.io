---
title: 折腾一下Kratos主题
date: 2017-03-17 22:05
tags: typecho
categories: 网站建设
permalink: 2432
---

前几日逛Typecho论坛，看到有朋友移植了原WP主题Kratos，看看挺喜欢的（其实是有点想念WP时用过的9IPHP主题了，能力有限移植不了），那就耍耍呗，换掉Hran的Mirages主题（不知道Hran童鞋会不会有意见呀），下载并上传，接着设置主题然后又…………（每次换主题都做着这些重复的事），大致还是OK的，就是有一些地方需要修改一下，先亮一下修改后的：


<!--more-->

![my_uu126_01.jpg][1]

封面上修改的不多，主要的不是在内容页上，增加了文章二维码，打赏，另外对代码高亮进行了美化，对文章页广告代码小修改，自认为比原先的要好看。
####打赏
网上找的我也只是略略修改一下，新建一个ds.js文件，内容如下：
```java
(function($){
	var id = Date.now();
	if($("#STYLE_"+id).size()<1){
		document.writeln("<style id='STYLE_"+id+"'>@CHARSET \"UTF-8\";*{-webkit-tap-highlight-color:rgba(255,0,0,0)}.box-size{box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box}.ds-hide{display:none}.ds-reward-stl{font-family:\"microsoft yahei\";text-align:center;background:#f1f1f1;padding:10px 0;color:#666;margin:20px auto;width:90%}#dsRewardBtn {position: absolute;background: #d32;left:45%;width: 60px;height: 60px;font-size: 16px;font-weight: 600;line-height: 43px;display: block;border: 4px solid #fff;border-radius: 40px;color: #FFF;}#dsRewardBtn span{display:inline-block;width:50px;height:50px;border-radius:100%;line-height:58px;color:#fff;font:400 25px/50px 'microsoft yahei';background:#FEC22C}#dsRewardBtn:hover{cursor:pointer}.ds-dialog{z-index:9999;width:100%;height:100%;position:fixed;top:0;left:0;border:1px solid #d9d9d9}.ds-dialog .ds-close-dialog{position:absolute;top:15px;right:20px;font:400 24px/24px Arial;width:20px;height:20px;text-align:center;padding:0;cursor:pointer;background:transparent;border:0;-webkit-appearance:none;font-weight:700;line-height:20px;opacity:.6;filter:alpha(opacity=20)}.ds-dialog .ds-close-dialog:hover{color:#000;text-decoration:none;cursor:pointer;opacity:.6;filter:alpha(opacity=40)}.ds-dialog-bg{position:absolute;opacity:.6;filter:alpha(opacity=30);background:#000;z-index:9999;left:0;top:0;width:100%;height:100%}.ds-dialog-content{font-family:'microsoft yahei';font-size:14px;background-color:#FFF;position:fixed;padding:0 20px;z-index:10000;overflow:hidden;border-radius:6px;-webkit-box-shadow:0 3px 7px rgba(0,0,0,.3);-moz-box-shadow:0 3px 7px rgba(0,0,0,.3);box-shadow:0 3px 7px rgba(0,0,0,.3);-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box}.ds-dialog-pc{width:390px;height:380px;top:50%;left:50%;margin:-190px 0 0 -195px}.ds-dialog-wx{width:90%;height:280px;top:50%;margin-top:-140px;margin-left:5%}.ds-dialog-content h5{text-align:left;font-size:15px;font-weight:700;margin:15px 0;color:#555}.ds-payment-way{text-align:left}.ds-payment-way label{cursor:pointer;font-weight:400;display:inline-block;font-size:14px;margin:0 15px 0 0;padding:0}.ds-payment-way input[type=radio]{vertical-align:middle;margin:-2px 5px 0 0}.ds-payment-img{margin:15px 0;text-align:center}p.ds-pay-info{font-size:15px;margin:0 0 10px}.ds-pay-money{font-size:14px;margin-top:10px}.ds-pay-money p{margin:0}.ds-pay-money .ds-pay-money-sum{margin-bottom:4px}.ds-payment-img img{margin:0 auto;width:185px;}.ds-payment-img #qrCode_1{display:none}.ds-payment-img .qrcode-border{margin:0 auto}.ds-payment-img .qrcode-tip{width:48.13px;position:relative;margin:0 auto;font-size:12px;font-weight:700;background:#fff;height:15px;line-height:15px;margin-top:-12px}#qrCode_0 .qrcode-tip{color:#3caf36}#qrCode_3 .qrcode-tip{color:#e10602}.ds-payment-img #qrCode_3{display:none}.ds-payment-img #qrCode_2{display:none}#qrCode_2 .qrcode-tip{color:#eb5f01}#qrCode_1 .qrcode-tip{color:#6699cc}.wx_qrcode_container{text-align:center}.wx_qrcode_container h2{font-size:17px}.wx_qrcode_container p{font-size:14px}.ds-reward-stl{text-align:left;background:#fff;padding:0;color:#666;margin:0;width:0}#dsRewardBtn span{position:absolute;left:115px;top:-7px;background:#7ab951;width:50px;height:50px;font-size:16px;font-weight:600;line-height:43px;border:4px solid #fff;border-radius:40px}.share-s a{margin-top:-25px} .ds-payment-img .qrcode-border{border-radius: 29.97px; width: 236.89px; height: 236.89px; padding: 18.05px; margin-top: 25.53px; } </style>");
	}
	function write(){
		var content = "<div class=\"ds-dialog\" id='PAY_"+id+"'>"
		+"   <div class=\"ds-dialog-bg\" onclick=\"PaymentUtils.hide();\"></div>"
		+"   <div class=\"ds-dialog-content ds-dialog-pc \">"
		+"    <i class=\"ds-close-dialog\">&times;</i>"
		+"    <h5>选择打赏方式：</h5>"
		+"    <div class=\"ds-payment-way\">"
		+"     <label for=\"wechat\"><input type=\"radio\" id=\"wechat\" class=\"reward-radio\" value=\"0\" checked=\"checked\" name=\"reward-way\" />微信红包</label>"
		+"     <label for=\"alipay\"><input type=\"radio\" id=\"alipay\" class=\"reward-radio\" value=\"2\" name=\"reward-way\" />支付宝</label>"
		+ "    </div>"
		+ "    <div class=\"ds-payment-img\">"
		+ "     <div class=\"qrcode-img qrCode_0\" id=\"qrCode_0\">"
		+ "      <div class=\"qrcode-border box-size\" style=\"border: 9.02px solid rgb(60, 175, 54\">"
		+ "       <img  class=\"qrcode-img qrCode_0\" id=\"qrCode_0\" src=\"//cdn.cdn.uu126.cn/weixin.png\" />"
		+ "      </div>"
		+ "      <p class=\"qrcode-tip\">打赏</p>"
		+ "     </div>"
 
       
		+ "     <div class=\"qrcode-img qrCode_2\" id=\"qrCode_2\">"
		+ "      <div class=\"qrcode-border box-size\" style=\"border: 9.02px solid rgb(235, 95, 1\">"
		+ "       <img  class=\"qrcode-img qrCode_2\" id=\"qrCode_2\" src=\"//cdn.cdn.uu126.cn/zhifubao.png\" />" 
		+ "      </div>"
		+ "      <p class=\"qrcode-tip\">打赏</p>"
		+ "     </div>"
		
		+ "    </div>"
		+ "   </div>"
		+ "  </div> ";
		$("body").append(content);
	}
$(function(){
	write();
	var $pay = $("#PAY_"+id).hide();
	$pay.find(".ds-payment-way").bind("click",function(){
                $pay.find(".qrcode-img").hide();
		$pay.find(".qrCode_"+$pay.find("input[name=reward-way]:checked").val()).show();
	  });
	 $pay.find(".ds-close-dialog").bind("click",function(){
		  $pay.hide();
	 });
  });
  var PaymentUtils = window['PaymentUtils']={};
  PaymentUtils.show=function(){
	  $("#PAY_"+id).show();
  }
  PaymentUtils.hide=function(){
	  $("#PAY_"+id).hide();
  }
})(jQuery);
```
记得修改代码中的二维码图片地址（不改嘛，你晓在会有什么后果的），然后在` post.php `中找到合适的位置加入：
```php
<button id="dsRewardBtn" onclick="PaymentUtils.show();">赏</button>
```
####文章二维码
```php
<img id="erwei_qr" src="https://api.imjad.cn/qrcode?text=<?php $this->permalink(); ?>&logo=https%3A%2F%2Fapi.imjad.cn%2Fqrcode%2Flogo.png&size=145&level=M&bgcolor=%23ffffff&fgcolor=%23000000" alt="文章二维码"/> <p>扫描二维码，在手机上阅读！</p> 
```
将以上代码放到文章页末尾处即可
####代码高亮
这个用的是Prism提供的，可以直接去：[http://prismjs.com/download.html][2] 下载，挑选好需要的代码格式，如PHP等，另好还可以选好自己喜欢的样式，选好后下载prism.js和prism.css两个文件并上传，接着在` Header.php `或` footer.php `中增加如下代码：
```php
<link rel='stylesheet' id='kratos-diy-style-css'  href='<?php $this->options->themeUrl('css/prism.css'); ?>' type='text/css' media='all' />
<script type='text/javascript' src='<?php $this->options->themeUrl('js/prism.js'); ?>'></script>
```
还有些修改如友链等其它的，整理好再来啰嗦吧，先看会电影了%%>>>>>>




  [1]: https://cdn.uu126.cn/usr/uploads/2017/03/960484595.jpg
  [2]: http://prismjs.com/download.html