---
title: WordPress 批量删除所有文章的特色图像
date: 2016-08-20 19:34
tags: WordPress
categories: 网站建设
permalink: 2185
---

一直用Wordpress做自己的博客站，也算是一路摸索过来吧，Wordpress的功能还是挺赞的，但是文章特色图片一直都没用过，可能是自己不喜欢那种样式吧（心里作怪，哈哈），这不前不久在Wordpress后台安装了一个英文主题，当时就想换个看看也没太大注意，后来换回到自己原来用的主题时，发现所有文章都被自动添加了特色图片，导致俺不喜欢的样式到处都是，怎么办？一个一个删除？工作量太大了，向来都是偷懒的人怎么可能受得了呀，想起了度娘也得到了度娘的真传，就是把以下代码添加到到当前主题的 functions.php里（如果不能在线编辑的话，可以使用WinSCP下载后再编辑，个人建议采用后者），上传覆盖后刷新一下首页，你会发现所有的特色图片都不见了。具体的代码如下：


<!--more-->

```php
/**
 * WordPress 批量删除所有文章的特色图像
 */
global $wpdb;
$attachments = $wpdb-&gt;get_results( "
	SELECT * 
	FROM $wpdb-&gt;postmeta 
	WHERE meta_key = '_thumbnail_id'
	" );
foreach ( $attachments as $attachment ) {
	wp_delete_attachment( $attachment-&gt;meta_value, true );
}
$wpdb-&gt;query( "
	DELETE FROM $wpdb-&gt;postmeta 
	WHERE meta_key = '_thumbnail_id'
	" );
```
温馨提醒一下，这样通过代码保存在 当前主题的functions.php ，所有文章的特色图像都会被删除（只删除文章的特色图像设置数据，图片仍旧会保留在你的媒体库，不会删除），执行了一次以后，你应该删除这段代码，否则你将不可能给文章再添加特色图像（它会继续自动删除）。
