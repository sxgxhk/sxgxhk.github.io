---
title: 调用WordPress 文章标签[带文章数统计]
date: 2016-10-29 12:53
tags: WordPress
categories: 网站建设
permalink: 2300
---

我们都知道<code>the_tags</code>和<code>get_the_tags</code>可以调用文章标签，但也无非就是调用标签，其实每个标签都包含了很多参数，只调用名字和链接有点太浪费了，所以我们在加上一个小小的文章数统计，瞬间变的高大上起来。
<h3>实现方法</h3>
下面的代码加到<code>functions.php</code>中:
```php
    function fa_get_the_term_list( $id, $taxonomy ) {
    $terms = get_the_terms( $id, $taxonomy );
    $term_links = "";
    if ( is_wp_error( $terms ) )
        return $terms;
    if ( empty( $terms ) )
        return false;
    foreach ( $terms as $term ) {
        $link = get_term_link( $term, $taxonomy );
        if ( is_wp_error( $link ) )
            return $link;
        $term_links .= '&lt;a href="' . esc_url( $link ) . '" class="js-loaded post--keyword" data-title="' . $term-&gt;name . '" data-type="'. $taxonomy .'" data-term-id="' . $term-&gt;term_id . '"&gt;' . $term-&gt;name . '&lt;sup&gt;['. $term-&gt;count .']&lt;/sup&gt;&lt;/a&gt;';
    }

    return $term_links;
}
```

<!--more-->
调用方法:在loop中使用下面代码即可:
```php
    &lt;?php echo fa_get_the_term_list( get_the_ID(), 'post_tag' );?&gt;
```
