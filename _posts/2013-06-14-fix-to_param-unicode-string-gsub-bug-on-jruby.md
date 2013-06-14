---
layout: post
title: "Fix to_param unicode string gsub bug on jruby"
description: "how to fix invalid byte seqence error for to_param call"
category: Ruby
tags: [Ruby, Rails, Jruby]
---
{% include JB/setup %}
###万恶的Windows系统下跑rails应用

伟大的国家单位只给我提供windows系统的服务器，我只能在有限的条件下发挥无限的本能去跑心爱的rails。<br/>
最初使用Nginx + Thin + Ruby 1.9，速度是一个字----慢。<br/>
穷则思变，为了性能，我把 Rails 从 ruby 环境迁移到 JRuby，结果速度还真的比MRI要快很多。
但有得必有失，原先为了url读的方便，我把 model的 to_param 重新定义了，采用了名称作为缺省的params[:id],在

    jruby -S trinidad

之后，可恶的事情发生了。

    application error
    org.jruby.exceptions.RaiseException: (ArgumentError) invalid byte sequence in GBK
            at org.jruby.RubyString.gsub(org/jruby/RubyString.java:3051)
            at org.jruby.RubyString.gsub(org/jruby/RubyString.java:3019)
            at RUBY.escape_glob_chars(E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/actionpack-3.2.2/lib/action_dispatch/middleware/static.rb:41)
            at RUBY.match?(E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/actionpack-3.2.2/lib/action_dispatch/middleware/static.rb:14)
            at RUBY.call(E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/actionpack-3.2.2/lib/action_dispatch/middleware/static.rb:55)
            at RUBY.call(E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/railties-3.2.2/lib/rails/engine.rb:479)
            at RUBY.call(E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/railties-3.2.2/lib/rails/application.rb:220)
            at RUBY.call(file:E:/downloads/jruby-bin-1.7.0/jruby-1.7.0/lib/ruby/gems/shared/gems/jruby-rack-1.1.10/lib/jruby-rack-1.1.10.jar!/rack/handler/servlet.rb:22)

### 伪解决方案

看这个原因，应该是Jruby 没有按照程序的要求使用 utf-8 编码，还是按照 console 环境的编码 GBK进行字符串的处理。
苦逼的中国人，编个程序还要比老外要复杂些。
看了看 stackoverflow, 提出的解决方案是

    #environment.rb
    Encoding.default_external = Encoding::UTF_8
    Encoding.default_internal = Encoding::UTF_8

照做，继续失败。

### 最终解决方案
ruby 最能干的事情就是开包，其次就是把A搞成B, B搞成A， 实践 Metaprogramming 的时候到了。

    # config/initializers/jruby_patch.rb
    require 'action_dispatch/middleware/static'

    module ActionDispatch
      class FileHandler
        alias_method :match_orig?, :match?

        def match_helper?(path)
          ec = Encoding::Converter.new 'gbk', 'utf-8'
          path = ec.convert path
          match_orig? path.to_s
        end

        alias_method :match?, :match_helper? 
      end
    end

世界又恢复正常了:)