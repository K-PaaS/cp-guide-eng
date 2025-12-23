### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) >  K-PaaS Container Platform Cluster Installation Guide (Single)

<br><br>

## Table of Contents

1. [Document Overview](#1)<br>
  1.1. [Purpose](#1.1)<br>
  1.2. [Scope](#1.2)<br>
  1.3. [System Configuration Diagram](#1.3)<br>
  1.4. [Reference](#1.4)

2. [Installing The K-PaaS Container Platform Cluster](#2)<br>
  2.1. [Prerequisite](#2.1)<br>
  2.1.1. [Operating System](#2.1.1)<br>
  2.1.2. [Python Package](#2.1.2)<br>
  2.1.3. [Key Software](#2.1.3)<br>
  2.1.4. [Firewall](#2.1.4)<br>
  2.1.5. [Storage](#2.1.5)<br>
  2.1.6. [Ingress Nginx Service Configuration](#2.1.6)<br>
  2.1.6.1. [Control Plane Node Addition Interface](#2.1.6.1)<br>
  2.1.6.2. [Cloud Load Balancer Service](#2.1.6.2)<br>
  2.1.7. [HA Control Plane Load Balancer](#2.1.7)<br>
  2.1.7.1. [Cloud Load Balancer Service](#2.1.7.1)<br>
  2.1.7.2. [HAProxy](#2.1.7.2)<br>
  2.2. [Generate And Distribute SSH Key](#2.2)<br>
  2.3. [Download K-PaaS Container Platform Cluster Deployment](#2.3)<br>
  2.4. [Preparing To Install The K-PaaS Container Platform Cluster](#2.4)<br>
  2.5. [Installing The K-PaaS Container Platform Cluster](#2.5)<br>
  2.6. [Verifying The K-PaaS Container Platform Cluster Installation](#2.6)

3. [Deleting a K-PaaS Container Platform Cluster (Reference)](#3)
   
4. [Caution For Creating Resources](#4)
   
5. [Install Kubeflow](#5)

<br><br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Cluster Installation Guide) describes how to install a cluster of the K-PaaS Container Platform, an open PaaS platform with a support environment for planners, developers, and operators.

<br><br>

### <div id='1.2'> 1.2. Scope
The installation scope is based on the **`single cloud`** environment for cluster installation, which is the basis of the K-PaaS container platform environment.

<br><br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system configuration consists of a Kubernetes **`single cluster`** (Control Plane, Worker) environment.

<br>

Kubernetes **`single cluster`** through K-PaaS container platform deployment * Configure and distribute the K-PaaS container platform portal environment through each resource to provide environments such as dashboards, databases, repositories, etc.

<br>

The instance environment required for the K-PaaS container platform cluster is configured below. Please refer to it.

<br>

|Instance type|Number of instances|Required|Remarks|
|---|---|---|---|
|Install|1| |Install instance configuration is recommended<br>Control Can be replaced with Plane instances|
|Control Plane|1 or more|O|1 test environment<br>3 or more production environments|
|Worker|1 to 3 or more|O|NFS storage 1 or more when using<br>3 or more when using Rook-Ceph storage|
|Storage|1| |Required when using NFS storage|
|LoadBalancer|1~2| |Required when configuring Private Cloud HA Control Plane|

<br>

The system configuration diagram for each deployment type is as follows.

<br>

<details>
<summary>System configuration diagram</summary>
<br>

***[ Single Control Plane, NFS storage configuration ]***

![image 001]

<br><br>

***[ Single Control Plane, Rook-Ceph Storage Configuration ]***

![image 002]

<br><br>

***[ HA Control Plane , ETCD Stacked, NFS storage configuration ]***

![image 003]

<br><br>

***[ HA Control Plane, ETCD External, NFS storage configuration ]***

![image 004]

<br><br>

***[ HA Control Plane, ETCD Stacked, Rook-Ceph storage configuration ]***

![image 005]

<br><br>

***[ HA Control Plane, ETCD External, Rook-Ceph storage configuration ]***

![image 006]

</details>

<br><br>

### <div id='1.4'> 1.4. Reference
> https://kubespray.io<br>
> https://github.com/kubernetes-sigs/kubespray

<br><br>

## <div id='2'> 2. Installing The K-PaaS Container Platform Cluster

### <div id='2.1'> 2.1. Prerequisite
This document (K-PaaS Container Platform Cluster Installation Guide) recommends the following for K-PaaS Container Platform cluster deployment:

- One or more machines running a deb/rpm compatible Linux OS (Ubuntu)
- At least 8GB RAM (minimum) or 16GB RAM (recommended) per machine
- At least 2 CPUs (minimum) or 4 CPUs (recommended) for the machine used as the control-plane node
- Complete network connectivity between all systems in the cluster

<br>

The prerequisites for K-PaaS Container Platform Cluster Installation are described below.

<br><br>

### <div id='2.1.1'> 2.1.1. Operating System (***`Required Check`***)
The required OS environment information for K-PaaS Container Platform Cluster installation is as follows.

<br>

|Supported OS|Version|
|---|---|
|Ubuntu|22.04|
|Ubuntu|24.04|

<br>

For each CSP, when creating an instance based on Ubuntu 22.04, the following steps should be taken.

<br>

<details>
<summary>KT Cloud</summary>
<br>
When creating an instance with the Ubuntu 22.04 image, the "Last password change" date is set to 2023-07-04. After a certain amount of time, when attempting to SSH, it tries to log in with password authentication.

<br>

Since the ubuntu account does not have a password set by default, access to the instance cannot be made. Therefore, change it to the current date.

```
$ sudo chage -d 20xx-xx-xx ubuntu
```

<br>

</details>

<br>

<details>
<summary>Naver Cloud</summary>
<br>
When creating an instance with the main account, root is used. When creating an instance with a sub-account, the ncloud account is used by default. For the container platform, the default account used is ubuntu, so the procedure to create the ubuntu account is followed.

<br>

Perform the following steps on the Install instance or the first Control Plane node instance.

<br>

Generate the RSA public key.

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

Copy the private key to the local PC environment for access to the instance.

```
## Copy the displayed private key and create a file in the local PC environment

$ cat ~/.ssh/id_rsa
```

<br>

View and copy the public key.

```
## Copy the displayed public key

$ cat ~/.ssh/id_rsa.pub
```

<br>

Perform the following steps on all instances.

```
$ sudo useradd -m -s /bin/bash ubuntu
$ echo "ubuntu ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers

$ sudo mkdir -p /home/ubuntu/.ssh
$ echo "{{ public key copied to the clipboard }}" | sudo tee -a /home/ubuntu/.ssh/authorized_keys
$ sudo chown -R ubuntu:ubuntu /home/ubuntu/.ssh
```

<br>

After completing this process, use the created ***`ubuntu account to access the instance`*** and proceed with the installation steps.

<br>

</details>

<br><br>

### <div id='2.1.2'> 2.1.2. Python Package (Reference)
The following are the main Python packages required for the installation of the K-PaaS container platform cluster.

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
The following are the main software required for the installation of the K-PaaS container platform cluster.

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

<br><br>

### <div id='2.1.4'> 2.1.4. Firewall (***`Required Settings`***)
The following are the firewall configurations required for the installation of the K-PaaS container platform cluster.

<br>

Control Plane Node

|Protocol|Port|Note|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|2379-2380|etcd server client API|
|TCP|6443|Kubernetes API Server|
|TCP|10250|Kubelet API|
|TCP|10251|kube-scheduler|
|TCP|10252|kube-controller-manager|
|TCP|10255|Read-Only Kubelet API|
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Ingress Nginx Controller|
|TCP|443|Ingress Nginx Controller|

<br>

WorkerNode

|Protocol|Port|Note|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|10250|Kubelet API|
|TCP|10255|Read-Only Kubelet API|
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Ingress Nginx Controller|
|TCP|443|Ingress Nginx Controller|

<br><br>

### <div id='2.1.5'> 2.1.5. Storage (***`Required Settings`***)
The following is the storage information required for the installation of the K-PaaS container platform cluster.

<br>

|Supported Storage|Note|
|---|---|
|NFS Server|Create a new instance|
|Rook-Ceph|Add volume to an existing instance|

<br>

When configuring NFS storage<br>
[NFS Server Installation Guide](../nfs-server-install-guide.md)

<br>

When configuring Rook-Ceph storage<br>
In addition to the Root Volume, ***`Additional Volumes must be pre-allocated to each Worker node`***.

<br><br>

### <div id='2.1.6'> 2.1.6. Ingress Nginx Service Configuration (***`Required Settings`***)
The following are the Ingress Nginx service configuration details required for the K-PaaS container platform service setup.

<br>

|Service|Description|Note|
|---|---|---|
|Ingress Nginx Controller|Service for exposing K-PaaS container platform services to external access through Ingress| ***`1 interface or 1 load balancer`*** must be created<br>Public IP assignment is required|

<br>

In the K-PaaS container platform cluster, the External IP for load balancer-type services is assigned through MetalLB.<br>
To support external communication and services through this External IP, ***`choose one of the following two methods`*** and proceed with the configuration.

<br>

|Method|Description|Note|
|---|---|---|
|Add Interface|Add a new interface with a Public IP assigned to one Control Plane node|Only the cost of using the Public IP will be incurred<br>If the node fails in an HA configuration, external access to the Ingress Nginx service will not be available|
|Create Load Balancer|Create a load balancer service with a Public IP assigned|Additional cost will incur for the load balancer service<br>In the case of some Control Plane node failures in an HA configuration, the Ingress Nginx service will still be functional<br>Recommended for production environments|

<br>

> In the K-PaaS Container Platform Cluster v1.7.0 release, installing the load balancer controller automatically creates and assigns load balancer services, and MetalLB is not installed. (Applicable to NHN and Naver Cloud environments)

<br>

|Method|Description|Note|
|---|---|---|
|Load Balancer Controller|Automatically creates load balancer services with assigned public IPs|**Supported on NHN and Naver Cloud**<br>**MetalLB not installed**<br>Additional costs incurred for load balancer services<br>Ingress Nginx service remains operational even if some control plane nodes fail in an HA configuration<br>Recommended for production environments|

<br><br>

### <div id='2.1.6.1'> 2.1.6.1. Control Plane Node Addition Interface
The method for adding interfaces to Control Plane nodes varies by CSP as follows.

<br>

<details>
<summary>NHN Cloud Add Interface</summary>
<br>
1. Go to <b><code>Network > Network Interface</code></b> menu and click the "Create Network Interface" button.
<br><br>

![image if nhn 001]

<br><br>
2. Enter the name, select the same network VPC and subnet as the Control Plane node, and click the "OK" button.

![image if nhn 002]

<br><br>
3. After selecting the created interface, click the "Floating IP Management" button.

![image if nhn 003]

<br><br>
4. Choose to create a new Floating IP or select an existing Floating IP, then click the "Connect" button.

![image if nhn 004]

<br><br>
5. Go to <b><code>Compute > Instance</code></b> menu, select the Control Plane node (the Control Plane node where the interface needs to be added for HA Control Plane setup), and click the "Stop Instance" button.

![image if nhn 005]

<br><br>
6. In the lower network tab, click the "Add Network Interface Connection" button.

![image if nhn 006]

<br><br>
7. Select the existing network interface and choose the created interface, then click the "OK" button.

![image if nhn 007]

<br><br>
8. Click the "Start Instance" button.

![image if nhn 008]

<br>

</details>

<br>

<details>
<summary>KT Cloud Add Interface</summary>
<br>
1. In the <b><code>Servers > Virtual IP</code></b> menu, click the "Create Virtual IP" button.

<br><br>
2. Choose the same network Zone and Tier as the Control Plane node, enter the Name, and click the "Create" button.

![image if kt 001]

<br><br>
3. After selecting the Virtual IP, click the "Connect" button to assign the Virtual IP to the Control Plane VM.

![image if kt 002]

<br><br>
4. In the <b><code>Servers > Networking</code></b> menu, click the "Create IP" button to create a Public IP in the same network Zone as the Control Plane node.

![image if kt 003]

<br><br>
5. After selecting the created Public IP, click the "Connection Settings" button to select the Virtual IP and configure Port Forwarding for ports 80 and 443.

![image if kt 004]

![image if kt 005]

<br><br>
6. Click the "Firewall Settings" button to configure the firewall with the registered connection settings. (Source CIDR, Destination CIDR should be set according to each installation environment)

![image if kt 006]

<br><br>
7. Execute the following command on the Control Plane node. (For HA Control Plane setup, execute this command on the Control Plane node where the Virtual IP is connected)

```
## Check Interface
## Example interface name: eth0, ens3 ...

$ sudo ifconfig
```

```
## Add Interface
## Example: sudo ifconfig ens3:1 192.168.0.10 up

$ sudo ifconfig {{ interface name }}:1 {{ Virtual IP (Private) }} up
```

<br>

</details>

<br>

<details>
<summary>Naver Cloud Add Interface</summary>
<br>
Naver Cloud does not allow assigning two Public IPs to a single instance due to its policy.<br>
Therefore, it is necessary to proceed with the creation and configuration of a load balancer service in Naver Cloud as described below.

<br>

</details>

<br><br>

### <div id='2.1.6.2'> 2.1.6.2. Cloud Load Balancer Service
The method for creating load balancer services differs by CSP as follows.

<br>

<details>
<summary>NHN Cloud Load Balancer Service Creation</summary>
<br>

Before creating the load balancer service, first ***`create only the Public IP to be assigned to the load balancer service`***, and then ***`create the load balancer service after completing the cluster deployment`***.

<br>
1. In the <b><code>Network > Floating IP</code></b> menu, click the "Create Floating IP" button to create a Public IP.
<br><br>

![image lb nhn 001]

<br>

Since the load balancer configuration can be done after confirming the port and node port information assigned to the service when creating the load balancer, the process of creating and configuring the load balancer, as described below, should be done after the [2.6. Check K-PaaS Container Platform Cluster Installation](#2.6) step.

<br><br>
2. In the <b><code>Network > Load Balancer > Manage</code></b> menu, click the "Create Load Balancer" button, and in the load balancer creation mode pop-up, select "L4 Routing."

![image lb nhn 002]

<br><br>
3. Enter the configuration information.

|Item|Description|Notes|
|---|---|---|
|Name|Enter the name of the load balancer||
|VPC|Select the same network VPC as the Control Plane node||
|Subnet|Select the same Public subnet as the Control Plane node||

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

Member Group

|Item|Description|Notes|
|---|---|---|
|Name|Enter the member group name||
|Protocol|Select HTTP||
|Port|Enter the node port value assigned to the 80 port of the Ingress Nginx service||
|Health Check Protocol|Select TCP||
|Health Check Port|Enter a port that can check the instance status, such as the SSH port (TCP 22)|
|Member List|Add all node instances||

<br>

> Check the Ingress Nginx Service Port<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

![image lb nhn 004]

<br><br>
5. Click the "Add Listener" button, and enter the listener and member group information for port 443.

Listener

|Item|Description|Notes|
|---|---|---|
|Name|Enter the listener name||
|Protocol|Select HTTPS||
|Load Balancer Port|Enter 443||

<br>

Member Group

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
6. Click the "Create Load Balancer" button at the bottom.

<br><br>
7. After selecting the created load balancer, click the "Floating IP Management" button.

<br><br>
8. Select the previously created floating IP (from step 1), choose the created interface, and click the "Connect" button.

![image lb nhn 006]

<br>

</details>

<br>

<details>
<summary>KT Cloud Load Balancer Service Creation</summary>
<br>

Before creating the load balancer service, first ***`create only the Public IP to be assigned to the load balancer service`***, and then ***`create the load balancer service after completing the cluster deployment`***.

<br>
1. In the <b><code>Servers > Networking</code></b> menu, click the "Create IP" button to create a Public IP.
<br><br>

![image if kt 003]

<br>

Since the load balancer configuration can be done after confirming the port and node port information assigned to the service when creating the load balancer, the process of creating and configuring the load balancer, as described below, should be done after the [2.6. Check K-PaaS Container Platform Cluster Installation](#2.6) step.

<br><br>
2. In the <b><code>Load Balancer > Load Balancer Management</code></b> menu, click the "Create Load Balancer" button.

![image lb kt 001]

<br><br>
3. Enter the load balancer information and click the "OK" button.

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
4. Click the "Create Load Balancer" button.

<br><br>
5. Enter the load balancer information and click the "OK" button.

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
6. After selecting the created load balancer, click the "VM Connect/Disconnect" button. (For both port 80 and 443 load balancers)

<br><br>
7. Enter the information and click the "Add" button.

|Item|Description|Notes|
|---|---|---|
|Tier|Select the same network Tier as the Control Plane node||
|VM|Select all nodes of the cluster sequentially||
|Public Port|Enter the node port information assigned to ports 80 and 443||

<br>

> Check the Ingress Nginx Service Port<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
8. In the <b><code>Servers > Networking</code></b> menu, select the previously created Public IP (from step 1), click the "Static NAT" button, and select one of the created load balancers.

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
9. Click the "Firewall Settings" button to register the firewall with the Static NAT settings.

![image lb kt 008]

<br>

</details>

<br>

<details>
<summary>Naver Cloud Load Balancer Service Creation</summary>
<br>

Before creating the load balancer service, first ***`create only the Public IP to be assigned to the load balancer service`***, and then ***`create the load balancer service after completing the cluster deployment`***.

<br>
1. In the <b><code>Server > Public IP</code></b> menu, click the "Apply for Public IP" button to create an unassigned Public IP.
<br><br>

![image lb naver 001]

<br>

Since the load balancer configuration can be done after confirming the port and node port information assigned to the service when creating the load balancer, the process of creating and configuring the load balancer, as described below, should be done after the [2.6. Check K-PaaS Container Platform Cluster Installation](#2.6) step.

<br><br>
2. In the <b><code>Load Balancer > Target Group</code></b> menu, click the "Create Target Group" button.

![image lb naver 002]

<br><br>
3. Enter the following Target Group information and click the "Next" button.

|Item|Description|Notes|
|---|---|---|
|Target Group Name|Enter the target group name||
|Target Type|Select VPC Server||
|VPC|Select the same network VPC as the Control Plane node||
|Protocol|Select PROXY_TCP||
|Port|Enter the node port information assigned to port 80||

<br>

> Check the Ingress Nginx Service Port<br>

```
$ kubectl get svc ingress-nginx-controller -n ingress-nginx
```

<br>

![image lb naver 003]

<br><br>
4. Enter the Health Check settings and click the "Next" button.

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter a port that can check the instance status, such as the SSH port (TCP 22)|

<br>

![image lb naver 004]

<br><br>
5. Select all nodes and include them in the Target, then create the Target Group.

![image lb naver 005]

![image lb naver 006]

<br><br>
6. Repeat steps 2–5 to add a Target Group for TCP port 443.

<br><br>
7. In the <b><code>Load Balancer > Load Balancer</code></b> menu, click the "Create Load Balancer" button, then click the "Network Proxy Load Balancer" button.

![image lb naver 007]

<br><br>
8. Enter the following load balancer information and click the "Next" button.

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
9. Enter the listener configuration information, click "Add", and then click "Next".

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Load Balancer Port|Enter port 80||

<br>

![image lb naver 009]

<br><br>
10. Select the Target Group and create the load balancer.

![image lb naver 010]

<br><br>
11. After selecting the created load balancer, click the "Change Listener Settings" button, then click the "Add Listener" button.

![image lb naver 011]

<br><br>
12. Enter the listener configuration information for port 443, then add the listener.

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter port 443||
|Target Group|Select the Target Group set for port 443||

<br>

![image lb naver 012]

![image lb naver 013]

<br>

</details>

<br><br>

### <div id='2.1.7'> 2.1.7. HA Control Plane Load Balancer (***`Required Settings for HA Control Plane`***)
> If not configuring an HA Control Plane, skip the load balancer configuration process and proceed to the next step: [2.2. Generate And Distribute SSH Key](#2.2)

<br>

When configuring the K-PaaS container platform cluster as an HA Control Plane, the required load balancer information is as follows:

<br>

|Cloud|Load Balancer|Notes|
|---|---|---|
|Public|Use the load balancer service provided by each CSP|
|Private|Configure load balancer instances with Keepalived, HAProxy|

<br><br>

### <div id='2.1.7.1'> 2.1.7.1. Cloud Load Balancer Service
For public cloud environments, create the load balancer service provided by each CSP.

<br>

Refer to [2.1.6.2. Cloud Load Balancer Service](#2.1.6.2) for the creation of the load balancer service for each CSP, and proceed with the configuration by changing the following information.

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

Member Group

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

Load Balancer Information

|Item|Changes|Notes|
|---|---|---|
|Service IP / Port|Assign new IP, enter 6443||
|Service Type|Select TCP||

<br>

Load Balancer VM Connection Information

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

Target Group Information

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||
|Apply Target|Add Control Plane instances||

<br>

Load Balancer Listener Information

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||

<br>

</details>

<br><br>

### <div id='2.1.7.2'> 2.1.7.2. HAProxy
In a private cloud environment, the load balancer is configured using Keepalived and HAProxy.

|Environment|Configuration|Notes|
|---|---|---|
|Test Environment|Single Keepalived, HAProxy configuration|Use the private IP of the load balancer instance as VIP|
|Production Environment|HA configuration with Keepalived, HAProxy|Create a separate interface, assign it to the instance, and use it as VIP|

<br>

For configuring the load balancer in a private cloud environment, follow the procedure outlined below.

For HA configuration of the load balancer, the VIP is configured as follows:
- Create a new interface
- Assign a Public IP to the newly created interface
- Assign the newly created interface to the load balancer instance as VIP

<br>

<details>
<summary>Install Keepalived and HAProxy</summary>
<br>

For HA configuration of the load balancer, install Keepalived and HAProxy on both load balancer instances.

<br>

To install Keepalived on the load balancer instance, run the following commands:
```
$ sudo su -

# apt-get update

# apt-get install -y keepalived

# echo 'net.ipv4.ip_nonlocal_bind=1' >> /etc/sysctl.conf
# echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
# sysctl -p
```

<br>

Proceed with the configuration of Keepalived.
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

Next, install HAProxy on the load balancer instance.
```
# apt-get install -y haproxy
```

<br>

Proceed with the configuration of HAProxy. Add the following content at the bottom of the haproxy.cfg file.
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

### <div id='2.2'> 2.2. Generate And Distribute SSH Key
To install the K-PaaS container platform cluster, the SSH Key must be copied to all the servers in the inventory.<br>
This document (K-PaaS container platform cluster installation guide) uses an RSA public key to set up SSH access.

After generating and deploying the SSH Key, all installation steps are carried out on the ***`Install instance or Control Plane node to be used for installation`***.

<br>

Generate the RSA public key on the ***`Install instance or Control Plane node to be used for installation`***.
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

Copy the public key to the ***`Install, Control Plane, Worker nodes`***.
```
## Copy the output public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

Copy the public key to the authorized_keys file at the end of the content (add below the existing content) on the ***`Install, Control Plane, Worker nodes`***.
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLzwTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTDvRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br><br>

### <div id='2.3'> 2.3. Download K-PaaS Container Platform Cluster Deployment

> From section 2.3 onwards, perform the steps only on the ***`Install instance or Control Plane node to be used for installation`***.

<br>

Download the Deployment needed for installing the K-PaaS container platform cluster, navigate to the installation work directory, and proceed with the installation.

> K-PaaS container platform cluster Deployment Download URL: https://github.com/k-paas/cp-deployment

<br>

Use the git clone command to download the K-PaaS container platform cluster Deployment in the HOME directory.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.7.x
```

<br><br>

### <div id='2.4'> 2.4. Preparing To Install The K-PaaS Container Platform Cluster
Before installing the K-PaaS container platform cluster, predefined environment variables must be set, and the installation will be carried out via a shell script.

Navigate to the K-PaaS container platform cluster installation directory.
```
$ cd ~/cp-deployment/single
```

<br>

Input the environment variable information required for the K-PaaS container platform cluster installation.
```
$ vi cp-cluster-vars.sh
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

<br><br>

### <div id='2.5'> 2.5. Installing The K-PaaS Container Platform Cluster
Through the shell script, the necessary packages will be installed, the cluster installation environment variables set, and the K-PaaS container platform cluster will be installed step by step using Ansible playbook.

```
$ ./deploy-cp-cluster.sh
```

<br><br>

### <div id='2.6'> 2.6. Verifying The K-PaaS Container Platform Cluster Installation
Verify the K-PaaS container platform cluster installation by checking the nodes and pods in the kube-system namespace.<br>
The nodes and pod information displayed may vary depending on the configuration, and the following is the result when deployed with a single Control Plane configuration.

```
$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE   VERSION
cp-master            Ready    control-plane          12m   v1.33.5
cp-worker-1          Ready    <none>                 10m   v1.33.5
cp-worker-2          Ready    <none>                 10m   v1.33.5
cp-worker-3          Ready    <none>                 10m   v1.33.5

$ kubectl get pods -n kube-system
NAME                                          READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh       1/1     Running   0             9m22s
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
nginx-proxy-cp-worker-1                       1/1     Running   0             8m8s
nginx-proxy-cp-worker-2                       1/1     Running   0             8m8s
nginx-proxy-cp-worker-3                       1/1     Running   0             8m8s
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7grn                            1/1     Running   0             8m8s
```

<br><br>

## <div id='3'> 3. Deleting a K-PaaS Container Platform Cluster (Reference)
The K-PaaS container platform cluster can be deleted using a shell script.

<br>

```
$ ./reset-cp-cluster.sh
```

<br><br>

## <div id='4'> 4. Caution For Creating Resources
When creating resources manually, avoid using the following prefixes.

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

## <div id='5'> 5. Install Kubeflow (Optional)
In the **`single cloud`** environment, Kubeflow installation is supported through a separate shell script after the cluster deployment.

<br>

```
$ ./deploy-cp-kubeflow.sh
```

<br><br>

[image 001]:images/kpaas-cp-cluster-1.png
[image 002]:images/kpaas-cp-cluster-2.png
[image 003]:images/kpaas-cp-cluster-3.png
[image 004]:images/kpaas-cp-cluster-4.png
[image 005]:images/kpaas-cp-cluster-5.png
[image 006]:images/kpaas-cp-cluster-6.png
[image 007]:images/kpaas-cp-cluster-7.png

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

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) >  K-PaaS Container Platform Cluster Installation Guide (Single)
