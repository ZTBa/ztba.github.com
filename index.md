---
layout: page
title: Hello World!
tagline: and shut up
---
{% include JB/setup %}

写在前面的话

## 建站目的
首先是有保存零碎知识条目的需要，以前是用docs.google.com，
后来在寻找答案的时候发现，很多我已经解决的问题对于其他人
也许有借鉴的作用。为了避免重复造轮子，干脆换一个渠道，利己利人。

其次也是想练练手，很久没有写字了，偶尔写几句便觉面目可憎，需要进行
回复行训练。加上jekyll生成的效果的确好看，让我有写点东西的欲望。

其实主要是好玩了。

## 建站原则
坚持原创，毕竟网络上面复制黏贴的东西一大堆，没有必要再去增加垃圾。

## 为什么禁止评论
主要是因为懒。有评论，就要有回复，不想失礼，干脆不要。


## 已有文章列表

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do
先把谷歌文档里的知识点倒腾出来
