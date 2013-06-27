---
layout: post
title: "What's new in Rails 4.0"
description: "the new feature in Rails 4.0"
category: rails
tags: [ruby, rails, 4.0]
---
{% include JB/setup %}

## Strong parameter

和.net mvc中的strong type view model 的绑定相类似，是对从http params 到 model 赋值的一种安全保护机制。
具体做法是在 Controller 中 增加一个 private函数
```[ModelName]_params```。示例如下：

    private
      def person_params
        params.required(:person).permit(:name, :age)
      end

## singularize
    # rails 3
    "police".singularize
    => "polouse"
    # rails 4
    => "police"

## 被踢掉的 queue
风传增加的后台处理队列，据tweet上的DHH说已经被干掉了。个人觉得做个gem还可以，没必要放在专注于前台的rails 里。
ActiveSupport::Queue

    Class LongRunOperation
      def run
        # ...
      end
    end

Rails.queue.push(LongRunOperation.new)

## [Turbolinks](https://github.com/rails/turbolinks/)

这个东西据说是在app内部link点击时，通过javascript 只load body和head 里的title，而不变更浏览器当前页面的实例，所以会提升页面访问。它只会在GET method 下起作用。

## [Cache Digests](https://github.com/rails/cache_digests)

在view中控制fragment cache是个比较复杂的操作，cache digests为每个进入缓存的fragment view自动添加摘要--digest,大大简化页面的缓存操作。

## [ActionController::Live](http://edgeapi.rubyonrails.org/classes/ActionController/Live.html)

rails web socket的实现，用于实时和网页程序交换信息。

## PATCH verb (REST)

PUT的语义是将一个完整的对象发送至服务器，而针对对象局部的修改则使用PATCH方法，具体做法是在form中设定method 为:PATCH。

## Relation

### all
Rails 4之前 Post.all 返回的是数组，现在返回的是 ActiveRecord::Relation，和 scope的返回类型一致。

### none
类似于在chained query 后面再加个```.where("1=0")```,在你想返回个空集的时候使用。

### first和last
和mongoid的first和last类似，默认是按照id进行排序

### dynamic finders
被彻底干掉了，取而代之的是用where.

