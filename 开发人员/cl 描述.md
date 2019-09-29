# 写一个好的CL描述

一个CL描述是一个公共的记录包含了改动点（what）和作出改动的原因（why），这将成为我们版本控制历史里一个永远存在的部分，并且可能会在未来几年里被除了你的review人以外的数以百计的人阅读。
将来的开发者将会根据你的描述来搜索你的CL，未来有人可能会因为相关的记忆的缺失但是却没有一个具体的有用的信息来搜索你所做的改动。如果所有的重要的信息都在代码里而不是在描述里，这将使得定位到你的CL变得更加困难。
A CL description is a public record of **what** change is being made and **why**
it was made. It will become a permanent part of our version control history, and
will possibly be read by hundreds of people other than your reviewers over the
years.

## 第一行

* 简要的总结一下你干了啥。
* 一个完整的句子，就像一条命令。
* 接下来空一行。

关于CL描述的第一行应该是一个简要的说明关于这个CL具体完成了写什么，接下来空一行。这就是未来搜索代码的人，在他浏览一个代码片段的版本控制历史的时候将会看到的东西，所以第一行应该是要提供足够的有利用价值的信息，这样他们不必去读你的CL或者整个描述就能得到一个关于你的CL实际干了什么的大概思想。

依照传统，CL的描述的第一行是一个完整的句子，就像一条命令（祈使句）。例如，“删除 FizzBuzz RPC 并且用新系统替代” 应该是 \"**Delete** the FizzBuzz RPC and **replace** it with the new system." 而不是 \"**Deleting** the FizzBuzz RPC and **replacing** it with the new system." 尽管这样，你不用把剩下的描述也写成一个祈使句。

描述的本身应该是有提供有用信息的。

剩下的描述应该是能提供有用的信息的。它可能会包含一个关于解决了什么问题的简洁描述和为什么这是最好的方法。如果有任何关于这个方法的缺点，这个描述里也应该写清楚。如果有关联，应该包含背景信息例如bug号，压测结果，并且链接到设计文档里去。
即使是一些小的CL值得注意细节，把这些CL都放到上下文里去。

## 不好的CL描述

“Fix bug” 是一个不合适的CL描述。什么bug？你是怎么修复的？其他相似的不好的描述包括：

* "Fix build."(修复编译错误)。
* "Add patch." (打补丁)。
* "Mode code from A to B"(把代码A迁移到B)。
* "Phase 1." (1 阶段)。
* "kill weird URLs." (去除奇怪的URL)。

其中一些是来自真实的CL描述。作者或许认为他提供了有价值的信息，但是他们没有交代清楚写这个CL描述的目的。

## 好的CL描述

下面是一写好的CL描述的例子

### Functionality change (功能变更)

> rpc: 删除rpc服务器消息自由列表上的大小限制
>
> FizzBuzz 服务器有非常大的消息并且能从提高重用性来从中获益。把自由列表扩大，并且添加一个协程来释放自由列表项。随着时间的推移，这些空闲的服务最终都会释放所有的自由列表项。

开始的几个字描述了CL实际做了什么。剩下就来谈谈这个问题是怎么被解决的，为什么这是一个好的方案和一些关于这个问题的其他信息。

### 重构

> 构造一个带TimeKeeper的Task去使用它的TimeStr 和 Now 方法
>
> 给Task添加一个Now 方法, 因此 borglet() 方法可以被移除（因为仅仅只有 OOMCandidate 调用了 borglet 的Now方法）。这将取代Borglet上委托给一个TimeKeeper 的方法。
>
> 允许Task 提供Now 是朝着解除对Borglet依赖的一个步骤。最终，依赖Task 的Now方法的调用方应该改成直接赢一个TimeKeeper，但是这是一种适应小步骤的重构的方法。
>
> 继续重构Borglet 层级结构的长期目标

第一行描述了描述了CL具体做了什么和从过去是怎么改变的。剩下的描述谈论了具体的实现，关于CL的上下文，并且这个方法不是理想的，并且可能是未来的方向。它也解释了为什么会作出这个改动。

### 小的CL需要一些上下文

> 为status.py建立一个 Python3的编译规则。
>
> 这样一来，允许已经使用此功能的消费者的Python3用户就可以去去依赖一个原始的状态构建规则的旁边的规则，而不是在他自己的文件树里的某个地方。这个鼓励新的消费者去尽可能的使用Python3而不是Python2，并且显著地简化了一些自动自动构建文件，当前正在使用的重构工具。

第一句描述了实际上做了什么。身心描述解释了为什么要作出这个改变，并且给reviewer一个上下文。

## 先review描述，然后再提交你的CL

CLs 可以在review期间发生重大的改变。先review一个CL描述，然后再提交CL是非常有价值的。这样能保证你的描述能一直反应出CL实际干了些什么。

Next: [Small CLs](small-cls.md)