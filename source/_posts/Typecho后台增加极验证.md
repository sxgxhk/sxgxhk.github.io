---
title: Typecho后台增加极验证
date: 2017-06-26 19:22
tags: typecho
categories: 网站建设
permalink: 2480
---

得空逛了一下Typecho社区，看见有后台登录验证插件，那就赶紧给自己的后台武装一下吧，省的一天到晚的裸奔（多少总会提升一点安全性吧@(你懂的)）。注册的步骤咱就不说了（不知道网址？百度搜索` 极验证 `），插件之前用的是摸鱼的（下载地址：[https://www.moyu.win/archives/28.html][1]），可是我从头到尾试了好几遍也没成功过，老是显示启用中，考虑到他也是修改前者的，那我就去找前者看看吧。


<!--more-->

 1. 先下载插件（链接: [http://pan.baidu.com/s/1pLx52tX][2] 密码: i7u5），我修改了Plugin.php，把其中一个JS文件的路径由http改为https（不改过的话，像我这种使用https 的站点会提示安全风险）
 2. 开始魔改代码了
 - 增加/admin/geetest-code.php文件，用来返回极验服务响应数据：
```php
<?php
if (!defined('__DIR__')) {
    define('__DIR__', dirname(__FILE__));
}
define('__TYPECHO_ADMIN__', true);
/** 载入配置文件 */
if (!defined('__TYPECHO_ROOT_DIR__') && !@include_once __DIR__ . '/../config.inc.php') {
    file_exists(__DIR__ . '/../install.php') ? header('Location: ../install.php') : print('Missing Config File');
    exit;
}
/** 初始化组件 */
Typecho_Widget::widget('Widget_Init');
/** 如果插件已启用, 则输出极验服务器响应数据 */
if (!empty(Helper::options()->plugins['activated']['Geetest'])) {
    Typecho_Plugin::factory('gt')->server();
}

```
 - 把插件嵌入到登录页面（放到密码框和提交按钮代码中间）login.php：
```php
<?php
include 'common.php';
if ($user->hasLogin()) {
    $response->redirect($options->adminUrl);
}
$rememberName = htmlspecialchars(Typecho_Cookie::get('__typecho_remember_name'));
Typecho_Cookie::delete('__typecho_remember_name');
$bodyClass = 'body-100';
include 'header.php';
?>

<div class="typecho-login-wrap">
    <div class="typecho-login">
        <h1><a href="http://typecho.org" class="i-logo">Typecho</a></h1>
        <form action="<?php $options->loginAction(); ?>" method="post" name="login" role="form">
            <p>
                <label for="name" class="sr-only"><?php _e('用户名'); ?></label>
                <input type="text" id="name" name="name" value="<?php echo $rememberName; ?>" placeholder="<?php _e('用户名'); ?>" class="text-l w-100" autofocus />
            </p>
            <p>
                <label for="password" class="sr-only"><?php _e('密码'); ?></label>
                <input type="password" id="password" name="password" class="text-l w-100" placeholder="<?php _e('密码'); ?>" />
            </p>
            <?php Typecho_Plugin::factory('gt')->render(); ?>
            <p class="submit">
                <button type="submit" class="btn btn-l w-100 primary"><?php _e('登录'); ?></button>
                <input type="hidden" name="referer" value="<?php echo htmlspecialchars($request->get('referer')); ?>" />
            </p>
            <p>
                <label for="remember"><input type="checkbox" name="remember" class="checkbox" value="1" id="remember" /> <?php _e('下次自动登录'); ?></label>
            </p>
        </form>
        
        <p class="more-link">
            <a href="<?php $options->siteUrl(); ?>"><?php _e('返回首页'); ?></a>
            <?php if($options->allowRegister): ?>
            &bull;
            <a href="<?php $options->registerUrl(); ?>"><?php _e('用户注册'); ?></a>
            <?php endif; ?>
        </p>
    </div>
</div>
<?php 
include 'common-js.php';
/** 如果插件已启用, 则输出极验服务器响应数据 */
$geePluginEnable = !empty(Helper::options()->plugins['activated']['Geetest']) ? true : false;
?>
<script>
$(document).ready(function () {
    $('#name').focus();
    <?php if($geePluginEnable):?>
    //极客验证码验证
    (function(){
        var handler = function (captchaObj) {
            // 将验证码加到id为captcha的元素里
            captchaObj.appendTo("#captcha");
        };
        $.ajax({
            // 获取id，challenge，success（是否启用failback）
            url: "geetest-code.php?rand="+Math.random()*100,
            type: "get",
            dataType: "json", // 使用jsonp格式
            success: function (data) {
                // 使用initGeetest接口
                // 参数1：配置参数，与创建Geetest实例时接受的参数一致
                // 参数2：回调，回调的第一个参数验证码对象，之后可以使用它做appendTo之类的事件
                initGeetest({
                    gt: data.gt,
                    challenge: data.challenge,
                    product: type, // 产品形式float-浮动式 embed-嵌入式
                    offline: !data.success //支持本地验证
                }, handler);
            }
        });
    })();
    <?php endif;?>
});
</script>
<?php
include 'footer.php';
?>
```
 - 处理后端登录验证逻辑/var/Widget/Login.php：
```php
<?php
if (!defined('__TYPECHO_ROOT_DIR__')) exit;
/**
 * 登录组件
 *
 * @category typecho
 * @package Widget
 * @copyright Copyright (c) 2008 Typecho team (http://www.typecho.org)
 * @license GNU General Public License 2.0
 */
class Widget_Login extends Widget_Abstract_Users implements Widget_Interface_Do
{
    /**
     * 初始化函数
     *
     * @access public
     * @return void
     */
    public function action()
    {
        // protect
        $this->security->protect();
        /** 如果已经登录 */
        if ($this->user->hasLogin()) {
            /** 直接返回 */
            $this->response->redirect($this->options->index);
        }
        /** 初始化验证类 */
        $validator = new Typecho_Validate();
        $validator->addRule('name', 'required', _t('请输入用户名'));
        $validator->addRule('password', 'required', _t('请输入密码'));
        /** 截获验证异常 */
        if ($error = $validator->run($this->request->from('name', 'password'))) {
            Typecho_Cookie::set('__typecho_remember_name', $this->request->name);
            /** 设置提示信息 */
            $this->widget('Widget_Notice')->set($error);
            $this->response->goBack();
        }
        /** 极客验证码校验 **/
        $this->verifyGeeTest();
        /** 开始验证用户 **/
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
        /** 跳转验证后地址 */
        if (NULL != $this->request->referer) {
            $this->response->redirect($this->request->referer);
        } else if (!$this->user->pass('contributor', true)) {
            /** 不允许普通用户直接跳转后台 */
            $this->response->redirect($this->options->profileUrl);
        } else {
            $this->response->redirect($this->options->adminUrl);
        }
    }
    /**
     * 开始验证用户行为
     */
    private function verifyGeeTest()
    {
        $status = array(
            'empty'   => '请进行拼图验证',
            'failed'  => '验证失败',
            'success' => '验证通过',
            'down'    => '请求超时，请重试',
            'error'   => '服务器异常，请重试'
        );
        /** 如果插件已启用, 则进行验证 */
        if (!empty(Helper::options()->plugins['activated']['Geetest'])) {
            $data = $this->request->from('geetest_challenge', 'geetest_validate', 'geetest_seccode');
            $response = Typecho_Plugin::factory('gt')->verify($data);
            if($response == 'success') {
            } else {
                $error  = !empty($status[$response]) ? $status[$response] : $status['error'];
                $this->widget('Widget_Notice')->set($error);
                $this->response->goBack();
            }
        }
    }
}
```
 - 后台控制台-》插件-》极客登录验证码-》启用
![jyz02.png][3]

 - 填写官网申请的极验验证ID和极验验证Key
![jyz03.png][4]

 - 打开后台登录网址，瞧瞧吧
![jyz04.png][5]

大功告成，心里稳当当的@(酷)




  [1]: https://www.moyu.win/archives/28.html
  [2]: http://pan.baidu.com/s/1pLx52tX
  [3]: https://cdn.uu126.cn/usr/uploads/2017/06/2221809380.png
  [4]: https://cdn.uu126.cn/usr/uploads/2017/06/2935627449.png
  [5]: https://cdn.uu126.cn/usr/uploads/2017/06/91033285.png