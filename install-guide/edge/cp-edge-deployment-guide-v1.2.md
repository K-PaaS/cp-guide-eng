### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Edge Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Range](#1.2)  
  1.3. [System Configuration Diagram](#1.3)  
  1.4. [References](#1.4)  

2. [KubeEdge Installation](#2)  
  2.1. [Prerequisite](#2.1)  
  2.2. [Kubernetes Native Cluster Deployment](#2.2)  
  2.3. [KubeEdge keadm Installation](#2.3)  
  2.4. [KubeEdge CloudCore Installation](#2.4)  
  2.5. [KubeEdge EdgeCore Installation](#2.5)  
  2.6. [Change DaemonSet Settings](#2.6)  
  2.7. [Enable the kubectl logs feature](#2.7)  
  2.8. [EdgeMesh Deployment](#2.8)  
  2.9. [KubeEdge Installation Check](#2.9)  

3. [KubeEdge Reset (Refer)](#3)  

4. [Create Container Platform Operator and Acquire Token](#4)  
  4.1. [Create Cluster Role Operator and Acquire Token](#4.1)  
  4.2. [Acquire Namespace User Token](#4.2)  

5. [Precautions When Creating Resource](#5)

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (KubeEdge Installation Guide) describes how to install KubeEdge to upgrade the open PaaS platform and install a container platform deployment to Open PaaS based on a developer support environment.

Starting with K-PaaS 5.5 version, it supports exclusive deployment based on KubeEdge. To install it based on an existing Container service, refer to the K-PaaS 5.0 or lower version of the document. 

<br>

### <div id='1.2'> 1.2. Range
The installation range was prepared based on the basic installation to verify KubeEdge.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system configuration consists of a Kubernetes Cluster (Master, Worker, Edge) environment. <br>
Install the Kubernetes Cluster (Master, Worker) through Kubespray and KubeEdge in the Kubernetes Cluster and Edge environment. The pod provides middleware environments such as Database and Private registry to deploy to Container Platform portal environments to Kubernetes clusters with Container Image. <br>
For the total required VM environment,  **1 Master VM, 1 or more Worker VM, 1 or more Edge VM** are required. This document contains the installation of Master VM, Worker VM, and Edge VM to configure the Kubernetes Cluster environment.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://docs.docker.com/engine/install/  
> https://kubeedge.io/en/docs/   
> https://github.com/kubeedge/kubeedge

<br>

## <div id='2'> 2. KubeEdge Installation

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installation in the **Ubuntu 18.04** environment. For EdgeNode, **arm64 architecture**, perform installation in **Ubuntu 20.04** environment for CRI-O installation. In this guide, the environment of EdgeNode was written based on **Ubuntu 20.04 arm64**. For KubeEdge installation, CRI-O, Kubernetes Native Cluster must be deployed on the system.

The main software and package version information required for KubeEdge installation are as follows.

|Main Software|Version|
|---|---|
|KubeEdge|v1.8.2|
|Kubernetes Native|v1.20.5|
|Kubernetes Native (Edge Node)|v1.19.3|
|CRI-O|v1.20.0|
|CRI-O (Edge Node)|v1.19.0|

The Kubernetes official guide document recommends the following when deploying clusters:

- One or more machines running a debug / rpm compatible Linux OS (Ubuntu or CentOS)
- More than 2G RAM per machine
- Two or more CPUs on a machine that is used as a control-plane node
- Full network connectivity between all systems in the cluster

#### Firewall Insformation
- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 179 | Calio BGP Network |  
| TCP | 2049 | NFS |  
| TCP | 2379-2380 | etcd server client API |  
| TCP | 6443 | kubernetes API Server |  
| TCP | 9443 | cloudcore router port |  
| TCP | 10000-10004 | cloudHub websocket port |  
| TCP | 10001 | cloudHub quic port |  
| TCP | 10002 | cloudHub https port |  
| TCP | 10003 | cloudStream streamPort |  
| TCP | 10004 | cloudStream tunnelPort |  
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

- Edge Node

| <center>Protocol</center> | <center>Port</center> | <center>Note</center> |  
| :---: | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 1883-1884 | eventBus mqttPort |  
| TCP | 2049 | NFS |  
| TCP | 10001 | edgeHub quic port |  
| TCP | 10250 | Kubelet API |  
| TCP | 10255 | Read-Only Kubelet API |  
| TCP | 10350 | Use kubectl logs |  
| TCP | 10550 | edgecore list-watch port |  
| TCP | 30000-32767 | NodePort Services |  
| TCP | 40001 | edgeMesh listenPort |

<br>

### <div id='2.2'> 2.2. Kubernetes Native Cluster Deployment
For KubeEdge installation, the Kubernetes cluster must be deployed in the Cloud area, and after deployment, the Edge Node must be deployed in the Edge area.

- Deploy Kubernetes Cluster through Kubespray in the Cloud area.

> [cp-standalone-deployment-guide](../standalone/cp-standalone-deployment-guide-v1.2.md)

<br>

### <div id='2.3'> 2.3. KubeEdge keadm Installation
Install keadm for installing KubeEdge. Super User or root authentication is required when running keadm, so proceed with the installation with **root authentication**.

- Download and install keadm on VMs that will be used as **Master Node** in Cloud and **Edge Node** in Edge.

```
$ sudo su -

# git clone https://github.com/K-PaaS/cp-deployment.git -b branch_v1.2.x

## When Ubuntu architecture is amd64 (ex: Cloud Area Master Node)
# cp cp-deployment/edge/keadm/amd64/keadm /usr/bin/keadm

## When Ubuntu architecture is arm64 (ex: Edge Area Edge Node)
# cp cp-deployment/edge/keadm/arm64/keadm /usr/bin/keadm
```

<br>

### <div id='2.4'> 2.4. KubeEdge CloudCore Installation
Install KubeEdge CloudCore on the Master Node in the Cloud area and proceed with the setup.

- Install CloudCore in **Master Node** at the cloud area with the keadm init command.
```
## {MASTER_PUB_IP} : Master Node Public IP
## {MASTER_PRIV_IP} : Master Node Private IP

# keadm init --advertise-address={MASTER_PUB_IP} --kubeedge-version 1.8.2 --master=https://{MASTER_PRIV_IP}:6443
```

- Get the Token value for installing Edge Core in the Edge area.
```
# keadm gettoken
```

<br>

### <div id='2.5'> 2.5. KubeEdge EdgeCore Installation
After pre-installing CRI-O on **Edge Node** in the Edge area, install KubeEdge Edge Core to proceed with the setup. In the case of EdgeNode, the installation should proceed in **Ubuntu 20.04** environment for CRI-Oarm64 installation.

- If the EdgeNode environment is **Raspberry Pi**, add the following information. If it is not a Raspberry Pi environment, skip the following two steps and proceed from the Edge Node CRI-O installation process.
```
# vi /boot/firmware/cmdline.txt

... cgroup_enable=memory cgroup_memory=1 (Add at the very last)
```

- Before installing CRI-O, there is an APT issue in the **Raspberry Pi Ubuntu 20.04** arm64 version, and the following action is taken.
```
# killall apt apt-get

# rm /var/lib/apt/lists/lock
# rm /var/cache/apt/archives/lock
# rm /var/lib/dpkg/lock*

# dpkg --configure -a

# apt-get update
```

- Process **Raspberry Pi** Reboot.
```
# reboot
```

- Process CRI-O installation at **Edge Node**.
```
## When installing CRI-O after the Raspberry Pi Reboot, switch to Root authority.
$ sudo su -

# cd cp-deployment/edge

# source crio-install.sh
```

- Install CNI Plugin for CRI-O on**Edge Node**.
```
# mkdir -p /opt/cni/bin
# cp cni-plugins/* /opt/cni/bin/
```

- Register and launch CRI-O service at **Edge Node**.
```
# source enable-crio.sh
```

- Proceed with EdgeCore installation with the keadm join command at **Edge Node**.
```
## {MASTER_PUB_IP} : Master Node Public IP
## {GET_TOKEN} : Token value called after CloudCore installation in the Cloud area

# keadm join --cloudcore-ipport={MASTER_PUB_IP}:10000 --token={GET_TOKEN} --cgroupdriver systemd --remote-runtime-endpoint unix:///var/run/crio/crio.sock --runtimetype remote --kubeedge-version 1.8.2
```

<br>

### <div id='2.6'> 2.6. Change DaemonSet Settings
Since KubeEdge does not support Ingress and CNI at the time of writing this installation guide, measures are needed to prevent Ingress Controller and Calico CNI from being deployed on Edge Node. In addition, OpenStack will take additional measures to prevent csi cinder nodeplugin from being deployed.

- Modify DaemonSet yaml to prevent Ingress Controller from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps ingress-nginx-controller -n ingress-nginx
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- Modify DaemonSet yaml to prevent Calico CNI from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps calico-node -n kube-system
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- Modify DaemonSet yaml to prevent Node Local DNS from being deployed on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps nodelocaldns -n kube-system
```

- Add the following content at the spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

- In the case of OpenStack, modify DaemonSet yaml to prevent csi cinder nodeplugin on Edge Node at **Master Node**.
```
# kubectl edit daemonsets.apps csi-cinder-nodeplugin -n kube-system
```

- Add the following content at spec.template.spec path.
```
     affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/edge
                operator: DoesNotExist
```

<br>


### <div id='2.7'> 2.7. Enable the kubectl logs feature
In KubeEdge, there is an issue that cannot use the kubectl logs command as basic. The installation guide provides a setting guide for activating a corresponding function.  

- Modify the cloudcore.yaml file at **Master Node**. (change enable: true)
```
# vi /etc/kubeedge/config/cloudcore.yaml
```

```
cloudStream:
  enable: true (modified)
  streamPort: 10003
  tlsStreamCAFile: /etc/kubeedge/ca/streamCA.crt
  tlsStreamCertFile: /etc/kubeedge/certs/stream.crt
  tlsStreamPrivateKeyFile: /etc/kubeedge/certs/stream.key
  tlsTunnelCAFile: /etc/kubeedge/ca/rootCA.crt
  tlsTunnelCertFile: /etc/kubeedge/certs/server.crt
  tlsTunnelPrivateKeyFile: /etc/kubeedge/certs/server.key
  tunnelPort: 10004
```

- Modify the IP information in the script to activate the kubectl logs function and execute it at the **Master Node**.
```
# cd cp-deployment/edge

# vi enable-logs.sh

export CLOUDCOREIPS="{MASTER_PUB_IP}" (Modified)
...
```

```
# source enable-logs.sh
```

- Restart cloudcore at the **Master Node**.
```
# source restart-cloudcore.sh
```

- Modify edgecore.yaml file at the **Edge Node**. (enable: true)
```
# vi /etc/kubeedge/config/edgecore.yaml
```

```
edgeStream:
  enable: true (Modified)
  handshakeTimeout: 30
  readDeadline: 15
  server: xxx.xxx.xxx.xxx:10004
  tlsTunnelCAFile: /etc/kubeedge/ca/rootCA.crt
  tlsTunnelCertFile: /etc/kubeedge/certs/server.crt
  tlsTunnelPrivateKeyFile: /etc/kubeedge/certs/server.key
  writeDeadline: 15
```

Describes how to bypass the deployment of kube-proxy on edgecore restart because edgecore restart is not possible if kube-proxy is deployed. 

- Modify edgecore.service file at the **EdgeNode**.
```
# vi /etc/kubeedge/edgecore.service
```

- Add the following at the [Service] of edgecore.service file.
```
Environment="CHECK_EDGECORE_ENVIRONMENT=false"
```

- Restart edgecore at **Edge Node**.
```
# source restart-edgecore.sh
```

<br>

### <div id='2.8'> 2.8. EdgeMesh Deployment 
Starting with KubeEdge v1.8, EdgeMesh is separated from EdgeCore modules as a separate Pod, providing the EdgeMesh Server, and Agent Pod deployment guide.

- **Master Node** deploys relevant CRDs before Edge Mesh Pod is deployed.
```
#  kubectl apply -f edgemesh/crds/istio/
```

-  Enable List-Watch on EdgeNode by changing Edge Core settings and restarting services on **Edge Node**.
```
# vi /etc/kubeedge/config/edgecore.yaml
```

```
modules:
  ..
  edgeMesh: (Added)
    enable: false (Added)
  ..
  metaManager:
    metaServer:
      enable: true (Modified)
..
```

```
# source restart-edgecore.sh
```

- change the settings of CloudCore and restart the service at **Master Node**.
```
# vi /etc/kubeedge/config/cloudcore.yaml
```

```
modules:
  ..
  dynamicController:
    enable: true (Modified)
..
```

```
# source restart-cloudcore.sh
```

- On **Master Node**, modify the hostname information for the VM where the EdgeMesh Server will be deployed.
```
# vi edgemesh/server/06-deployment.yaml

...
spec:
  hostNetwork: true
#     use label to selector node
  nodeName: {MASTER_HOSTNAME} (Modified)
...
```

- Deploy EdgeMesh Server at **Master Node**.
```
# kubectl apply -f edgemesh/server/
```

- Deploy EdgeMesh Agent at **Master Node**.
```
# kubectl apply -f edgemesh/agent/
```

<br>

### <div id='2.9'> 2.9. KubeEdge Installation Check
Check the Pod of the Kubernetes Node and kube-system Namespace to verify the installation of the KubeEdge.

```
# kubectl get nodes
NAME                 STATUS   ROLES                  AGE     VERSION
cp-edge       Ready    agent,edge             5m40s   v1.19.3-kubeedge-v1.8.2
cp-master     Ready    control-plane,master   39m     v1.20.5
cp-worker-1   Ready    <none>                 38m     v1.20.5
cp-worker-2   Ready    <none>                 38m     v1.20.5
cp-worker-3   Ready    <none>                 38m     v1.20.5

# kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-7c5b64bf96-5qdqv   1/1     Running   0          37m
calico-node-4hbw5                          1/1     Running   0          4m34s
calico-node-8q5tv                          1/1     Running   0          5m9s
calico-node-qlq5k                          1/1     Running   0          5m26s
calico-node-xc55c                          1/1     Running   0          4m53s
coredns-657959df74-grflz                   1/1     Running   0          37m
coredns-657959df74-wbdl6                   1/1     Running   0          37m
dns-autoscaler-b5c786945-cbcv9             1/1     Running   0          37m
kube-apiserver-cp-master            1/1     Running   0          36m
kube-controller-manager-cp-master   1/1     Running   1          39m
kube-proxy-2ckfd                           1/1     Running   0          38m
kube-proxy-hb8p2                           1/1     Running   0          38m
kube-proxy-nnh6d                           1/1     Running   0          38m
kube-proxy-p9srm                           1/1     Running   0          6m4s
kube-proxy-zmp95                           1/1     Running   0          38m
kube-scheduler-cp-master            1/1     Running   1          39m
metrics-server-5cd75b7749-57sc2            2/2     Running   0          37m
nginx-proxy-cp-worker-1             1/1     Running   0          38m
nginx-proxy-cp-worker-2             1/1     Running   0          38m
nginx-proxy-cp-worker-3             1/1     Running   0          38m
nodelocaldns-24vq4                         1/1     Running   0          6m4s
nodelocaldns-jjrjj                         1/1     Running   0          37m
nodelocaldns-kgzxb                         1/1     Running   0          37m
nodelocaldns-l9s47                         1/1     Running   0          37m
```

<br>

## <div id='3'> 3. KubeEdge Reset (Refer)
Stop KubeEdge from Cloud Side and Edge Side. Do not delete the required components.

- Stop cloudcore on the Cloud Side and delete KubeEdge-related resources on the Kubernetes Master, such as kubeEdge Namespace.
```
# keadm reset --kube-config=$HOME/.kube/config
```

- Stop edgecore from Edge Side.
```
# keadm reset
```

<br>

## <div id='4'> 4. Create Container Platform Operator and Acquire Token (Refer)

### <div id='4.1'> 4.1. Create Cluster Role Operator and Acquire Token
After installing KubeEdge, create a service account for an operator with a Cluster Role. Token of the service account is used to create a Super Admin account in the operator portal.

- Create Service Account.
```
## {SERVICE_ACCOUNT} : Service Account Name

$ kubectl create serviceaccount {SERVICE_ACCOUNT} -n kube-system
(eg. kubectl create serviceaccount k8sadmin -n kube-system)
```

- Bind at the Cluster Role at the Service Account created.
```
$ kubectl create clusterrolebinding {SERVICE_ACCOUNT} --clusterrole=cluster-admin --serviceaccount=kube-system:{SERVICE_ACCOUNT}
```

- Acquire the Token of the created Service Account.
```
## {SECRET_NAME} : Check Mountable secrets Value

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

### <div id='4.2'> 4.2. Acquire Namespace User Token
It is used to obtain the Token value after the creation of Namespace and user registration in the portal.

- Acquired Token of Namespace User.
```
## {SECRET_NAME} : Check Mountable secrets Value
## {NAMESPACE} : Name of Namespace 

$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n {NAMESPACE}

$ kubectl describe secret {SECRET_NAME} -n {NAMESPACE} | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

## <div id='5'> 5. Precautions When Creating Resource
When users create their resources, be careful not to use the following prefixes.

|Resource Name|prefix to exclude when creating|
|---|---|
|All Resource|kube*|
|Namespace|all|
||kubernetes-dashboard|
||container-platform-temp-namespace|
|Role|container-platform-init-role|
||container-platform-admin-role|
|ResourceQuota|container-platform-low-rq|
||container-platform-medium-rq|
||container-platform-high-rq|
|LimitRanges|container-platform-low-limit-range|
||container-platform-medium-limit-range|
||container-platform-high-limit-range|
|Pod|nodes|
||resources|

<br>

[image 001]:images/edge-v1.2.png

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Edge Installation Guide
