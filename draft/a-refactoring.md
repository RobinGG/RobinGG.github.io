---
layout: page
title: 记一次重构
tags: [Architecture]
---

最近对自己手上负责的代码进行了一点点重构。是一个比较典型的 action，service，DAO 的三层架构。

### 有什么问题？

在业务逻辑方面，三层已经实现了比较好的解耦，上次依赖下层接口编程。下层的代码实现已经可以随时替换掉了。但是事实上下层并不能随时替换，为什么呢？

因为上层调用了下层的 model，如果替换掉下层实现。那么整个上层对于 model 的操作就不能完成。这里就产生了耦合。

### 怎么解决呢？

我们现在知道了一个 model 不能穿透整个三层架构。那怎么去解决这个问题呢？其实答案也很简单，每一层都拥有自己的 model 就可以了。然后添加一个类去处理下层类向上层的转化。这样不能完全解耦，但能将更换下层实现带来的影响尽量减小。

### 那就干吧

问题咱知道，该怎么解决也知道了。那就改吧，还说啥啊？

#### 重构遵守的准则

* 重构的时候，步子要小

    步子要小，也就是每次只改一小个部分的代码。步子小就方便我们如果重构代码出错能够快速定位问题以及快速回滚。

* 重构的时候，影响要小
    
    然后就是重构的时候要从影响小的部分开始。在这一次重构的话，我选择从 action 层开始重构而不是 DAO 层。我们前面说过 model 是击穿了三层，其实就是 DAO 层的 model 被 service，action 层拿去使用了。如果我们从 DAO 层开始改，那很有可能会影响 service 和 action 的业务逻辑处理，导致不能提供服务。虽然步子小，但是影响大。所以我选择从 action 开始重构

#### 开始动手

##### Action

1. 新建 Action 自己的 model 类，这一步是不会对现有的功能产生任何的影响的
2. 建立工具类，用于将 Service model 转化为 Action model
    
    2.1. 建立工具类和相应的转化的方法，将 Service model 转化为 Action model
    2.2. 替换 Action 层的 model，使用工具类转换后的 model。
    2.3. 运行测试代码，确保更改没有影响功能

没有了。很简单，两步就把 Action 的 model 分离出去了。

##### Service

Service 的过程和 Action 很相似，不过有一点不一样。Action 在重构完以后使用的仍然是 DAO 传递上来的 model。我们在对 Service 重构完以后也要对 Action 做一些相应的更改，保证 Action 正确的调用了 Service 的新 model。

##### DAO 

相比之下，DAO 好像什么也不用改。因为 DAO 一直都是用的自己的 model。但我还是做了一些修改。改了什么呢？我把 model 分离为了通用的 model 以及特定数据库的 model。

Business 基于通用的 model 做开发，而 DAO 自己则根据数据库的不同可以使用不同的 model。

##### 完成

至此，三层 model 的改造就完成了。每一层都有自己的 model，相互分离开来。任何一层替换都只要做小部分的更改就可以了。