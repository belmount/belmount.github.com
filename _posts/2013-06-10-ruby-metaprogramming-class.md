---
layout: post
title: "Ruby Metaprogramming Class"
description: " Class definination"
category: Ruby 
tags: ['Ruby' , 'Metaprogramming']
---
{% include JB/setup %}

### Class 定义中的 self
self 这个变量是和环境息息相关的，典型的见人说人话，见鬼说鬼话的家伙。
在Class 定义的过程中，self代表的是class object，而不是具体化的instance。
这个时候self 就像某些洗脑成功的人士，出去说话就说代表某某组织，不是代表个人一样。
因此

    def self.xxxx
    end

代表的是定义class里面的函数，调用的时候只能通过 Class名称而不能通过instance进行调用。

### class_eval
举一反三，他和instance_eval的唯一区别是他干的是抽象的工作， 后者干的是具体活。

### Class instance variable
注意这和class variable 之间的区别，具体看例子

    class InstanceVariable
      @my_var =1 # class instance variable

      def p_var
        @my_var = 2  # instance variable
        puts @my_var    
      end

      def self.p_var
        puts @my_var   
      end
    end

    a = InstanceVariable.new
    a.p_var #=>2
    a.class.p_var #=>1

class variable 就是@@这种变量，两者的区别如下

* class variable 对子类和普通intance method 可见，但是定义在class 范围内的@是不可见的
* class variable 的scope容易把你搞昏， 例如

        @@x = 1  
        class B   
          @@x = 2  
        end  
        puts @@x # => 2  

所以nb的rubyist基本上会放弃对@@的使用