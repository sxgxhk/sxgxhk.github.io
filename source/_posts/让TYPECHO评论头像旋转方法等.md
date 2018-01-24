---
title: 让TYPECHO评论头像旋转方法等
date: 2017-04-07 23:04
tags: typecho
categories: 网站建设
permalink: 2456
---

查找当前主题下的style.css。CSS文件根据主题自行选择可能名称不符。找到以下代码：


<!--more-->

```css
#comments .comment-author .avatar{display:block;float:left;width:40px;height:40px;margin:1.4rem 1rem 0 0;border-radius:50%}
```
修改为：
```css
#comments .comment-author .avatar{display:block;float:left;width:40px;height:40px;margin:1.4rem 1rem 0 0;border-radius:50%;transition: all 0.5s}
#comments .comment-author .avatar:hover{transform:rotate(360deg)}
```
即可大功告成@(太开心)
具体演示效果，可参照本博客评论头像，如果你跟我一样使用Hran的主题，可以直接查找替换。

` transition: all 0.5s `是表示旋转在0.5秒内完成，可自行修改速度
` transform: rotate(360deg) `表示图片旋转360度，可自行修改

到处逛博客圈，又发现好多好东西，赶紧收藏起来，感谢Destined博主的奉献！
#### Typecho免插件实现博主认证

打开` comments.php `，找到下面这条代码：
```php
<span class="comment-author"><?php $comments->author(); ?></span>
```
在后面加上如下代码：
```php
<?php
$me = md5(strtolower('jslm@qq.com')); //这里填入自己的邮箱
$rz = md5(strtolower($comments->mail)); //用于判断邮箱
//博主样式
$str =  '<span class="commentapprove" style="color: #FFF;padding: .1rem .25rem;font-size: .7rem;border-radius: .25rem;background-color:#1ECD97;" >博主</span>';
//开始判断
if($me==$rz){
echo $str;          //如果条件成立则输出'博主'样式
}
?>
```
#### Typecho留言加上@
 - 打开模板文件夹里的functions.php文件，添加以下代码:
```php
/*评论增加@功能*/
function getPermalinkFromCoid($coid) {   
$db       = Typecho_Db::get();   
$options  = Typecho_Widget::widget('Widget_Options');   
$contents = Typecho_Widget::widget('Widget_Abstract_Contents');   

$row = $db->fetchRow($db->select('cid, type, author, text')->from('table.comments')   
          ->where('coid = ? AND status = ?', $coid, 'approved'));   

if (empty($row)) return 'Comment not found!';   
$cid = $row['cid'];   

$select = $db->select('coid, parent')->from('table.comments')   
          ->where('cid = ? AND status = ?', $cid, 'approved')->order('coid');   

if ($options->commentsShowCommentOnly)   
    $select->where('type = ?', 'comment');   

$comments = $db->fetchAll($select);   

if ($options->commentsOrder == 'DESC')   
    $comments = array_reverse($comments);   

foreach ($comments as $key => $val)   
    $array[$val['coid']] = $val['parent'];   

$i = $coid;   
while ($i != 0) {   
    $break = $i;   
    $i = $array[$i];   
}   

$count = 0;   
foreach ($array as $key => $val) {   
    if ($val == 0) $count++;    
    if ($key == $break) break;    
}   

$parentContent = $contents->push($db->fetchRow($contents->select()->where('table.contents.cid = ?', $cid)));   
$permalink = rtrim($parentContent['permalink'], '/');   

$page = ($options->commentsPageBreak)   
      ? '/comment-page-' . ceil($count / $options->commentsPageSize)   
      : ( substr($permalink, -5, 5) == '.html' ? '' : '/' );   

return array(   
    "author" => $row['author'],   
    "text" => $row['text'],   
    "href" => "{$permalink}{$page}#{$row['type']}-{$coid}"  
);   
}  
```
 - 然后打开你的` comment.php `文件，找到以下代码：
```php
<?php $comments->content(); ?>
```
在其前面加入如下代码：
```php
<?php  
if($comments->parent){   
    $p_comment = getPermalinkFromCoid($comments->parent);   
    $p_author = $p_comment['author'];   
    $p_text = mb_strimwidth(strip_tags($p_comment['text']), 0, 100,"...");   
    $p_href = $p_comment['href'];   
    echo "<span>@</span><a href='$p_href' title='$p_text'>$p_author</a>";   
}   
?> 
```
#### 网站运行时间
将以下代码，放置于主题的` functions.php `中：
```php
// 设置时区
date_default_timezone_set('Asia/Shanghai');
/**
* 秒转时间，格式 年 月 日 时 分 秒
*
* @author Roogle
* @return html
*/
function getBuildTime(){
// 在下面按格式输入本站创建的时间
$site_create_time = strtotime('2010-09-10 00:00:00');
$time = time() - $site_create_time;
if(is_numeric($time)){
$value = array(
"years" => 0, "days" => 0, "hours" => 0,
"minutes" => 0, "seconds" => 0,
);
if($time >= 31556926){
$value["years"] = floor($time/31556926);
$time = ($time%31556926);
}
if($time >= 86400){
$value["days"] = floor($time/86400);
$time = ($time%86400);
}
if($time >= 3600){
$value["hours"] = floor($time/3600);
$time = ($time%3600);
}
if($time >= 60){
$value["minutes"] = floor($time/60);
$time = ($time%60);
}
$value["seconds"] = floor($time);
 
echo '<span class="btime">'.$value['years'].'年'.$value['days'].'天'.$value['hours'].'小时'.$value['minutes'].'分</span>';
}else{
echo '';
}
}
```
然后在需要显示的地方使用以下代码调用即可：
```php
<?php getBuildTime(); ?>
```