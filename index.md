---
layout: page
title:  丑人博客
tagline: 丑的人才能写出丑的博客。
---
{% include JB/setup %}

在很久以前我就打算搭建一个博客，奈何天生愚钝，未遂。然近日啃kivy和c++时，发现还是需要写一个博客，记录自己的学习心得。

一方面是方便自己总结学习，另一方面也可以装装逼。没错，我主要还是为了装逼。虽然我很丑，但也要装逼。


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>







最重要的事情要写在最下面：

如果哪位不幸误入本博客，我只能表示深深的歉意。



这个博客还真是丑啊……
<section>
<h4>About me</h4>
<div>
 一个无所事事混吃等毕业的烟酒僧，就读于浙大光电系，半路出家xjb敲代码。
 <br/>
 <br/>
 联系博主：wwtdsg@gmail.com
 </div>
 </section>
