### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Cluster Installation Guide (Multi)

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
  2.1.6. [Istio Gateway Service Configuration](#2.1.6)<br>
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

<br><br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Cluster Installation Guide) provides instructions for installing the K-PaaS container platform cluster, which is an open PaaS platform for planners, developers, and operators, in a containerized environment.

<br><br>

### <div id='1.2'> 1.2. Scope
The installation scope is based on the installation of the cluster, which is the foundation of the K-PaaS container platform, in a **`multi-cloud`** environment.

<br><br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system architecture is based on a Kubernetes **`multi-cluster`** (Control Plane, Worker) environment.

<br>

Through the K-PaaS container platform deployment, a Kubernetes **`multi-cluster`** is set up, and various resources are used to deploy the K-PaaS container platform portal environment, providing services such as dashboards, databases, and repositories.

<br>

The instance environment required for the K-PaaS container platform cluster is as follows.

> In multi-cloud deployments, instances other than the Install instance are configured separately for each cluster as needed.

<br>

|Instance Type|Number of Instances|Required|Remarks|
|---|---|---|---|
|Install|1| |It is recommended to configure the Install instance<br>Can be replaced with a Control Plane instance|
|Control Plane|At least 1 per cloud|Yes|1 for test environments<br>At least 3 for production environments|
|Worker|At least 1-3 per cloud|Yes|At least 1 if using NFS storage<br>At least 3 if using Rook-Ceph storage|
|Storage|1 per cloud| |Required when using NFS storage|
|LoadBalancer|1-2 per cloud| |Required when configuring HA Control Plane in private cloud|

<br>

The system architecture diagrams for each deployment type are as follows.

<br>

<details>
<summary>System Architecture Diagram</summary>
<br>

***[ Single Control Plane, NFS Storage Configuration ]***

![image 001]

<br><br>

***[ Single Control Plane, Rook-Ceph Storage Configuration ]***

![image 002]

<br><br>

***[ HA Control Plane, ETCD Stacked, NFS Storage Configuration ]***

![image 003]

<br><br>

***[ HA Control Plane, ETCD External, NFS Storage Configuration ]***

![image 004]

<br><br>

***[ HA Control Plane, ETCD Stacked, Rook-Ceph Storage Configuration ]***

![image 005]

<br><br>

***[ HA Control Plane, ETCD External, Rook-Ceph Storage Configuration ]***

![image 006]

<br><br>

> In a multi-cloud environment, the cluster configuration is the same as in a single-cloud environment. The diagram below shows the service flow between clusters in a multi-cloud environment.

<br><br>

***[ Service Flow Between Clusters in Multi-Cloud Environment ]***

![image 007]

</details>

<br><br>

### <div id='1.4'> 1.4. Reference
> https://kubespray.io<br>
> https://github.com/kubernetes-sigs/kubespray

<br><br>

## <div id='2'> 2. Installing The K-PaaS Container Platform Cluster

### <div id='2.1'> 2.1. Prerequisite
This document (K-PaaS Container Platform Cluster Installation Guide) recommends the following for the deployment of the K-PaaS container platform cluster:

- One or more machines running a deb/rpm compatible Linux OS (Ubuntu)
- A minimum of 8GB (minimum requirement) or 16GB (recommended requirement) of RAM per machine
- A minimum of 2 CPUs (minimum requirement) or 4 CPUs (recommended requirement) for the machine used as the control-plane node
- Full network connectivity between all systems in the cluster

<br>

The prerequisites for installing the K-PaaS container platform cluster are outlined below.

<br><br>

### <div id='2.1.1'> 2.1.1. Operating System (***`Required Check`***)
The required OS environment for installing the K-PaaS container platform cluster is as follows.

<br>

|Supported OS|Version|
|---|---|
|Ubuntu|22.04|

<br>

When creating instances on each CSP based on Ubuntu 22.04, the following steps should be followed.

<br>

<details>
<summary>KT Cloud</summary>
<br>
When creating an instance with the Ubuntu 22.04 image, the "Last password change" date is set to 2023-07-04. After a certain period, SSH login will attempt password-based authentication.

<br>

Since the ubuntu account does not have a password set by default, an issue occurs where access to the instance is not possible. To resolve this, perform the following change:

```
$ sudo chage -d 20xx-xx-xx ubuntu
```

<br>

</details>

<br>

<details>
<summary>Naver Cloud</summary>
<br>
When creating an instance with the main account, the root account is used by default, and when creating an instance with a sub-account, the ncloud account is used. For the container platform, the ubuntu account is used by default, so the ubuntu account creation process should be followed.

<br>

On the Install instance or the first Control Plane node instance, follow the steps below:

```
$ sudo useradd -m -s /bin/bash ubuntu
$ echo "ubuntu ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers

$ sudo mkdir -p /home/ubuntu/.ssh
$ sudo ssh-keygen -t rsa -m PEM -N '' -f /home/ubuntu/.ssh/id_rsa
$ sudo cat /home/ubuntu/.ssh/id_rsa.pub | sudo tee -a /home/ubuntu/.ssh/authorized_keys
$ sudo chown -R ubuntu:ubuntu /home/ubuntu/.ssh
```

<br>

Generate the RSA public key:

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

Copy the private key to your local environment to access the instance.

```
## Copy the outputted private key and create a file in the local environment

$ sudo cat ~/.ssh/id_rsa
```

<br>

View and copy the public key.

```
## Copy the outputted public key

$ sudo cat ~/.ssh/id_rsa.pub
```

<br>

Proceed with the following steps on all instances:

```
$ sudo useradd -m -s /bin/bash ubuntu
$ echo "ubuntu ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers

$ sudo mkdir -p /home/ubuntu/.ssh
$ echo "{{ Public Key }}" | sudo tee -a /home/ubuntu/.ssh/authorized_keys
$ sudo chown -R ubuntu:ubuntu /home/ubuntu/.ssh
```

<br>

After completing these steps, access the instance with the newly created ***`ubuntu account`*** and proceed with the installation.

<br>

</details>

<br><br>

### <div id='2.1.2'> 2.1.2. Python Package (Reference)
The key Python packages required for installing the K-PaaS container platform cluster are as follows:

<br>

|Python Package|Version|
|---|---|
|ansible|9.8.0|
|jmespath|1.0.1|
|jsonschema|4.23.0|
|netaddr|1.3.0|
|configparser|>=3.3.0|
|ipaddress||
|ruamel.yaml|>=0.15.88|

<br><br>

### <div id='2.1.3'> 2.1.3. Key Software (Reference)
The key software required for installing the K-PaaS container platform cluster are as follows:

<br>

|Key Software|Version|
|---|---|
|Kubespray|2.26.0|
|Kubernetes Native|1.30.4|
|CRI-O|1.30.3|
|Calico|3.28.1|
|MetalLB|0.13.9|
|Ingress Nginx Controller|1.11.3|
|Helm|3.15.4|
|Istio|1.23.2|
|Podman|3.4.4|
|OpenTofu|1.8.3|
|nfs-subdir-external-provisioner|4.0.2|
|Rook Ceph|1.15.4|

<br><br>

### <div id='2.1.4'> 2.1.4. Firewall (***`Required Settings`***)
The required firewall configurations for installing the K-PaaS container platform cluster are as follows:

> In multi-cloud deployments, additional firewall rules for Istio Ingress and Eastwest Gateway have been added.

<br>

Control Plane Node

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
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Istio Ingress Gateway|
|TCP|443|Istio Ingress Gateway|
|TCP|15021|Istio East-West Gateway Status|
|TCP|15443|Istio East-West Gateway mTLS|
|TCP|15012|Istio East-West Gateway istiod|
|TCP|15017|Istio East-West Gateway webhook|

<br>

Worker Node

|Protocol|Port|Notes|
|---|---|---|
|TCP|111|NFS PortMapper|
|TCP|2049|NFS|
|TCP|10250|Kubelet API|
|TCP|10255|Read-Only Kubelet API|
|TCP|30000-32767|NodePort Services|
|UDP|4789|Calico networking VXLAN|
|TCP|80|Istio Ingress Gateway|
|TCP|443|Istio Ingress Gateway|
|TCP|15021|Istio East-West Gateway Status|
|TCP|15443|Istio East-West Gateway mTLS|
|TCP|15012|Istio East-West Gateway istiod|
|TCP|15017|Istio East-West Gateway webhook|

<br><br>

### <div id='2.1.5'> 2.1.5. Storage (***`Required Settings`***)
The storage configurations required for installing the K-PaaS container platform cluster are as follows:

> In multi-cloud deployments, storage should be configured separately in each cloud environment.

<br>

|Supported Storage|Notes|
|---|---|
|NFS Server|Create a new instance|
|Rook-Ceph|Add volumes to existing instances|

<br>

For NFS storage configuration, refer to the [NFS Server Installation Guide](../nfs-server-install-guide.md).

<br>

For Rook-Ceph storage configuration, **`additional volumes must be pre-allocated to each Worker node`** other than the root volume.

<br><br>

### <div id='2.1.6'> 2.1.6. Istio Gateway Service Configuration (***`Required Settings`***)
The necessary Istio Gateway service configuration information for K-PaaS container platform service setup is as follows:

<br>

|Service|Description|Notes|
|---|---|---|
|Istio Ingress Gateway<br>(including Eastwest Gateway functionality)|Gateway service for external service communication and<br>service communication between multi-clouds|Each cloud requires ***`1 interface or 1 load balancer`*** to be created<br>Public IP assignment is required|

<br>

> In multi-cloud deployments, interfaces or load balancer services to be assigned to services should be configured separately for each cloud environment.

<br>

In the K-PaaS container platform cluster, MetalLB is used to assign External IPs for load balancer-type services.<br>
To support external communication and services with the assigned External IP, ***`choose one of the following two methods`*** and configure accordingly.

<br>

|Method|Description|Notes|
|---|---|---|
|Add Interface|Add a new interface with a Public IP assigned to one Control Plane node|Only the cost for Public IP usage will be incurred<br>In HA configuration, if that node fails, the Istio Gateway service will be inaccessible externally|
|Create Load Balancer|Create a load balancer service with a Public IP assigned|Additional costs for the load balancer service will be incurred<br>In HA configuration, even if some Control Plane nodes fail, the Istio Gateway service will remain operational<br>Recommended for production environments|

<br><br>

### <div id='2.1.6.1'> 2.1.6.1. Control Plane Node Addition Interface

<br>

<details>
<summary>NHN Cloud - Add Interface</summary>
<br>
1. Go to <b><code>Network > Network Interface</code></b> menu and click the "Create Network Interface" button.
<br><br>

![image if nhn 001]

<br><br>
2. Enter the name, select the same network VPC and subnet as the Control Plane node, then click the "Confirm" button.

![image if nhn 002]

<br><br>
3. Select the created interface and click the "Floating IP Management" button.

![image if nhn 003]

<br><br>
4. Create a floating IP or select an existing floating IP and click the "Connect" button.

![image if nhn 004]

<br><br>
5. Go to <b><code>Compute > Instance</code></b> menu and select the Control Plane node (for HA Control Plane setup, select the Control Plane node to which the interface will be added), then click the "Stop Instance" button.

![image if nhn 005]

<br><br>
6. Under the network tab at the bottom, click the "Add Network Interface" button.

![image if nhn 006]

<br><br>
7. Select the created interface from the existing network interface and click the "Confirm" button.

![image if nhn 007]

<br><br>
8. Click the "Start Instance" button.

![image if nhn 008]

<br>

</details>

<br>

<details>
<summary>KT Cloud - Add Interface</summary>
<br>
1. In the <b><code>Servers > Virtual IP</code></b> menu, click the "Create Virtual IP" button.

<br><br>
2. Select the same network zone and tier as the Control Plane node, enter the name, then click the "Create" button.

![image if kt 001]

<br><br>
3. Select the Virtual IP and click the "Connect" button to link the Virtual IP to the Control Plane VM.

![image if kt 002]

<br><br>
4. In the <b><code>Servers > Networking</code></b> menu, click the "Create IP" button to create a Public IP in the same network zone as the Control Plane node.

![image if kt 003]

<br><br>
5. Select the created Public IP, click the "Connection Settings" button, and set up Port Forwarding for ports 80, 443, 15021, 15443, 15012, and 15017.

![image if kt 004]

![image if kt 005]

<br><br>
6. Click the "Firewall Settings" button to configure the firewall with the registered connection settings. (Source CIDR and Destination CIDR should be configured according to the specific environment.)

![image if kt 006]

<br><br>
7. Run the following commands on the Control Plane node. (For HA Control Plane, execute this on the Control Plane node with the Virtual IP connected)

```
## View Interface
## Example: eth0, ens3 ...

$ sudo ifconfig
```

```
## Add Interface
## Example: sudo ifconfig ens3:1 192.168.0.10 up

$ sudo ifconfig {{ Interface Name }}:1 {{ Virtual IP (Private) }} up
```

<br>

</details>

<br>

<details>
<summary>Naver Cloud - Add Interface</summary>
<br>
Due to Naver Cloud's policy, more than one Public IP cannot be assigned to a single instance.<br>
Therefore, it is necessary to follow the instructions for creating and configuring the Naver Cloud load balancer service below.

<br>

</details>

<br><br>

### <div id='2.1.6.2'> 2.1.6.2. Cloud Load Balancer Service
> The example for creating a cloud load balancer service is based on the Ingress and Eastwest Gateway ports of the Istio Gateway service.<br>

<br>

<details>
<summary>NHN Cloud - Create Load Balancer Service</summary>
<br>

Before creating the load balancer service, ***`only create the Public IP to be assigned to the load balancer service`*** first, and then ***`create the load balancer service after the cluster deployment is completed`***.

<br>
1. In the <b><code>Network > Floating IP</code></b> menu, click the "Create Floating IP" button to create a Public IP.
<br><br>

![image lb nhn 001]

<br>

Since the load balancer can be configured after confirming the ports and node ports assigned to the service, the load balancer creation and configuration process should be followed after [2.6. K-PaaS Container Platform Cluster Installation Verification](#2.6).

<br><br>
2. In the <b><code>Network > Load Balancer > Manage</code></b> menu, click the "Create Load Balancer" button, and select "L4 Routing" in the load balancer creation mode popup.

![image lb nhn 002]

<br><br>
3. Enter the configuration information.

|Item|Description|Notes|
|---|---|---|
|Name|Enter the load balancer name||
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
|Port|Enter the node port value assigned to port 80 of the Ingress IngressGateway service||
|Health Check Protocol|Select TCP||
|Health Check Port|Enter the port where instance health checks are possible|Example: Instance SSH port (TCP 22)|
|Member List|Add all node instances||

<br>

> Check the Istio Gateway Service Port<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb nhn 004]

<br><br>
5. Click the "Add Listener" button and enter the listener and member group information for port 443.

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
|Port|Enter the node port value assigned to port 443 of the Istio IngressGateway service||
|Health Check Protocol|Select TCP||
|Health Check Port|Enter the port where instance health checks are possible|Example: Instance SSH port (TCP 22)|
|Member List|Add all node instances||

<br>

![image lb nhn 005]

<br><br>
6. Add listeners and member groups for ports 15021, 15443, 15012, and 15017 in the same way as step 5.

<br><br>
7. Click the "Create Load Balancer" button at the bottom.

<br><br>
8. After creating the load balancer, select it and click the "Floating IP Management" button.

<br><br>
9. Select the floating IP created in step 1, select the created interface, and click the "Connect" button.

![image lb nhn 006]

<br>

</details>

<br>

<details>
<summary>KT Cloud - Create Load Balancer Service</summary>
<br>

Before creating the load balancer service, ***`only create the Public IP to be assigned to the load balancer service`*** first, and then ***`create the load balancer service after the cluster deployment is completed`***.

<br>
1. In the <b><code>Servers > Networking</code></b> menu, click the "Create IP" button to generate a Public IP.
<br><br>

![image if kt 003]

<br>

Since the load balancer can be configured after confirming the ports and node ports assigned to the service, the load balancer creation and configuration process should be followed after [2.6. K-PaaS Container Platform Cluster Installation Verification](#2.6).

<br><br>
2. In the <b><code>Load Balancer > Load Balancer Management</code></b> menu, click the "Create Load Balancer" button.

![image lb kt 001]

<br><br>
3. Enter the load balancer information and click the "Confirm" button.

|Item|Description|Notes|
|---|---|---|
|Zone|Select the same network Zone as the Control Plane node|Same concept as VPC|
|Tier|Select the same network Tier as the Control Plane node|Same concept as subnet|
|Name|Enter the load balancer name||
|Service IP / Port|Assign a new IP and enter port 80||
|Service Type|Select HTTP||

<br>

![image lb kt 002]

<br><br>
4. Click the "Create Load Balancer" button.

<br><br>
5. Enter the load balancer information and click the "Confirm" button.

|Item|Description|Notes|
|---|---|---|
|Zone|Select the same network Zone as the Control Plane node|Same concept as VPC|
|Tier|Select the same network Tier as the Control Plane node|Same concept as subnet|
|Name|Enter the load balancer name||
|Service IP / Port|Select the Private IP created for the load balancer, and enter port 443||
|Service Type|Select HTTPS(Bridge)||

<br>

![image lb kt 003]

<br><br>
6. Add load balancers for ports 15021, 15443, 15012, and 15017, following the same steps as in step 5.

<br><br>
7. After selecting the created load balancer, click the "VM Connect/Disconnect" button. (For all load balancers with ports 80, 443, 15021, 15443, 15012, 15017)

<br><br>
8. Enter the information and click the "Add" button.

|Item|Description|Notes|
|---|---|---|
|Tier|Select the same network Tier as the Control Plane node||
|VM|Select all nodes of the cluster sequentially||
|Public Port|Enter the node port information assigned to ports 80, 443, 15021, 15443, 15012, 15017||

<br>

> Check the Istio Gateway Service Port<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
9. Go to the <b><code>Servers > Networking</code></b> menu, select the previously created Public IP (created in step 1), and click the "Static NAT" button to select one of the created load balancers.

![image lb kt 004]

![image lb kt 005]

![image lb kt 006]

<br><br>
10. Click the "Firewall Settings" button to register the firewall with the Static NAT settings.

![image lb kt 008]

<br>

</details>

<br>

<details>
<summary>Naver Cloud - Create Load Balancer Service</summary>
<br>

Before creating the load balancer service, ***`only create the Public IP to be assigned to the load balancer service`*** first, and then ***`create the load balancer service after the cluster deployment is completed`***.

<br>
1. In the <b><code>Server > Public IP</code></b> menu, click the "Apply for Public IP" button to create an unassigned Public IP.
<br><br>

![image lb naver 001]

<br>

Since the load balancer can be configured after confirming the ports and node ports assigned to the service, the load balancer creation and configuration process should be followed after [2.6. K-PaaS Container Platform Cluster Installation Verification](#2.6).

<br><br>
2. In the <b><code>Load Balancer > Target Group</code></b> menu, click the "Create Target Group" button.

![image lb naver 002]

<br><br>
3. Enter the following Target Group information and click the "Next" button.

|Item|Description|Notes|
|---|---|---|
|Target Group Name|Enter the Target Group name||
|Target Type|Select VPC Server||
|VPC|Select the same network VPC as the Control Plane node||
|Protocol|Select PROXY_TCP||
|Port|Enter the node port information assigned to port 80||

<br>

> Check the Istio Gateway Service Port<br>

```
$ kubectl get svc istio-ingressgateway -n istio-system
```

<br>

![image lb naver 003]

<br><br>
4. Enter the following Health Check settings and click the "Next" button.

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter the port where instance health checks are possible|Example: Instance SSH port (TCP 22)|

<br>

![image lb naver 004]

<br><br>
5. Select all nodes and include them in the Target, then create the Target Group.

![image lb naver 005]

![image lb naver 006]

<br><br>
6. Repeat steps 2–5 to create additional Target Groups for ports TCP 443, 15021, 15443, 15012, and 15017.

<br><br>
7. In the <b><code>Load Balancer > Load Balancer</code></b> menu, click the "Create Load Balancer" button, then click the "Network Proxy Load Balancer" button.

![image lb naver 007]

<br><br>
8. Enter the following load balancer information and click the "Next" button.

|Item|Description|Notes|
|---|---|---|
|Load Balancer Name|Enter the load balancer name||
|Network|Select Public||
|Target VPC|Select the same network VPC as the Control Plane node||
|Subnet Selection|Select the load balancer subnet||
|Public IP|Select the previously created Public IP (from step 1)||

<br>

![image lb naver 008]

<br><br>  
9. Enter the listener settings and click the "Add", then "Next" button.

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
12. Enter the listener settings for port 443 and add the listener.

|Item|Description|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter port 443||
|Target Group|Select the Target Group created for port 443||

<br>

![image lb naver 012]

![image lb naver 013]

<br><br>
13. Add listeners for ports 15021, 15443, 15012, and 15017, following the same steps as in step 12.

<br>

</details>

<br>

### <div id='2.1.7'> 2.1.7. HA Control Plane Load Balancer (***`Required Settings for HA Control Plane Setup`***)
> If not configuring an HA Control Plane, skip the load balancer configuration and proceed to the next step: [2.2. Generate And Distribute SSH Key](#2.2).

<br>

When configuring a K-PaaS container platform cluster with HA Control Plane, the following load balancer information is required.

<br>

|Cloud Provider|Load Balancer|Notes|
|---|---|---|
|Public|Use the load balancer service provided by each CSP|
|Private|Configure a load balancer instance using Keepalived, HAProxy|

<br><br>

### <div id='2.1.7.1'> 2.1.7.1. Cloud Load Balancer Service
For public cloud environments, use the load balancer service provided by each CSP.

<br>

Refer to [2.1.6.2. Cloud Load Balancer Service](#2.1.6.2) for creating the load balancer service on each CSP. During configuration, make the following changes.

<br>

<details>
<summary>NHN Cloud</summary>
<br>

**Listener**

|Item|Changes|Notes|
|---|---|---|
|Protocol|Select TCP||
|Load Balancer Port|Enter 6443||

**Member Group**

|Item|Changes|Notes|
|---|---|---|
|Protocol|Select TCP||
|Port|Enter 6443||
|Member List|Add Control Plane instances||

</details>

<br>

<details>
<summary>KT Cloud</summary>
<br>

**Load Balancer Information**

|Item|Changes|Notes|
|---|---|---|
|Service IP / Port|Assign new IP, enter 6443||
|Service Type|Select TCP||

**Load Balancer VM Connection Information**

|Item|Changes|Notes|
|---|---|---|
|VM|Select Control Plane nodes sequentially||
|Public Port|Enter 6443||

</details>

<br>

<details>
<summary>Naver Cloud</summary>
<br>

**Target Group Information**

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||
|Applied Target|Add Control Plane instances||

<br>

**Load Balancer Listener Information**

|Item|Changes|Notes|
|---|---|---|
|Port|Enter 6443||

<br>

</details>

<br><br>

### <div id='2.1.7.2'> 2.1.7.2. HAProxy
In a private cloud environment, configure the load balancer using Keepalived and HAProxy.

|Environment|Configuration|Notes|
|---|---|---|
|Test Environment|Single configuration of Keepalived, HAProxy|Use the Private IP of the load balancer instance as the VIP|
|Production Environment|HA configuration of Keepalived, HAProxy|Create a separate interface and assign it to the instance to use as the VIP|

<br>

For configuring the load balancer in a private cloud environment, follow the procedure below.

For HA configuration of the load balancer, configure the VIP as follows:
- Create a new interface
- Assign a public IP to the newly created interface
- Assign the newly created interface to the load balancer instance

<br>

<details>
<summary>Install Keepalived and HAProxy</summary>
<br>

For HA configuration of the load balancer, install Keepalived and HAProxy on two load balancer instances.

<br>

Install Keepalived on the load balancer instance as follows:
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
|STATE|Set "MASTER" for the first load balancer instance and "BACKUP" for the second one|For single load balancer instance configuration, only set MASTER|
|INTERFACE_NAME|Check using `ifconfig` on each instance's shell| |
|VIP|The Private IP of the interface assigned to the load balancer instance|In the test environment, use the Private IP of the load balancer instance<br>In the production environment, use the separately configured VIP|

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

Install HAProxy on the load balancer instance.
```
# apt-get install -y haproxy
```

<br>

Configure HAProxy. Add the following at the bottom of the `haproxy.cfg` file.
```
# vi /etc/haproxy/haproxy.cfg
```

|Environment Variable|Description|Notes|
|---|---|---|
|VIP|The Private IP of the interface assigned to the load balancer instance|In the test environment, use the Private IP of the load balancer instance<br>In the production environment, use the separately configured VIP|
|MASTER_NODE_IP{n}|The Private IP of each Control Plane node|This example shows a configuration with three Control Plane nodes; add more according to your configuration|

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

### <div id='2.2'> 2.2. Generate And Distribute SSH Key]
To install the K-PaaS container platform cluster, the SSH Key must be copied to all servers in the inventory.<br>
This document (K-PaaS Container Platform Cluster Installation Guide) uses RSA public key for SSH access configuration.

After generating and distributing the SSH Key, all installation steps are performed on the ***`Install instance or the Control Plane node where the installation is carried out`***.

<br>

Generate the RSA public key on the ***`Install instance or the Control Plane node where the installation is carried out`***.
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

Copy the public key to the ***`Install instance, all Control Plane and Worker nodes of the cluster`***.
```
## Copy the outputted public key

$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br>

Add the public key at the end of the authorized_keys file (append it to the existing content) on ***`Install instance, all Control Plane and Worker nodes of the cluster`***.
```
$ vi .ssh/authorized_keys

ex)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRueywSiuwyfmCSecHu7iwyi3xYS1xigAnhR/RMg/Ws3yOuwbKfeDFUprQR24BoMaD360uyuRaPpfqSL3LS9oRFrj0BSaQfmLcMM1+dWv+NbH/vvq7QWhIszVCLzwTqlHrhgNsh0+EMhqc15KEo5kHm7d7vLc0fB5tZkmovsUFzp01Ceo9+Qye6+j+UM6ssxdTmatoMP3ZZKZzUPF0EZwTcGG6+8rVK2G8GhTqwGLj9E+As3GB1YdOvr/fsTAi2PoxxFsypNR4NX8ZTDvRdAUzIxz8wv2VV4mADStSjFpE7HWrzr4tZUjvvVFptU4LbyON9YY4brMzjxA7kTuf/e3j Generated-by-Nova
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5QrbqzV6g4iZT4iR1u+EKKVQGqBy4DbGqH7/PVfmAYEo3CcFGhRhzLcVz3rKb+C25mOne+MaQGynZFpZk4muEAUdkpieoo+B6r2eJHjBLopn5quWJ561H7EZb/GlfC5ThjHFF+hTf5trF4boW1iZRvUM56KAwXiYosLLRBXeNlub4SKfApe8ojQh4RRzFBZP/wNbOKr+Fo6g4RQCWrr5xQCZMK3ugBzTHM+zh9Ra7tG0oCySRcFTAXXoyXnJm+PFhdR6jbkerDlUYP9RD/87p/YKS1wSXExpBkEglpbTUPMCj+t1kXXEJ68JkMrVMpeznuuopgjHYWWD2FgjFFNkp ubuntu@cp-master
```

<br><br>

### <div id='2.3'> 2. Download K-PaaS Container Platform Cluster Deployment

> Starting from 2.3, this process is only performed on the ***`Install instance or the Control Plane node where the installation is carried out`***.

<br>

Download the necessary Deployment for the K-PaaS container platform cluster and move to the installation path to begin the installation.

> K-PaaS Container Platform Cluster Deployment Download URL: https://github.com/k-paas/cp-deployment

<br>

Use the git clone command to download the K-PaaS Container Platform Cluster Deployment to the HOME directory path.
```
$ git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.6.x
```

<br><br>

### <div id='2.4'> 2.4. Preparing To Install The K-PaaS Container Platform Cluster

The installation of the K-PaaS container platform cluster proceeds after pre-defining the necessary environment variables and running installation through shell scripts.

> For multi-cloud deployments, there are some differences in the installation path and the components of the vars file.

<br>

Navigate to the installation directory of the K-PaaS container platform cluster.
```
$ cd ~/cp-deployment/multi
```

<br>

Set the number of multi-clusters for the K-PaaS container platform and create an environment variable script file.
```
$ vi create-vars.sh
```

```
...
export CLUSTER_CNT={Number of clusters}
...
```

```
$ source create-vars.sh
```

<br>

Enter the environment variables required for the installation of the K-PaaS container platform cluster.
```
## K-PaaS Container Platform Cluster Environment Variables
$ vi cp-cluster-vars.sh
```

<br>

Cluster separation is done by prefixing the environment variables with CLUSTER{n}_ .<br>
For example, CLUSTER1_KUBE_CONTROL_HOSTS, CLUSTER2_KUBE_CONTROL_HOSTS, CLUSTER3_KUBE_CONTROL_HOSTS...

Control Plane

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_CONTROL_HOSTS|Number of Control Plane nodes||
|ETCD_TYPE|ETCD deployment method<br>external : ETCD configured on a separate node<br>stacked : ETCD configured on the Control Plane node|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|LOADBALANCER_DOMAIN|VIP or Domain information of the pre-configured load balancer|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more|
|ETCD1_NODE_HOSTNAME|Hostname of ETCD node 1|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set when **`ETCD_TYPE`** value is external|
|ETCD1_NODE_PRIVATE_IP|Private IP of ETCD node 1|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set when **`ETCD_TYPE`** value is external|
|ETCD{n}_NODE_HOSTNAME|Hostname of ETCD node n|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set when **`ETCD_TYPE`** value is external<br>Set for each **`KUBE_CONTROL_HOSTS`**|
|ETCD{n}_NODE_PRIVATE_IP|Private IP of ETCD node n|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set when **`ETCD_TYPE`** value is external<br>Set for each **`KUBE_CONTROL_HOSTS`**|
|MASTER1_NODE_HOSTNAME|Hostname of Control Plane node 1||
|MASTER1_NODE_PUBLIC_IP|Public IP of Control Plane node 1|Public IP information is only needed for Control Plane node 1|
|MASTER1_NODE_PRIVATE_IP|Private IP of Control Plane node 1||
|MASTER{n}_NODE_HOSTNAME|Hostname of Control Plane node n|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set for each **`KUBE_CONTROL_HOSTS`**|
|MASTER{n}_NODE_PRIVATE_IP|Private IP of Control Plane node n|Set when **`KUBE_CONTROL_HOSTS`** value is 2 or more<br>Set for each **`KUBE_CONTROL_HOSTS`**|

<br>

Worker

|Environment Variable|Description|Remarks|
|---|---|---|
|KUBE_WORKER_HOSTS|Number of Worker nodes||
|WORKER1_NODE_HOSTNAME|Hostname of Worker node 1||
|WORKER1_NODE_PRIVATE_IP|Private IP of Worker node 1||
|WORKER{n}_NODE_HOSTNAME|Hostname of Worker node n|Set for each **`KUBE_WORKER_HOSTS`**|
|WORKER{n}_NODE_PRIVATE_IP|Private IP of Worker node n|Set for each **`KUBE_WORKER_HOSTS`**|

<br>

Storage

|Environment Variable|Description|Remarks|
|---|---|---|
|STORAGE_TYPE|Storage information<br>nfs : NFS storage<br>rook-ceph : Rook Ceph storage||
|NFS_SERVER_PRIVATE_IP|Private IP of the NFS Server instance|Set when **`STORAGE_TYPE`** is nfs|

<br>

> Multi-cloud deployment replaces the functionality of the Ingress Nginx Controller with Istio IngressGateway.

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
|ISTIO_GATEWAY_PUBLIC_IP|Public IP assigned to the interface for Istio Gateway Service (interface, load balancer service)|Ensure it does not overlap with **`METALLB_IP_RANGE`**|

<br>

```
#!/bin/bash

export CLUSTER_CNT=3

#################################
# CLUSTER1
#################################

# Master Node Count Variable (eg. 1, 3, 5 ...)
export CLUSTER1_KUBE_CONTROL_HOSTS=

# if KUBE_CONTROL_HOSTS > 1 (eg. external, stacked)
export CLUSTER1_ETCD_TYPE=

# if KUBE_CONTROL_HOSTS > 1
# HA Control Plane LoadBalanncer IP or Domain
export CLUSTER1_LOADBALANCER_DOMAIN=

# if ETCD_TYPE=external
# The number of ETCD node variable is set equal to the number of KUBE_CONTROL_HOSTS
export CLUSTER1_ETCD1_NODE_HOSTNAME=
export CLUSTER1_ETCD1_NODE_PRIVATE_IP=
export CLUSTER1_ETCD2_NODE_HOSTNAME=
export CLUSTER1_ETCD2_NODE_PRIVATE_IP=
export CLUSTER1_ETCD3_NODE_HOSTNAME=
export CLUSTER1_ETCD3_NODE_PRIVATE_IP=

...

# MetalLB Ingress Nginx Controller LoadBalancer Service External IP
export CLUSTER1_INGRESS_NGINX_IP=

# MetalLB Istio Gateway LoadBalancer Service External IP
export CLUSTER1_ISTIO_GATEWAY_PRIVATE_IP=
export CLUSTER1_ISTIO_GATEWAY_PUBLIC_IP=

#################################
# CLUSTER2
#################################

# Master Node Count Variable (eg. 1, 3, 5 ...)
export CLUSTER2_KUBE_CONTROL_HOSTS=

# if KUBE_CONTROL_HOSTS > 1 (eg. external, stacked)
export CLUSTER2_ETCD_TYPE=

# if KUBE_CONTROL_HOSTS > 1
# HA Control Plane LoadBalanncer IP or Domain
export CLUSTER2_LOADBALANCER_DOMAIN=

# if ETCD_TYPE=external
# The number of ETCD node variable is set equal to the number of KUBE_CONTROL_HOSTS
export CLUSTER2_ETCD1_NODE_HOSTNAME=
export CLUSTER2_ETCD1_NODE_PRIVATE_IP=
export CLUSTER2_ETCD2_NODE_HOSTNAME=
export CLUSTER2_ETCD2_NODE_PRIVATE_IP=
export CLUSTER2_ETCD3_NODE_HOSTNAME=
export CLUSTER2_ETCD3_NODE_PRIVATE_IP=

...
```

<br><br>

### <div id='2.5'> 2.5. Installing The K-PaaS Container Platform Cluster
The installation of the K-PaaS container platform cluster proceeds sequentially by installing necessary packages, setting environment variables for the cluster installation, and deploying the K-PaaS container platform cluster via Ansible playbook.

```
$ source deploy-cp-cluster.sh
```

<br><br>

### <div id='2.6'> 2.6. Verifying The K-PaaS Container Platform Cluster Installation
Verify the K-PaaS container platform cluster installation by checking the nodes and Pods in the kube-system namespace. 
The information for nodes and Pods displayed may vary depending on the configuration, and below is the output when deployed with a single Control Plane configuration.

```
$ kubectl get nodes --context=cluster1
NAME                   STATUS   ROLES                  AGE   VERSION
cp-cluster1-master     Ready    control-plane          12m   v1.30.4
cp-cluster1-worker-1   Ready    <none>                 10m   v1.30.4
cp-cluster1-worker-2   Ready    <none>                 10m   v1.30.4
cp-cluster1-worker-3   Ready    <none>                 10m   v1.30.4

$ kubectl get pods -n kube-system --context=cluster1
NAME                                          READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh       1/1     Running   0             9m22s
calico-node-d8sg6                             1/1     Running   0             9m22s
calico-node-kfvjx                             1/1     Running   0             10m
calico-node-khwdz                             1/1     Running   0             10m
calico-node-nc58v                             1/1     Running   0             10m
coredns-657959df74-td5c2                      1/1     Running   0             8m15s
coredns-657959df74-ztnjj                      1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0             8m9s
kube-apiserver-cp-cluster1-master             1/1     Running   0             12m
kube-controller-manager-cp-cluster1-master    1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                              1/1     Running   0             10m
kube-proxy-kkvhk                              1/1     Running   0             10m
kube-proxy-nfttc                              1/1     Running   0             10m
kube-proxy-znfgk                              1/1     Running   0             10m
kube-scheduler-cp-cluster1-master             1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0             7m57s
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7grn                            1/1     Running   0             8m8s
```

<br>

```
$ kubectl get nodes --context=cluster2
NAME                   STATUS   ROLES                  AGE   VERSION
cp-cluster2-master     Ready    control-plane          12m   v1.30.4
cp-cluster2-worker-1   Ready    <none>                 10m   v1.30.4
cp-cluster2-worker-2   Ready    <none>                 10m   v1.30.4
cp-cluster2-worker-3   Ready    <none>                 10m   v1.30.4

$ kubectl get pods -n kube-system --context=cluster2
NAME                                          READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh       1/1     Running   0             9m22s
calico-node-d8sg6                             1/1     Running   0             9m22s
calico-node-kfvjx                             1/1     Running   0             10m
calico-node-khwdz                             1/1     Running   0             10m
calico-node-nc58v                             1/1     Running   0             10m
coredns-657959df74-td5c2                      1/1     Running   0             8m15s
coredns-657959df74-ztnjj                      1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0             8m9s
kube-apiserver-cp-cluster2-master             1/1     Running   0             12m
kube-controller-manager-cp-cluster2-master    1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                              1/1     Running   0             10m
kube-proxy-kkvhk                              1/1     Running   0             10m
kube-proxy-nfttc                              1/1     Running   0             10m
kube-proxy-znfgk                              1/1     Running   0             10m
kube-scheduler-cp-cluster2-master             1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0             7m57s
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7grn                            1/1     Running   0             8m8s
```

<br>

```
$ kubectl get nodes --context=cluster3
NAME                   STATUS   ROLES                  AGE   VERSION
cp-cluster3-master     Ready    control-plane          12m   v1.30.4
cp-cluster3-worker-1   Ready    <none>                 10m   v1.30.4
cp-cluster3-worker-2   Ready    <none>                 10m   v1.30.4
cp-cluster3-worker-3   Ready    <none>                 10m   v1.30.4

$ kubectl get pods -n kube-system --context=cluster3
NAME                                          READY   STATUS    RESTARTS      AGE
calico-kube-controllers-b5f8f6849-hhbgh       1/1     Running   0             9m22s
calico-node-d8sg6                             1/1     Running   0             9m22s
calico-node-kfvjx                             1/1     Running   0             10m
calico-node-khwdz                             1/1     Running   0             10m
calico-node-nc58v                             1/1     Running   0             10m
coredns-657959df74-td5c2                      1/1     Running   0             8m15s
coredns-657959df74-ztnjj                      1/1     Running   0             8m7s
dns-autoscaler-b5c786945-rhlkd                1/1     Running   0             8m9s
kube-apiserver-cp-cluster2-master             1/1     Running   0             12m
kube-controller-manager-cp-cluster2-master    1/1     Running   1 (11m ago)   12m
kube-proxy-dj5c8                              1/1     Running   0             10m
kube-proxy-kkvhk                              1/1     Running   0             10m
kube-proxy-nfttc                              1/1     Running   0             10m
kube-proxy-znfgk                              1/1     Running   0             10m
kube-scheduler-cp-cluster2-master             1/1     Running   1 (11m ago)   12m
metrics-server-5cd75b7749-xcrps               2/2     Running   0             7m57s
nodelocaldns-556gb                            1/1     Running   0             8m8s
nodelocaldns-8dpnt                            1/1     Running   0             8m8s
nodelocaldns-pvl6z                            1/1     Running   0             8m8s
nodelocaldns-x7gr
```

<br><br>

## <div id='3'> 3. Deleting a K-PaaS Container Platform Cluster (Reference)
To delete the K-PaaS container platform cluster, use a shell script as follows:

```
$ source reset-cp-cluster.sh
```

<br><br>

## <div id='4'> 4. Caution For Creating Resources
When users create resources manually, avoid using the following prefixes:

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

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Cluster Installation Guide (Multi)
