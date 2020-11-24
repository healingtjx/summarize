## 前置知识

linux,docker



## K8S概述和特性

### 概述

k8s是 Google 开源的一个容器编排引擎，它支持全自动部署、大规模可伸缩、应用容器部署。

容器化集群管理系统

使用k8s进行容器化应用部署

使用k8s利于应用扩展

k8s目标实施让部署容器化应用更加简洁和高效



### 特性

1.自动装箱

2.自动修复

3.水平扩展

4.服务发现（对外提供统一入口，有点类似于网关）

5.滚动更新

6.版本回退

7.密钥和配置管理

8.存储编排（自动实现存储系统挂载及应用，特别对有状态应用实现数据持久化非常重要，存储目录可以来自本地、网络存储、公共云）

9.批处理（提供一次性任务，定时任务，满足披露数据处理和分析场景）



## K8S集群架构组件

<img src="/Users/tjx/Library/Application Support/typora-user-images/image-20201112214207664.png" alt="image-20201112214207664" style="zoom:40%;" />



### Master（管理节点）

##### apiserver 

集群统一入口，以restful方式，交给etcd存储

##### scheduler 

节点调度，选择node节点应用部署

##### controller-manager

处理集群中常规后台任务，一个资源pod对应一个控制器

##### etcd

存储系统，用于保存集群相关数据

### node（工作节点）

##### kubelet

master派到node节点代表，管理本机容器

##### kube-proxy

提供网络代理，负载均衡等操作



### 核心概念

##### Pod

最小的部署单元

一组容器集合

共享网络

生命周期短暂的(重启就没了)

##### Controller

确保预期的pod副本状态

有状态部署

无状态部署

确保所有的node运行同一个pod

一次性任务和定时任务

##### Service

定义一组pod的访问规则





## 搭建k8s

### 安装要求（虚拟机）

一台2核2gb内存的master，和2台4g内存的node节点

禁止swap分区

禁止selinux

配置iptables

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

遇到的问题Init:ImagePullBackOff 

docker pull quay.io/coreos/flannel:v0.13.1-rc1

### 部署的方式

##### kubeadm

k8s 部署工具，提供kubeadm init 和kubeadm join，用于快速部署k8s集群

##### 二进制包

从github下载发行的二进制包，手动部署每一个组件，组成k8s

