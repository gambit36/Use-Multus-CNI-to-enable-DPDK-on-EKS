# Use-Multus-CNI-to-enable-DPDK-on-EKS

Amazon Elastic Kubernetes Service (Amazon EKS) 在2021年8月宣布支持 Multus 容器网络接口 (CNI) 插件，使客户能够附加多个网络接口并将高级网络配置应用于基于 Kubernetes 的应用程序。 凭借 Multus 的可用性，通信服务提供商和其他具有独特网络要求的客户可以配置他们的 EKS 集群来运行连接到多个接口的多宿主 Kubernetes pod。

在这篇文章中，我们将介绍 Multus CNI，涵盖适用的用例，并演练如何部署和设置 Multus CNI。 

## 什么是Multus CNI？

CNI 是容器网络接口，它提供应用程序编程接口来配置容器中的网络接口。 [Multus CNI](https://github.com/k8snetworkplumbingwg/multus-cni) 是 Kubernetes 的容器网络接口插件，可以将多个网络接口附加到 pod。 在 Kubernetes 中，除了本地环回之外，每个 pod 默认只有一个网络接口。 使用 Multus，您可以创建具有多个接口的多宿主 Pod。 Multus 充当“元”插件，可以调用其他 CNI 插件来配置其他接口。 

Pod 的多个网络接口在各种用例中都很有用； 例子包括： 
* 流量拆分：运行需要分离控制/管理和数据/用户平面网络流量以满足低延迟服务质量 (QoS) 要求的网络功能 (NF)。
* 性能：附加接口通常利用专门的硬件规范，例如单根 I/O 虚拟化 [(SR-IOV)](https://github.com/k8snetworkplumbingwg/sriov-network-device-plugin) 和数据平面开发套件 [(DPDK)](https://www.dpdk.org/)，它们绕过操作系统内核以提高带宽和网络性能。
* 安全性：支持对流量隔离要求严格的多租户网络。 将多个子网连接到 Pod 以满足合规性要求。 
