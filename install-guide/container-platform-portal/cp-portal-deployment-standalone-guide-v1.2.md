### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Stand-alone Deployment Portal Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
    1.1. [Purpose](#1.1)  
    1.2. [Range](#1.2)  
    1.3. [System Configuration](#1.3)  
    1.4. [References](#1.4)  

2. [Prerequisite](#2)  
    2.1. [Firewall Information](#2.1)  
    2.2. [NFS Server Installation](#2.2)    

3. [Container Platform Portal Deployment](#3)  
    3.1. [CRI-O insecure-registry Setting](#3.1)  
    3.2. [Container Platform Portal Deployment](#3.2)  
    3.2.1. [Container Platform Portal Deployment File Download](#3.2.1)  
    3.2.2. [Define Container Platform Portal Variable](#3.2.2)    
    3.2.3. [Execute Container Platform Portal Deployment Script](#3.2.3)  
    3.2.4. [(Refer) Delete Container Platform Portal Resource](#3.2.4)

4. [Container Platform Operator/ User Portal Access](#4)      
    4.1. [Container Platform Operator Portal Login](#4.1)      
    4.2. [Container Platform User Portal Sign in/Login](#4.2)      
    4.3. [Container Platform User/Operator Portal Use Guide](#4.3)          

5. [Refer Container Platform Portal](#5)       
    5.1. [Create Operator Cluster Role Token](#5.1)  
    5.2. [Cautions when creating Kubernetes Resource](#5.2)    


## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform Standalone Portal Installation Guide) describes how to install the Kubernetes Cluster and deploy the Container Platform Standalone Portal.<br>
<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration
<p align="center"><img src="images-v1.2/cp-001.png"></p>    

System configuration consists of **Kubernetes Cluster(Master, Worker)** environment and **Network File System(NFS)** ) storage server for data management. It provides containerized middleware environments such as **Harbor** managing container platform portal images and HelmCharts, **Keycloak** managing container platform portal user authentication, and **MariaDB (RDBMS)** managing container platform portal metadata. For the total required VM environment, **1 Master Node VM, 1 or more Worker Node VM, NFS Server: 1**. This document is about deploying a container platform portal environment in a Kubernetes cluster. **Network File System (NFS)** is the built-in storage provided by the container platform, and various types of storage can be used depending on the user environment.  

<br>    

### <div id='1.4'>1.4. References
> https://kubernetes.io/ko/docs<br>
> https://goharbor.io/docs<br>
> https://www.keycloak.org/documentation

<br>

## <div id='2'>2. Prerequisite
This installation guide is prepared based on installation in the **Ubuntu 18.04** environment.

### <div id='2.1'>2.1. Firewall Information
Set the port that should be opened for the IaaS Security Group.

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

### <div id='2.2'>2.2. NFS Server Installation
**NFS Storage Server** installation to be used by container platform portal service must be installed ahead of time.<br>
Refer to the guide below for NFS Storage Server installation.  
> [NFS Server Installation](../nfs-server-install-guide.md)      

<br>

## <div id='3'>3. Container Platform Portal Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
The container platform portal deployment includes a private repository (Harbor) deployment. To upload images and package files related to the container platform portal to the Private Repository and to set up HTTP connections, install podman in Kubernetes **Master Node, Worker Node**, and set 'insure-registries' in the config file before deployment.

- **:bulb: Master Node, Worker Node needs to set add all**
- **For {K8S_MASTER_NODE_IP} value, enter Kubernetes Master Node Public IP**

#### 1. podman Installation

```
$ sudo apt-get update
$ sudo apt-get install -y podman
```

#### 2.  Set'insecure-registries' inside crio.conf 
```
$ sudo vi /etc/crio/crio.conf
```

```
#  Add "{K8S_MASTER_NODE_IP}:30002" to the 'insecure_registries' category
...    
insecure_registries = [
 "xx.xxx.xxx.xx:30002"
  ]
...    
```

```
# restart crio 
$ sudo systemctl restart crio
```

#### 3. Set insecure in 'registry' category inside registries.conf
```
$ sudo vi /etc/containers/registries.conf
```

```
# Within the registries.conf file, '[[registry]]' needs to be uncommented because it is annotated      
# Add the content below , Set location value as "{K8S_MASTER_NODE_IP}:30002"
...    
[[registry]]
insecure = true
location = "xx.xxx.xxx.xx:30002"
...
```

```
# Restart podman
$ sudo systemctl restart podman
```

<br>

### <div id='3.2'>3.2. Container Platform Portal Deployment

#### <div id='3.2.1'>3.2.1. Container Platform Portal Deployment File Download
Download the container platform portal Deployment file and locate it in the path below for container platform portal deployment.<br>
:bulb: Process the content at Kubernetes **Master Node**.

+ Container Platform Portal Deployment File Download :
   [paas-ta-container-platform-portal-deployment.tar.gz](https://nextcloud.paas-ta.org/index.php/s/wYJ3wim3WCxG7Ed/download)

```
# Create Deployment File Download Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Deployment File Download and Check File Path
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/wYJ3wim3WCxG7Ed/download

$ ls ~/workspace/container-platform
  paas-ta-container-platform-portal-deployment.tar.gz

# Unzip Deployment File
$ tar -xvf paas-ta-container-platform-portal-deployment.tar.gz
```

- Configure Deployment File Directory
```
├── script          # Container Platform Portal Deployment Related Variable and Script File Location
├── images          # Container Platform Portal Image File Location
├── charts          # Container Platform Portal Helm Charts File Location
├── values_orig     # Container Platform Portal Helm Charts values.yaml File Location
└── keycloak_orig   # Location of files related to Keycloak deployment for container platform portal user authentication management
```

<br>

#### <div id='3.2.2'>3.2.2. Define Container Platform Portal Variable
Defining variable values is necessary before deploying the container platform portal. Set the variable by checking the information required for deployment.

:bulb: Keycloak's default deployment method is **HTTP**. If you want to set **HTPS** through a certificate, refer to the guide below for pre-processing.
> [Keycloak TLS Setting](cp-portal-deployment-keycloak-tls-setting-guide-v1.2.md#2-keycloak-tls-설정)       

<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/script
$ vi container-platform-portal-vars.sh
```

```                                                     
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}"             # Kubernetes Master Node Public IP
K8S_AUTH_BEARER_TOKEN="{k8s auth bearer token}"              # Kubernetes Authorization Bearer Token
NFS_SERVER_IP="{nfs server ip}"                              # NFS Server IP
PROVIDER_TYPE="{container platform portal provider type}"    # Container Platform Portal Provider Type (Please enter 'standalone' or 'service')
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"                 
K8S_AUTH_BEARER_TOKEN="qY3k2xaZpNbw3AJxxxxx...."                 
NFS_SERVER_IP="xx.xxx.xxx.xx"                                  
PROVIDER_TYPE="standalone"           
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **K8S_AUTH_BEARER_TOKEN** <br>Enter Kubernetes Bearer Token<br>
   + Refer to [[5.1. Create Operator Cluster Role Token]](#5.1) to create and enter the Token value <br><br>
- **NFS_SERVER_IP** <br>Enter NFS Server Private IP<br>
   + Enter the NFS Server Private IP installed through [[NFS Server Installation](../nfs-server-install-guide.md)] guide<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Portal Providing Type <br>
   + This guide is a portal standalone installation guide that requires **'standalone'** values

<br>


#### <div id='3.2.3'>3.2.3. Execute Container Platform Portal Deployment Script
Execute a deployment script for deploying a container platform portal.

```
$ chmod +x deploy-container-platform-portal.sh
$ ./deploy-container-platform-portal.sh
```
<br>

Verify if the resources related to the container platform portal are deployed normally.<br>
In the case of the resource Pod, it takes several seconds to enter the Running state after binding and container creation at the Node.


- **Retrieve NFS Resource**
>`$ kubectl get all -n nfs-storageclass`  
```
$ kubectl get all -n nfs-storageclass
NAME                                       READY   STATUS    RESTARTS   AGE
pod/nfs-pod-provisioner-7f7c444487-nxztc   1/1     Running   0          3m22s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nfs-pod-provisioner   1/1     1            1           3m22s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/nfs-pod-provisioner-7f7c444487   1         1         1       3m22s
```

- **Retrieve Harbor Resource**
>`$ kubectl get all -n harbor`      
```
$ kubectl get all -n harbor
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-harbor-chartmuseum-59b784bfd4-55twb    1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-core-6b7b8768f9-r27nn           1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-database-0                      1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-jobservice-69556cd7cd-hk75s     1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-nginx-755cf6876f-rkwk2          1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-notary-server-896c47dcd-lg78k   1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-notary-signer-b747fb89d-lpw86   1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-portal-c79c78f5b-nk5vm          1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-redis-0                         1/1     Running   0          4m
pod/paas-ta-container-platform-harbor-registry-54c4f69579-zwjmp       2/2     Running   0          4m
pod/paas-ta-container-platform-harbor-trivy-0                         1/1     Running   0          4m

NAME                                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                       AGE
service/harbor                                            NodePort    10.233.3.114    <none>        80:30002/TCP,4443:30004/TCP   4m1s
service/paas-ta-container-platform-harbor-chartmuseum     ClusterIP   10.233.28.112   <none>        80/TCP                        4m1s
service/paas-ta-container-platform-harbor-core            ClusterIP   10.233.14.27    <none>        80/TCP                        4m
service/paas-ta-container-platform-harbor-database        ClusterIP   10.233.9.5      <none>        5432/TCP                      4m1s
service/paas-ta-container-platform-harbor-jobservice      ClusterIP   10.233.5.82     <none>        80/TCP                        4m1s
service/paas-ta-container-platform-harbor-notary-server   ClusterIP   10.233.22.118   <none>        4443/TCP                      4m1s
service/paas-ta-container-platform-harbor-notary-signer   ClusterIP   10.233.57.71    <none>        7899/TCP                      4m1s
service/paas-ta-container-platform-harbor-portal          ClusterIP   10.233.48.102   <none>        80/TCP                        4m1s
service/paas-ta-container-platform-harbor-redis           ClusterIP   10.233.34.72    <none>        6379/TCP                      4m1s
service/paas-ta-container-platform-harbor-registry        ClusterIP   10.233.41.153   <none>        5000/TCP,8080/TCP             4m1s
service/paas-ta-container-platform-harbor-trivy           ClusterIP   10.233.33.126   <none>        8080/TCP                      4m1s

NAME                                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/paas-ta-container-platform-harbor-chartmuseum     1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-core            1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-jobservice      1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-nginx           1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-notary-server   1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-notary-signer   1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-portal          1/1     1            1           4m
deployment.apps/paas-ta-container-platform-harbor-registry        1/1     1            1           4m

NAME                                                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/paas-ta-container-platform-harbor-chartmuseum-59b784bfd4    1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-core-6b7b8768f9           1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-jobservice-69556cd7cd     1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-nginx-755cf6876f          1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-notary-server-896c47dcd   1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-notary-signer-b747fb89d   1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-portal-c79c78f5b          1         1         1       4m
replicaset.apps/paas-ta-container-platform-harbor-registry-54c4f69579       1         1         1       4m

NAME                                                          READY   AGE
statefulset.apps/paas-ta-container-platform-harbor-database   1/1     4m
statefulset.apps/paas-ta-container-platform-harbor-redis      1/1     4m
statefulset.apps/paas-ta-container-platform-harbor-trivy      1/1     4m
```  

- **Retrieve MariaDB Resource**
>`$ kubectl get all -n mariadb`       
```
$ kubectl get all -n mariadb
NAME                                       READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-mariadb-0   1/1     Running   0          3m4s

NAME                                         TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/paas-ta-container-platform-mariadb   NodePort   10.233.37.208   <none>        3306:31306/TCP   3m4s

NAME                                                  READY   AGE
statefulset.apps/paas-ta-container-platform-mariadb   1/1     3m4s
```    

- **Retrieve Keycloak Resource**
>`$ kubectl get all -n keycloak`     
```
$ kubectl get all -n keycloak
NAME                            READY   STATUS    RESTARTS   AGE
pod/keycloak-7d47d9f577-7vfzj   1/1     Running   0          4m18s
pod/keycloak-7d47d9f577-zfwj7   1/1     Running   0          4m18s

NAME                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/keycloak           NodePort    10.233.42.192   <none>        8080:32710/TCP   4m18s
service/keycloak-cluster   ClusterIP   None            <none>        8080/TCP         4m18s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/keycloak   2/2     2            2           4m18s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/keycloak-7d47d9f577   2         2         2       4m18s
```

- **Retrieve Container Platform Portal Resource**
>`$ kubectl get all -n paas-ta-container-platform-portal`        
```
$ kubectl get all -n paas-ta-container-platform-portal
NAME                                                            READY   STATUS    RESTARTS   AGE
pod/container-platform-api-deployment-55d995498d-9vlpg          1/1     Running   0          5m9s
pod/container-platform-common-api-deployment-5c959bcdf6-kwfpt   1/1     Running   0          5m8s
pod/container-platform-webadmin-deployment-b75dc47cc-sxvl2      1/1     Running   0          5m8s
pod/container-platform-webuser-deployment-d4755ccf7-sn48b       1/1     Running   0          5m7s

NAME                                            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-api-service          NodePort   10.233.52.211   <none>        3333:32701/TCP   5m9s
service/container-platform-common-api-service   NodePort   10.233.44.252   <none>        3334:32700/TCP   5m8s
service/container-platform-webadmin-service     NodePort   10.233.12.200   <none>        8090:32703/TCP   5m8s
service/container-platform-webuser-service      NodePort   10.233.15.118   <none>        8091:32702/TCP   5m7s

NAME                                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-api-deployment          1/1     1            1           5m9s
deployment.apps/container-platform-common-api-deployment   1/1     1            1           5m8s
deployment.apps/container-platform-webadmin-deployment     1/1     1            1           5m8s
deployment.apps/container-platform-webuser-deployment      1/1     1            1           5m7s

NAME                                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-api-deployment-55d995498d          1         1         1       5m9s
replicaset.apps/container-platform-common-api-deployment-5c959bcdf6   1         1         1       5m8s
replicaset.apps/container-platform-webadmin-deployment-b75dc47cc      1         1         1       5m8s
replicaset.apps/container-platform-webuser-deployment-d4755ccf7       1         1         1       5m7s
```    

<br>

#### <div id='3.2.4'>3.2.4. (Refer) Delete Container Platform Portal Resource
To delete the deployed container platform portal resource, perform the scrip below.<br>
:loudspeaker: (Caution) Be aware that the  **whole resource necessary for the operation gets deleted** if the script is performed while the container platform portal is being operated.<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/script
$ chmod +x uninstall-container-platform-portal.sh
$ ./uninstall-container-platform-portal.sh
```
```    
Are you sure you want to delete the container platform portal? <y/n> y
....
release "paas-ta-container-platform-harbor" uninstalled
release "paas-ta-container-platform-mariadb" uninstalled
release "paas-ta-container-platform-keycloak" uninstalled
release "paas-ta-container-platform-nfs-storageclass" uninstalled
release "paas-ta-container-platform-api" uninstalled
release "paas-ta-container-platform-common-api" uninstalled
release "paas-ta-container-platform-webadmin" uninstalled
release "paas-ta-container-platform-webuser" uninstalled
namespace "harbor" deleted
namespace "mariadb" deleted
namespace "keycloak" deleted
namespace "nfs-storageclass" deleted
namespace "paas-ta-container-platform-portal" deleted
"paas-ta-container-platform-repository" has been removed from your repositories
Uninstalled plugin: cm-push
....    
```

<br>    


## <div id='4'>4. Container Platform Operator/ User Portal Access
The container platform operator/ user portal can be accessed with the link below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Operator's Portal Access URI : **http://{K8S_MASTER_NODE_IP}:32703** <br>
- Container Platform User's Portal Access URI : **http://{K8S_MASTER_NODE_IP}:32702** <br>

<br>

### <div id='4.1'/>4.1. Container Platform Operator Portal Login
The initial information on accessing the container platform operator portal is as follows.
- Access to http://{K8S_MASTER_NODE_IP}:32703.   
- With username : **admin** / password : **admin** account, log in to the container platform's operators portal.
![image 002]

<br>    


### <div id='4.2'/>4.2. Container Platform User Portal Sign-in/log in
#### User Sign in     
- Access to http://{K8S_MASTER_NODE_IP}:32702.
- Click the 'Register' button below.
![image 003]

- Input the user account information to register and click the 'Register' button to sign in to the container platform user portal.
![image 004]  

- The Container Platform User's Portal cannot be used until the Cluster Admin or Namespace Admin will give the registered user a Namespace and Role to use.
Refer to [[4.3. Container Platform Operator/ User Portal Use Guide]](#4.3) for Namespace and Role Assigning.    
![image 005]    

#### User Log in   
- Access to http://{K8S_MASTER_NODE_IP}:32702.
- Log in to the Container Platform User's Portal with the registered account.
![image 006]

<br>    

### <div id='4.3'/>4.3. Container Platform User/ Operator Portal Use Guide
- Refer to the use guide below for Container Platform Portal usage.  
  + [Container Platform Operator's Portal Use Guide](../../use-guide/portal/container-platform-admin-portal-guide.md)    
  + [Container Platform User's Portal Use Guide](../../use-guide/portal/container-platform-user-portal-guide.md)


<br>

## <div id='5'>5. Refer Container Platform Portal

### <div id='5.1'>5.1. Create Operator Cluster Role Token
Create a service account of an operator with a cluster role and obtain the token value of the service account.<br>
The acquired Token value is used when deploying the container platform portal.

- Create Service Account.
```
## {SERVICE_ACCOUNT} : Service Account Name to create

$ kubectl create serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl create serviceaccount k8sadmin -n kube-system)
```

- Bind the created Service Account and the ClusterRole 'cluster-admin' provided by Kubernetes.
```
$ kubectl create clusterrolebinding {SERVICE_ACCOUNT} --clusterrole=cluster-admin --serviceaccount=kube-system:{SERVICE_ACCOUNT}
(ex. kubectl create clusterrolebinding k8sadmin --clusterrole=cluster-admin --serviceaccount=kube-system:k8sadmin)
```

- Check the value of Mountable secrets in the Service Account.
```
$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl describe serviceaccount k8sadmin -n kube-system)

...

Mountable secrets:   k8sadmin-token-xxxx
```

- Obtain the Token of Service Account.

```
## {SECRET_NAME} : Enter Mountable secrets value

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

### <div id='5.2'>5.2. Cautions when creating Kubernetes Resource

Be careful not to use the following prefixes when creating resources while using the container platform.

|Resource Name|prefix to exclude when creating|
|---|---|
|All Resource|kube*|
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

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Stand-Alone Deployment Portal Installation Guide

[image 001]:images-v1.2/cp-001.png
[image 002]:images-v1.2/cp-002.png
[image 003]:images-v1.2/cp-003.png
[image 004]:images-v1.2/cp-004.png
[image 005]:images-v1.2/cp-005.png
[image 006]:images-v1.2/cp-006.png
[image 007]:images-v1.2/cp-007.png
[image 008]:images-v1.2/cp-008.png
[image 009]:images-v1.2/cp-009.png
[image 010]:images-v1.2/cp-010.png
[image 011]:images-v1.2/cp-011.png
