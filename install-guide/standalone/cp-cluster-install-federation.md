### [Index](https://github.com/K-PaaS/container-platform/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Cluster Installation Guide

<br><br>

## Table of Contents

1. [Document Overview](#1)<br>
  1.1. [Purpose](#1.1)<br>
  1.2. [Scope](#1.2)<br>
  1.3. [System Architecture](#1.3)<br>
  1.4. [References](#1.4)

2. [Install K‑PaaS Container Platform Cluster](#2)<br>
  2.1. [Prerequisite](#2.1)<br>
  2.1.1. [OS](#2.1.1)<br>
  2.1.2. [Python Packages](#2.1.2)<br>
  2.1.3. [Key Software](#2.1.3)<br>
  2.1.4. [Firewall](#2.1.4)<br>
  2.1.5. [Storage](#2.1.5)<br>
  2.1.6. [Ingress Nginx and Istio Gateway Service Configuration](#2.1.6)<br>
  2.1.6.1. [Additional Interface for Control Plane Nodes](#2.1.6.1)<br>
  2.1.6.2. [Cloud Load Balancer Service](#2.1.6.2)<br>
  2.1.7. [HA Control Plane Load Balancer](#2.1.7)<br>
  2.1.7.1. [Cloud Load Balancer Service](#2.1.7.1)<br>
  2.1.7.2. [HAProxy](#2.1.7.2)<br>
  2.2. [Create and Distribute SSH Keys](#2.2)<br>
  2.3. [Download K‑PaaS Container Platform Cluster Deployment](#2.3)<br>
  2.4. [Prepare for K‑PaaS Container Platform Cluster Installation](#2.4)<br>
  2.5. [Install K‑PaaS Container Platform Cluster](#2.5)<br>
  2.6. [Verify K‑PaaS Container Platform Cluster Installation](#2.6)

3. [Delete K‑PaaS Container Platform Cluster (Reference)](#3)

4. [Precautions When Creating Resources](#4)

<br><br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Cluster Installation Guide) describes how to install a cluster for the K‑PaaS Container Platform, an open PaaS platform that supports environments for planners, developers, and operators.

<br><br>

### <div id='1.2'> 1.2. Scope
The installation scope is written based on the **`federation cluster`** environment, which serves as the foundation of the K‑PaaS Container Platform environment.

<br><br>

### <div id='1.3'> 1.3. System Architecture
The system is composed of a Kubernetes **`federation cluster`** (Control Plane, Worker) environment.

<br>

Using the K‑PaaS Container Platform Deployment, you configure a Kubernetes **`federation cluster`** and deploy the K‑PaaS Container Platform Portal environment through various resources, providing dashboards, databases, repositories, and more.

<br>

Refer to the configuration below for the instance environment required for the K‑PaaS Container Platform cluster.

<br>

|Instance Type|Number of Instances|Required|Notes|
|---|---|---|---|
|Install|1||Install instance configuration is recommended<br>Control Can be replaced with Plane instances|
|Control Plane|1 or more|O|1 test environment<br>3 or more production environments|
|Worker|1 to 3 or more|O|NFS storage 1 or more when using<br>3 or more when using Rook-Ceph storage|
|Storage|1||Required when using NFS storage|
|LoadBalancer|1~2||Required when configuring Private Cloud HA Control Plane|

<br>

The system architecture for each deployment type is as follows.

<br>

<details>
<summary>System Architecture</summary>
<br>

***[ Federation Host Cluster and Member Cluster Configuration ]***

![image 008]

</details>

<br><br>

### <div id='1.4'> 1.4. References
> https://kubespray.io<br>
> https://github.com/kubernetes-sigs/kubespray

<br><br>

## <div id='2'> 2. K‑PaaS Container Platform Cluster Installation

### <div id='2.1'> 2.1. Prerequisite
This document recommends the following when deploying a K‑PaaS Container Platform cluster.

- One or more machines running a deb/rpm-compatible Linux OS (Ubuntu)
- At least 8 GB RAM per machine (minimum) or 16 GB (recommended)
- At least 2 CPUs per control-plane node machine (minimum) or 4 CPUs (recommended)
- Full network connectivity between all systems in the cluster

<br>

Prerequisites for installing the K‑PaaS Container Platform cluster are described below.

<br><br>

### <div id='2.1.1'> 2.1.1. OS (***`Required Check`***)
The OS environment information required to install the K‑PaaS Container Platform cluster is as follows.

<br>

|Supported OS|Version|
|---|---|
|Ubuntu|22.04|
|Ubuntu|24.04|

<br>

When creating instances based on Ubuntu 22.04 for each CSP, perform the following.

<br>

<details>
<summary>KT Cloud</summary>
<br>
When creating an instance using the Ubuntu 22.04 image, the **Last password change** date is set to 2023-07-04, so after some time SSH connections may attempt password-based login.

<br>

Because the ubuntu account does not have a password set by default, you may not be able to access the instance; update it to the current date.

```
$ sudo chage -d 20xx-xx-xx ubuntu
```

<br>

</details>

<br>

<details>
<summary>Naver Cloud</summary>
<br>
When creating an instance with the main account, **root** is used; when creating an instance with a sub-account, **ncloud** is used by default.
For the container platform, the default is to use the **ubuntu** account, so follow the procedure to create the ubuntu account.

<br>

On the Install instance or the first Control Plane node instance, perform the following steps.

<br>

Generate an RSA key pair.

```
$ ssh-keygen -t rsa -m PEM -N '' -f $HOME/.ssh/id_rsa
Generating public/private rsa key pair.
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:odWdv3PDIEpkPuoS53yM0hrsEQZL4mHvM0KwLK2uC57 ubuntu@cp-master
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|         . . .   |
|.+ o    = . o    |
|++= o  * .   .   |
|oo+o .. S . . .  |
|.+..o  o o . + . |
|o +o O. .     *  |
|=o.o=o*        o |
|E++o ++          |
+----[SHA256]-----+
```

<br>

Copy the private key to your local PC environment to access the instance.

```
## Copy the output public key

$ cat ~/.ssh/id_rsa
```

<br>

View the public key and copy it to the clipboard.

```
## Copy the displayed public key

$ cat ~/.ssh/id_rsa.pub
```

<br>


Perform the following steps on all instances.

<br>

```
$ sudo useradd -m -s /bin/bash ubuntu
$ echo "ubuntu ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers

$ sudo mkdir -p /home/ubuntu/.ssh
$ echo "{{ public key }}" | sudo tee -a /home/ubuntu/.ssh/authorized_keys
$ sudo chown -R ubuntu:ubuntu /home/ubuntu/.ssh
```

<br>

After completing these steps, connect to the instances using the created ***`ubuntu account`*** and continue the installation.

<br>

</details>

<br><br>

### <div id='2.1.2'> 2.1.2. Python Packages (Reference)
The key Python packages required to install the K‑PaaS Container Platform cluster are as follows.

<br>

|Python Package|Version|
|---|---|
|ansible|10.7.0|
|cryptography|46.0.2|
|jmespath|1.0.1|
|netaddr|1.3.0|
|ansible-core|~=2.17.7|
|typing-extensions|>=4.13.2|
|cffi|>=2.0.0|
|PyYAML|>=5.1|
|jinja2|>=3.0.0|
|resolvelib|<1.1.0,>=0.5.3|

<br><br>

### <div id='2.1.3'> 2.1.3. Key Software (Reference)
The key software required to install the K‑PaaS Container Platform cluster is as follows.

<br>

|Major Software|Version|
|---|---|
|Kubespray|2.29.0|
|Kubernetes Native|1.33.5|
|CRI-O|1.33.5|
|Calico|3.30.3|
|MetalLB|0.14.9|
|Ingress Nginx Controller|1.13.3|
|Helm|3.17.4-1|
|Istio|1.27.3|
|Podman|3.4.7|
|OpenTofu|1.10.6|
|nfs-subdir-external-provisioner|4.0.18|
|Rook Ceph|1.18.5|
|Kubeflow|1.7.0|
|Kyverno|1.15.2|
|OpenBAO|2.2.0|
|Karmada|1.15.2|

<br><br>

### <div id='2.1.4'> 2.1.4. Firewall (***`Required Configuration`***)
The firewall requirements for installing the K‑PaaS Container Platform cluster are as follows.

<br>

Control Plane nodes

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|2379-2380|etcd server client API|
|TCP|6443|Kubernetes API Server|
|TCP|10250|Kubelet API|
|TCP|10251|kube-scheduler|
|TCP|10252|kube-controller-manager|
|TCP|10255|Read-Only Kubelet API|
|TCP|30000-32767| NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Ingress Nginx Controller|
|TCP|443|Ingress Nginx Controller|

<br>

Worker nodes

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|10250|Kubelet API|
|TCP|10255|Read-Only Kubelet API|
|TCP|30000-32767| NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Ingress Nginx Controller|
|TCP|443|Ingress Nginx Controller|

<br><br>

### <div id='2.1.5'> 2.1.5. Storage (***`Required Configuration`***)
The storage information required to install the K‑PaaS Container Platform cluster is as follows.

<br>

|Supported Storage|Notes|
|---|---|
|NFS Server|Create a new instance|
|Rook-Ceph|Add volume to an existing instance|

<br>

When configuring NFS storage<br>
[NFS Server Installation Guide](../nfs-server-install-guide.md)

<br>

When configuring Rook-Ceph storage<br>
In addition to the root volume, you must ***`pre-allocate an additional volume to each Worker node in advance`***.

<br><br>

### <div id='2.1.6'> 2.1.6. Ingress Nginx and Istio Gateway Service Configuration (***`Required Configuration`***)
The Ingress Nginx and Istio Gateway service configuration required for K‑PaaS Container Platform services is as follows.

<br>

Host cluster

|Service|Description|Notes|
|---|---|---|
|Ingress Nginx Controller|Service for exposing K-PaaS container platform services to external access through Ingress|***`1 interface or 1 load balancer`*** must be created
Public IP assignment is required|

<br>

Member cluster

|서비스|Description|Notes|
|---|---|---|
|Istio Ingress Gateway<br>(Includes Eastwest Gateway capability)|A gateway service for external service communication and multi-cloud inter-service communication|For each cloud, you must create either one network interface or one load balancer.<br>A Public IP assignment is required.|

<br>

In the K‑PaaS Container Platform cluster, MetalLB assigns External IPs for LoadBalancer-type services.<br>
To enable external communication and services using that External IP, configure it by selecting ***`one of the following two methods`***.

<br>

|방식|Description|Notes|
|---|---|---|
|Add Interface|Add a new interface with a Public IP assigned to one Control Plane node|Only the cost of using the Public IP will be incurred<br>If the node fails in an HA configuration, external access to the Ingress Nginx service will not be available|
|Create Load Balancer|Create a load balancer service with a Public IP assigned|Additional cost will incur for the load balancer service<br>In the case of some Control Plane node failures in an HA configuration, the Ingress Nginx service will still be functional<br>Recommended for production environments|

<br>

> In the K‑PaaS Container Platform cluster v1.7.0 release, when installing the load balancer controller, it ***`automatically creates and assigns a load balancer service`*** and MetalLB is not installed. (Applies to NHN and Naver Cloud environments)

<br>

|방식|Description|Notes|
|---|---|---|
|Load Balancer Controller|Automatically creates a load balancer service with an assigned Public IP|***`Supported on NHN and Naver Cloud`***<br>***`MetalLB is not installed`***<br>Additional costs apply for the load balancer service<br>In an HA setup, the Ingress Nginx service remains available even if some Control Plane nodes fail<br>운Recommended for production environments|

<br><br>

### <div id='2.1.6.1'> 2.1.6.1. Additional Interface for Control Plane Nodes
The method for adding an interface to Control Plane nodes differs by CSP as follows.

<br>

<details>
<summary>Add Interface (NHN Cloud)</summary>
<br>
1. Go to the <b><code>Network > Network Interface</code></b> menu and click the "Create Network Interface" button.
<br><br>

![image if nhn 001]

<br><br>
2. Enter a name, select the same VPC and subnet as the Control Plane node, and click "Confirm".

![image if nhn 002]

<br><br>
3. Select the interface you created and click "Manage Floating IP".

![image if nhn 003]

<br><br>
4. Create a Floating IP or select an existing Floating IP, then click "Connect".

![image if nhn 004]

<br><br>
5. Go to <b><code>Compute > Instance</code></b>, select the Control Plane node (in an HA Control Plane setup, select the Control Plane node where you will add the interface), then click "Stop Instance".

![image if nhn 005]

<br><br>
6. In the Network tab at the bottom, click "Add Network Interface Connection".

![image if nhn 006]

<br><br>
7. Choose to specify an existing network interface, select the interface you created, and click "Confirm".

![image if nhn 007]

<br><br>
8. Click "Start Instance".

![image if nhn 008]

<br>

</details>

<br>

<details>
<summary>Add Interface (KT Cloud)</summary>
<br>
1. In <b><code>Servers > Virtual IP</code></b>, click "Create Virtual IP".

<br><br>
2. Select the same Zone and Tier as the Control Plane node, enter a name, then click "Create".

![image if kt 001]

<br><br>
3. Select the Virtual IP and click "Connect" to attach it to the Control Plane VM.

![image if kt 002]

<br><br>
4. In <b><code>Servers > Networking</code></b>, click "Create IP" to create a Public IP in the same Zone as the Control Plane node.

![image if kt 003]

<br><br>
5. Select the Public IP you created and click "Connection Settings". Select the Virtual IP and configure port forwarding for ports 80 and 443 (Ingress Nginx Controller service) or ports 80, 443, 15021, 15443, 15012, and 15017 (Istio Ingress Gateway service).

![image if kt 004]

![image if kt 005]

<br><br>
6. Click "Firewall Settings" and configure the firewall using the registered connection settings. (Set Source CIDR and Destination CIDR according to your environment.)

![image if kt 006]

<br><br>
7. Run the following command on the Control Plane node. (In an HA Control Plane setup, run it on the Control Plane node connected to the Virtual IP.)

```
## Check Interface
## Example interface name : eth0, ens3 ...

$ sudo ifconfig
```

```
## Add Interface
## Example : sudo ifconfig ens3:1 192.168.0.10 up

$ sudo ifconfig {{ interface name }}:1 {{ Virtual IP (Private) }} up
```

<br>

</details>

<br>

<details>
<summary>Add Interface (Naver Cloud)</summary>
<br>
Due to policy, Naver Cloud does not allow assigning two Public IPs to a single instance.<br>
Therefore, you must create and configure the Naver Cloud load balancer service described below.

<br>

</details>

<br><br>

### <div id='2.1.6.2'> 2.1.6.2. Cloud Load Balancer Service
The method for creating a load balancer service differs by CSP as follows.

> The example for creating a cloud load balancer service is written based on the Ingress and Eastwest Gateway ports among the Istio Gateway services.<br>

<br>

<details>
<summary>Create Load Balancer Service (NHN Cloud)</summary>
<br>

Before creating the load balancer service, ***`create only the Public IP first`***, and then ***`after the cluster deployment is complete`***, ***`create the load balancer service`***.

<br>
1. In <b><code>Network > Floating IP</code></b>, click "Create Floating IP" to create a Public IP.
<br><br>

![image lb nhn 001]

<br>

Because load balancer settings can be configured only after verifying the service ports and NodePort information assigned during creation, perform the load balancer creation and configuration process below after step [2.6. Verify the K‑PaaS Container Platform Cluster Installation](#2.6).

<br><br>
2. In <b><code>Network > Load Balancer > Management</code></b>, click "Create Load Balancer". In the mode selection pop-up, choose "L4 Routing".

![image lb nhn 002]

<br><br>
3. Enter the configuration information.

|Item|Description|Notes|
|---|---|---|
|Name|Enter the name of the load balancer||
|VPC|Select the same network VPC as the Control Plane node||
|Subnet|Select the same Public subnet as the Control Plane node	||

<br>

![image lb nhn 003]

<br><br>
4. Enter the listener and member group information for port 80.

Listener

|Item|Description|Notes|
|---|---|---|
|Name|Enter the listener name||
|Protocol|Select HTTP||
|Load Balancer Port|Enter 80||

<br>

Member group

|Item|Description|Notes|
|---|---|---|
|Name|Enter the member group name||
|Protocol|Select HTTP||
|Port|Enter the node port value assigned to the 80 port of the Ingress Nginx service||
|Health Check Protocol|Select TCP||
|Health Check Port|Enter a port that can check the instance status, such as the SSH port (TCP 22)|
|Member List|Add all node instances||

<br>

> Check the Ingress Nginx service ports<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

> Check the Istio Gateway service ports<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb nhn 004]

<br><br>
5. Click "Add Listener" and enter the listener and member group information for port 443.

Listener

|Item|Description|Notes|
|---|---|---|
|Name|Enter the listener name||
|Protocol|Select HTTPS||
|Load Balancer Port|Enter 443||

<br>

Member group

|Item|Description|Notes|
|---|---|---|
|Name|Enter the member group name||
|Protocol|Select HTTPS||
|Port|Enter the node port value assigned to the 443 port of the Ingress Nginx service||
|Health Check Protocol|Select TCP||
|Health Check Port|Enter a port that can check the instance status, such as the SSH port (TCP 22)|
|Member List|Add all node instances||

<br>

![image lb nhn 005]

<br><br>
6. (For the Istio IngressGateway service) Add listeners/member groups for ports 15021, 15443, 15012, and 15017 the same way as in step 5.

<br><br>
7. Click "Create Load Balancer" at the bottom.

<br><br>
8. Select the load balancer you created and click "Manage Floating IP".

<br><br>
9. Select the existing Floating IP (created in step 1) and the interface you created, then click "Connect".

![image lb nhn 006]

<br>

</details>

<br>

<details>
<summary>Create Load Balancer Service (KT Cloud)</summary>
<br>

Before creating the load balancer service, ***`create only the Public IP first`***, and then ***`after the cluster deployment is complete`***, ***`create the load balancer service`***.

<br>
1. In <b><code>Servers > Networking</code></b>, click "Create IP" to create a Public IP.
<br><br>

![image if kt 003]

<br>

Because load balancer settings can be configured only after verifying the service ports and NodePort information assigned during creation, perform the load balancer creation and configuration process below after step [2.6. Verify the K‑PaaS Container Platform Cluster Installation](#2.6).

<br><br>
2. In <b><code>Load Balancer > Load Balancer Management</code></b>, click "Create Load Balancer".

![image lb kt 001]

<br><br>
3. Enter the load balancer information and click "Confirm".

|Item|Description|Notes|
|---|---|---|
|Zone|Select the same network Zone as the Control Plane node|Same concept as VPC|
|Tier|Select the same network Tier as the Control Plane node|Same concept as subnet|
|Name|Enter the name of the load balancer||
|Service IP / Port|Assign new IP, enter port 80||
|Service Type|Select HTTP||

<br>

![image lb kt 002]

<br><br>
4. Click "Create Load Balancer".

<br><br>
5. Enter the load balancer information and click "Confirm".

|Item|Description|Notes|
|---|---|---|
|Zone|Select the same network Zone as the Control Plane node|Same concept as VPC|
|Tier|Select the same network Tier as the Control Plane node|Same concept as subnet|
|Name|Enter the name of the load balancer||
|Service IP / Port|Select the Private IP of the created load balancer, enter port 443||
|Service Type|Select HTTPS (Bridge)||


<br>

![image lb kt 003]

<br><br>
6. (For the Istio IngressGateway service) Add additional load balancers for ports 15021, 15443, 15012, and 15017 the same way as in step 5.

<br><br>
7. Select the load balancers you created and click "VM Connect/Disconnect". (Do this for both the 80 and 443 port load balancers.)

<br><br>
8. Enter the information and click "Add".

|Item|Description|Notes|
|---|---|---|
|Tier|Select the same network Tier as the Control Plane node||
|VM|Select all nodes of the cluster sequentially||
|Public Port|Enter the node port information assigned to ports 80 and 443||

<br>

> Check the Ingress Nginx service ports<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

> Check the Istio Gateway service ports<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
9. Go to <b><code>Servers > Networking</code></b>, select the Public IP created in step 1, click "Static NAT", and select one of the load balancers you created.

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
10. Click "Firewall Settings" to register firewall rules for the Static NAT configuration.

![image lb kt 008]

<br>

</details>

<br>

<details>
<summary>Create Load Balancer Service (Naver Cloud)</summary>
<br>

Before creating the load balancer service, ***`create only the Public IP first`***, and then ***`after the cluster deployment is complete`***, ***`create the load balancer service`***.

<br>
1. In <b><code>Server > Public IP</code></b>, click "Request Public IP" to create an unassigned Public IP.
<br><br>

![image lb naver 001]

<br>

Because load balancer settings can be configured only after verifying the service ports and NodePort information assigned during creation, perform the load balancer creation and configuration process below after step [2.6. Verify the K‑PaaS Container Platform Cluster Installation](#2.6).

<br><br>
2. In <b><code>Load Balancer > Target Group</code></b>, click "Create Target Group".

![image lb naver 002]

<br><br>
3. Enter the Target Group information below and click "Next".

|Item|Description|Notes|
|---|---|---|
|Target Group Name|Enter the target group name||
|Target Type|Select VPC Server||
|VPC|Select the same network VPC as the Control Plane node||
|Protocol|Select PROXY_TCP||
|Port|Enter the node port information assigned to port 80||

<br>

> Check the Ingress Nginx service ports<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

> Check the Istio Gateway service ports<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb naver 003]

<br><br>
4. Enter the Health Check settings below and click "Next".

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter a port that can check the instance status, such as the SSH port (TCP 22)|

<br>

![image lb naver 004]

<br><br>
5. Select all nodes, include them in the targets, and create the Target Group.

![image lb naver 005]

![image lb naver 006]

<br><br>
6. Repeat steps 2–5 to create an additional Target Group for TCP port 443.

<br><br>
7. (For the Istio IngressGateway service) Repeat steps 2–5 to create additional Target Groups for TCP ports 443, 15021, 15443, 15012, and 15017.

<br><br>
8. In <b><code>Load Balancer > Load Balancer</code></b>, click "Create Load Balancer", then click "Network Proxy Load Balancer".

![image lb naver 007]

<br><br>
9. Enter the load balancer information below and click "Next".

|Item|Description|Notes|
|---|---|---|
|Load Balancer Name|Enter the name of the load balancer||
|Network|Select Public||
|Target VPC|Select the same network VPC as the Control Plane node||
|Select Subnet|Select the load balancer subnet||
|Public IP|Select the previously created Public IP (from step 1)||

<br>

![image lb naver 008]

<br><br>  
10. Enter the listener settings and click "Add" and then "Next".

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Load Balancer Port|Enter port 80||

<br>

![image lb naver 009]

<br><br>
11. Select the Target Group and create the load balancer.

![image lb naver 010]

<br><br>
12. Select the load balancer you created, click "Change Listener Settings", then click "Add Listener".

![image lb naver 011]

<br><br>
13. Enter the listener settings for port 443 and add the listener.

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter port 443||
|Target Group|Select the Target Group set for port 443||

<br><br>
14. (For the Istio IngressGateway service) Enter the listener settings for ports 15021, 15443, 15012, and 15017 and add the listeners.

<br>

![image lb naver 012]

![image lb naver 013]

<br>

</details>

<br><br>

### <div id='2.1.7'> 2.1.7. HA Control Plane Load Balancer (***`Required Configuration for HA Control Plane`***)
> If you are not using an HA Control Plane configuration, skip the load balancer configuration steps and proceed to the next step.<br> [2.2. Create and Distribute SSH Keys](#2.2)

<br>

If you configure the K‑PaaS Container Platform cluster as an HA Control Plane, the required load balancer information is as follows.

<br>

|Cloud|Load Balancer|Notes|
|---|---|---|
|Public|Use the load balancer service provided by each CSP|
|Private|Configure load balancer instances with Keepalived, HAProxy|

<br><br>

### <div id='2.1.7.1'> 2.1.7.1. Cloud Load Balancer Service
For public cloud environments, create the load balancer service provided by each CSP.

<br>

To create a load balancer service for each CSP, refer to [2.1.6.2. Cloud Load Balancer Service](#2.1.6.2), and update the following information during configuration.

<br>

<details>
<summary>NHN Cloud</summary>
<br>

Listener

|Item|Changes|Notes|
|---|---|---|
|Protocol|Select TCP||
|Load Balancer Port|Enter 6443||

<br>

Member group

|Item|Changes|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter 6443||
|Member List|Add Control Plane instances||


<br>

</details>

<br>

<details>
<summary>KT Cloud</summary>
<br>

Load balancer information

|Item|Changes|Notes|
|---|---|---|
|Service IP / Port|Assign new IP, enter 6443||
|Service Type|Select TCP||

<br>

Load balancer VM connection information

|Item|Changes|Notes|
|---|---|---|
|VM|Select Control Plane nodes sequentially||
|Public Port|Enter 6443||

<br>

</details>

<br>

<details>
<summary>Naver Cloud</summary>
<br>

Target Group information

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||
|Apply Target|Add Control Plane instances||

<br>

Load balancer listener information

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||

<br>

</details>

<br><br>

### <div id='2.1.7.2'> 2.1.7.2. HAProxy
For private cloud environments, configure the load balancer using Keepalived and HAProxy.

|Environment|Configuration|Notes|
|---|---|---|
|Test Environment|Single Keepalived, HAProxy configuration|Use the private IP of the load balancer instance as VIP|
|Production Environment|HA configuration with Keepalived, HAProxy|Create a separate interface, assign it to the instance, and use it as VIP|


<br>

To configure the load balancer in a private cloud environment, follow the procedure below.

For load balancer HA configuration, set up the VIP as follows.
- Create a new interface
- Assign a Public IP to the newly created interface
- Attach the newly created interface to the load balancer instance

<br>

<details>
<summary>Install Keepalived and HAProxy</summary>
<br>

For load balancer HA configuration, perform the Keepalived and HAProxy installation steps on both load balancer instances.

<br>

Install Keepalived on the load balancer instances as follows.
```
$ sudo su -

# apt-get update

# apt-get install -y keepalived

# echo 'net.ipv4.ip_nonlocal_bind=1' >> /etc/sysctl.conf
# echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
# sysctl -p
```

<br>

Configure Keepalived.
```
# vi /etc/keepalived/keepalived.conf
```

|Environment Variable|Description|Notes|
|---|---|---|
|STATE|Set "MASTER" for the first load balancer instance, "BACKUP" for the second load balancer instance|If configuring a single load balancer instance, set only MASTER|
|INTERFACE_NAME|Check the interface name for each instance by typing ifconfig in the shell||
|VIP|The Private IP of the interface assigned to the load balancer instance|For a test environment, specify the private IP of the load balancer instance.<br>For a production environment, specify the separately configured VIP|

```
vrrp_instance VI_1 {
  state {STATE}
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
```

<br>

Start the Keepalived service.
```
# systemctl start keepalived
# systemctl enable keepalived
```

<br>

Install HAProxy on the load balancer instances as follows.
```
# apt-get install -y haproxy
```

<br>

Configure HAProxy. Add the following content to the end of the haproxy.cfg file.
```
# vi /etc/haproxy/haproxy.cfg
```

|Environment Variable|Description|Notes|
|---|---|---|
|VIP|The Private IP of the interface assigned to the load balancer instance|For a test environment, specify the private IP of the load balancer instance.<br>For a production environment, specify the separately configured VIP|
|MASTER_NODE_IP{n}|The private IP of each Control Plane node|The example below shows the configuration with 3 Control Plane nodes. Add more entries depending on the number of Control Plane nodes|


```
...
listen kubernetes-apiserver-https
  bind {{VIP}}:6443
  mode tcp
  option log-health-checks
  timeout client 3h
  timeout server 3h
  server master1 {{MASTER_NODE_IP1}}:6443 check check-ssl verify none inter 10000
  server master2 {{MASTER_NODE_IP2}}:6443 check check-ssl verify none inter 10000
  server master3 {{MASTER_NODE_IP3}}:6443 check check-ssl verify none inter 10000
  balance roundrobin
```

<br>

Restart the HAProxy service.
```
# systemctl restart haproxy
```

<br>

</details>

<br><br>

### <div id='2.2'> 2.2. Create and Distribute SSH Keys
To install the K‑PaaS Container Platform cluster, the SSH key must be copied to all servers in the inventory.<br>
In this document (K‑PaaS Container Platform Cluster Installation Guide), SSH access is configured using an RSA public key.

After creating and distributing the SSH key, all remaining installation steps are performed on the ***`Install instance or the Control Plane node where you will run the installation`***.

<br>

Generate the RSA key pair on the ***`Install instance or the Control Plane node where you will run the installation`***.
```
$ ssh-keygen -t rsa -m PEM -N '' -f $HOME/.ssh/id_rsa
Generating public/private rsa key pair.
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:odWdv3PDIEpkPuoS53yM0hrsEQZL4mHvM0KwLK2uC57 ubuntu@cp-master
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|         . . .   |
|.+ o    = . o    |
|++= o  * .   .   |
|oo+o .. S . . .  |
|.+..o  o o . + . |
|o +o O. .     *  |
|=o.o=o*        o |
|E++o ++          |
+----[SHA256]-----+
```

<br>

Copy the public key to the ***`Install, Control Plane, and Worker nodes`*** you will use.
```
## Copy the output public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

Append the public key to the end of the authorized_keys file (add below the existing content) on the ***`Install, Control Plane, and Worker nodes`*** you will use.
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLzwTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTDvRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br><br>

### <div id='2.3'> 2.3. Download K‑PaaS Container Platform Cluster Deployment

> From 2.3 onward, proceed only on the ***`Install instance or the Control Plane node where you will run the installation`***.

<br>

Download the Deployment required for installing the K‑PaaS Container Platform cluster, move to the installation working directory, and proceed with the installation.

> K-PaaS container platform cluster Deployment Download URL : https://github.com/k-paas/cp-deployment

<br>

Use the git clone command in your HOME directory to download the K‑PaaS Container Platform cluster Deployment.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.7.x
```

<br><br>

### <div id='2.4'> 2.4. Prepare for K‑PaaS Container Platform Cluster Installation
Predefine the environment variables required for installing the K‑PaaS Container Platform cluster, then run the installation via a shell script.

Move to the K‑PaaS Container Platform cluster installation path.
```
$ cd ~/cp-deployment/federation
```

<br>

Set the number of federation member clusters and create the environment variable script file.
```
$ vi create-vars.sh
```

```
...
CLUSTER_CNT={Cluster Count}
...
```

```
$ ./create-vars.sh
```

<br>

Enter the environment variable values required to install the federation host cluster.
```
$ vi host-cp-cluster-vars.sh
```

<br>

Control Plane

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_CONTROL_HOSTS|Number of Control Plane nodes||
|MASTER1_NODE_HOSTNAME|Hostname of the first Control Plane node||
|MASTER1_NODE_USER|User Account of the first Control Plane node|Configured for use as a Bastion server<br>default : **`ubuntu`**|
|MASTER1_NODE_PRIVATE_IP|Private IP of the first Control Plane node||
|MASTER1_NODE_PUBLIC_IP|Public IP of the first Control Plane node|Only the first Control Plane node requires the Public IP information|
|MASTER{n}_NODE_HOSTNAME|Hostname of the nth Control Plane node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|MASTER{n}_NODE_PRIVATE_IP|Private IP of the nth Control Plane node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|ETCD_TYPE|ETCD deployment method<br>external: ETCD configured on a separate node<br>stacked: ETCD configured on Control Plane nodes|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|LOADBALANCER_DOMAIN|VIP or Domain information of the pre-configured load balancer|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|ETCD1_NODE_HOSTNAME|Hostname of the first ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external|
|ETCD1_NODE_PRIVATE_IP|Private IP of the first ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external|
|ETCD{n}_NODE_HOSTNAME|Hostname of the nth ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|ETCD{n}_NODE_PRIVATE_IP|Private IP of the nth ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|

<br>

Worker

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_WORKER_HOSTS|Number of Worker nodes||
|WORKER1_NODE_HOSTNAME|Hostname of the first Worker node||
|WORKER1_NODE_PRIVATE_IP|Private IP of the first Worker node||
|WORKER{n}_NODE_HOSTNAME|Hostname of the nth Worker node|Set as many as the value of **`KUBE_WORKER_HOSTS`**|
|WORKER{n}_NODE_PRIVATE_IP|Private IP of the nth Worker node|Set as many as the value of **`KUBE_WORKER_HOSTS`**|


<br>

Storage

|Environment Variable|Description|Remarks|
|---|---|---|
|STORAGE_TYPE|Storage type<br>nfs: NFS storage<br>rook-ceph: Rook Ceph storage||
|NFS_SERVER_PRIVATE_IP|Private IP of the NFS server instance|Set if **`STORAGE_TYPE`** is nfs|

<br>

LoadBalancer Service

|Environment Variable|Description|Remarks|
|---|---|---|
|METALLB_IP_RANGE|Private IP range to be used by MetalLB|Set the same network subnet range as the Control Plane nodes|
|INGRESS_NGINX_IP|Private IP (if interface) or Public IP (if load balancer service) to be used for Ingress Nginx Controller Service via MetalLB|Set so that it does not overlap with **`METALLB_IP_RANGE`**|

<br>

Kyverno

|Environment Variable|Description|Remarks|
|---|---|---|
|INSTALL_KYVERNO|Whether to deploy Kyverno policies||

<br>

Controller

|Environment Variable|Description|Remarks|
|---|---|---|
|CSP_TYPE|CSP information|Supports **NHN, NAVER** only|
|NHN_USERNAME|NHN Cloud account|Automatically deleted after cluster installation|
|NHN_PASSWORD|NHN Cloud password|Automatically deleted after cluster installation|
|NHN_TENANT_ID|NHN Cloud Tenant ID|Automatically deleted after cluster installation|
|NHN_VIP_SUBNET_ID|Subnet ID used to create the NHN Cloud Load Balancer|Automatically deleted after cluster installation|
|NHN_API_BASE_URL|NHN Cloud API URL|Default: **https://kr1-api-network-infrastructure.nhncloudservice.com**|
|NAVER_CLOUD_API_KEY|NAVER Cloud API Key|Automatically deleted after cluster installation|
|NAVER_CLOUD_API_SECRET|NAVER Cloud API Secret|Automatically deleted after cluster installation|
|NAVER_CLOUD_REGION|NAVER Cloud region|Default: **KR**|
|NAVER_CLOUD_VPC_NO|NAVER Cloud VPC No.|Automatically deleted after cluster installation|
|NAVER_CLOUD_SUBNET_NO|NAVER Cloud Subnet No.|Automatically deleted after cluster installation|
<br>

```
#!/bin/bash

# --------------------------------------------------------------------
# Control Plane Node Configuration
# --------------------------------------------------------------------

# Number of Control Plane (Master) Nodes (e.g., 1, 3, 5 ...)
KUBE_CONTROL_HOSTS=

# Control Plane (Master) Node Information
# Configure according to the number of Control Plane nodes
MASTER1_NODE_HOSTNAME=
MASTER1_NODE_USER=ubuntu
MASTER1_NODE_PRIVATE_IP=
MASTER1_NODE_PUBLIC_IP=
MASTER2_NODE_HOSTNAME=
MASTER2_NODE_PRIVATE_IP=
MASTER3_NODE_HOSTNAME=
MASTER3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# LoadBalancer Configuration
# --------------------------------------------------------------------

# Required when using two or more Control Plane nodes
# External Load Balancer domain or IP
LOADBALANCER_DOMAIN=

# --------------------------------------------------------------------
# ETCD Node Configuration
# --------------------------------------------------------------------

# ETCD deployment mode
# Required when two or more Control Plane nodes are used (e.g., external, stacked)
# - external : Deploy ETCD as separate nodes
# - stacked  : ETCD runs on each Control Plane node
ETCD_TYPE=

# Required when ETCD_TYPE=external
# Must match the number of Control Plane nodes
ETCD1_NODE_HOSTNAME=
ETCD1_NODE_PRIVATE_IP=
ETCD2_NODE_HOSTNAME=
ETCD2_NODE_PRIVATE_IP=
ETCD3_NODE_HOSTNAME=
ETCD3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# Worker Node Configuration
# --------------------------------------------------------------------

# Number of Worker nodes
KUBE_WORKER_HOSTS=

# Worker node information
# Configure according to the number of Worker nodes
WORKER1_NODE_HOSTNAME=
WORKER1_NODE_PRIVATE_IP=
WORKER2_NODE_HOSTNAME=
WORKER2_NODE_PRIVATE_IP=
WORKER3_NODE_HOSTNAME=
WORKER3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# Storage Configuration
# --------------------------------------------------------------------

# Storage type (e.g., nfs, rook-ceph)
STORAGE_TYPE=

# Private IP of the NFS server when STORAGE_TYPE='nfs'
NFS_SERVER_PRIVATE_IP=

# --------------------------------------------------------------------
# MetalLB Configuration
# --------------------------------------------------------------------

# MetalLB Address Pool range (e.g., 192.168.0.150-192.168.0.160)
METALLB_IP_RANGE=

# External IP for the Ingress Nginx Controller LoadBalancer Service
# - Interface expansion method: set the interface Private IP
# - LoadBalancer service method: set the LoadBalancer service Public IP
INGRESS_NGINX_IP=

# --------------------------------------------------------------------
# Kyverno Configuration
# --------------------------------------------------------------------

# Whether to install Kyverno (e.g., Y, N)
# - Y : Install Kyverno and automatically apply PSS (Pod Security Standards) and cp-policy (namespace network isolation)
# - N : Do not install
INSTALL_KYVERNO=

# --------------------------------------------------------------------
# CSP LoadBalancer Controller Configuration
# --------------------------------------------------------------------

# CSP type (e.g., NHN, NAVER)
CSP_TYPE=

# NHN Cloud environment variables (required when CSP_TYPE=NHN)
NHN_USERNAME=
NHN_PASSWORD=
NHN_TENANT_ID=
NHN_VIP_SUBNET_ID=
NHN_API_BASE_URL=https://kr1-api-network-infrastructure.nhncloudservice.com

# NAVER Cloud environment variables (required when CSP_TYPE=NAVER)
NAVER_CLOUD_API_KEY=
NAVER_CLOUD_API_SECRET=
NAVER_CLOUD_REGION=KR
NAVER_CLOUD_VPC_NO=
NAVER_CLOUD_SUBNET_NO=
```

<br>

Enter the environment variable values required to install the federation member clusters.
```
## K-PaaS Container Platform cluster Environment Variable
$ vi member-cp-cluster-vars.sh
```

<br>

Differentiate clusters by prefixing environment variables with CLUSTER{n}_.<br>
ex) CLUSTER1_KUBE_CONTROL_HOSTS, CLUSTER2_KUBE_CONTROL_HOSTS, CLUSTER3_KUBE_CONTROL_HOSTS...

Control Plane

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_CONTROL_HOSTS|Number of Control Plane nodes||
|MASTER1_NODE_HOSTNAME|Hostname of the first Control Plane node||
|MASTER1_NODE_USER|User Account of the first Control Plane node|Configured for use as a Bastion server<br>default : **`ubuntu`**|
|MASTER1_NODE_PRIVATE_IP|Private IP of the first Control Plane node||
|MASTER1_NODE_PUBLIC_IP|Public IP of the first Control Plane node|Only the first Control Plane node requires the Public IP information|
|MASTER{n}_NODE_HOSTNAME|Hostname of the nth Control Plane node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|MASTER{n}_NODE_PRIVATE_IP|Private IP of the nth Control Plane node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|ETCD_TYPE|ETCD deployment method<br>external: ETCD configured on a separate node<br>stacked: ETCD configured on Control Plane nodes|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|LOADBALANCER_DOMAIN|VIP or Domain information of the pre-configured load balancer|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|ETCD1_NODE_HOSTNAME|Hostname of the first ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external|
|ETCD1_NODE_PRIVATE_IP|Private IP of the first ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external|
|ETCD{n}_NODE_HOSTNAME|Hostname of the nth ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|
|ETCD{n}_NODE_PRIVATE_IP|Private IP of the nth ETCD node|Set if **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set if **`ETCD_TYPE`** is external<br>Set as many as the value of **`KUBE_CONTROL_HOSTS`**|

<br>

Worker

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_WORKER_HOSTS|Number of Worker nodes||
|WORKER1_NODE_HOSTNAME|Hostname of the first Worker node||
|WORKER1_NODE_PRIVATE_IP|Private IP of the first Worker node||
|WORKER{n}_NODE_HOSTNAME|Hostname of the nth Worker node|Set as many as the value of **`KUBE_WORKER_HOSTS`**|
|WORKER{n}_NODE_PRIVATE_IP|Private IP of the nth Worker node|Set as many as the value of **`KUBE_WORKER_HOSTS`**|

<br>

Storage

|Environment Variable|Description|Remarks|
|---|---|---|
|STORAGE_TYPE|Storage type<br>nfs: NFS storage<br>rook-ceph: Rook Ceph storage||
|NFS_SERVER_PRIVATE_IP|Private IP of the NFS server instance|Set if **`STORAGE_TYPE`** is nfs|

<br>

> In multi-cloud deployments, Istio IngressGateway replaces the functions of the Ingress Nginx Controller. 

LoadBalancer Service

|Environment Variable|Description|Remarks|
|---|---|---|
|METALLB_IP_RANGE|Private IP range to be used by MetalLB|Set to the same network subnet as the Control Plane nodes|
|INGRESS_NGINX_IP|Private IP (for interface) or Public IP (for load balancer service) to be used by Ingress Nginx Controller Service via MetalLB|Ensure it does not overlap with **`METALLB_IP_RANGE`**|

<br>

> Multi-cloud deployment adds configuration for the Istio Gateway service.

Istio Service

|Environment Variable|Description|Remarks|
|---|---|---|
|ISTIO_GATEWAY_PRIVATE_IP|Private IP (for interface) or Public IP (for load balancer service) to be used by Istio Gateway Service via MetalLB|Ensure it does not overlap with **`METALLB_IP_RANGE`**<br>If load balancer service, enter value to overlap with **`ISTIO_GATEWAY_PUBLIC_IP`**|
|ISTIO_GATEWAY_PUBLIC_IP|Public IP assigned to the interface for Istio Gateway Service (interface, load balancer service)|Ensure it does not overlap with **`METALLB_IP_RANGE`**<br>Not required when **`CSP_TYPE`** is set to NHN|

<br>

Controller

|Environment Variable|Description|Remarks|
|---|---|---|
|CSP_TYPE|CSP information|Supports **NHN, NAVER** only|
|NHN_USERNAME|NHN Cloud account|Automatically deleted after cluster installation|
|NHN_PASSWORD|NHN Cloud password|Automatically deleted after cluster installation|
|NHN_TENANT_ID|NHN Cloud Tenant ID|Automatically deleted after cluster installation|
|NHN_VIP_SUBNET_ID|Subnet ID used to create the NHN Cloud Load Balancer|Automatically deleted after cluster installation|
|NHN_API_BASE_URL|NHN Cloud API URL|Default: **https://kr1-api-network-infrastructure.nhncloudservice.com**|
|NAVER_CLOUD_API_KEY|NAVER Cloud API Key|Automatically deleted after cluster installation|
|NAVER_CLOUD_API_SECRET|NAVER Cloud API Secret|Automatically deleted after cluster installation|
|NAVER_CLOUD_REGION|NAVER Cloud region|Default: **KR**|
|NAVER_CLOUD_VPC_NO|NAVER Cloud VPC No.|Automatically deleted after cluster installation|
|NAVER_CLOUD_SUBNET_NO|NAVER Cloud Subnet No.|Automatically deleted after cluster installation|

<br>

```
#!/bin/bash

CLUSTER_CNT=3

######################################################################
# CLUSTER1
######################################################################

# --------------------------------------------------------------------
# Control Plane Node config
# --------------------------------------------------------------------

# Number of Control Plane (Master) Nodes (e.g., 1, 3, 5 ...)
CLUSTER1_KUBE_CONTROL_HOSTS=

# Control Plane (Master) Node Information
# Configure according to the number of Control Plane nodes
CLUSTER1_MASTER1_NODE_HOSTNAME=
CLUSTER1_MASTER1_NODE_USER=ubuntu
CLUSTER1_MASTER1_NODE_PRIVATE_IP=
CLUSTER1_MASTER1_NODE_PUBLIC_IP=
CLUSTER1_MASTER2_NODE_HOSTNAME=
CLUSTER1_MASTER2_NODE_PRIVATE_IP=
CLUSTER1_MASTER3_NODE_HOSTNAME=
CLUSTER1_MASTER3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# LoadBalancer Configuration
# --------------------------------------------------------------------

# Required when using two or more Control Plane nodes
# External Load Balancer domain or IP
CLUSTER1_LOADBALANCER_DOMAIN=

# --------------------------------------------------------------------
# ETCD Node Configuration
# --------------------------------------------------------------------

# ETCD deployment mode
# Required when two or more Control Plane nodes are used (e.g., external, stacked)
# - external : Deploy ETCD as separate nodes
# - stacked  : ETCD runs on each Control Plane node
CLUSTER1_ETCD_TYPE=

# Required when ETCD_TYPE=external
# Must match the number of Control Plane nodes
CLUSTER1_ETCD1_NODE_HOSTNAME=
CLUSTER1_ETCD1_NODE_PRIVATE_IP=
CLUSTER1_ETCD2_NODE_HOSTNAME=
CLUSTER1_ETCD2_NODE_PRIVATE_IP=
CLUSTER1_ETCD3_NODE_HOSTNAME=
CLUSTER1_ETCD3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# Worker Node Configuration
# --------------------------------------------------------------------

# Number of Worker nodes
CLUSTER1_KUBE_WORKER_HOSTS=

# Worker node information
# Configure according to the number of Worker nodes
CLUSTER1_WORKER1_NODE_HOSTNAME=
CLUSTER1_WORKER1_NODE_PRIVATE_IP=
CLUSTER1_WORKER2_NODE_HOSTNAME=
CLUSTER1_WORKER2_NODE_PRIVATE_IP=
CLUSTER1_WORKER3_NODE_HOSTNAME=
CLUSTER1_WORKER3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# Storage Configuration
# --------------------------------------------------------------------

# Storage type (예: nfs, rook-ceph)
CLUSTER1_STORAGE_TYPE=

# Private IP of the NFS server when STORAGE_TYPE='nfs'
CLUSTER1_NFS_SERVER_PRIVATE_IP=

# --------------------------------------------------------------------
# MetalLB Configuration
# --------------------------------------------------------------------

# MetalLB Address Pool range (e.g., 192.168.0.150-192.168.0.160)

CLUSTER1_METALLB_IP_RANGE=

# Ingress Nginx Controller LoadBalancer Service용 External IP
# - Interface expansion method: set the interface Private IP
# - LoadBalancer service method: set the LoadBalancer service Public IP
CLUSTER1_INGRESS_NGINX_IP=

# Istio Gateway LoadBalancer Service External IP
CLUSTER1_ISTIO_GATEWAY_PRIVATE_IP=
CLUSTER1_ISTIO_GATEWAY_PUBLIC_IP=

# --------------------------------------------------------------------
# CSP LoadBalancer Controller Configuration
# --------------------------------------------------------------------

# CSP type (예: NHN, NAVER)
CLUSTER1_CSP_TYPE=

# NHN Cloud environment variables (required when CSP_TYPE=NHN)
CLUSTER1_NHN_USERNAME=
CLUSTER1_NHN_PASSWORD=
CLUSTER1_NHN_TENANT_ID=
CLUSTER1_NHN_VIP_SUBNET_ID=
CLUSTER1_NHN_API_BASE_URL=https://kr1-api-network-infrastructure.nhncloudservice.com

# NAVER Cloud environment variables (required when CSP_TYPE=NAVER)
CLUSTER1_NAVER_CLOUD_API_KEY=
CLUSTER1_NAVER_CLOUD_API_SECRET=
CLUSTER1_NAVER_CLOUD_REGION=KR
CLUSTER1_NAVER_CLOUD_VPC_NO=
CLUSTER1_NAVER_CLOUD_SUBNET_NO=

######################################################################
# CLUSTER2
######################################################################

# --------------------------------------------------------------------
# Control Plane Node Configuration
# --------------------------------------------------------------------

# Number of Control Plane (Master) Nodes (e.g., 1, 3, 5 ...)
CLUSTER2_KUBE_CONTROL_HOSTS=

# Control Plane (Master) Node Information
# Configure according to the number of Control Plane nodes
CLUSTER2_MASTER1_NODE_HOSTNAME=
CLUSTER2_MASTER1_NODE_USER=ubuntu
CLUSTER2_MASTER1_NODE_PRIVATE_IP=
CLUSTER2_MASTER1_NODE_PUBLIC_IP=
CLUSTER2_MASTER2_NODE_HOSTNAME=
CLUSTER2_MASTER2_NODE_PRIVATE_IP=
CLUSTER2_MASTER3_NODE_HOSTNAME=
CLUSTER2_MASTER3_NODE_PRIVATE_IP=

# --------------------------------------------------------------------
# LoadBalancer Configuration
# --------------------------------------------------------------------

# Required when using two or more Control Plane nodes
# External Load Balancer domain or IP
CLUSTER2_LOADBALANCER_DOMAIN=
...
```

<br><br>

### <div id='2.5'> 2.5. K‑PaaS Container Platform Cluster Installation
Using the shell script, sequentially install required packages, set cluster installation environment variables, and install the K‑PaaS Container Platform cluster via an Ansible playbook.

```
$ ./deploy-cp-cluster.sh
```

<br><br>

### <div id='2.6'> 2.6. Verify K‑PaaS Container Platform Cluster Installation
Verify the K‑PaaS Container Platform cluster installation by checking the nodes and the Pods in the kube-system namespace.<br>
Depending on the configuration, the nodes and Pods shown may differ. The following is an example output from a deployment with a single Control Plane configuration. 

```
$ kubectl get nodes --context=cluster-host
NAME                 STATUS   ROLES                  AGE   VERSION
cp-host-master       Ready    control-plane          12m   v1.33.5
cp-host-worker-1     Ready    <none>                 10m   v1.33.5
cp-host-worker-2     Ready    <none>                 10m   v1.33.5
cp-host-worker-3     Ready    <none>                 10m   v1.33.5

$ kubectl get pods -n kube-system --context=cluster-host
NAME                                          READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh       1/1     Running   0             9m22s
calico-node-d8sg6                             1/1     Running   0             9m22s
calico-node-kfvjx                             1/1     Running   0             10m
calico-node-khwdz                             1/1     Running   0             10m
calico-node-nc58v                             1/1     Running   0             10m
coredns-657959df74-td5c2                      1/1     Running   0             8m15s
coredns-657959df74-ztnjj                      1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0             8m9s
kube-apiserver-cp-host-master                 1/1     Running   0             12m
kube-controller-manager-cp-host-master        1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                              1/1     Running   0             10m
kube-proxy-kkvhk                              1/1     Running   0             10m
kube-proxy-nfttc                              1/1     Running   0             10m
kube-proxy-znfgk                              1/1     Running   0             10m
kube-scheduler-cp-host-master                 1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0             7m57s
nginx-proxy-cp-worker-1                       1/1     Running   0             8m8s
nginx-proxy-cp-worker-2                       1/1     Running   0             8m8s
nginx-proxy-cp-worker-3                       1/1     Running   0             8m8s
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7grn                            1/1     Running   0             8m8s
```

<br>

```
$ kubectl get nodes --context=cluster1
NAME                       STATUS   ROLES                  AGE   VERSION
cp-mem-cluster1-master     Ready    control-plane          12m   v1.33.5
cp-mem-cluster1-worker-1   Ready    <none>                 10m   v1.33.5
cp-mem-cluster1-worker-2   Ready    <none>                 10m   v1.33.5
cp-mem-cluster1-worker-3   Ready    <none>                 10m   v1.33.5

$ kubectl get pods -n kube-system --context=cluster1
NAME                                             READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh          1/1     Running   0             9m22s
calico-node-d8sg6                                1/1     Running   0             9m22s
calico-node-kfvjx                                1/1     Running   0             10m
calico-node-khwdz                                1/1     Running   0             10m
calico-node-nc58v                                1/1     Running   0             10m
coredns-657959df74-td5c2                         1/1     Running   0             8m15s
coredns-657959df74-ztnjj                         1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                   1/1     Running   0             8m9s
kube-apiserver-cp-mem-cluster1-master            1/1     Running   0             12m
kube-controller-manager-cp-mem-cluster1-master   1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                                 1/1     Running   0             10m
kube-proxy-kkvhk                                 1/1     Running   0             10m
kube-proxy-nfttc                                 1/1     Running   0             10m
kube-proxy-znfgk                                 1/1     Running   0             10m
kube-scheduler-cp-mem-cluster1-master            1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps                  2/2     Running   0             7m57s
nginx-proxy-cp-cluster1-worker-1                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster1-worker-2                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster1-worker-3                 1/1     Running   0             8m8s
nodelocaldns-556gb                               1/1     Running   0             8m8s
nodelocaldns-8dpnt                               1/1     Running   0             8m8s
nodelocaldns-pvl6z                               1/1     Running   0             8m8s
nodelocaldns-x7grn                               1/1     Running   0             8m8s
```

<br>

```
$ kubectl get nodes --context=cluster2
NAME                       STATUS   ROLES                  AGE   VERSION
cp-mem-cluster2-master     Ready    control-plane          12m   v1.33.5
cp-mem-cluster2-worker-1   Ready    <none>                 10m   v1.33.5
cp-mem-cluster2-worker-2   Ready    <none>                 10m   v1.33.5
cp-mem-cluster2-worker-3   Ready    <none>                 10m   v1.33.5

$ kubectl get pods -n kube-system --context=cluster2
NAME                                             READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh          1/1     Running   0             9m22s
calico-node-d8sg6                                1/1     Running   0             9m22s
calico-node-kfvjx                                1/1     Running   0             10m
calico-node-khwdz                                1/1     Running   0             10m
calico-node-nc58v                                1/1     Running   0             10m
coredns-657959df74-td5c2                         1/1     Running   0             8m15s
coredns-657959df74-ztnjj                         1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                   1/1     Running   0             8m9s
kube-apiserver-cp-mem-cluster2-master            1/1     Running   0             12m
kube-controller-manager-cp-mem-cluster2-master   1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                                 1/1     Running   0             10m
kube-proxy-kkvhk                                 1/1     Running   0             10m
kube-proxy-nfttc                                 1/1     Running   0             10m
kube-proxy-znfgk                                 1/1     Running   0             10m
kube-scheduler-cp-mem-cluster2-master            1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps                  2/2     Running   0             7m57s
nginx-proxy-cp-cluster2-worker-1                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster2-worker-2                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster2-worker-3                 1/1     Running   0             8m8s
nodelocaldns-556gb                               1/1     Running   0             8m8s
nodelocaldns-8dpnt                               1/1     Running   0             8m8s
nodelocaldns-pvl6z                               1/1     Running   0             8m8s
nodelocaldns-x7grn                               1/1     Running   0             8m8s
```

<br>

```
$ kubectl get nodes --context=cluster3
NAME                       STATUS   ROLES                  AGE   VERSION
cp-mem-cluster3-master     Ready    control-plane          12m   v1.33.5
cp-mem-cluster3-worker-1   Ready    <none>                 10m   v1.33.5
cp-mem-cluster3-worker-2   Ready    <none>                 10m   v1.33.5
cp-mem-cluster3-worker-3   Ready    <none>                 10m   v1.33.5

$ kubectl get pods -n kube-system --context=cluster1
NAME                                             READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh          1/1     Running   0             9m22s
calico-node-d8sg6                                1/1     Running   0             9m22s
calico-node-kfvjx                                1/1     Running   0             10m
calico-node-khwdz                                1/1     Running   0             10m
calico-node-nc58v                                1/1     Running   0             10m
coredns-657959df74-td5c2                         1/1     Running   0             8m15s
coredns-657959df74-ztnjj                         1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                   1/1     Running   0             8m9s
kube-apiserver-cp-mem-cluster3-master            1/1     Running   0             12m
kube-controller-manager-cp-mem-cluster3-master   1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                                 1/1     Running   0             10m
kube-proxy-kkvhk                                 1/1     Running   0             10m
kube-proxy-nfttc                                 1/1     Running   0             10m
kube-proxy-znfgk                                 1/1     Running   0             10m
kube-scheduler-cp-mem-cluster1-master            1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps                  2/2     Running   0             7m57s
nginx-proxy-cp-cluster3-worker-1                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster3-worker-2                 1/1     Running   0             8m8s
nginx-proxy-cp-cluster3-worker-3                 1/1     Running   0             8m8s
nodelocaldns-556gb                               1/1     Running   0             8m8s
nodelocaldns-8dpnt                               1/1     Running   0             8m8s
nodelocaldns-pvl6z                               1/1     Running   0             8m8s
nodelocaldns-x7grn                               1/1     Running   0             8m8s
```

<br><br>

## <div id='3'> 3. Delete K‑PaaS Container Platform Cluster (Reference)
Delete the K‑PaaS Container Platform cluster via the shell script.

<br>

```
$ ./reset-cp-cluster.sh
```

<br><br>

## <div id='4'> 4. Precautions When Creating Resources
When creating resources manually, be careful not to use the following prefixes.

<br>

|Resource Name|Prefix to Avoid|
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

[image 008]:images/kpaas-cp-cluster-8.png

[image if nhn 001]:images/kpaas-cp-cluster-if-nhn-01.png
[image if nhn 002]:images/kpaas-cp-cluster-if-nhn-02.png
[image if nhn 003]:images/kpaas-cp-cluster-if-nhn-03.png
[image if nhn 004]:images/kpaas-cp-cluster-if-nhn-04.png
[image if nhn 005]:images/kpaas-cp-cluster-if-nhn-05.png
[image if nhn 006]:images/kpaas-cp-cluster-if-nhn-06.png
[image if nhn 007]:images/kpaas-cp-cluster-if-nhn-07.png
[image if nhn 008]:images/kpaas-cp-cluster-if-nhn-08.png

[image if kt 001]:images/kpaas-cp-cluster-if-kt-01.png
[image if kt 002]:images/kpaas-cp-cluster-if-kt-02.png
[image if kt 003]:images/kpaas-cp-cluster-if-kt-03.png
[image if kt 004]:images/kpaas-cp-cluster-if-kt-04.png
[image if kt 005]:images/kpaas-cp-cluster-if-kt-05.png
[image if kt 006]:images/kpaas-cp-cluster-if-kt-06.png

[image lb nhn 001]:images/kpaas-cp-cluster-lb-nhn-01.png
[image lb nhn 002]:images/kpaas-cp-cluster-lb-nhn-02.png
[image lb nhn 003]:images/kpaas-cp-cluster-lb-nhn-03.png
[image lb nhn 004]:images/kpaas-cp-cluster-lb-nhn-04.png
[image lb nhn 005]:images/kpaas-cp-cluster-lb-nhn-05.png
[image lb nhn 006]:images/kpaas-cp-cluster-lb-nhn-06.png

[image lb kt 001]:images/kpaas-cp-cluster-lb-kt-01.png
[image lb kt 002]:images/kpaas-cp-cluster-lb-kt-02.png
[image lb kt 003]:images/kpaas-cp-cluster-lb-kt-03.png
[image lb kt 004]:images/kpaas-cp-cluster-lb-kt-04.png
[image lb kt 005]:images/kpaas-cp-cluster-lb-kt-05.png
[image lb kt 006]:images/kpaas-cp-cluster-lb-kt-06.png
[image lb kt 007]:images/kpaas-cp-cluster-lb-kt-07.png
[image lb kt 008]:images/kpaas-cp-cluster-lb-kt-08.png

[image lb naver 001]:images/kpaas-cp-cluster-lb-naver-01.png
[image lb naver 002]:images/kpaas-cp-cluster-lb-naver-02.png
[image lb naver 003]:images/kpaas-cp-cluster-lb-naver-03.png
[image lb naver 004]:images/kpaas-cp-cluster-lb-naver-04.png
[image lb naver 005]:images/kpaas-cp-cluster-lb-naver-05.png
[image lb naver 006]:images/kpaas-cp-cluster-lb-naver-06.png
[image lb naver 007]:images/kpaas-cp-cluster-lb-naver-07.png
[image lb naver 008]:images/kpaas-cp-cluster-lb-naver-08.png
[image lb naver 009]:images/kpaas-cp-cluster-lb-naver-09.png
[image lb naver 010]:images/kpaas-cp-cluster-lb-naver-10.png
[image lb naver 011]:images/kpaas-cp-cluster-lb-naver-11.png
[image lb naver 012]:images/kpaas-cp-cluster-lb-naver-12.png
[image lb naver 013]:images/kpaas-cp-cluster-lb-naver-13.png

### [Index](https://github.com/K-PaaS/container-platform/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Cluster Installation Guide
