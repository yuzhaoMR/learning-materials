# k8s 简介

> 是什么：

基于容器技术的分布式架构领先方案，一个开发的平台，不局限于语言

> 为什么：

- IT 行业由新技术来驱动
- 可以“轻装上阵”的开发复杂系统
- 可以全面拥抱微服务架构
- 可以随时的将系统搬迁到共有云上
- k8s 内部的服务弹性扩容机制可以轻松应付突发流量。
- k8s 的横向扩容能力提供竞争力

> 基本概念：

- Service，是分布式集群架构核心
  - 唯一的名称
  - 拥有一个虚拟 ip 和端口号
  - Service 可以连接指定 Service
- Pod，通过 Label 与 Service 相连
  - Pause 容器，每个 Pod 含有一个，Pod 里的所有其他容器共享 Pasue 的网络栈和 Volume 挂载卷
- Node,Pod 运行在 Node 节点上，节点可以是物理机，也可以是云的虚拟机
  - kubelet
  - kube-proxy
  - Docker Engine
- Master，实现集群的资源管理
  - apiserver
  - controller-manager
  - scheduler
