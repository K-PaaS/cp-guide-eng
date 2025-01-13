### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/README.md) > Cluster Installation Guide

<br>

## Table of Contents
1. [Document Overview](#1)<br>
   1.1. [Purpose](#1.1)<br>
   1.2. [Range](#1.2)<br>
   1.3. [System configuration diagram](#1.3)<br>
   1.4. [Reference](#1.4)<br>

2. [Container Platform Cluster Installation](#2)<br>
   2.1. [Prerequisite](#2.1)<br>
   2.2. [Generate and distribute SSH Key](#2.2)<br>
   2.3. [Download Container Platform Cluster Deployment](#2.3)<br>
   2.4. [Preparing to install Container Platform Cluster](#2.4)<br>
   2.5. [Container Platform Cluster Installation](#2.5)<br>
   2.6. [Check Container Platform Cluster installation](#2.6)<br>

3. [Delete Container Platform Cluster (Reference)](#3)<br>

4. [Precautions when creating a resource](#4)

<br>

## <div id='1'> 1. Document overview

### <div id='1.1'> 1.1. purpose
This document (Container Platform Cluster Installation Guide) describes how to install Kubernetes Native to install the container platform deployed on Open PaaS based on the open PaaS platform advancement and developer support environment.

<br>

### <div id='1.2'> 1.2. range
The installation scope was created based on the basic installation of Container Platform Cluster to verify Kubernetes Native.

<br>

### <div id='1.3'> 1.3. System configuration diagram
The system configuration consists of a Kubernetes Cluster (Master, Worker) environment.<br>
Install Kubernetes Cluster through Container Platform Cluster Deployment, provide middleware environment such as database and private registry through Pod, and deploy Container Platform portal environment to Kubernetes Cluster with Container Image. <br>
The total required VM environment requires **Master VM: 1, Worker VM: 3 or more**, and this document describes the installation of Master VM and Worker VM to configure a Kubernetes Cluster environment.

> Starting with container platform version v1.4, storage is deployed together with cluster deployment.

> When deploying NFS, NFS-Server installation must proceed first.
> [NFS Server Installation](../nfs-server-install-guide.md)

> When deploying Rook-Ceph, **allocation of additional volumes in addition to the Root Volume to each Worker VM** must be done first.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://kubespray.io
> https://github.com/kubernetes-sigs/kubespray

<br>

## <div id='2'> 2. Install Container Platform Cluster

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installation in the **Ubuntu 20.04** environment. To install Container Platform Cluster, Ansible v5.7+, Jinja 3.1+, and python-netaddr must be installed on the system where Ansible commands will be executed, and installation proceeds sequentially according to the installation guide.


The major software and package version information required to install Kubespray is as follows.

|Main Software|Version|Python Package|Version
|---|---|---|---|
|Kubespray|v2.22.1|ansible|5.7.1|
|Kubernetes Native|v1.26.5|ansible-core|2.12.5|
|CRI-O|v1.26.0|cryptography|3.4.8|
|Helm|v3.12.0|jinja2|3.1.2|
|Istio|1.16.0|netaddr|0.8.0|
|Podman|3.4.2|pbr|5.11.1|
|Terraform|1.3.4|jmespath|1.0.1|
|NFS Common||ruamel.yaml|0.17.21|
|Rook Ceph|1.10.3|ruamel.yaml.clib|0.2.7|
|Kubeflow|1.7.0|MarkupSafe|2.1.2|
|Vault|1.11.3|
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
| TCP | 6443 | Kubernetes API Server |
| TCP | 10250 | Kubelet API |
| TCP | 10251 | kube-scheduler |
| TCP | 10252 | kube-controller-manager |
| TCP | 10255 | Read-Only Kubelet API |
| UDP | 4789 | Calico networking VXLAN |

- Worker Nodes

| <center>Protocol</center> | <center>Port</center> | <center>Remarks</center> |
| :---: | :---: | :--- |
| TCP | 111 | NFS PortMapper |
| TCP | 2049 | NFS |
| TCP | 10250 | Kubelet API |
| TCP | 10255 | Read-Only Kubelet API |
| TCP | 30000-32767 | NodePort Services |
| UDP | 4789 | Calico networking VXLAN |

<br>

### <div id='2.2'> 2.2. SSH Key creation and distribution
To install Container Platform Cluster, the SSH Key must be copied to all servers in the inventory. In this installation guide, SSH connection setup is performed using an RSA public key.

All installation processes after SSH Key creation and distribution are performed on **Master Node**.

- Generate RSA public key in **Master Node**.
```
$ ssh-keygen -t rsa -m PEM
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): [Press Enter key]
Enter passphrase (empty for no passphrase): [Press Enter key]
Enter same passphrase again: [Press Enter key]
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:pIG4/G309Dof305mWjdNz1OORx9nQgQ3b8yUP5DzC3w ubuntu@cp-master
The key's randomart image is:
+---[RSA 2048]----+
|            ..= o|
|   . .       * B |
|  . . . .   . = *|
| . .   +     + E.|
|  o   o S     +.O|
|   . o o .     XB|
|    . o . o   *oO|
|     .  .. o B oo|
|        .o. o.o  |
+----[SHA256]-----+
```
- Copy the public key to the **Master, Worker Node** you will use.
```
## Copy the printed public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ5 61H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUY P9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

- Copy the public key to the last part of the authorized_keys file body (added below the existing body content) of the **Master, Worker Node** to be used.
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLz wTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTD vRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ5 61H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUY P9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

### <div id='2.3'> 2.3. Download Container Platform Cluster Deployment
From 2.3, you only need to proceed on the **Master Node** (there is no additional work on the Worker Node).
Download the source file required for Container Platform Cluster installation and place it in the Kubespray installation path.

- Container Platform Cluster Deployment Download URL: https://github.com/k-paas/cp-deployment

- Download Container Platform Cluster Deployment from the following path using the git clone command.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.4.x
```

<br>

### <div id='2.4'> 2.4. Preparing to install Container Platform Cluster
After predefining the environment variables required for Container Platform Cluster installation, proceed with installation through a shell script.

- Go to the Container Platform Cluster installation path.
```
$ cd cp-deployment/standalone/single_control_plane
```

- Define environment variables required for Container Platform Cluster installation. HostName and IP information can be checked through the following.
```
$ vi cp-cluster-vars.sh
```

```
## HostName information = Enter the hostname command in the shell of each host.
## Private IP information = Enter ifconfig in the shell of each host, then enter inet ip
## Public IP information = Enter assigned public IP information, if not assigned, enter private IP information

#!/bin/bash

export MASTER_NODE_HOSTNAME={Enter HostName information of Master Node}
export MASTER_NODE_PUBLIC_IP={Enter Master Node's Public IP information}
export MASTER_NODE_PRIVATE_IP={Enter Private IP information of Master Node}

## Worker Node Count Info
export WORKER_NODE_CNT={Number of Worker Nodes}

## Add Worker Node Info
export WORKER1_NODE_HOSTNAME={Enter HostName information of Worker No. 1}
export WORKER1_NODE_PRIVATE_IP={Enter private IP information of Worker No. 1}
export WORKER2_NODE_HOSTNAME={Enter HostName information of Worker No. 2}
export WORKER2_NODE_PRIVATE_IP={Enter private IP information of Worker No. 2}
export WORKER3_NODE_HOSTNAME={Enter HostName information of Worker Node 3}
export WORKER3_NODE_PRIVATE_IP={Enter private IP information of Worker Node 3}
...
export WORKER{n}_NODE_HOSTNAME={Add HostName information variable according to the number of Worker Nodes}
export WORKER{n}_NODE_PRIVATE_IP={Add Private IP information variable according to the number of Worker Nodes}
```

- Select the storage to install.
If it is not possible to add a volume to a node, it is recommended to select NFS, and if it is possible to add a volume, it is recommended to select Rook-Ceph.
When selecting NFS, enter the private IP information of the NFS server.
```
...
## Storage Type Info (eg. nfs, rook-ceph)
export STORAGE_TYPE={Enter Storage Type information to be installed}
export NFS_SERVER_PRIVATE_IP={Enter Private IP information of NFS Server when setting Storage Type nfs}
...
```

<br>

### <div id='2.5'> 2.5. Install Container Platform Cluster
Install necessary packages, set Node configuration information, set Container Platform Cluster installation information, and install Container Platform Cluster through Ansible playbook all at once through shell script.

- Proceed with installation using a shell script.
```
$ source deploy-cp-cluster.sh
```

<br>

### <div id='2.6'> 2.6. Verify Container Platform Cluster installation
Check the Container Platform Cluster installation by checking the Kubernetes Node and the Pod in the kube-system Namespace.

```
$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE   VERSION
cp-master            Ready    control-plane          12m   v1.26.5
cp-worker-1          Ready    <none>                 10m   v1.26.5
cp-worker-2          Ready    <none>                 10m   v1.26.5
cp-worker-3          Ready    <none>                 10m   v1.26.5

$ kubectl get pods -n kube-system
NAME                                          READY   STATUS    RESTARTS      AGE
calico-node-d8sg6                             1/1     Running   0             9m22s
calico-node-kfvjx                             1/1     Running   0             10m
calico-node-khwdz                             1/1     Running   0             10m
calico-node-nc58v                             1/1     Running   0             10m
coredns-657959df74-td5c2                      1/1     Running   0             8m15s
coredns-657959df74-ztnjj                      1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0             8m9s
kube-apiserver-cp-master                      1/1     Running   0             12m
kube-controller-manager-cp-master             1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                              1/1     Running   0             10m
kube-proxy-kkvhk                              1/1     Running   0             10m
kube-proxy-nfttc                              1/1     Running   0             10m
kube-proxy-znfgk                              1/1     Running   0             10m
kube-scheduler-cp-master                      1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0             7m57s
nginx-proxy-cp-worker-1                       1/1     Running   0             10m
nginx-proxy-cp-worker-2                       1/1     Running   0             10m
nginx-proxy-cp-worker-3                       1/1     Running   0             10m
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7grn                            1/1     Running   0             8m8s
```

<br>

## <div id='3'> 3. Delete Container Platform Cluster (Reference)
Delete Container Platform Cluster using Ansible playbook.

```
$ source reset-cp-cluster.sh
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

[image 001]:images/standalone-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > Cluster Installation Guide
