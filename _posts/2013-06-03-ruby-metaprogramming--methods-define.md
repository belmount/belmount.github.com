---
layout: post
title: "Ruby Metaprogramming -- Methods define"
description: "using dynamic method make program DRY"
category: Ruby
tags: [ruby, metaprogramming]
---
{% include JB/setup %}

### 从java到Ruby的转变

2003年我毕业的时候，玩的是C++。当时java正是在国内开始兴盛的时候，一堆标准，一堆framework像山一样的堆过来。

到了鬼子国，鬼子的银行、大企业各个用java,迫于生计的需要，我和java打了几年交道。 
回想这几年曾经敲过的一堆get,set还有为了创建一个对象要用这个registry,那个factory就想骂人，更别提那些莫名其妙的xml文件。
不就是绕吗？不就是代码数多点好体现点工作量吗？至于吗？

而且java这东西和.net一样没什么门槛，在鬼子国大学搞AV创作的人也加入了码农的行列，极大的污染了整个码农的市场环境。
相对而言，我更喜欢C、C++, 光一个指针就可以把人玩死，更别说什么引用，泛型了。

爱折腾的人，基本上都是很懒的。我个人喜欢代码生代码。日本鬼子喜欢用excel生，但是VBA我不喜欢。
后来找到本Code Generation in Action，里面用Erb生代码，于是接触到了ruby。

再后来接触到Instiki，知道了Rails这个东西，就更看不惯struts那些玩意，一样的效果，用得着搞这么复杂吗？
看看ruby怎么搞的，你java惭愧不？
ruby是如何搞出 find_by_*这种东西？答案是metaprogramming。

### Why Need Dynamic

打个比方, Methods代表你的能力, 有些能力是在大学毕业时就写在简历上的（静态的）。
结果你参加工作了，老板突然问你会不会搞A？你回答说不会，证明你不够smart。
要是你不会但是知道谁可以帮着搞，你回答说会，然后call会的人帮你搞定，包装成你自己搞定的，证明你够Dynamic。
这就是 Dynamic的作用，可以用来做Design pattern 中的 Adapter。

### define_method

实现Dynamic有两种途径，一种是改简历。把老板要的能力写到简历里面，要什么我些什么。
老板你第一次问的时候，我回答含糊点，第二次问，我就拿出简历，“老板，你看我会”。
define_method就是ruby 给码农们批量改简历用的。
改完以后要干的事情就是dynamic dispatch,也就是动态的把这件事情交给其他人去完成。在ruby 里用send函数搞定。

### method_missing

还有一种情况是老板问你会干A不？实际上你简历上的A++就代表A也能干，但老板也不知道，只知道A起来比较爽。
，这个时候就需要临时包把A++包装成A交给老板，让他老人家爽去。method_missing 就是你用来忽悠老板的利器。

### respond_to?

修改简历辅助函数，让全世界都知道你这也会，那也会。

### 关于性能

dynamic 在哪种语言下都要比不dy的要慢，鱼和熊掌不能兼得的道理你懂的。但windows比dos慢你也懂，现在让你只装个dos你干不？所以不要纠结慢不慢，慢了才能体现你程序的复杂，慢的话你就可以喝杯咖啡再看结果，慢工才能出细活，觉悟吧！码农们！



