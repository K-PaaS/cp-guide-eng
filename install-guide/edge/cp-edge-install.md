### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Edge Deployment Guide

<br><br>

## Table of Contents

1. [Document Overview](#1)<br>
  1.1. [Purpose](#1.1)<br>
  1.2. [Scope](#1.2)<br>
  1.3. [System Configuration Diagram](#1.3)<br>
  1.4. [Reference](#1.4)

2. [K-PaaS Container Platform Edge Deployment](#2)<br>
  2.1. [Prerequisite](#2.1)<br>
  2.1.1. [Cluster](#2.1.1)<br>
  2.1.2. [Operating System](#2.1.2)<br>
  2.1.3. [Key Software](#2.1.3)<br>
  2.1.4. [Firewall](#2.1.4)<br>
  2.1.5. [CloudCore Service Configuration](#2.1.5)<br>
  2.2. [Installing The K-PaaS Container Platform Cluster](#2.2)<br>
  2.3. [Preparation for K-PaaS Container Platform Edge Deployment](#2.3)<br>
  2.4. [K-PaaS Container Platform Edge Deployment](#2.4)<br>
  2.5. [Verifying The K-PaaS Container Platform Edge Deployment](#2.5)

3. [Deleting a K-PaaS Container Platform Edge Deployment (Reference)](#3)

4. [Caution For Creating Resources](#4)

<br><br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Cluster Edge Deployment Guide) describes how to configure the Edge nodes of the K-PaaS Container Platform Cluster, an open PaaS platform that supports planners, developers, and operators.

<br><br>

### <div id='1.2'> 1.2. Scope
The installation scope is written based on a `single cloud` environment, which provides the foundation for configuring Edge nodes in the K-PaaS Container Platform Edge environment.

<br><br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system is configured as a `single cluster` Kubernetes environment (Control Plane, Worker, Edge).

Using the K-PaaS Container Platform Deployment, a `single cluster` Kubernetes environment is established, and Edge deployment is performed to provide Edge node functionality to the cluster.

Refer to the following configuration for the instance environment required to configure K-PaaS Container Platform Edge nodes.

|Environment|Instance Type|Number of Instances|Notes|
|---|---|---|---|
|Cloud Environment|-|-|Use the nodes of an already deployed cluster|
|Edge Environment|Edge|1 or more|Create instances with `ARM64` architecture OS|

<br><br>

The system configuration diagram for each deployment type is as follows.

***[ Single Cloud Cluster with Edge Node Configuration ]***

![image 001]

<br><br>

### <div id='1.4'> 1.4. Reference
> https://kubeedge.io/en/docs/<br>
> https://github.com/kubeedge/kubeedge

<br><br>

## <div id='2'> 2. K-PaaS Container Platform Edge Deployment

### <div id='2.1'> 2.1. Prerequisite
The prerequisites for deploying the K-PaaS Container Platform Edge are described below.

<br><br>

### <div id='2.1.1'> 2.1.1. Cluster (***Required Installation***)
Perform the installation of the K-PaaS Container Platform `single cloud`.

<br><br>

### <div id='2.1.2'> 2.1.2. Operating System (***Required Check***)
The required OS environment information for deploying the K-PaaS Container Platform Edge is as follows.

|Environment|Node|Supported OS|Version|Architecture|
|---|---|---|---|---|
|Cloud Environment|Control Plane<br>Worker|Ubuntu|22.04|amd64|
|Edge Environment|Edge|Ubuntu|20.04|arm64|
|Edge Environment|Edge|Ubuntu|22.04|arm64|

<br><br>

### <div id='2.1.3'> 2.1.3. Key Software (Reference)
The key software required for deploying the K-PaaS Container Platform Edge is as follows.

|Key Software|Version|
|---|---|
|Kubernetes Native|v1.30.4|
|Kubernetes Native (Edge Node)|v1.24.17|
|CRI-O|1.30.3|
|CRI-O (Edge Node)|v1.24.0|
|KubeEdge|v1.14.4|
|EdgeMesh|v1.12.0|

<br><br>

### <div id='2.1.4'> 2.1.4. Firewall (***Required Settings***)
The firewall information required for deploying the K-PaaS Container Platform Edge is as follows.

> Some firewalls have been added for Edge deployment.

Control Plane Node

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|2379-2380|etcd server client API|
|TCP|6443|Kubernetes API Server|
|TCP|9443|cloudcore router port|
|TCP|10001|cloudHub quic port|
|TCP|10002|cloudHub https port|
|TCP|10003|cloudStream streamPort|
|TCP|10004|cloudStream tunnelPort|
|TCP|10250|Kubelet API|
|TCP|10251|kube-scheduler|
|TCP|10252|kube-controller-manager|
|TCP|10255|Read-Only Kubelet API|
|TCP|20004|edgeMesh server containerPort|
|TCP|20006|edgeMesh tunnel listenPort|
|TCP|40001|edgeMesh edgeProxy listenPort|
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|

<br>

Worker Node

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|10250|Kubelet API|
|TCP|10255|Read-Only Kubelet API|
|TCP|20006|edgeMesh tunnel listenPort|
|TCP|40001|edgeMesh edgeProxy listenPort|
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|

<br>

Edge Node

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|1883-1884|eventBus mqttPort|
|TCP|2049|NFS|
|TCP|10001|edgeHub quic port|
|TCP|10250|Kubelet API|
|TCP|10255|Read-Only Kubelet API|
|TCP|10350|Use kubectl logs|
|TCP|10550|edgecore list-watch port|
|TCP|20006|edgeMesh tunnel listenPort|
|TCP|40001|edgeMesh edgeProxy listenPort|
|TCP|30000-32767|NodePort Services|

<br><br>

### <div id='2.1.6'> 2.1.6. CloudCore Service Configuration (***Required Settings***)
The CloudCore service configuration information required for K-PaaS Container Platform service setup is as follows.

<br>

|Service|Description|Notes|
|---|---|---|
|CloudCore|Service for connecting to EdgeNode in the K-PaaS Container Platform service|***`Create 1 interface or 1 load balancer`*** required<br>Public IP allocation required|

<br>

In the K-PaaS Container Platform cluster, MetalLB assigns an External IP for load balancer-type services.<br>
To support external communication and services via the assigned External IP, ***`choose one of the two methods below`*** and configure accordingly.

<br>

|Method|Description|Notes|
|---|---|---|
|Add Interface|Add a new interface with a Public IP assigned to 1 Control Plane node|Only incurs the cost of using the Public IP<br>CloudCore service external access is unavailable in case of a node failure in HA configuration|
|Create Load Balancer|Create a load balancer service with a Public IP assigned|Additional cost for the load balancer service<br>CloudCore service remains operational even during partial Control Plane node failures in HA configuration<br>Recommended for production environments|

<br>

Refer to the following sections of the K-PaaS Container Platform Cluster Installation Guide for CloudCore service configuration methods:
- [2.1.6.1. Control Plane Node Addition Interface](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/standalone/cp-cluster-install-single.md#2.1.6.1)
- [2.1.6.2. Cloud Load Balancer Service](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/standalone/cp-cluster-install-single.md#2.1.6.2)

<br>

Additionally, when configuring using the load balancer service method, create the load balancer based on the following port mapping information:

|Protocol|Port|NodePort|Notes|
|---|---|---|---|
|TCP|10000|30000||
|TCP|10001|30001||
|TCP|10002|30002||
|TCP|10003|30003||
|TCP|10004|30004||

<br><br>

### <div id='2.2'> 2.2. Installing The K-PaaS Container Platform Cluster
To deploy K-PaaS Container Platform Edge, the K-PaaS Container Platform cluster must be deployed in the cloud environment, followed by Edge node deployment in the Edge environment.

<br>

Deploy a Kubernetes cluster through the K-PaaS Container Platform cluster deployment in the Cloud environment.

> [K-PaaS Container Platform Cluster Installation Guide](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/standalone/cp-cluster-install-single.md)

<br><br>

### <div id='2.3'> 2.3. Preparation for K-PaaS Container Platform Edge Deployment
Predefine the environment variables required for deploying K-PaaS Container Platform Edge, then proceed with installation using a shell script.

If the Edge node environment is `Raspberry Pi`, add the following information.<br>
If not in a `Raspberry Pi` environment, skip the steps below and proceed with defining the environment variables for the K-PaaS Container Platform Edge deployment.
```
# vi /boot/firmware/cmdline.txt

... cgroup_enable=memory cgroup_memory=1 (Add to the end)
```

<br>

Reboot the `Raspberry Pi`.
```
# reboot
```

<br>

Navigate to the installation path for K-PaaS Container Platform Edge deployment. From this point onward, perform the steps only on the **Control Plane node**.
```
$ cd ~/cp-deployment/standalone
```

<br>

Define the environment variables required for K-PaaS Container Platform Edge deployment.
```
$ vi cp-edge-vars.sh
```

<br>

|Environment Variable|Description|Notes|
|---|---|---|
|CLOUDCORE_PRIVATE_VIP|Private IP for the interface used by the CloudCore Service through MetalLB|Duplicate entry with **`CLOUDCORE_VIP`** when using load balancer service|
|CLOUDCORE_VIP|Public IP assigned to the interface or load balancer service used by the CloudCore Service||
|CLOUDCORE1_NODE_HOSTNAME|Hostname of the node where CloudCore will be installed|Enter information for 1 Control Plane or Worker node|
|CLOUDCORE2_NODE_HOSTNAME|Hostname of the node where CloudCore will be installed|Enter information for 1 Control Plane or Worker node|
|EDGE_NODE_CNT|Number of Edge nodes||
|EDGE1_NODE_HOSTNAME|Hostname of the first Edge node||
|EDGE1_NODE_PUBLIC_IP|Public IP of the first Edge node||
|EDGE{n}_NODE_HOSTNAME|Hostname of the nth Edge node|Set when **`EDGE_NODE_CNT`** is 2 or more<br>Set for the value of **`EDGE_NODE_CNT`**|
|EDGE{n}_NODE_PUBLIC_IP|Public IP of the nth Edge node|Set when **`EDGE_NODE_CNT`** is 2 or more<br>Set for the value of **`EDGE_NODE_CNT`**|

<br>

```
#!/bin/bash

export CLOUDCORE_PRIVATE_VIP=
export CLOUDCORE_VIP=

export CLOUDCORE1_NODE_HOSTNAME=
export CLOUDCORE2_NODE_HOSTNAME=

export EDGE_NODE_CNT=

export EDGE1_NODE_HOSTNAME=
export EDGE1_NODE_PUBLIC_IP=
...
export EDGE{n}_NODE_HOSTNAME=
export EDGE{n}_NODE_PUBLIC_IP=
```

<br><br>

### <div id='2.4'> 2.4. K-PaaS Container Platform Edge Deployment
Sequentially perform package installation, Edge deployment environment variable configuration, and K-PaaS Container Platform Edge deployment using an Ansible playbook via a shell script.

```
$ source deploy-cp-edge.sh
```

<br><br>

### <div id='2.5'> 2.5. Verifying The K-PaaS Container Platform Edge Deployment
Verify the K-PaaS Container Platform Edge deployment by checking the nodes and the Pods in the `kube-system` and `kubeedge` namespaces.<br>
The node and Pod information displayed may vary depending on the configuration. Below is an example of the result for a single Control Plane setup.

```
$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE     VERSION
cp-edge              Ready    agent,edge             5m40s   v1.24.17-kubeedge-v1.14.4
cp-master            Ready    control-plane,master   39m     v1.30.4
cp-worker-1          Ready    <none>                 38m     v1.30.4
cp-worker-2          Ready    <none>                 38m     v1.30.4
cp-worker-3          Ready    <none>                 38m     v1.30.4

$ kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-b5f8f6849-hhbgh    1/1     Running   0          9m22s
calico-node-4hbw5                          1/1     Running   0          4m34s
calico-node-8q5tv                          1/1     Running   0          5m9s
calico-node-qlq5k                          1/1     Running   0          5m26s
calico-node-xc55c                          1/1     Running   0          4m53s
coredns-657959df74-grflz                   1/1     Running   0          37m
coredns-657959df74-wbdl6                   1/1     Running   0          37m
dns-autoscaler-b5c786945-cbcv9             1/1     Running   0          37m
kube-apiserver-cp-master                   1/1     Running   0          36m
kube-controller-manager-cp-master          1/1     Running   1          39m
kube-proxy-2ckfd                           1/1     Running   0          38m
kube-proxy-hb8p2                           1/1     Running   0          38m
kube-proxy-nnh6d                           1/1     Running   0          38m
kube-proxy-p9srm                           1/1     Running   0          6m4s
kube-scheduler-cp-master                   1/1     Running   1          39m
metrics-server-5cd75b7749-57sc2            2/2     Running   0          37m
nodelocaldns-24vq4                         1/1     Running   0          6m4s
nodelocaldns-jjrjj                         1/1     Running   0          37m
nodelocaldns-kgzxb                         1/1     Running   0          37m
nodelocaldns-l9s47                         1/1     Running   0          37m

$ kubectl get pods -n kubeedge
NAME                         READY   STATUS    RESTARTS   AGE
cloudcore-758b4f4b97-4s57p   1/1     Running   0          3m49s
cloudcore-758b4f4b97-ndxbl   1/1     Running   0          3m49s
edgemesh-agent-9cmt2         1/1     Running   0          25s
edgemesh-agent-b8btq         1/1     Running   0          87s
edgemesh-agent-ntf24         1/1     Running   0          87s
edgemesh-agent-vhggk         1/1     Running   0          87s
edgemesh-agent-vzpdj         1/1     Running   0          87s
```

<br><br>

## <div id='3'> 3. Deleting a K-PaaS Container Platform Edge Deployment (Reference)
Delete the K-PaaS Container Platform Edge deployment using a shell script.
```
$ source reset-cp-edge.sh
```

<br><br>

## <div id='4'> 4. Caution For Creating Resources
When creating resources manually, avoid using the following prefixes for the respective resource types:

|Resource Name|Prefixes to Exclude|
|---|---|
|All Resources|kube*|
|Namespace|all|
||kubernetes-dashboard|
||cp-portal-temp-namespace|
|Role|cp-init-role|
||cp-admin-role|
|ResourceQuota|cp-low-resourcequota|
||cp-medium-resourcequota|
||cp-high-resourcequota|
|LimitRanges|cp-low-limitrange|
||cp-medium-limitrange|
||cp-high-limitrange|
|Pod|nodes|
||resources|

<br><br>

[image 001]:images/edge-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Edge Deployment Guide