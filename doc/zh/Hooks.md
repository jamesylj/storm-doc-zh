---
title: Hooks
layout: documentation
documentation: true
---
Storm 提供了一种hook机制，你可以用来实现在Storm各种事件上运行自定义的代码。通过继承 [BaseTaskHook](javadocs/org/apache/storm/hooks/BaseTaskHook.html) 类创建一个hook，并且可以覆盖你想要跟踪的事件的一些方法。

一共有两种方式注册你的hook:

1. 在 spout 的 open 方法或者 bolt 的 prepare 方法中使用  [TopologyContext](javadocs/org/apache/storm/task/TopologyContext.html#addTaskHook) 方法.
2. 使用 Storm 配置表中的  ["topology.auto.task.hooks"](javadocs/org/apache/storm/Config.html#TOPOLOGY_AUTO_TASK_HOOKS) 配置项. 这些 hooks 会自动注册到每个 spout 和 bolt 中，这样就可以很方便地处理一些事情，例如集成自定义的监控系统.
