---
layout: post
title: "Ruby Metaprogramming Tricks Open Class"
description: "Open class in Ruby"
category: Ruby
tags: [metaprogamming, ruby]
---
{% include JB/setup %}

> Metaprogramming is the writing of computer programs that write or manipulate other programs (or themselves) as their data
> -- wikipedia

### 什么是元编程

元编程就是编写生成程序的程序，拗口吧？ 拗口就用e文。这个meta就是程序的meta data,包括class name, type, ancestors, paramter count, 
paramter name 等信息，通过使用和修改这些信息，实现新程序的产生。

### 为什么Ruby 适合进行 metaprogramming

> Ruby’s  reflection  API—along  with  its  generally  dynamic  nature,  its  blocks-and-iterators
> control  structures,  and  its  parentheses-optional  syntax—makes  it  an  ideal
> language  for  metaprogramming.
> -- Matz in __Ruby Programming Language__

### Ruby 的 Open Classes 特性

Ruby 的 Class 不是一经定义就无法修改，相反Class 是可以随时打开的。

例如 Rails 中的 string 就比标准库中的 string 多出了许多函数。

但Open classes 的不当使用会导致 Monkey See, Monkey Patch 的问题，也就是说A码农为了自己方便 Open 了 B 码农的 class, 最后导致 C 码农在不知情的情况下按照B码农的思维使用了A码农的代码，结果程序 :(了。

### Open 有风险，Writing 需谨慎

为了避免 ABC码农乱搞对象的事情的重复发生，码程序的时候务必要一停，二看，三通过。
首先要理解Ruby的对象模型，也就是 Class是干嘛的， Module是扮演什么角色。Kernel 和 Basic Object 又是哪路的。

所以对一个Class open之前，先打听下它是什么背景。具体问话如下：

    a.class # 你是哪一路的？
    a.class.methods # 你们那一路都有什么本事？  
    a.methods # 具体到你个人，有什么本事？
    a.superclass # 你老爹是谁？
    a.class.constants # 你家有什么标志？
    a.ancestors # 你家族到底是怎么个关系，包括包养的--include 进来的module

问的差不多了，发现没有家族遗传史，该干就干，直接开包:)。 