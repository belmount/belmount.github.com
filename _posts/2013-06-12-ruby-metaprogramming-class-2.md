---
layout: post
title: "Ruby Metaprogramming Class 2"
description: " Class defination for metaprogramming"
category: Ruby
tags: [Ruby, Metaprogramming]
---
{% include JB/setup %}

### Class.new
定义一个Class的另一种方式就是通过Class#new。Class#new 出来的是一个匿名class。
要把它给正名，Ruby 提供了一种非常规的方式，就是直接把匿名class 变量给 常量赋值。

 c = Class.new(Lover) do 
    def make
      puts "heixiu" 
    end 
 end

 TheThird = c # 为小三正名

 ### Dynamic Language and Singleton Methods
 
 Ruby是个动态语言，因此它的Object 并不是所在Class的完全翻版，这是他和其他静态语言的重要区别之一。
 
 Singleton Methods 就是符合这一特性的，他是为某个对象专门定义的 methods，因此称作 singleton methods。
 这又引发了另外一个概念，叫做 duck typing。说白了不存在类型之分， Duck class 的 object 和一般的object 定义了 duck的功能，即即使你不是鸭子，只要你下定决心当一只鸭子，并且以鸭子的行为准则来约束自己，Ruby 很宽容的就认为你是一只鸭。

 Singleton Methods在 Ruby 中用的相当广泛，最普及的是 Class Methods,也就是那些self 开头的methods， 他就是Class Object 的singleton methods。

 ### 3 Ways to define Class Methods
 孔乙己说茴香豆的回有三种写法，Ruby中 Class Methods 也有三种定义方法

    Class A
      def self.some_method
      end
    end

    def A.some_method 
    end

    Class A
      class << self
        def some_method
        end
      end
    end

哪一种最不好用？ 第二种！其他两种以个人口味酌情使用。
