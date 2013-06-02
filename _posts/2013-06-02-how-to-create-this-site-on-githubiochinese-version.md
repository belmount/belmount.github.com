---
layout: post
title: "how to create this site on github.io(Chinese Version)"
description: ""
category: posts
tags: [github,blog,jekyll,bootstrap]
---
{% include JB/setup %}

## github的设置

* 首先你必须有个github账号
* 然后你去建立一个yourusername.github.com的repo
* 在你本地建一个repo和github上一致

具体操作如下


    $git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
    $cd USERNAME.github.com
    $git remote set-url origin https://github.com/USERNAME/USERNAME.github.com.git
    $git push -u origin master


然后等待个几分钟，你的USERNAME.github.io的站点就出现了。

## Trouble shooting

本人在windows下使用ruby 2.0.0, pik 0.2.8 环境下，出现以下问题

###1  liquid 找不到 2.0/yajl 文件

  _原因：_yajl没有为2.0进行编译，解决办法

    $cd \yajl-ruby-1.1.0-x86-mingw32\ext\yajl
    $ruby extconf.rb
    $set PATH=%PATH%;[make path]
    $make 
    $make install
    $mkdir ..\yajl-ruby-1.1.0-x86-mingw32\lib\yajl\2.0
    $copy [install destination] ..\yajl-ruby-1.1.0-x86-mingw32\lib\yajl\2.0

###2 不识别python命令

  没办法找个python装上，并include到path里面去

###3 invalid byte sequence

    chcp 65001

## Enjoy Yourself on Github.io
