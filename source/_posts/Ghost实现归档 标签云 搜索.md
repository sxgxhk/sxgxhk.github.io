---
title: Ghost实现归档 标签云 搜索
date: 2017-07-25 20:52
tags: Ghost
categories: 网站建设
permalink: 2487
---

前面说过，Ghost本身并不自带类似文章归档、标签云，搜索等（后台不知道会不会改进），虽然官方没有给出，但这也不能阻止民间高手们，通过API就能实现上述这些功能。

<!--more-->

#### 归档
 - 首先得在后台的` 实验功能 `中启用` API `，如图:
![](https://cdn.uu126.cn/image/a/63/912947500d1432b9e4c07155674de.jpg)

 - 新建自定义页面：

 1.创建一个静态页面：在ghost后台新建页面，发布为 独立页面 ，标题为Archives，网址可以设置为` 域名/archives-post `

 2.接着创建一个自定义页面模板：该模板是第一步创建的静态页面的模板，创建一个page-url.hbs模板，如果第一步设置的页面网址为` 域名/archives-post `，那么模板即为` page-archives-post.hbs `。将该模板上传至主题根目录下即可，此时访问域名/archives-post，即会调用自定义的page-archives-post.hbs这个模板。

 - 在page-archives.hbs中调用Ghost API即可：
```php
!< default&

<main class="content" role="main">  
<article class="archives"></article>  
</main>

<script src="//cdn.bootcss.com/jquery/3.1.0/jquery.min.js"></script>  
<script src="//cdn.bootcss.com/moment.js/2.14.1/moment.min.js"></script>  
<script type = "text/javascript">  
/**
 * 调用ghost API，完成文章归档功能
 * 所需组件：jQuery、moment.js
 * @ldsun.com
 */
jQuery(document).ready(function() &#123;  
    //获取所有文章数据，按照发表时间排列
    $.get(ghost.url.api('posts', &#123;
        limit: 'all',
        order: "published_at desc"
    &#125;)).done(function(data) &#123;
        var posts = data.posts;
        var count = posts.length;
        for (var i = 0; i < count; i++) &#123;
            //调用comentjs对时间戳进行操作
            //由于ghost默认是CST时区，所以日期会有出入，这里消除时区差,如果不需要，把下面第二行后面8小时改为-00:00!!!
            var time = moment(posts[i].published_at).utcOffset("-08:00");
            var year = time.get('y');
            var month = time.get('M')+1;
            var date = time.get('D');
            if( date<10 ) date = "0"+date;
            var title = posts[i].title;
            var url = "@blog.url&"+posts[i].url;
            //首篇文章与其余文章分步操作
            if (i > 0) &#123;
                var pre_month = moment(posts[i - 1].published_at).utcOffset("-08:00").get('month')+1;
                //如果当前文章的发表月份与前篇文章发表月份相同，则在该月份ul下插入该文章，这里消除时区差,如果不需要，把下面第二行后面8小时改为-00:00!!!
                if (month == pre_month) &#123;
                    var html = "<li><time>"+date+"日</time><a href='"+url+"'>"+title+"</a></li>";
                    $(html).appendTo(".archives .list-"+year+"-"+month);
                &#125;
                //当月份不同时，插入新的月份
                else&#123;
                    var html = "<div class='item'><h3><i class='fa fa-calendar fa-fw' aria-hidden='true'></i> "+year+"-"+month+"</h3><ul class='archives-list list-"+year+"-"+month+"'><li><time>"+date+"日</time><a href='"+url+"'>"+title+"</a></li></ul></div>";
                    $(html).appendTo('.archives');
                &#125;
            &#125;else&#123;
                var html = "<div class='item'><h3><i class='fa fa-calendar fa-fw' aria-hidden='true'></i> "+year+"-"+month+"</h3><ul class='archives-list list-"+year+"-"+month+"'><li><time>"+date+"日</time><a href='"+url+"'>"+title+"</a></li></ul></div>";
                $(html).appendTo('.archives');
            &#125;
        &#125;
    &#125;).fail(function(err) &#123;
        console.log(err);
    &#125;);
&#125;); 
</script>
```
相应的CSS代码如下：
```css
.archives .item &#123;

&#125;
.archives h3 &#123;
    font-size: 17px;
    font-weight: bold;
    margin: 0 0 15px 0;
&#125;
.archives-list &#123;
    list-style: none;
    font-size: 16px;
    line-height: 20px;
    padding-left: 25px;
&#125;
.archives-list li &#123;
    padding: 3px 0;
    overflow: hidden;
&#125;
.archives-list time &#123;
    margin-right: 5px;
    color: #999;
    display: inline-block;
    width: 35px;
    font-family: monospace;
&#125;
.archives-list a &#123;
    text-decoration: none;
&#125;
```

#### 标签云
 - 介绍一下` tag_cloud `

这是中文版中增加的一个 handlebars 助手（helper），根据标签所关联的文章数量进行逆排序，也就是关联文章多的先输出；支持一个 limit 参数，用于限定输出的标签数量：all 表示输出所有标签；数字（例如 10）指定输出个数。调用方式如下：
```php
//输出所有标签
tag_cloud limit="all"&

或者

//输出前20个标签（标签按照对应的文章数量逆排序）
tag_cloud limit=20&
```

 - 使用方法

在需要输出“标签云”的地方调用` tag_cloud `助手即可。我们以 casper 默认主题为例。打开` index.hbs `文件，在` </header> `标签后面添加` tag_cloud limit=10& `，保存此文件、重启 Ghost、打开首页，看看是否输出了一堆标签。我是直接放在归档里的，具体放哪又或是新增页面就看各自喜好了，相应的css代码如下：
```css
.tag-cloud &#123;
    padding-left: 25px;
&#125;
.tag-cloud &#123;
    padding-left: 1.725rem;
&#125;
.tag-cloud a:hover &#123;
    background: #eee;
    color: #4A4A4A;
&#125;
.tag-cloud a &#123;
    position: relative;
    display: inline-block;
    padding: .725rem;
    font-style: normal;
    line-height: 1.125rem;
    border-radius: .3rem;
    text-decoration: none;
    font-size: 16px;
    margin-bottom: 3px;
&#125;
```

#### 搜索

使用` jQuery.gsearch.js `插件实现Ghost博客的搜索，gsearch是一款为Ghost量身打造的搜索插件，使Ghost轻松具有搜索功能。效果类似于SwifType，通过Ghost的RSS实现搜索功能，您可以点击右上角搜索图标体验效果。

 -  [下载](https://github.com/itobee/gsearch/archive/master.zip)最新版的gsearch，将` dist `文件夹下的` libs `和` partials `文件夹复制到当前主题的根目录下（如果遇到同名文件夹，覆盖即可），并在当前主题的` default.hbs `文件中添加如下代码：
```html
<!DOCTYPE html>  
<html lang="zh-cn">  
<head>  
   ……
   <!-- CSS -->
   <link rel="stylesheet" type="text/css" href="/libs/remodal/remodal.min.css">
   <link rel="stylesheet" type="text/css" href="/libs/gsearch/css/jquery.gsearch.min.css">
   ……
</head>

<body>  
   ……

   <!-- gsearch模板 -->
   > "gsearch"&

   <!-- JS -->
   <script type="text/javascript" src="//cdn.staticfile.org/jquery/1.12.4/jquery.min.js"></script> <!-- 如果主题中未引入jQuery，请引入jQuery -->
   <script type="text/javascript" src="/libs/remodal/remodal.min.js"></script>
   <script type="text/javascript" src="/libs/gsearch/js/jquery.gsearch.min.js"></script>
</body>  
</html>  
```

 - 接下来，我们再通过如下代码调用插件即可使用：
```python
<script>  
$(document).one('opening', '.remodal', function () &#123;
    $('#search').gsearch();
&#125;);
</script>  
```
这段代码我是直接丢在后台的` 插入代码 `里
注：以上示例使用了` remodal `弹层插件显示搜索信息，可以用其他插件代替，但是需要注意一点：请勿对同一元素重复调用插件。示例中使用jQuery的` .one() `方法来委托事件。
关于搜索功能等信息，可详见原作者[博客](http://www.tobee.me/gsearch/)

实现了这三个功能，暂时可以让我消停一下了，要知道菜鸟要实现这些功能可也是花了好久功夫的，这个过程还是挺享受的，这不现地又得找点可以折腾的东西来……