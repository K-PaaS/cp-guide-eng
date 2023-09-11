### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Cluster Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Range](#1.2)  
  1.3. [System Configuration](#1.3)  
  1.4. [References](#1.4)  

2. [Kubespray Installation](#2)  
  2.1. [Prerequisite](#2.1)  
  2.2. [SSH Key Creation and Deployment](#2.2)  
  2.3. [Kubespray Download](#2.3)  
  2.4. [Kubespray Installation Preparation](#2.4)  
  2.5. [Kubespray Installation](#2.5)  
  2.6. [Kubespray Installation Check](#2.6)  

3. [Kubespray Deletion (Refer)](#3)  

4. [Create Container Platform Administrator and Acquired Token(Refer)](#4)  
  4.1. [Create Cluster Role Administrator and Acquire Token](#4.1)  
  4.2. [Namespace User Token Acquisition](#4.2)  

5. [Cautions when creating a resource](#5)  

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (Kubespray Installation Guide) describes how to install Kubespray to upgrade the open PaaS platform and install Kubespray to install a container platform deployed to Open PaaS based on a developer support environment.

From version 5.5 of K-PaaS, single deployment is supported based on Kubespray. If you want to install based on the existing container service, refer to the document of K-PaaS 5.0 or the lower version.

<br>

### <div id='1.2'> 1.2. Range
The installation range was created based on the Kubespray basic installation to verify Kubernetes Native.

<br>

### <div id='1.3'> 1.3. System Configuration
System configuration consists of a Kubernetes cluster (Master, Worker) environment.<br>
Kubernetes Cluster is installed through Kubespary and middleware environments such as Database and Private registry are provided through Pod to deploy the Container Platform portal environment to Kubernetes Cluster with Container Image. <br>
The total required VM environment is **Master VM: 1 and Worker VM: 1 or more**, and this document contains the installation of Master VM and Worker VM to configure the Kubernetes Cluster environment.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://kubespray.io  
> https://github.com/kubernetes-sigs/kubespray  

<br>

## <div id='2'> 2. Kubespray Installation

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on Ubuntu 18.04 Environment. To install Kubespray, Ansible v2.9 +, Jinja 2.11+, and python-netaddr must be installed on the system to execute Ansible commands and proceed sequentially according to the installation guide.


The main software and package version information required for Kubespray installation are as follows.

|Main software|Version|Python Package|Version
|---|---|---|---|
|Kubespray|v2.16.0|ansible|2.9.20|
|Kubernetes Native|v1.20.5|jinja2|2.11.3|
|CRI-O|v1.20.0|netaddr|0.7.19|
|Helm|3.5.4|pbr|5.4.4|
|Istio|1.11.4|jmespath|0.9.5|
|NFS Common||ruamel.yaml|0.16.10|
|||cryptography|2.8|
|||MarkupSafe|1.1.1|

Kubernetes official guide document recommends the following when deploying a Cluster.

- One or more machines (Ubuntu or CentOS) running a debug / rpm compatible Linux OS
- 2G or more RAM per machine
- Two or more CPUs on a machine that is used as a control-plane node
- Full network connectivity between all systems in the cluster


#### firewall information
- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP Network |  
| TCP | 2049 | NFS |  
| TCP | 2379-2380 | etcd server client API |  
| TCP | 6443 | Kubernetes API Server |  
| TCP | 10250 | Kubelet API |  
| TCP | 10251 | kube-scheduler |  
| TCP | 10252 | kube-controller-manager |  
| TCP | 10255 | Read-Only Kubelet API |  
| IP-in-IP (Protocol Num 4) || Calico Overlay Network |  

- Worker Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP network |  
| TCP | 2049 | NFS |  
| TCP | 10250 | Kubelet API |  
| TCP | 10255 | Read-Only Kubelet API |  
| TCP | 30000-32767 | NodePort Services |  
| IP-in-IP (Protocol Num 4) || Calico Overlay Network |  

<br>

### <div id='2.2'> 2.2. SSH Key Creation and Deployment
For Kubespray installation, the SSH key must be copied to all servers in the inventory. In this installation guide, the SSH connection setting is performed using the RSA public key.

All installation processes after SSH key generation and deployment are performed at **Master Node**.

- Create RSA public key in Master Node.
```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): [Input Enter key]
Enter passphrase (empty for no passphrase): [Input Enter key]
Enter the same passphrase again: [Input Enter key]
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

- Copy the public key to the **Master, Worker Node** to use.
```
## Copy the printed public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

- Copy the public key to the end of the authorized_keys file body of the **Master, Worker Node** to use (add below the existing body content).
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLzwTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTDvRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

### <div id='2.3'> 2.3. Kubespray Download
from 2.3., you may proceed only at the **Master Node**. (No further work in Worker Node)
Download the source file required for Kubespray installation and place it in the Kubespray installation work path.

- Kubespray Download URL : https://github.com/K-PaaS/cp-deployment

- Download Kubespray from the following path through the git clone command. The version of Kubespray in this installation guide is v2.16.0.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.2.x
```

<br>

### <div id='2.4'> 2.4. Kubespray Installation Preparation
After pre-defining the environment variables required for Kubespray installation, installation is carried out through the shell script.

- Move the Kubespray installation path.
```
## When installing AWS Environment

$ cd cp-deployment/standalone/aws
```

```
## When installing Openstack Environment

$ cd cp-deployment/standalone/openstack
```

- Define the environment variables required for Kubespray installation. HostName and IP information can be found by:
```
$ vi kubespray_var.sh
```

```
## HostName Information = Enter the hostname command from the shell of each host
## Private IP Information = Enter ifconfig and inet ip in the shell of each host
## Public IP Information = Enter assigned public IP information, enter private IP information if not assigned

#!/bin/bash

export MASTER_NODE_HOSTNAME={Enter HostName Information of Master Node}
export MASTER_NODE_PUBLIC_IP=Enter  Public IP Information of Master Node}
export MASTER_NODE_PRIVATE_IP={Enter Private IP Information of Master Node}
export WORKER1_NODE_HOSTNAME={Enter HostName Information of Worker Number1 Node}
export WORKER1_NODE_PRIVATE_IP={Enter Private IP Information of Worker Number1 Node}
export WORKER2_NODE_HOSTNAME={Enter HostName Information of Worker Number 2 Node}
export WORKER2_NODE_PRIVATE_IP={Enter Private IP Information of Worker Number2 Node}
export WORKER3_NODE_HOSTNAME={Enter HostName Information of Worker Number 3 Node}
export WORKER3_NODE_PRIVATE_IP={Enter Private IP Information of Worker Number3 Node}
...
```

- When installed in an OpenStack environment, the following variables are added in the kubespray_var.sh script:
If the MTU value of the OpenStack network interface is not the default value of 1450, modify the following values to change the CNI Plugin MTU setting.
```
...
export CALICO_MTU=1450 (Modify when needed)
```
- Additional environmental variables are required when installing in the Openstack environment, and environmental variables can be automatically registered by downloading the configuration file.
```
## Openstack UI Login > Select Project > Select API Access Menu > Click Download OpenStack RC File
## Enter Openstack account password after running script file

$ source {OPENSTACK_PROJECT_NAME}-openrc.sh
Please enter your OpenStack Password for project admin as user admin: {Enter password}
```

<br>

### <div id='2.5'> 2.5. Kubespray Installation
Installation of required packages, setting node configuration information, setting Kubespray installation information through shell scripts, and installation of Kubespray through Ansible playbook is carried out collectively.

- Proceed installation through the shell script.
```
$ source deploy_kubespray.sh
```

- If you set the environment variable incorrectly or if an issue arises during the installation process, you can proceed with the installation using each separated script.

```
1. kubespray_var.sh: Declare environment variables needed for Kubespray installation
2. package_install.sh : install pip package
3. kubespray_setting.sh: Node Configuration Information, Kubespray installation information settings
4. kubespray_install.sh: Installation of Kuberspary through Ansible playbook
```

<br>

### <div id='2.6'> 2.6. Kubespray Installation Check
Check the Pod of the Kubernetes Node and kube-system Namespace to check the installation of Kubespray.

```
$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE   VERSION
cp-master     Ready    control-plane,master   12m   v1.20.5
cp-worker-1   Ready    <none>                 10m   v1.20.5
cp-worker-2   Ready    <none>                 10m   v1.20.5
cp-worker-3   Ready    <none>                 10m   v1.20.5

$ kubectl get pods -n kube-system
NAME                                          READY   STATUS    RESTARTS   AGE
calico-kube-controllers-7c5b64bf96-xwdgn      1/1     Running   0          8m52s
calico-node-d8sg6                             1/1     Running   0          9m22s
calico-node-kfvjx                             1/1     Running   0          10m
calico-node-khwdz                             1/1     Running   0          10m
calico-node-nc58v                             1/1     Running   0          10m
coredns-657959df74-td5c2                      1/1     Running   0          8m15s
coredns-657959df74-ztnjj                      1/1     Running   0          8m7s
csi-cinder-controllerplugin-99c9dd87b-hz62k   5/5     Running   0          7m42s
csi-cinder-nodeplugin-jjkg5                   2/2     Running   0          7m41s
csi-cinder-nodeplugin-njdrc                   2/2     Running   0          7m41s
csi-cinder-nodeplugin-sb9vg                   2/2     Running   0          7m41s
csi-cinder-nodeplugin-t5fxh                   2/2     Running   0          7m41s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0          8m9s
kube-apiserver-cp-master               1/1     Running   0          12m
kube-controller-manager-cp-master      1/1     Running   1          12m
kube-proxy-dj5c8                              1/1     Running   0          10m
kube-proxy-kkvhk                              1/1     Running   0          10m
kube-proxy-nfttc                              1/1     Running   0          10m
kube-proxy-znfgk                              1/1     Running   0          10m
kube-scheduler-cp-master               1/1     Running   1          12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0          7m57s
nginx-proxy-cp-worker-1                1/1     Running   0          10m
nginx-proxy-cp-worker-2                1/1     Running   0          10m
nginx-proxy-cp-worker-3                1/1     Running   0          10m
nodelocaldns-556gb                            1/1     Running   0          8m8s
nodelocaldns-8dpnt                            1/1     Running   0          8m8s
nodelocaldns-pvl6z                            1/1     Running   0          8m8s
nodelocaldns-x7grn                            1/1     Running   0          8m8s
openstack-cloud-controller-manager-mct28      1/1     Running   0          8m57s
snapshot-controller-0                         1/1     Running   0          7m33s
```

<br>

## <div id='3'> 3. Kubespray Deletion (Refer)
Delete Kubespray by using Ansible playbook.

```
$ source remove_kubespray.sh
```

<br>

## <div id='4'> 4. Create Container Platform Administrator and Acquired Token(Refer)

### <div id='4.1'> 4.1. Create Cluster Role Administrator and Acquire Token
After installing Kubespray, create a service account for an operator with a Cluster Role. The token of the service account is used to create a Super Admin account in the operator portal.

- Create a Service Account
```
## {SERVICE_ACCOUNT} : Service Account Name to Create

$ kubectl create serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl create serviceaccount k8sadmin -n kube-system)
```

- Bind to the Service Account that created the Cluster Role.
```
$ kubectl create clusterrolebinding {SERVICE_ACCOUNT} --clusterrole=cluster-admin --serviceaccount=kube-system:{SERVICE_ACCOUNT}
```

- Acquire the token of the created Service Account.
```
## {SECRET_NAME} : Check the Mountable secrets value 

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

### <div id='4.2'> 4.2. Namespace User Token Acquisition
It is used to obtain the token value after creating a Namespace and registering a user in the portal.

- Acquire Token of Namespace user.
```
## {SECRET_NAME} : Check the Mountable secrets value
## {NAMESPACE} : Namespace Name

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n {NAMESPACE}

$ kubectl describe secret {SECRET_NAME} -n {NAMESPACE} | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

## <div id='5'> 5. Cautions when creating a resource
When users create their own resources, be careful not to use the following prefixes.

|Resource Name|Prefix to be excluded when creating|
|---|---|
|All Resources|kube*|
|Namespace|all|
||kubernetes-dashboard|
||paas-ta-container-platform-temp-namespace|
|Role|paas-ta-container-platform-init-role|
||paas-ta-container-platform-admin-role|
|ResourceQuota|paas-ta-container-platform-low-rq|
||paas-ta-container-platform-medium-rq|
||paas-ta-container-platform-high-rq|
|LimitRanges|paas-ta-container-platform-low-limit-range|
||paas-ta-container-platform-medium-limit-range|
||paas-ta-container-platform-high-limit-range|
|Pod|nodes|
||resources|

<br>

[image 001]:images/standalone-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Cluster Installation Guide
