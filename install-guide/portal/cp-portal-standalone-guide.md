### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Single Cluster Container Platform Portal Installation Guide

<br>

## Table of Contents

1. [Document Overview](#1)  
   1.1. [Purpose](#1.1)  
   1.2. [Range](#1.2)  
   1.3. [System Configuration](#1.3)  
   1.4. [Reference](#1.4)

2. [Reference](#2)  
   2.1 [Prerequisite](#2.1)  
   2.2. [Firewall Information](#2.2)    
   2.3. [Installation List](#2.3)

3. [Deploy the Container Platform Portal](#3)   
   3.1. [Download the Container Platform Portal Deployment](#3.1)  
   3.2. [Define Container Platform Portal Variables](#3.2)    
   3.3. [Run the Container Platform Portal Deployment Script](#3.3)  
   3.4. [(Reference) Delete Container Platform Portal Resources](#3.4)

4. [Access the Container Platform Portal](#4)      
   4.1. [Login to the Container Platform Portal Administrator Account](#4.1)      
   4.2. [Login to the Container Platform Portal User Account](#4.2)      
   4.3. [Container Platform Portal Use Guide](#4.3)

5. [Refer to the Container Platform Portal](#5)       
   5.1. [Caution for creating Kubernetes resources](#5.1)

<br>

## <span id='1'>1. Document Overview
### <span id='1.1'>1.1. Purpose
This document (Single Cluster Container Platform Portal Deployment Guide) describes how to install a Kubernetes cluster and deploy the Container Platform Portal. <br><br>


### <span id='1.2'>1.2. Range
The installation scope is based on a Kubernetes cluster deployment.

<br>

### <span id='1.3'>1.3. System Configuration
<p align="center"><img src="../images/portal/cp-001.png" width="850" height="530"></p>

The system configuration consists of a **Kubernetes cluster (master, worker)** environment and a storage server for data management.
It provides a middleware environment as a container, including **OpenBao** to manage secret information and authentication data,
**MariaDB (RDBMS)** to manage metadata, **Harbor** to manage container images, **Keycloak** to manage container platform portal user authentication,
**ChartMuseum** to manage Helm charts, and **Chaos Mesh** to simulate various types of failures in Kubernetes. 
The total required VM environment is at least **1 Master VM and 3 Worker** VMs. 
This document is about deploying a container platform portal environment on a Kubernetes cluster.



<br>    

### <span id='1.4'>1.4. Reference
> https://kubernetes.io<br>

<br>

## <span id='2'>2. Reference

### <span id='2.1'>2.1. Prerequisite
This installation guide is based on **Ubuntu 22.04** installation.

<br>

### <span id='2.2'>2.2. Firewall Information
Set the port that should be opened for the IaaS Security Group.

- Master Node

| <center>Protocol</center> | <center>Port</center> | <center>Description</center> |  
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

- Worker Node

| <center>Protocol</center> | <center>Port</center> | <center>Description</center> |  
| :--- | :---: | :--- |  
| TCP | 111 | NFS PortMapper |  
| TCP | 2049 | NFS |  
| TCP | 10250 | Kubelet API |  
| TCP | 10255 | Read-Only Kubelet API |  
| TCP | 30000-32767 | NodePort Services |  
| UDP | 4789 | Calico networking VXLAN |  


<br>

### <span id='2.3'>2.3. Installation List
The service information included and deployed in the container platform portal is as follows.
|Service| Application Version| Chart Version|
|:--- | :---:|  :---: |  
|[OpenBao](https://github.com/openbao/openbao)|2.2.0|0.12.0|
|[MariaDB](https://github.com/mariadb)|11.4.7|20.5.6|
|[Harbor](https://github.com/goharbor/harbor)|2.13.1|1.17.1|
|[Keycloak](https://github.com/keycloak/keycloak)|25.0.6|23.0.0|
|[ChartMuseum](https://github.com/helm/chartmuseum)|0.16.3|3.10.4|
|[Chaos Mesh](https://github.com/chaos-mesh/chaos-mesh)|2.7.2|2.7.2|

<br>

## <span id='3'>3. Deploy the Container Platform Portal

### <span id='3.1'>3.1. Download the Container Platform Portal Deployment
For container platform portal deployment, download the container platform portal deployment file and place it in the path below. <br>
:bulb: This will be done on the **Master Node**.

+ Download the Container Platform Portal Deployment file :
  [cp-portal-deployment-v1.6.1.1.tar.gz](https://nextcloud.k-paas.org/index.php/s/jyjGsowwx3AHNPk/download)

```bash
# Create Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment File and Verify File
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/jyjGsowwx3AHNPk/download

$ ls ~/workspace/container-platform
  cp-portal-deployment-v1.6.1.1.tar.gz

# Decompress Deployment Files
$ tar -xvf cp-portal-deployment-v1.6.1.1.tar.gz
```

- Configure the Deployment File Directory
```bash
cp-portal-deployment
├── script          # (Single) Variables and script files for portal deployment
├── script_mc       # (Multi) Variables and script files for portal deployment
├── images          # Images file
├── charts          # Helm chart file
├── values_orig     # Helm chart values file
├── secmg_orig      # Secrets management deployment file
└── istio_mc        # Istio Service Mesh Related File
```

<br>

### <span id='3.2'>3.2. Define Container Platform Portal Variables
Define the variable values required for container platform portal deployment.
```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/script
$ vi cp-portal-vars.sh
```

```bash                                                 
# COMMON VARIABLE (Please change the value of the variables below.)
K8S_MASTER_NODE_IP="{k8s master node public ip}"                     # Kubernetes Master Node Public IP
K8S_CLUSTER_API_SERVER="https://${K8S_MASTER_NODE_IP}:6443"          # kubernetes API Server (e.g. https://${K8S_MASTER_NODE_IP}:6443)
K8S_STORAGECLASS="cp-storageclass"                                   # Kubernetes StorageClass Name (e.g. cp-storageclass)
HOST_CLUSTER_IAAS_TYPE="1"                                           # Kubernetes Cluster IaaS Type ([1] AWS, [2] OPENSTACK, [3] NAVER, [4] NHN, [5] KT)
HOST_DOMAIN="{host domain}"                                          # Host Domain (e.g. xx.xxx.xxx.xx.nip.io)
```
```bash    
# (Example input values)
K8S_MASTER_NODE_IP="103.xxx.xxx.xxx"
K8S_CLUSTER_API_SERVER="https://${K8S_MASTER_NODE_IP}:6443"
K8S_STORAGECLASS="cp-storageclass"
HOST_CLUSTER_IAAS_TYPE="2"
HOST_DOMAIN="105.xxx.xxx.xxx.nip.io"
```

|Variable|Description|Additional|
|---|---|---|
|**K8S_MASTER_NODE_IP**|Enter Kubernetes Master Node Public IP|Enter Worker Node Public IP if Master Node is not accessible| 
|**K8S_CLUSTER_API_SERVER**|Enter Kubernetes API Server URL|The cluster deployed through the container platform is `https://${K8S_MASTER_NODE_IP}:6443` by default.<br>If the port number 6443 of the Master Node is not in the receiving format, modify the value. <br><br>:small_blue_diamond:**In case of HA Control Plane configuration**<br> Enter `https://{Load Balancer IP or Domain}:6443`|
|**K8S_STORAGECLASS**|Enter StorageClass Name|The cluster deployed through the container platform is <br> `cp-storageclass` by default. <br> When using another StorageClass, enter the resource name.|
|**HOST_CLUSTER_IAAS_TYPE**|Enter Kubernetes Cluster IaaS Environment|[1] AWS [2] OPENSTACK [3] NAVER [4] NHN [5] KT |
|**HOST_DOMAIN**|Enter Host Domain|Enter `{ingress-nginx-controller Service EXTERNAL-IP}.nip.io` <br> [Check the contents below](#host_domain)|

#### Viewing resources
```bash
# Get Kubernetes API Server
## If the port number 6443 of the Master Node is not in the receiving format
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://63c4f2d9-xxxx.xxxx.com (Check)

# Get StorageClass
$ kubectl get storageclass 
NAME                   PROVISIONER
block-storage (Check)  blk.csi...
```
#### HOST_DOMAIN
Use the **EXTERNAL-IP** (external accessible IP) of the Ingress NGINX Controller and free wildcard DNS service **nip.io**.

The container platform portal in a single-cluster environment routes each service through Kubernetes resource ingress and includes the following two services in the cluster installation.
> <b>[MetalLB](https://metallb.universe.tf/)</b> (load-balancer implementation for bare metal Kubernetes clusters)<br>
> <b>[Ingress NGINX Controller](https://kubernetes.github.io/ingress-nginx/)</b> (Kubernetes Ingress NGINX Controller) <br>

If the EXTERNAL-IP of the Ingress NGINX Controller service assigned via MetalLB is not accessible from the outside,
You need to create a network interface for that IP, associate it with a floating IP, and then connect that interface to a cluster node.

```bash
# Get EXTERNAL-IP of 'ingress-nginx-controller' service (LoadBalancer Type)
$ kubectl get svc -n ingress-nginx
NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP             PORT(S)                      AGE
ingress-nginx-controller    LoadBalancer   10.233.49.255   192.168.0.xxx (Check)   80:30465/TCP,443:32226/TCP   26h

# If EXTERNAL-IP is not accessible from outside (curl)
$ curl http://192.168.0.xxx
curl: (28) Failed to connect to 192.168.0.xxx port 80

# Create a network interface for that IP, associate it with a floating IP, and then attach that interface to a cluster node
192.168.0.xxx -> 105.xxx.xxx.xxx (floating IP)

# If you try curl from outside with a floating IP and it returns a 404 error, then it's normal.
$ curl http://105.xxx.xxx.xxx
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>

# Enter the host domain value with floating ip and nip.io
HOST_DOMAIN="105.xxx.xxx.xxx.nip.io"
```

#### TLS_CERT_AUTO_GENERATED
##### :bulb: All services deployed through the container platform portal are configured with an HTTPS connection. <br>
During the portal deployment process, a TLS certificate is automatically generated based on the HOST_DOMAIN value. If you already have a separate certificate and want to use it, modify the variable value below.
```bash
# If you are using an existing certificate
...
# TLS_CERT (Go to the comment location)
TLS_CERT_AUTO_GENERATED="N"  # Change to 'N'
TLS_CERT_PATH="/home/ubuntu/tls/mydomain.crt"  # Enter the absolute path to the host_domain crt file
TLS_KEY_PATH="/home/ubuntu/tls/mydomain.key"   # Enter the absolute path to the host_domain key file
```

<details>
<summary><h4> ⚠️ [Note] When installing the portal on a container runtime environment other than CRI-O </h4></summary>
<h1></h1>

When installing a K-PaaS cluster, the default container runtime is **CRI-O (crio)**.  
K-PaaS cluster users can skip this section.  
However, if deploying the portal on a cluster using a different container runtime (such as containerd), the following settings must be adjusted.

<br>

**Chaos Mesh**, which is included and deployed with the portal, requires container runtime configuration.  
Check the runtime and socketPath for your cluster environment and modify the settings below accordingly before proceeding with the installation.

**Reference settings**

> This guide is for reference only. For accurate and up-to-date details, refer to the official [Install Chaos Mesh in different environments](https://chaos-mesh.org/docs/production-installation-using-helm/#step-4-install-chaos-mesh-in-different-environments) documentation.

| Runtime      | Runtime value | Example socketPath                 |
|--------------|---------------|------------------------------------|
| CRI-O        | `crio`        | `/var/run/crio/crio.sock`          |
| containerd   | `containerd`  | `/run/containerd/containerd.sock`  |

#### Modify runtime and socketPath

```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/values_orig
$ vi chaos-mesh.yaml
```

```yaml
···
chaosDaemon:
  runtime: crio # change
  socketPath: /var/run/crio/crio.sock # change
···
```

<h1></h1>
</details>

<br>

### <span id='3.3'>3.3. Run the Container Platform Portal Deployment Script
Run the script for deploying the container platform portal.

```bash
$ chmod +x deploy-cp-portal.sh
$ ./deploy-cp-portal.sh
```
<br>

Verify that the container platform portal related resources have been deployed normally.<br>
For Resource Pod, it takes a few seconds to bind to Node and create a container before it transitions to Running state.

<br>

- **List OpenBao Pods**
>`$ kubectl get pods -n openbao`
```bash
NAME                                      READY   STATUS    RESTARTS   AGE
openbao-0                                 1/1     Running   0          4m33s
openbao-agent-injector-55bbd89f66-l7mxr   1/1     Running   0          4m33s
```

- **List MariaDB Pods**
>`$ kubectl get pods -n mariadb`
```bash
NAME        READY   STATUS    RESTARTS   AGE
mariadb-0   1/1     Running   0          4m24s
```    

- **List Harbor Pods**
>`$ kubectl get pods -n harbor`
```bash
NAME                                READY   STATUS    RESTARTS   AGE
harbor-core-64494dcd9b-spxbm        1/1     Running   0          4m35s
harbor-database-0                   1/1     Running   0          4m35s
harbor-jobservice-6c88f89d9-mm2kh   1/1     Running   0          4m35s
harbor-portal-6fcddb995f-pjmch      1/1     Running   0          4m35s
harbor-redis-0                      1/1     Running   0          4m35s
harbor-registry-7d98c6df6b-86dhk    2/2     Running   0          4m35s
harbor-trivy-0                      1/1     Running   0          4m35s
```  

- **List Keycloak Pods**
>`$ kubectl get pods -n keycloak`
```bash
NAME         READY   STATUS    RESTARTS   AGE
keycloak-0   1/1     Running   0          3m38s
keycloak-1   1/1     Running   0          3m38s
```

- **List Container Platform Portal Pods**
>`$ kubectl get pods -n cp-portal`
```bash
NAME                                                    READY   STATUS    RESTARTS   AGE
cp-portal-api-deployment-8c4d87657-58rr9                1/1     Running   0          3m19s
cp-portal-catalog-api-deployment-54f56948bb-ml7gz       1/1     Running   0          3m19s
cp-portal-chaos-api-deployment-7d9959c57c-42hpd         1/1     Running   0          3m19s
cp-portal-chaos-collector-deployment-6ff45d89d4-2v57b   1/1     Running   0          3m19s
cp-portal-common-api-deployment-65b87c5ddb-h8b42        1/1     Running   0          3m19s
cp-portal-metric-api-deployment-69ccbbd775-6gm26        1/1     Running   0          3m19s
cp-portal-terraman-deployment-599795db7d-8m97k          1/1     Running   0          3m19s
cp-portal-ui-deployment-5b5f465f7b-nnhpg                1/1     Running   0          3m19s
```

- **List ChartMuseum Pods**
>`$ kubectl get pods -n chartmuseum`
```bash
NAME                           READY   STATUS    RESTARTS   AGE
chartmuseum-684cbdcd6c-gq5rt   1/1     Running   0          4m5s
```

- **List Chaos Mesh Pods**
>`$ kubectl get pods -n chaos-mesh`
```bash
NAME                                       READY   STATUS    RESTARTS   AGE
chaos-controller-manager-cdf78b66c-75dxt   1/1     Running   0          3m53s
chaos-daemon-8t7qk                         1/1     Running   0          3m53s
chaos-daemon-94c59                         1/1     Running   0          3m53s
chaos-daemon-wv78l                         1/1     Running   0          3m53s
chaos-dashboard-6755fb7bf-k7f4w            1/1     Running   0          3m53s
chaos-dns-server-675758fb6b-6fdfg          1/1     Running   0          3m53s
```

- **List Service Access Hosts**
>`$ kubectl get ingress -A`
```bash
NAMESPACE     NAME                CLASS   HOSTS                                ADDRESS          PORTS     AGE
chartmuseum   chartmuseum         nginx   chartmuseum.105.xxx.xxx.xxx.nip.io   192.168.0.xxx    80, 443   4m31s
cp-portal     cp-portal-ingress   nginx   portal.105.xxx.xxx.xxx.nip.io        192.168.0.xxx    80, 443   4m3s
harbor        harbor-ingress      nginx   harbor.105.xxx.xxx.xxx.nip.io        192.168.0.xxx    80, 443   5m47s
keycloak      keycloak            nginx   keycloak.105.xxx.xxx.xxx.nip.io      192.168.0.xxx    80, 443   4m31s
openbao       openbao             nginx   openbao.105.xxx.xxx.xxx.nip.io       192.168.0.xxx    80, 443   6m8s
```

<br>

#### <span id='3.4'>3.4. (Reference) Delete Container Platform Portal Resources
If you want to delete a deployed container platform portal resource, run the script below.<br>
:loudspeaker: (Caution) If you run the script while the container platform portal is running, **all resources will be deleted**, so be careful.<br>
> If the StorageClass type of the cluster installed through the container platform is NFS, the reclaim policy is Retain. <br>
> The `Retain` policy requires manual data cleanup because data still exists on the storage NFS server even if the Persistent Volume is deleted.
```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/script
$ chmod +x uninstall-cp-portal.sh
$ ./uninstall-cp-portal.sh
Are you sure you want to delete the container platform portal? <y/n> y # Enter 'Y'
```

<br>    

## <span id='4'>4. Access the Container Platform Portal
Access the Container Platform portal. <br><br>
**Container Platform Portal URL** : `https://portal.${HOST_DOMAIN}`
+ Enter the `HOST_DOMAIN` values defined in [[3.2. Define Container Platform Portal Variables]](#3.2)

<br>

### <span id='4.1'/>4.1. Login to the Container Platform Portal Administrator Account
The administrator account needs to set the password initialization, so it is pre-processed by referring to the contents below.
<details>
<summary><h4> :key: Set container platform portal administrator account password </h4></summary>

<h1></h1>  

### 1. View Keycloak Admin Account Information
To check your Keycloak Admin account information, follow the command below.
```bash
# Get Keycloak Admin Username
$ kubectl get secret cp-portal-secret -n cp-portal -o jsonpath="{.data.KEYCLOAK_ADMIN_USERNAME}" | base64 -d; echo
# Get Keycloak Admin Password
$ kubectl get secret cp-portal-secret -n cp-portal -o jsonpath="{.data.KEYCLOAK_ADMIN_PASSWORD}" | base64 -d; echo
```

<br>

### 2. Access and login to the Keycloak Admin Console
Access the Keycloak Admin Console and login with the Keycloak Admin account you viewed.<br><br>

**Keycloak Admin Console URL** : `https://keycloak.${HOST_DOMAIN}`
+ Enter the `HOST_DOMAIN` values defined in [[3.2. Define Container Platform Portal Variables]](#3.2)

![image 011]

<br>

### 3. Reset the container platform administrator account password
- Change the Realm information at the top left to `Cp-realm`.

![image 012]

- Select the menu [Users] and click the account with Username `admin`

![image 013]

- Select the menu [Credentials] and click the button [Reset password] to reset the password.
    + **Password** : `Enter new password`
    + **Temporary** : Select `Off`
        - If selected 'On', the user will need to re-change their password on the next login

![image 014]
![image 015]

<h1></h1>

</details>

- Login to the container platform portal with an administrator account.
    + **Username** : `admin`
    + **Password** : `Enter new password`

![image 002]

<br>

### <span id='4.2'/>4.2. Login to the Container Platform Portal User Account
#### User Registration
- Click the 'Register' button at the bottom.

![image 003]

- Enter the user account information to register and click the 'Register' button to sign up for the container platform portal.

![image 004]

- After signing up, you cannot access the portal right away. You must be assigned a Namespace and Role by the administrator before you can use the portal.
For namespace and role assignments, refer to [[4.3. Container Platform Portal Use Guide]](#4.3)

![image 005]

####  User Login
- Login to the container platform portal with the account registered through user registration.

![image 006]

<br>    

### <span id='4.3'/>4.3. Container Platform Portal Use Guide
- For more information on how to use the container platform portal, please refer to the user guide below.
    + [Portal Use Guide](../../use-guide/portal/cp-portal-use-guide.md)

<br>

## <span id='5'>5. Refer to the Container Platform Portal
### <span id='5.1'>5.1. Caution for creating Kubernetes resources

When creating resources while using the container platform, be careful not to use the following prefixes.

|Resource Type|Prefixes to exclude at creation|
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

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Single Cluster Container Platform Portal Installation Guide

[image 001]:../images/portal/cp-001.png
[image 002]:../images/portal/cp-002.png
[image 003]:../images/portal/cp-003.png
[image 004]:../images/portal/cp-004.png
[image 005]:../images/portal/cp-005.png
[image 006]:../images/portal/cp-006.png
[image 007]:../images/portal/cp-007.png
[image 008]:../images/portal/cp-008.png
[image 009]:../images/portal/cp-009.png
[image 010]:../images/portal/cp-010.png
[image 011]:../images/portal/cp-011.png
[image 012]:../images/portal/cp-012.png
[image 013]:../images/portal/cp-013.png
[image 014]:../images/portal/cp-014.png
[image 015]:../images/portal/cp-015.png
[image 016]:../images/portal/cp-016.png
[image 017]:../images/portal/cp-017.png
[image 018]:../images/portal/cp-018.png
[image 019]:../images/portal/cp-019.png
[image 020]:../images/portal/cp-020.png
[image 021]:../images/portal/cp-021.png
