---
title: 守护进程容错
layout: documentation
documentation: true
---

Storm 有几个不同的守护进程.
调度 workers 的 Nimbus, 启动和杀死 workers 的 supervisors, 可以访问日志的 log viewer（日志查看器）以及显示集群状态 UI.

## 当一个 worker 挂掉时会发生什么?

当一个 worker 挂掉时, supervisor 将会重启它.
如果在启动它时继续发生故障并且没有发送 hearbeat（心跳）给 Nimbus, 那么 Nimbus 将会重新调度 worker.

## 当一个 node（节点）挂掉时会发生什么?

分配给该机器的 task（任务）将超时, Nimbus 将这些 task（任务）重新分配给其他机器.

## 当 Nimbus 或 Supervisor 挂掉时会发生什么?

Nimbus 和 Supervisor 守护进程是为 fail-fast（快速失败）（遇到任何意外情况时进程自毁）和无状态（所有状态都保存在 Zookeeper 或磁盘上）而设计的.
像 [部署 Storm 集群](Setting-up-a-Storm-cluster.html) 中描述的一样, Nimbus 和 Supervisor 守护进程必须使用 daemontools 或 monit 工具进行监督.
所以如果 Nimbus 或 Supervisor 守护进程挂掉后, 它们会像什么都没发生一样重启.

最值得注意的是, 没有 worker 进程受到 Nimbus 或 Supervisors 挂掉的影响.
这与 Hadoop 相反, 如果 JobTracker 挂掉, 所有正在运行的 job 作业都将丢失.

## Nimbus 是单点故障的吗?

如果你失去了 Nimbus 节点, workers 仍然会继续工作.
此外，supervisors 如果挂掉, 将继续重新启动 workers.
但是，如果没有 Nimbus，worker 在必要时不会重新分配给其他机器（如失去 worker 机器）.

Storm Nimbus 自 1.0.0 以来是 highly available（高可用的）.
更多信息请参阅 [Nimbus HA 高可用设计](nimbus-ha-design.html) 文档.

## Storm 是如何保证数据处理的?

Storm 提供了保证数据处理的机制, 即使节点挂掉或 messages（消息）丢失.
更多细节请参阅 [保证消息处理](Guaranteeing-message-processing.html).
