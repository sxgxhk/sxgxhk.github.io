---
title: 给Wordpress评论者信息栏加入新浪微博账号输入框
date: 2016-10-29 12:48
tags: WordPress
categories: 网站建设
permalink: 2299
---

首先就是无论你采用哪种方法，都要在<code>functions.php</code>中加入如下代码:
```php
    add_action( 'comment_post','save_comment_meta_data' );
    function save_comment_meta_data( $comment_id ) {
        add_comment_meta( $comment_id, 'sinawb', $_POST['sinawb'] );
        $expire = time() + 99999999;
        $domain = ($_SERVER['HTTP_HOST'] != 'localhost') ? $_SERVER['HTTP_HOST'] : false; // make cookies work with localhost
        setcookie('bigfa_sinawb',$_POST['sinawb'],$expire,'/',$domain,false);
    }
    add_filter( 'get_comment_author_link',    'attach_twitter_to_author' );
    function attach_twitter_to_author( $author ) {
        $tw = get_comment_meta( get_comment_ID(), 'sinawb', true );
        if($tw)
            $author .= " / &lt;a href='http://weibo.com/n/$tw' title='@$tw' target='_blank'&gt;@$tw&lt;/a&gt;";
        return $author;
    }
```
<!--more-->

如果是自定义评论，则在<code>comments.php</code>相应位置:
<pre class="pure-highlightjs"><code class="php">&lt;label id="author_sinawb" for="sinawb"&gt;Weibo.&lt;input id="sinawb" type="text" tabindex="5" value="&lt;?php if(isset($_COOKIE['bigfa_sinawb'])) echo $_COOKIE['bigfa_sinawb'];?&gt;" name="sinawb"&gt;
&lt;/label&gt;</code></pre>
如果是默认的comment_form，则下面的代码加到<code>functions.php</code>中:
<pre class="pure-highlightjs"><code class="php">add_filter( 'comment_form_defaults',    'change_comment_form_defaults');
function change_comment_form_defaults($default) {
    $commenter = wp_get_current_commenter();
    $default['fields']['url'] .= '&lt;p class="comment-form-author"&gt;&lt;label id="author_sinawb" for="sinawb"&gt;Weibo&lt;input id="sinawb" type="text" tabindex="5" value="';
    if(isset($_COOKIE['bigfa_sinawb'])) $default['fields']['url'] .= $_COOKIE['bigfa_sinawb'];
    $default['fields']['url'] .='" name="sinawb"&gt;&lt;/label&gt;&lt;/p&gt;';
    return $default;
}</code></pre>
&nbsp;
