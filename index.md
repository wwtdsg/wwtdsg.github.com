---
layout: page
title:  Fuck the world otherwise fucked by the world.
tagline: 
---
{% include JB/setup %}

第一次做博客有点紧张啊！

妈哒弄了一天才搞清除jekyll是什么玩意儿，<摔>！

我还以为可以直接打开一个UI呢！！

结果这货就在那里runing。。runing。。

原来要在浏览器里面啊啊啊！！不看说明书谁知道啊！！

## 还有啊！！

我安装jekyll的时候也是各种报错啊！

## 妈哒！

# stackoverflow确实是个好东西啊思密达。
    
这两天学visual studio c++ 真是一个头两个大，真的是一门单疼的语言啊


    
## 吐槽结束，言归正传：

在很久以前我就打算搭建一个博客，奈何天生愚钝，未遂。然近日啃kivy和c++时，发现还是需要写一个博客，记录自己的学习心得。
一方面是为了自己以后回顾方便，另一方面也可以装装逼。没错，我主要还是为了装逼。


#悄悄的，我来了，这就是我写的博客：
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


最重要的事情要写在最下面：

如果哪位不幸误入本博客，我只能表示深深的歉意。
