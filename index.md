---
layout: page
title: 分享，乐趣
preview_size: 100
---
{% include JB/setup %}

![avatar](https://github.com/belmount/belmount.github.com/blob/master/assets/images/avatar.JPG?raw=true)

<ul>
  {% for post in site.posts %}
    <li>
      <h3>{{ post.title}} </h3>
      <p>{{ post.description }} 
        <span class="pull-right">
          <a href="{{ post.url }}">全文</a>  {{post.date | date_to_string}}
        </span>
      </p>
      <hr/>
      
    </li>
  {% endfor %}
</ul>
