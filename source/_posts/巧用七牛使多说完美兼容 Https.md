---
title: 巧用七牛使多说完美兼容 Https
date: 2016-11-26 14:22
tags: typecho
categories: 网站建设
permalink: 2365
---

之前写过一篇文章“[让Typecho中的多说头像支持HTTPS][1]"，头像是可以支持Https了，但里面的表情却不行，这不又看到好文说可以[完美兼容Https][2]，赶紧照样画葫芦用起来呗：
1、设置七牛，将镜像源设置为https://yourdomain.com/proxy/即可。
2、修改 Embed.js，首先从 [https://static.duoshuo.com/embed.js][3] 下载然后格式化。
3、修改头像，位于 580 行附近，搜索 avatarUrl: function(e) { 直达。


<!--more-->


    if (document.location.protocol == "https:") {
    if (e.avatar_url) {
        var matched;
        if (matched = e.avatar_url.match(/^([^\/]*(\/\/q\.)*(\/\/app\.)*qlogo\.cn\/.+\/)\d{2}$/)) {
            e.avatar_url = matched[1] + "100"; // 50的图高分屏下会很模糊，改不改随你。不改的话整个 If 分支就不需要了。
            e.avatar_url = e.avatar_url.replace(/^http\:\/\//, "https://");
        } else if (matched = e.avatar_url.match(/^(.*avatar\.duoshuo\.com\/avatar\-)\d{2}(\/.+)$/)) {
            e.avatar_url = matched[1] + "100" + matched[2]; // 50的图高分屏下会很模糊，改不改随你。不改的话整个 If 分支就不需要了。
            e.avatar_url = e.avatar_url.replace(/^http\:\/\//, "https://");
        } else if (e.avatar_url.match(/^http:\/\/wx\.qlogo\.cn.*$/) || e.avatar_url.match(/^.*avatar\.duoshuo\.com.*$/)) {
            e.avatar_url = e.avatar_url.replace(/^http\:\/\//, "https://");
        } else if (matched = e.avatar_url.match(/^(.*qlogo\.cn\/.+\/)\d{2}$/)) {
            e.avatar_url = matched[1] + "100"; // 50的图高分屏下会很模糊，改不改随你。不改的话整个 If 分支就不需要了。
        }  else if (e.avatar_url.match(/^.*img\d+\.douban\.com.*$/)) {
            e.avatar_url = e.avatar_url.replace(/img\d+\.douban\.com/, "img2.doubanio.com");//豆瓣无需镜像
        } 
        if (e.avatar_url.match(/^http:\/\/.+$/)) {
            e.avatar_url = "https://yours.qnssl.com/" + base64_encode(e.avatar_url);
            // 像大神们的那种反代大概就可以改成这样：
            // e.avatar_url = "https://yours.qnssl.com/" + e.avatar_url.replace(/^http\:\/\//, "");
            // 普通的地址：
            // e.avatar_url = "https://yourdomain.com/proxy?src=" + e.avatar_url;
        }
    } else {
        var matched = rt.data.default_avatar_url.match(/^(.*avatar\.duoshuo\.com\/avatar\-)\d{2}(\/.+)$/);
        if (matched != null) {
            rt.data.default_avatar_url = matched[1] + "100" + matched[2]; // 50的图高分屏下会很模糊，改不改随你。不改的话整个 If 分支就不需要了。
        }
    }
}
// 默认头像现在多说已经支持 Https, 因此不需要修改。
return e.avatar_url || rt.data.default_avatar_url
4、修改评论中的表情和图片，位于 740 行附近，搜索s.message直达。

        var matched;
    while(matched=s.message.match(/<img\s+src\s*=\s*\"(http:\/\/[^\"]+)\"/)) {
        s.message = s.message.replace(matched[1], "https://yours.qnssl.com/" + base64_encode(matched[1]));
        // 像大神们的那种反代大概就可以改成这样：
        // s.message = s.message.replace(matched[1], "https://yours.qnssl.com/" + matched[1].replace(/^http\:\/\//, ""));
        // 普通的地址：
        // s.message = s.message.replace(matched[1], "https://yourdomain.com/proxy?src=" + matched[1]);
    }
    
    // 下面这一行为`s.message`所在行，不需要修改，在此行前加上上面几行代码即可。
    if (t += "<p>", s.parents.length >= i.max_depth && (!i.show_context || i.max_depth > 1) && s.parent_id && lt[s.parent_id] && (t += '<a class="ds-comment-context" data-post-id="' + s.post_id + '" data-parent-id="' + s.parent_id + '">' + D.reply_to + u(lt[s.parent_id].toJSON().author.name) + ": </a>"), t += s.message + '</p>
    
    <div class="ds-comment-footer ds-comment-actions', s.vote > 0 && (t += " ds-post-liked"), t += '">', t += s.url ? et.timeAnchor(s.created_at, s.url) : et.timeText(s.created_at), "duoshuo" == s.source ? (t += '<a class="ds-post-reply" href="javascript:void(0);"><span class="ds-icon ds-icon-reply"></span>' + D.reply + "</a>" + et.likePost(s) + '<a class="ds-post-repost" href="javascript:void(0);"><span class="ds-icon ds-icon-share"></span>' + D.repost + '</a><a class="ds-post-report" href="javascript:void(0);"><span class="ds-icon ds-icon-report"></span>' + D.report + "</a>", s.privileges["delete"] && (t += '<a class="ds-post-delete" href="javascript:void(0);"><span class="ds-icon ds-icon-delete"></span>' + D["delete"] + "</a>")) : ("qqt" == s.source || "weibo" == s.source) && (t += '<a class="ds-weibo-comments" href="javascript:void(0);">' + D.comments, s.type.match(/\-comment$/) || (t += '(<span class="ds-count">' + s.comments + "</span>)"), t += '</a><a class="ds-weibo-reposts" href="javascript:void(0);">' + D.reposts, s.type.match(/\-comment$/) || (t += '(<span class="ds-count">' + s.reposts + "</span>)"), t += "</a>"), t += "</div>
    
    </div></div>", i.max_depth > 1 && (s.childrenArray || s.children) && "weibo" != s.source && "qqt" != s.source) {
        t += '
    
    <ul class="ds-children">';
        var c = lt.getJSON(s.childrenArray || s.children);
        if (c) for (var s, d = -1, p = c.length - 1; p > d;) s = c[d += 1], t += et.post({
            post: s,
            options: i
        });
        t += "</ul>
    
    "
    }

5、修改表情框中的表情，位于 1448 行附近，搜索http://img.t.sinajs.cn/t35/style/images/common/face/ext/normal/直达

    function t(t, s) {
    // 仅需要修改微博的表情，Wordpress 的不需要修改，默认为https
    // 仅需修改这一行
    var i = 0 === e.indexOf("微博") ? "https://yours.qnssl.com/" + base64_encode("http://img.t.sinajs.cn/t35/style/images/common/face/ext/normal/" + s.replace("_org", "_thumb")) : S.STATIC_URL + "/images/smilies/" + s;
    // 像大神们的那种反代大概就可以改成这样：
    // var i = 0 === e.indexOf("微博") ? "https://yours.qnssl.com/" + "img.t.sinajs.cn/t35/style/images/common/face/ext/normal/" + s.replace("_org", "_thumb") : S.STATIC_URL + "/images/smilies/" + s;
    // 普通的地址：
    // var i = 0 === e.indexOf("微博") ? "https://yourdomain.com/proxy?src=" + "http://img.t.sinajs.cn/t35/style/images/common/face/ext/normal/" + s.replace("_org", "_thumb") : S.STATIC_URL + "/images/smilies/" + s;
    "WordPress" === e && (t = " " + t + " "), a += '<li><img src="' + i + '" title="' + _(t) + '" /></li>'
}
6、附带一个 base64_encode 方法：放到开头处!function(e, t, s) {的后面即可。

    function base64_encode(str){
    var c1, c2, c3;
    var base64EncodeChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+-";                
    var i = 0, len= str.length, string = '';

    while (i < len){
        c1 = str.charCodeAt(i++) & 0xff;
        if (i == len){
            string += base64EncodeChars.charAt(c1 >> 2);
            string += base64EncodeChars.charAt((c1 & 0x3) << 4);
            string += "==";
            break;
        }
        c2 = str.charCodeAt(i++);
        if (i == len){
            string += base64EncodeChars.charAt(c1 >> 2);
            string += base64EncodeChars.charAt(((c1 & 0x3) << 4) | ((c2 & 0xF0) >> 4));
            string += base64EncodeChars.charAt((c2 & 0xF) << 2);
            string += "=";
            break;
        }
        c3 = str.charCodeAt(i++);
        string += base64EncodeChars.charAt(c1 >> 2);
        string += base64EncodeChars.charAt(((c1 & 0x3) << 4) | ((c2 & 0xF0) >> 4));
        string += base64EncodeChars.charAt(((c2 & 0xF) << 2) | ((c3 & 0xC0) >> 6));
        string += base64EncodeChars.charAt(c3 & 0x3F)
    }
    return string
}
代码弄好以后，接下来就可以自定义表情了，[好文][4]还有说道（仔细看应该都能会的吧，这里我就不啰嗦了），还有关键的地方就是多说设置中的自定义CS里添加：

    #ds-smilies-tooltip ul.ds-smilies-tabs {
        display: none;
    }
    #ds-smilies-tooltip .ds-smilies-container {
        padding: 15px 15px;
        height: 200px;
        margin-left: 0 !important;
    }
    #ds-smilies-tooltip .ds-smilies-container li {
        width: auto;
        max-width: 100px;
        height: 40px !important;
        margin: 0 7px;
    }

天冷，少点几行字，有问题留言吧

  [1]: https://cdn.uu126.cn/post/2352.html
  [2]: https://hran.me/archives/use-qiniu-to-make-duoshuo-compliant-with-https.html
  [3]: https://static.duoshuo.com/embed.js
  [4]: https://hran.me/archives/caisiduo-composition-collection.html