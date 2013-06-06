---
layout: post
title: "ruby metaprogramming -- callable"
description: " callable of ruby, inluding closures, procs, methods and lambda"
category: 
tags: [ruby, metaprogramming]
---
{% include JB/setup %}

###做什么和怎么做
之前修改class或者object的metadata主要涉及动态的变更做什么，没有动态的变更怎么做？
callable就是用来动态改变怎么做的。打个比方，领导交代你个计划：把这几件事情给我搞定。
你把计划看一遍对领导说，有个地方具体怎么搞不是很明确。“日，到时候告诉你。”这个到时候给你的东西就是callable。
在C/C++里面叫做函数指针，给你callback用的,不过C系的语言太静态，指针还有类型，你要是类型没给对，说不定给你个内存访问出错，噎死你。

Ruby的callable包括 closures, procs, methods 和 lambda 四大天王，各有各的用途。

### closures
就是把代码块作为函数隐形的参数跟在函数pp后面传过去，然后用 yield 调用。
例子如下：

    def my_method
      yied(2)
    end

    my_method {|x| print "#{x}B"}
    # or
    my_method do |x|
      print "#{x}B"
    end

结果都一样，是某个铅笔。
也就是说我把主材，辅材，调料都准备好，至于到时候是清蒸还是干煸等通知。

###关于scope
####Scope Gate

看着这意思，就知道穿过这个门就是另外一个世界。看门的有三个主, class, module, def，在这几个主和end之间，他们内部是由自己为object命名的而不受外界影响。打个比方，
在在舞台上唱歌的李娜和场上打网球的李娜不是一个人，class代表舞台，def 代表网球场， 同样的名称，因为context不一致，所指的对象也不一致。

class, module 和 def 这三个主对scope唯一的不同是， def 中定义的变量只有在函数执行的时候定义才会产生，其他两个主在写的时候就把位置都占了。

####Scope Flatten
扁平化就是要跨越gate,要唱歌的李娜和打网球的李娜是一个人，这是不是有点困难？
说困难也不苦难，让李娜通知前面用$标记为全局不久行了？的确，你够聪明，但我们不是说的全局变量跨越gate,因为人家天生就可以跨越gate，这么聪明的你只想到这么简单的搞法是不是脑袋临时短路了？

正确答案是在new 后面加block, 然后用 define_method 进行穿越， 看个例子吧！

    lina='穿越'
    Najie = Class.new do 
      puts "#{lina} in 娜姐" # 穿越 in 娜姐
      define_method :call_me_najie do
        puts "叫我#{lina}姐" # 叫我穿越姐 
      end
    end

这个 lina 就实现了穿越，在Class外面的世界和内部的世界实现了统一。

还有种搞法叫做 Shared Scope, 就是你不想把变量暴露在广大人民群众之前任人使用，只在几个函数之间共享。(多么像会员制的高级会所)
这个事情也可以搞，但是要有后台撑着，这个后台就是 Kernal 老大。ruby 是这样通过K老大实现会所的。

    def hi_club   # 高级Club
      mms = '34f'

      Kernel.send :define_method, :tall_hansome_rich do  # 高帅富可以 do mm
        mms
      end

      Kernel.send :define_method, :gov_officer do        # 高官可以 do mm
        mms
      end
    end

    hi_club

其他不是t_h_r和 g_o的主，根本看不到34F的mm，K老大保护的多严格。

#### context probe
问题来了，私人俱乐部有时也要突然接待陌生贵宾，贵宾是不用办会员卡照样可以看 mm的，ruby也提供了解决方案。这就是nb的instance_eval

    private_club.instance_eval do 
      visitant.happy_with @mms  #临时happy 一下
    end
多人性化的功能啊！

### Callable Objects

#### Proc 
Closure有个问题，就是不能往下继续传递，在pp给他用的object之后就停止了。如果需要将closure里面包含的处理告诉给另外的人，你就要用到Proc的。
我们继续用领导交办工作来说，大领导叫二领导这件事应该这么这么去办，二领导收到旨意后交代给下面的马仔，这个旨意就是Proc。
我们也可以通过使用&操作符将一个closure转换为Proc。

    def lolo_china(building, owner)
      yeild(owner, building)
    end

    def vice_china(building, owner, &how_to)
      lolo_china(building, owner, &how_to)
    end

    # 供boss 使用
    vice_china(building, owner){|b,o| kill o; burn b}

    # 最后lolo们把这令人发指的事情给干了

#### lambda
lambda 的实质是一个proc，但它和proc还是有少量的区别

* return的scope， lambda的 return 是针对于本身的，proc的 return 是针对当前scope的。
* arity (元数)。 proc对参数的数量限制不那么严格，也就是你多传少传我都不管，我还是按照我的数量去算，少了我当是nil，多了就丢到垃圾箱去。lambda就严肃些。

#### method
method是属于instance的，对instance变量可以进行操作。但是他也可以和相关的instance进行unbind,而与另外一个instance进行绑定（跳槽？）。
