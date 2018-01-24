---
title: 给Typecho后台添加图片验证
date: 2016-11-26 21:15
tags: typecho
categories: 网站建设
permalink: 2370
---

Typecho比之Wordpress的确比较轻巧，使用起来也比较容易上手，唯一的不足就是使用人群少，相对的主题、插件、教程要比Wordpress少，但却丝毫不影响我使用。使用Typecho有段时间了，平常写写小博客那是绰绰有余，就是感觉这后台登录是不是缺了点什么，虽然可以自定义后台路径，可感觉还是得纶它加点什么，哪怕只是那点小验证，那就动手吧。也说一下Typecho，ypecho有很好的路由机制，并且MVC模型模块化做的很好。系统代码在/var/中，包括了：Widget工具，Typecho模型等。开发者二次开发代码在/usr/中，主要放置Theme主题，Plugins插件等。添加验证码不能用插件形式，因此只能直接在源码中更改。在Wordpress的时候俺就用过极验证，东西不错挺好的，但免费版不支持Https，所以只能作罢（心里作用还吧，呵呵），那么就用php生成图片的验证方法。
<!--more-->
1、修改前端代码，修改/admin/login.php，添加如下：

```
<p><img style="cursor:pointer" title="刷新验证码" id="refresh" border='0' src='verify.php' onclick="document.getElementById('refresh').src='typecho-code.php?t='+Math.random()"/></p>
<p><label for="name" class="sr-only"><?php _e('验证码'); ?></label>
<input type="text" id="name" name="verify" value="" placeholder="<?php _e('验证码'); ?>" class="text-l w-100" /></p>
```
这里的verify.php建议修改成一个图片验证或按钮什么的，美观点（不相信你自己先试着不改看看呗）
2、添加php生成验证码的php插件，添加到/admin/typecho-code.php、/admin/typecho-t1.ttf，代码下载:
快点[百度网盘][1]  密码: yrp5
3、修改后台验证代码，修改/var/Widget/Login.php如下：
```php

        /** 截获验证异常 */
        if ($error = $validator->run($this->request->from('name', 'password'))) {
            Typecho_Cookie::set('__typecho_remember_name', $this->request->name);

            /** 设置提示信息 */
            $this->widget('Widget_Notice')->set($error);
            $this->response->goBack();
        }
        /*比对验证码*/
        if (strtolower($_POST['verify']) != strtolower($_SESSION['verify']) ) {
            /** 防止穷举,休眠3秒 */
            sleep(3);

            $this->widget('Widget_Notice')->set(_t('验证码错误！'), 'error');
            $this->response->goBack('?referer=' . urlencode($this->request->referer));
            $flag = false;
        }else{
        	/** 如果验证码对了，开始验证用户 **/
        	$valid = $this->user->login($this->request->name, $this->request->password,
        	false, 1 == $this->request->remember ? $this->options->gmtTime + $this->options->timezone + 30*24*3600 : 0);

	        /** 比对密码 */
    	    if (!$valid) {
        	    /** 防止穷举,休眠3秒 */
            	sleep(3);

            	$this->pluginHandle()->loginFail($this->user, $this->request->name,
            	$this->request->password, 1 == $this->request->remember);

            	Typecho_Cookie::set('__typecho_remember_name', $this->request->name);
            	$this->widget('Widget_Notice')->set(_t('用户名或密码无效'), 'error');
            	$this->response->goBack('?referer=' . urlencode($this->request->referer));
        	}

        	$this->pluginHandle()->loginSucceed($this->user, $this->request->name,
        	$this->request->password, 1 == $this->request->remember);
	    }

        /** 跳转验证后地址 */
        if (NULL != $this->request->referer) {
            $this->response->redirect($this->request->referer);
        } else if (!$flag ){
            /** 不允许普通用户直接跳转后台 */
            $this->response->redirect($this->options->siteUrl);
        } else if (!$this->user->pass('contributor', true) ){
            /** 不允许普通用户直接跳转后台 */
            $this->response->redirect($this->options->siteUrl);
        } else {
            //echo $this->options->adminUrl;
            $this->response->redirect($this->options->adminUrl);
        }
    }
}
```
直接复制替换掉Login.php里面的相应的内容，保存后上传覆盖，刷新一下你的登录界面看一下，是不是多了一个图片验证了（有用的哦）
![20161126211430.jpg][2]


  [1]: https://pan.baidu.com/s/1o81zQA2
  [2]: https://cdn.uu126.cn/usr/uploads/2016/11/2069790031.jpg