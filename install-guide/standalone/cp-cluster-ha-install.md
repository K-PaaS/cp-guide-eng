### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > Cluster Installation Guide (HA)
<br>

## Table of Contents
1. [Document Overview](#1)<br>
    1.1. [Purpose](#1.1)<br>
    1.2. [Range](#1.2)<br>
    1.3. [System configuration diagram](#1.3)<br>
    1.4. [Reference](#1.4)<br>

2. [Container Platform Cluster HA Installation](#2)<br>
    2.1. [Prerequisite](#2.1)<br>
    2.2. [LoadBalancer configuration](#2.2)<br>
    2.3. [Generate and distribute SSH Key](#2.3)<br>
    2.4. [Download Container Platform Cluster](#2.4)<br>
    2.5. [Preparing to install Container Platform Cluster](#2.5)<br>
    2.6. [Container Platform Cluster Installation](#2.6)<br>
    2.7. [Check Container Platform Cluster installation](#2.7)<br>

3. [Delete Container Platform Cluster (Reference)](#3)<br>

4. [Precautions when creating a resource](#4)

<br>

## <div id='1'> 1. Document overview

### <div id='1.1'> 1.1. purpose
This document (Container Platform Cluster HA Installation Guide) describes how to install Kubernetes Native using Container Platform Cluster Deployment to install a container platform deployed on Open PaaS based on open PaaS platform advancement and developer support environment.

<br>

### <div id='1.2'> 1.2. range
The installation scope was created based on Container Platform Cluster HA installation to verify Kubernetes Native.

<br>

### <div id='1.3'> 1.3. System configuration diagram
System configuration varies depending on the IaaS environment and varies depending on the External ETCD and Stacked ETCD methods.<br>

The system configuration consists of a LoadBalancer and Kubernetes Cluster environment. After configuring a redundant LoadBalancer, Kubernetes Cluster is installed through Container Platform Cluster Deployment, and a middleware environment such as database and private registry is provided through Pod to connect to the Kubernetes Cluster using Container Image and Container Platform portal. Deploy the environment.<br>

**Based on OpenStack, vSphere External ETCD**, the total required VM environment requires **LoadBalancer VM: 2, Master VM: 3, External ETCD VM: 3, Worker VM: 3 or more**. <br>

**Based on OpenStack, vSphere Stacked ETCD**, the total required VM environment requires **LoadBalancer VM: 2, Master VM: 3, Worker VM: 3 or more**.<br>

**Based on AWS External ETCD**, the total required VM environment requires **Master VM: 3, External ETCD VM: 3, and Worker VM: 3 or more**.<br>

**Based on AWS Stacked ETCD**, the total required VM environment requires **Master VM: 3, Worker VM: 3 or more**.<br>

This document describes the installation of LoadBalancer, Master VM, External ETCD VM, and Worker VM to configure a Kubernetes Cluster environment.<br>

> Starting with container platform version v1.4, storage is deployed together with cluster deployment.

> When deploying NFS, NFS-Server installation must proceed first.
> [NFS Server Installation](../nfs-server-install-guide.md)

> When deploying Rook-Ceph, **allocation of additional volumes in addition to the Root Volume to each Worker VM** must be done first.

- External ETCD<br>
![image 001]

- Stacked ETCD<br>
![image 002]

<br>

### <div id='1.4'> 1.4. References
> https://kubespray.io
> https://github.com/kubernetes-sigs/kubespray

<br>

## <div id='2'> 2. Install Container Platform Cluster HA

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installation in the **Ubuntu 20.04** environment. To install Container Platform Cluster HA, Ansible v5.7 +, Jinja 3.1+, and python-netaddr must be installed on the system where Ansible commands will be executed, and installation proceeds sequentially according to the installation guide.

The major software and package version information required for Container Platform Cluster installation is as follows.
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

### <div id='2.2'> 2.2. LoadBalancer Configuration
LoadBalancer is configured using Keepalived and HAProxy, and a separate VIP is required for HA configuration. Install Keepalived and HAProxy on two VMs for LoadBalancer configuration as follows.

- **Briefly describes how to set up LoadBalancer based on AWS.** Unlike OpenStack, it does not create a separate VM and uses Keepalived or HAProxy, but uses its own LoadBalancer. A Master Node VM must be created in advance.

1. Go to **AWS Service > EC2 > Load Balancing > Load Balancer > Create Load Balancer**
<br>
2. Go to **Classic Load Balancer > Create**
<br>
3. Enter Load Balancer name, select VPC to create LB, select Protocol: TCP / Port: 6443 for listener configuration, select subnet and move to next.
<br>
4. After selecting the security group, move to the Add EC instance menu.
<br>
5. In Add EC2 Instance, select all Master Nodes and move to Next.
<br>
6. Create Load Balancer
<br>
7. Skip the subsequent steps and proceed to **2.3. Proceed with SSH Key creation and distribution** process
<br>
- **Briefly describes the LoadBalancer VM VIP allocation method based on OpenStack.**
1. OpenStack Horizon connection
<br>
2. Go to **Network > Network > “Select the network name to use” > Port tab > Create port**
<br>
3. Create after entering name and static IP address information
<br>
4. Go to **Compute > Instances > "Select LoadBalancer Instance Name" > Interfaces tab > "Select Interface Name" > Allowed Address Pairs tab > Add Allowed Address Pair**
<br>
5. Enter port private IP information in the IP address or CIDR field.
<br>

- **Briefly describes the vSphere-based LoadBalancer VM VIP allocation method.**
1. Connect to pfsense
<br>
2. Go to **Firewall > Virtual IPs**
<br>
3. After clicking the Add button, select **Type: IP Alias, the Interface to use, and enter Address information.
<br>

- Install Keepalived on two LoadBalancer VMs as follows.
```
$ sudo su -

# apt-get update

# apt-get install -y keepalived

# echo 'net.ipv4.ip_nonlocal_bind=1' >> /etc/sysctl.conf
# echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
# sysctl -p
```

- Proceed with Keepalived settings.
```
# vi /etc/keepalived/keepalived.conf
```

```
## Interface Name information: Check after entering ifconfig in the shell of each host.
## VIP information: Private IP of the port assigned to LoadBalancer VM

## Proceed with settings on the Master VM.
vrrp_instance VI_1 {
  state MASTER
  interface {{INTERFACE_NAME}}
  virtual_router_id 51
  priority 110
  advert_int 1
  authentication {
    auth_type PASS
    auth_pass 1111
  }
  virtual_ipaddress {
    {{VIP}}
  }
}
## Proceed with setting up the Backup VM.
vrrp_instance VI_1 {
   state BACKUP
   interface {{INTERFACE_NAME}}
   virtual_router_id 51
   priority 100
   advert_int 1
   authentication {
     auth_type PASS
     auth_pass 1111
   }
   virtual_ipaddress {
     {{VIP}}
   }
}
```

- Start the Keepalived service.
```
# systemctl start keepalived
# systemctl enable keepalived
```

- Install HAProxy on two LoadBalancer VMs as follows.
```
# apt-get install -y haproxy
```

- Proceed with HAProxy settings. Add the following to the bottom of the haproxy.cfg file.

```
# vi /etc/haproxy/haproxy.cfg
```

```
## VIP information: Private IP of the port assigned to LoadBalancer VM
## Master Node IP information: Enter each Master Node Private IP

...
listen kubernetes-apiserver-https
   bind {{VIP}}:6443
   mode tcp
   optionlog-health-checks
   timeout client 3h
   timeout server 3h
   server master1 {{MASTER_NODE_IP1}}:6443 check check-ssl verify none inter 10000
   server master2 {{MASTER_NODE_IP2}}:6443 check check-ssl verify none inter 10000
   server master3 {{MASTER_NODE_IP3}}:6443 check check-ssl verify none inter 10000
   balance roundrobin
```

- Restart the HAProxy service.
```
# systemctl restart haproxy
```

<br>

### <div id='2.3'> 2.3. SSH Key creation and distribution
To install Container Platform Cluster, the SSH Key must be copied to all servers in the inventory. In this installation guide, SSH connection setup is performed using an RSA public key.

All installation processes after SSH Key creation and distribution are carried out on **Master Node 1**.

- Generate an RSA public key at **Master Node 1**.
```
$ ssh-keygen -t rsa -m PEM
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): Press Enter key]
Enter passphrase (empty for no passphrase): Press Enter key
Enter same passphrase again: Press Enter key]
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
- Copy the public key to **all Master, ETCD, and Worker Nodes** to be used.
```
## Copy the printed public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ5 61H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUY P9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

- Copy the public key to the last part of the authorized_keys file body (added below the existing body content) of **all Master, ETCD, and Worker Node** to be used.
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLz wTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTD vRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ5 61H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUY P9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

### <div id='2.4'> 2.4. Download Container Platform Cluster Deployment
From 2.4, you only need to proceed on **1 Master Node** (no additional work is required on the remaining nodes).
Download the source file required for Container Platform Cluster installation and place it in the Container Platform Cluster installation work path.

- Container Platform Cluster Deployment Download URL: https://github.com/k-paas/cp-deployment

- Download Container Platform Cluster Deployment from the following path using the git clone command.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.4.x
```

<br>

### <div id='2.5'> 2.5. Preparing to install Container Platform Cluster
After predefining the environment variables required for Container Platform Cluster installation, proceed with installation through a shell script.

- Go to the Container Platform Cluster installation path.
```
$ cd cp-deployment/standalone/ha_control_plane
```

- Define environment variables required for Container Platform Cluster installation.
```
## For External ETCD configuration
$ vi external-cp-cluster-vars.sh

## For Stacked ETCD configuration
$ vi stacked-cp-cluster-vars.sh
```

- Enter LoadBalancer Domain information.
```
## LoadBalancer information = Enter domain information or VIP information

#!/bin/bash

export LOADBALANCER_DOMAIN={Enter Domain information or VIP information of LoadBalancer}
...
```
- Enter IaaS common node information. HostName and IP information can be checked through the following.
```
## HostName information = Enter the hostname command in the shell of each host.
## Private IP information = Enter ifconfig in the shell of each host, then enter inet ip
## Public IP information = Enter assigned public IP information, if not assigned, enter private IP information

## External ETCD configuration

...
export MASTER1_NODE_HOSTNAME={Enter HostName information of Master Node 1}
export MASTER1_NODE_PUBLIC_IP={Enter the public IP information of Master Node 1}
export MASTER1_NODE_PRIVATE_IP={Enter Private IP information of Master Node 1}
export MASTER2_NODE_HOSTNAME={Enter HostName information of Master Node 2}
export MASTER2_NODE_PRIVATE_IP={Enter Private IP information of Master Node 2}
export MASTER3_NODE_HOSTNAME={Enter HostName information of Master Node 3}
export MASTER3_NODE_PRIVATE_IP={Enter Private IP information of Master Node 3}
export ETCD1_NODE_HOSTNAME={Enter HostName information of ETCD Node 1}
export ETCD1_NODE_PRIVATE_IP={Enter private IP information of ETCD Node 1}
export ETCD2_NODE_HOSTNAME={Enter HostName information of ETCD Node 2}
export ETCD2_NODE_PRIVATE_IP={Enter private IP information of ETCD Node 2}
export ETCD3_NODE_HOSTNAME={Enter HostName information of ETCD Node 3}
export ETCD3_NODE_PRIVATE_IP={Enter private IP information of ETCD Node 3}

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
...
## Storage Type Info (eg. nfs, rook-ceph)
export STORAGE_TYPE={Enter Storage Type information to be installed}
export NFS_SERVER_PRIVATE_IP={Enter Private IP information of NFS Server when setting Storage Type nfs}

## Stacked ETCD configuration

...
export MASTER1_NODE_HOSTNAME={Enter HostName information of Master Node 1}
export MASTER1_NODE_PUBLIC_IP={Enter the public IP information of Master Node 1}
export MASTER1_NODE_PRIVATE_IP={Enter Private IP information of Master Node 1}
export MASTER2_NODE_HOSTNAME={Enter HostName information of Master Node 2}
export MASTER2_NODE_PRIVATE_IP={Enter Private IP information of Master Node 2}
export MASTER3_NODE_HOSTNAME={Enter HostName information of Master Node 3}
export MASTER3_NODE_PRIVATE_IP={Enter Private IP information of Master Node 3}

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
...
## Storage Type Info (eg. nfs, rook-ceph)
export STORAGE_TYPE={Enter Storage Type information to be installed}
export NFS_SERVER_PRIVATE_IP={Enter Private IP information of NFS Server when setting Storage Type nfs}
```

<br>

### <div id='2.6'> 2.6. Install Container Platform Cluster
Install necessary packages, set Node configuration information, set Container Platform Cluster installation information, and install Container Platform Cluster through Ansible playbook all at once through shell script.

- Proceed with installation using a shell script.
```
## For External ETCD configuration
$ source deploy-external-cp-cluster.sh

## For Stacked ETCD configuration
$ source deploy-stacked-cp-cluster.sh
```

<br>

### <div id='2.7'> 2.7. Verify Container Platform Cluster installation
Check the Container Platform Cluster installation by checking the Kubernetes Node and the Pod in the kube-system Namespace.
```
$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE   VERSION
cp-master-1          Ready    control-plane          12m   v1.26.5
cp-master-2          Ready    control-plane          12m   v1.26.5
cp-master-3          Ready    control-plane          12m   v1.26.5
cp-worker-1          Ready    <none>                 10m   v1.26.5
cp-worker-2          Ready    <none>                 10m   v1.26.5
cp-worker-3          Ready    <none>                 10m   v1.26.5

$ kubectl get pods -n kube-system
NAME                                                           READY   STATUS    RESTARTS   AGE
calico-node-2xwst                                              1/1     Running   0          15h
calico-node-5rbmb                                              1/1     Running   0          15h
calico-node-cqsgz                                              1/1     Running   0          15h
calico-node-gp2vp                                              1/1     Running   0          15h
calico-node-j4mz5                                              1/1     Running   0          15h
calico-node-stjlh                                              1/1     Running   0          15h
coredns-657959df74-99gz9                                       1/1     Running   0          15h
coredns-657959df74-lqhvf                                       1/1     Running   0          15h
dns-autoscaler-b5c786945-hxnr9                                 1/1     Running   0          15h
kube-apiserver-cp-master-1                                     1/1     Running   0          15h
kube-apiserver-cp-master-2                                     1/1     Running   0          15h
kube-apiserver-cp-master-3                                     1/1     Running   0          15h
kube-controller-manager-cp-master-1                            1/1     Running   0          15h
kube-controller-manager-cp-master-2                            1/1     Running   0          15h
kube-controller-manager-cp-master-3                            1/1     Running   0          15h
kube-proxy-clskg                                               1/1     Running   0          15h
kube-proxy-lwjzg                                               1/1     Running   0          15h
kube-proxy-p8kcq                                               1/1     Running   0          15h
kube-proxy-q9wxp                                               1/1     Running   0          15h
kube-proxy-qbv9j                                               1/1     Running   0          15h
kube-proxy-zlkpv                                               1/1     Running   0          15h
kube-scheduler-cp-master-1                                     1/1     Running   0          15h
kube-scheduler-cp-master-2                                     1/1     Running   0          15h
kube-scheduler-cp-master-3                                     1/1     Running   0          15h
metrics-server-876f9ff55-tntqz                                 2/2     Running   0          15h
nodelocaldns-7b5kp                                             1/1     Running   0          15h
nodelocaldns-9hc28                                             1/1     Running   0          15h
nodelocaldns-kf7tb                                             1/1     Running   0          15h
nodelocaldns-nhwr4                                             1/1     Running   0          15h
nodelocaldns-sj2zd                                             1/1     Running   0          15h
nodelocaldns-t7xg9                                             1/1     Running   0          15h
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

[image 001]:images/stanalone-ha-external-etcd-v1.2.png
[image 002]:images/stanalone-ha-stacked-etcd-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > Cluster Installation Guide (HA)
