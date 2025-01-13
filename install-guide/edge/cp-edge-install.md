### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/README.md) > Edge installation guide

<br>
## Table of Contents

1. [Document Overview](#1)<br>
   1.1. [Purpose](#1.1)<br>
   1.2. [Range](#1.2)<br>
   1.3. [System configuration diagram](#1.3)<br>
   1.4. [Reference](#1.4)<br>

2. [Installation of KubeEdge](#2)<br>
   2.1. [Prerequisite](#2.1)<br>
   2.2. [Kubernetes Native Cluster Deployment](#2.2)<br>
   2.3. [Preparing to install KubeEdge](#2.3)<br>
   2.4. [Installation of KubeEdge](#2.4)<br>
   2.5. [Check KubeEdge installation](#2.5)<br>

3. [KubeEdge Reset (Reference)](#3)<br>

4. [Precautions when creating a resource](#4)<br>

<br>

## <div id='1'> 1. Document overview

### <div id='1.1'> 1.1. purpose
This document (KubeEdge Installation Guide) describes how to install KubeEdge to upgrade the open PaaS platform and install a container platform deployed on Open PaaS based on a developer support environment.

<br>

### <div id='1.2'> 1.2. range
The installation scope was created based on the basic installation to verify KubeEdge.

<br>

### <div id='1.3'> 1.3. System configuration diagram
The system configuration consists of a Kubernetes Cluster (Master, Worker, Edge) environment. <br>
Install Kubernetes Cluster (Master, Worker) through Container Platform Cluster Deployment and install KubeEdge in the Kubernetes Cluster and Edge environment. Through Pod, a middleware environment such as database and private registry is provided, and the Container Platform portal environment is distributed to the Kubernetes Cluster using Container Image. <br>
The total required VM environment requires **single deployment or HA deployment cluster, Edge VM: 1 or more**, and this document describes Edge VM installation to configure Edge Node in a Kubernetes Cluster environment.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://kubeedge.io/en/docs/
> https://github.com/kubeedge/kubeedge

<br>

## <div id='2'> 2. Install KubeEdge

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installing the **Master and Worker Node environments in Ubuntu 20.04 amd64** and the **Edge Node environments in Ubuntu 20.04 arm64**. To install KubeEdge, Kubernetes Native Cluster must be deployed on the system.

The major software and package version information required to install KubeEdge is as follows.

|Main Software|Version|
|---|---|
|KubeEdge|v1.12.0|
|EdgeMesh|v1.12.0|
|Kubernetes Native|v1.26.5|
|Kubernetes Native (Edge Node)|v1.22.6|
|CRI-O|v1.26.0|
|CRI-O (Edge Node)|v1.22.0|

This guide document recommends the following when deploying Container Platform.

- One or more machines running a deb/rpm compatible Linux OS (Ubuntu)
- RAM of 8G (minimum specification) or 16G (recommended specification) or more per machine
- 2 (minimum specifications) or 4 (recommended specifications) or more CPUs on the machine used as the control-plane node
- Full network connectivity between all systems in the cluster

#### Firewall information
- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Remarks</center> |
| :---: | :---: | :--- |
| TCP | 111 | NFS PortMapper |
| TCP | 2049 | NFS |
| TCP | 2379-2380 | etcd server client API |
| TCP | 6443 | kubernetes API Server |
| TCP | 9443 | cloudcore router port |
| TCP | 10000-10004 | cloudHub websocket port |
| TCP | 10001 | cloudHub quick port |
| TCP | 10002 | cloudHub https port |
| TCP | 10003 | cloudStream streamPort |
| TCP | 10004 | cloudStream tunnelPort |
| TCP | 10250 | Kubelet API |
| TCP | 10251 | kube-scheduler |
| TCP | 10252 | kube-controller-manager |
| TCP | 10255 | Read-Only Kubelet API |
| TCP | 20004 | edgeMesh server containerPort |
| TCP | 20006 | edgeMesh tunnel listenPort |
| TCP | 40001 | edgeMesh edgeProxy listenPort |
| UDP | 4789 | Calico networking VXLAN |

- Worker Nodes

| <center>Protocol</center> | <center>Port</center> | <center>Remarks</center> |
| :---: | :---: | :--- |
| TCP | 111 | NFS PortMapper |
| TCP | 2049 | NFS |
| TCP | 10250 | Kubelet API |
| TCP | 10255 | Read-Only Kubelet API |
| TCP | 20006 | edgeMesh tunnel listenPort |
| TCP | 30000-32767 | NodePort Services |
| TCP | 40001 | edgeMesh edgeProxy listenPort |
| UDP | 4789 | Calico networking VXLAN |

-Edge Node

| <center>Protocol</center> | <center>Port</center> | <center>Remarks</center> |
| :---: | :---: | :--- |
| TCP | 111 | NFS PortMapper |
| TCP | 1883-1884 | eventBus mqttPort |
| TCP | 2049 | NFS |
| TCP | 10001 | edgeHub quick port |
| TCP | 10250 | Kubelet API |
| TCP | 10255 | Read-Only Kubelet API |
| TCP | 10350 | Use kubectl logs |
| TCP | 10550 | edgecore list-watch port |
| TCP | 20006 | edgeMesh tunnel listenPort |
| TCP | 30000-32767 | NodePort Services |
| TCP | 40001 | edgeMesh edgeProxy listenPort |

<br>

### <div id='2.2'> 2.2. Kubernetes Native Cluster Deployment
To install KubeEdge, a Kubernetes Cluster must be deployed in the cloud area, and after deployment, an edge node must be deployed in the edge area.

- Deploy Kubernetes Cluster through Container Platform Cluster Deployment in the cloud area.

> https://github.com/K-PaaS/container-platform/blob/master/install-guide/standalone/cp-cluster-install.md (standalone distribution)

> https://github.com/K-PaaS/container-platform/blob/master/install-guide/standalone/cp-cluster-ha-install.md (HA distribution)

<br>

### <div id='2.3'> 2.3. Preparing to install KubeEdge
After pre-defining the environment variables required to install KubeEdge, proceed with the installation through a shell script.

- If the EdgeNode environment is **Raspberry Pi**, add the following information. If you are not in a Raspberry Pi environment, skip the steps below and start by defining KubeEdge installation environment variables.
```
# vi /boot/firmware/cmdline.txt

... cgroup_enable=memory cgroup_memory=1 (add at the end)
```

- **Raspberry Pi** Reboot.
```
#reboot
```

- Go to the Container Platform Cluster installation path. From now on, you only need to proceed on **Master Node**.
```
## In case of standalone distribution cluster
$ cd cp-deployment/standalone/single_control_plane

## In case of HA distribution cluster
$ cd cp-deployment/standalone/ha_control_plane
```

- Define the environment variables needed to install KubeEdge. HostName and IP information can be checked through the following.
```
$ vi cp-edge-vars.sh
```

```
## HostName information = Enter the hostname command in the shell of each host.
## Private IP information = Enter ifconfig in the shell of each host, then enter inet ip

#!/bin/bash

export CLOUDCORE_VIP={Enter Master Node's Public IP information}

export CLOUDCORE1_NODE_HOSTNAME={Enter the HostName information of the Node where CloudCore will be installed}
export CLOUDCORE2_NODE_HOSTNAME={Enter the HostName information of the Node where CloudCore will be installed}

## Edge Node Count Info
export EDGE_NODE_CNT={Number of Edge Nodes}

## Add Edge Node Info
export EDGE1_NODE_HOSTNAME={Enter HostName information of Edge Node 1}
export EDGE1_NODE_PRIVATE_IP={Enter private IP information of Edge Node 1}
...
export EDGE{n}_NODE_HOSTNAME={Add HostName information variable according to the number of Edge Nodes}
export EDGE{n}_NODE_PRIVATE_IP={Add Private IP information variable according to the number of Edge Nodes}
```

<br>


### <div id='2.4'> 2.4. Install KubeEdge
Install necessary packages, set Node configuration information, set KubeEdge installation information, and install KubeEdge through Ansible playbook all at once through a shell script.

- Proceed with installation using a shell script.
```
## In case of standalone distribution configuration
$ source deploy-cp-edge.sh

## For External ETCD configuration
$ source deploy-external-cp-edge.sh

## For Stacked ETCD configuration
$ source deploy-stacked-cp-edge.sh
```

<br>

### <div id='2.5'> 2.5. Verify KubeEdge installation
Confirm KubeEdge installation by checking the Kubernetes Node and the Pod in the kube-system Namespace.
```
# kubectl get nodes
NAME                 STATUS   ROLES                  AGE     VERSION
cp-edge              Ready    agent,edge             5m40s   v1.22.6-kubeedge-v1.12.0
cp-master            Ready    control-plane,master   39m     v1.26.5
cp-worker-1          Ready    <none>                 38m     v1.26.5
cp-worker-2          Ready    <none>                 38m     v1.26.5
cp-worker-3          Ready    <none>                 38m     v1.26.5

# kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
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
kube-proxy-zmp95                           1/1     Running   0          38m
kube-scheduler-cp-master                   1/1     Running   1          39m
metrics-server-5cd75b7749-57sc2            2/2     Running   0          37m
nginx-proxy-cp-worker-1                    1/1     Running   0          38m
nginx-proxy-cp-worker-2                    1/1     Running   0          38m
nginx-proxy-cp-worker-3                    1/1     Running   0          38m
nodelocaldns-24vq4                         1/1     Running   0          6m4s
nodelocaldns-jjrjj                         1/1     Running   0          37m
nodelocaldns-kgzxb                         1/1     Running   0          37m
nodelocaldns-l9s47                         1/1     Running   0          37m
```
<br>

## <div id='3'> 3. KubeEdge Reset (Reference)
Delete KubeEdge using Ansible playbook.

```
$ source reset-cp-edge.sh
```

<br>

## <div id='4'> 4. Precautions when creating resources
When users create their own resources, be careful not to use the following prefixes.

|Resource name|Prefix to be excluded when creating|
|---|---|
|All Resource|kube*|
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

<br>

[image 001]:images/edge-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > Edge installation guide
