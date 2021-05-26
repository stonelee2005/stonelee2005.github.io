---
layout:     post
title:      K8S 
subtitle:   K8S Command
date:       2021-05-20
author:     Stone
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    -  Kubenates

---

# Understanding Kubernetes

Kubernetes是一个运行容器(docker,containred..)的平台.

跟传统的运维系统比较理解,自己通俗的理解K8s就是一个运行大型的就像openstack 一样的平台。

Kubernetes只关心OS系统层级上的操作

## 1. Kubernetes组件

namespace层面

* context

单机层面

* User
* 资源
* 网络
* Storage
* 安全

集群层面

* 集群
  * POD
  * NODE
* POD调度
* 网络

监控

### 1.2.Kubernetes配置逻辑

Kubernetes配置逻辑是各个生成资源,互相绑定

## 2. Kubernetes的命令

### 2.1 kubeadm

kubeadm是创建,升级，初始化，节点加入等等关于全集群的命令

### 2.2 kubectl

kubectl是迁移,维护集群维护子命令

* POD
  * 运行
  * 网络
  * cp
  * log
  * port-forward
* 资源POD
  * crud 
  * configuration



### 2.3 sdd

##

