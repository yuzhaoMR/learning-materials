### k8s简介 ###

> 为什么：
- IT行业由新技术来驱动
- 可以“轻装上阵”的开发复杂系统
- 可以全面拥抱微服务架构
- 可以随时的将系统搬迁到共有与上
- k8s内部的服务弹性扩容机制可以轻松应付突发流量。
- k8s的横向扩容能力提供竞争力

> 是什么：基于容器技术的分布式架构领先方案，一个开发的平台，不局限于语言

> 基本知识：
- Service，是分布式集群架构核心
  - 唯一的名称
  - 拥有一个虚拟ip和端口号
  - Service可以连接指定Service
- Pod，通过Label与Service相连
  - Pause容器，每个Pod含有一个，Pod里的所有其他容器共享Pasue的网络栈和Volume挂载卷
- Node,Pod运行在Node节点上，节点可以是物理机，也可以是云的虚拟机
  - kubelet
  - kube-proxy
- Master，实现集群的资源管理
  - apiserver
  - controller-manager
  - scheduler

