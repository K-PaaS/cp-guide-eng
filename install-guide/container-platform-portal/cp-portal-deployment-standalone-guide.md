### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > Standalone deployment portal installation guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
     1.1. [Purpose](#1.1)<br>
     1.2. [Range](#1.2)<br>
     1.3. [System configuration diagram](#1.3)<br>
     1.4. [Reference Material](#1.4)<br>

2. [Prerequisite](#2)<br>
     2.1. [Firewall Information](#2.1)<br>

3. [Container Platform Portal Deployment](#3)<br>
     3.1. [Container Platform Portal Deployment](#3.1)<br>
     3.1.1. [Container Platform Portal Deployment File Download](#3.1.1)<br>
     3.1.2. [Container platform portal variable definition](#3.1.2)<br>
     3.1.3. [Run container platform portal deployment script](#3.1.3)<br>
     3.1.4. [(Reference) Delete Container Platform Portal Resource](#3.1.4)<br>

4. [Container platform portal access](#4)<br>
     4.1. [Log in to Container Platform Portal Administrator Account](#4.1)<br>
     4.2. [Container Platform Portal User Account Login](#4.2)<br>
     4.3. [Container Platform Portal User Guide](#4.3)<br>

5. [Refer to Container Platform Portal](#5)<br>
     5.1. [Precautions when creating Kubernetes resources](#5.1)<br>


## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. purpose
This document (Container platform-only deployment portal installation guide) describes how to install a Kubernetes Cluster and deploy a container platform-only portal.<br>
<br>

### <div id='1.2'>1.2. range
The installation scope was created based on Kubernetes Cluster deployment.


<br>

### <div id='1.3'>1.3. System configuration diagram
<p align="center"><img src="./images-v1.2/cp-001.png" width="850" height="530"></p>

The system configuration consists of a **Kubernetes Cluster (Master, Worker)** environment and a storage server for data management. **Harbor** manages the container platform portal image and Helm Chart in the Kubernetes Cluster environment installed through Kubespray, **Keycloak** manages container platform portal user authentication, **Vault** manages authentication data, and Meta A middleware environment such as **MariaDB (RDBMS)** that manages data is provided as a container. The total required VM environment requires **Master VM: 1, Worker VM: 3 or more**, and this document is about deploying a container platform portal environment in a Kubernetes Cluster.

<br>

### <div id='1.4'>1.4. reference material
> https://kubernetes.io/ko/docs<br>
> https://goharbor.io/docs<br>
> https://www.keycloak.org/documentation

<br>

## <div id='2'>2. Prerequisite
This installation guide was written based on installation in the **Ubuntu 20.04** environment.

### <div id='2.1'>2.1. firewall information
Set the port to be opened in the IaaS Security Group.

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

## <div id='3'>3. Deploying the Container Platform Portal

### <div id='3.1'>3.1. Deploying the Container Platform Portal

#### <div id='3.1.1'>3.1.1. Container Platform Portal Download Deployment File
To deploy the container platform portal, download the container platform portal deployment file and place it in the path below.<br>
:bulb: This content is carried out in Kubernetes **Master Node**.

+ Download Container Platform Portal Deployment file:
    [cp-portal-deployment-v1.4.0.tar.gz](https://nextcloud.k-paas.org/index.php/s/WtNQn2agk6epFHC/download)

```
# Create deployment file download path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment file and check file path
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/WtNQn2agk6epFHC/download

$ ls ~/workspace/container-platform
   cp-portal-deployment-v1.4.0.tar.gz

# Unzip the Deployment file
$ tar -xvf cp-portal-deployment-v1.4.0.tar.gz
```

- Deployment file directory configuration
```
├── script # Location of variables and script files related to container platform portal deployment
├── images # Container Platform Portal image file location
├── charts # Container Platform Portal Helm Charts file location
├── values_orig # Container Platform Portal Helm Charts values.yaml file location
└── keycloak_orig # Keycloak deployment-related file location for container platform portal user authentication management
```

<br>

#### <div id='3.1.2'>3.1.2. Container Platform Portal Variable Definitions
Before deploying the container platform portal, variable values need to be defined. Confirm the information required for distribution and set variables.

:bulb: Keycloak's default distribution method is **HTTP**, and if you want to set up **HTTPS** through a certificate, refer to the guide below to preprocess.
> [Keycloak TLS Settings](cp-portal-deployment-keycloak-tls-setting-guide.md#2-keycloak-tls-settings)

<br>

```
$ cd ~/workspace/container-platform/cp-portal-deployment/script
$ vi cp-portal-vars.sh
```

```                                                     
# COMMON VARIABLE (Please change the values of the four variables below.)
K8S_MASTER_NODE_IP="{k8s master node public ip}"            # Kubernetes Master Node Public IP
HOST_CLUSTER_IAAS_TYPE="{host cluster iaas type}"           # Host Cluster IaaS Type (Please enter 'AWS' or 'OPENSTACK')
PROVIDER_TYPE="{container platform portal provider type}"   # Container Platform Portal Provider Type (Please enter 'standalone' or 'service')
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"
HOST_CLUSTER_IAAS_TYPE="AWS"
PROVIDER_TYPE="standalone"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **HOST_CLUSTER_IAAS_TYPE** <br>Enter Kubernetes Cluster IaaS environment <br><br>
- **PROVIDER_TYPE** <br>Enter container platform portal provision type <br>
    + This guide is a portal stand-alone installation guide and requires entering the **'standalone'** value.

<br>


#### <div id='3.1.3'>3.1.3. Run the Container Platform Portal deployment script
Run the deployment script to deploy the container platform portal.

```
$ chmod +x deploy-cp-portal.sh
$ ./deploy-cp-portal.sh
```
<br>

Check whether the container platform portal related resources are deployed properly.<br>
In the case of resource Pods, it takes several seconds to transition to Running state after binding to Node and creating a container.


- **Harbor resource query**
>`$ kubectl get all -n harbor`
```
$ kubectl get all -n harbor
NAME                                           READY   STATUS    RESTARTS        AGE
pod/cp-harbor-chartmuseum-6fdd486868-266t9     1/1     Running   0               3m14s
pod/cp-harbor-core-794489c7b4-hbtmb            1/1     Running   0               3m14s
pod/cp-harbor-database-0                       1/1     Running   0               3m14s
pod/cp-harbor-jobservice-5fdbf6cb6b-vjxxd      1/1     Running   3 (2m27s ago)   3m14s
pod/cp-harbor-nginx-6db895bdbb-dp7zk           1/1     Running   0               3m14s
pod/cp-harbor-notary-server-57676cff76-7f292   1/1     Running   0               3m14s
pod/cp-harbor-notary-signer-64cc867bbb-hwvxp   1/1     Running   0               3m14s
pod/cp-harbor-portal-7f9d57dcf4-5f68g          1/1     Running   0               3m14s
pod/cp-harbor-redis-0                          1/1     Running   0               3m14s
pod/cp-harbor-registry-5fdf76f6cf-jhsxl        2/2     Running   0               3m14s
pod/cp-harbor-trivy-0                          1/1     Running   0               3m14s

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                       AGE
service/cp-harbor-chartmuseum     ClusterIP   10.233.10.224   <none>        80/TCP                        3m15s
service/cp-harbor-core            ClusterIP   10.233.7.180    <none>        80/TCP                        3m15s
service/cp-harbor-database        ClusterIP   10.233.55.195   <none>        5432/TCP                      3m15s
service/cp-harbor-jobservice      ClusterIP   10.233.19.242   <none>        80/TCP                        3m15s
service/cp-harbor-notary-server   ClusterIP   10.233.55.160   <none>        4443/TCP                      3m15s
service/cp-harbor-notary-signer   ClusterIP   10.233.15.151   <none>        7899/TCP                      3m15s
service/cp-harbor-portal          ClusterIP   10.233.30.190   <none>        80/TCP                        3m15s
service/cp-harbor-redis           ClusterIP   10.233.29.164   <none>        6379/TCP                      3m15s
service/cp-harbor-registry        ClusterIP   10.233.49.111   <none>        5000/TCP,8080/TCP             3m15s
service/cp-harbor-trivy           ClusterIP   10.233.48.64    <none>        8080/TCP                      3m15s
service/harbor                    NodePort    10.233.46.29    <none>        80:30002/TCP,4443:30004/TCP   3m15s

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-harbor-chartmuseum     1/1     1            1           3m15s
deployment.apps/cp-harbor-core            1/1     1            1           3m15s
deployment.apps/cp-harbor-jobservice      1/1     1            1           3m15s
deployment.apps/cp-harbor-nginx           1/1     1            1           3m15s
deployment.apps/cp-harbor-notary-server   1/1     1            1           3m15s
deployment.apps/cp-harbor-notary-signer   1/1     1            1           3m15s
deployment.apps/cp-harbor-portal          1/1     1            1           3m15s
deployment.apps/cp-harbor-registry        1/1     1            1           3m15s

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-harbor-chartmuseum-6fdd486868     1         1         1       3m15s
replicaset.apps/cp-harbor-core-794489c7b4            1         1         1       3m15s
replicaset.apps/cp-harbor-jobservice-5fdbf6cb6b      1         1         1       3m15s
replicaset.apps/cp-harbor-nginx-6db895bdbb           1         1         1       3m15s
replicaset.apps/cp-harbor-notary-server-57676cff76   1         1         1       3m15s
replicaset.apps/cp-harbor-notary-signer-64cc867bbb   1         1         1       3m15s
replicaset.apps/cp-harbor-portal-7f9d57dcf4          1         1         1       3m15s
replicaset.apps/cp-harbor-registry-5fdf76f6cf        1         1         1       3m15s

NAME                                  READY   AGE
statefulset.apps/cp-harbor-database   1/1     3m15s
statefulset.apps/cp-harbor-redis      1/1     3m15s
statefulset.apps/cp-harbor-trivy      1/1     3m15s
```  

- **MariaDB resource query**
>`$ kubectl get all -n mariadb`       
```
$ kubectl get all -n mariadb
NAME               READY   STATUS    RESTARTS   AGE
pod/cp-mariadb-0   1/1     Running   0          96s

NAME                 TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-mariadb   NodePort   10.233.19.252   <none>        3306:31306/TCP   97s

NAME                          READY   AGE
statefulset.apps/cp-mariadb   1/1     97s
```    

- **Keycloak resource query**
>`$ kubectl get all -n keycloak`     
```
$ kubectl get all -n keycloak
NAME                               READY   STATUS    RESTARTS      AGE
pod/cp-keycloak-7d49f84bc6-qdljr   1/1     Running   1 (55s ago)   119s
pod/cp-keycloak-7d49f84bc6-xbg92   1/1     Running   0             119s

NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-keycloak           NodePort    10.233.41.247   <none>        8080:32710/TCP   119s
service/cp-keycloak-cluster   ClusterIP   None            <none>        8080/TCP         119s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-keycloak   2/2     2            2           119s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-keycloak-7d49f84bc6   2         2         2       119s
```

- **Container Platform Portal Resource query**
>`$ kubectl get all -n cp-portal`        
```
$ kubectl get all -n cp-portal
NAME                                                      READY   STATUS    RESTARTS   AGE
pod/cp-portal-api-deployment-595dd4dfb6-wjw82             1/1     Running   0          95s
pod/cp-portal-common-api-deployment-c54d88fbc-zl5t8       1/1     Running   0          84s
pod/cp-portal-metric-api-deployment-6599f47b4b-lr7lp      1/1     Running   0          80s
pod/cp-portal-terraman-deployment-5ccfbf67fc-lsr4q        1/1     Running   0          74s
pod/cp-portal-ui-deployment-669db699c-2c7jh               1/1     Running   0          100s

NAME                                       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-portal-api-service              NodePort   10.233.47.49    <none>        3333:32701/TCP   95s
service/cp-portal-common-api-service       NodePort   10.233.9.255    <none>        3334:32700/TCP   84s
service/cp-portal-metric-api-service       NodePort   10.233.29.41    <none>        8900:30329/TCP   80s
service/cp-portal-terraman-service         NodePort   10.233.54.151   <none>        8091:32707/TCP   75s
service/cp-portal-ui-service               NodePort   10.233.58.184   <none>        8090:32703/TCP   100s

NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-portal-api-deployment              1/1     1            1           95s
deployment.apps/cp-portal-common-api-deployment       1/1     1            1           84s
deployment.apps/cp-portal-metric-api-deployment       1/1     1            1           80s
deployment.apps/cp-portal-terraman-deployment         1/1     1            1           75s
deployment.apps/cp-portal-ui-deployment               1/1     1            1           100s

NAME                                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-portal-api-deployment-595dd4dfb6             1         1         1       95s
replicaset.apps/cp-portal-common-api-deployment-c54d88fbc       1         1         1       84s
replicaset.apps/cp-portal-metric-api-deployment-6599f47b4b      1         1         1       80s
replicaset.apps/cp-portal-terraman-deployment-5ccfbf67fc        1         1         1       75s
replicaset.apps/cp-portal-ui-deployment-669db699c               1         1         1       100s    
```    

<br>

#### <div id='3.1.4'>3.1.4. (Reference) Deleting Container Platform Portal Resources
If you want to delete the deployed container platform portal resource, run the script below.<br>
:loudspeaker: (Caution) When running this script while the container platform portal is running, caution is required because **all resources required for operation will be deleted**.<br>

```
$ cd ~/workspace/container-platform/cp-portal-deployment/script
$ chmod +x uninstall-cp-portal.sh
$ ./uninstall-cp-portal.sh
```
```    
Are you sure you want to delete the container platform portal? <y/n> y
.... 
    
release "cp-harbor" uninstalled
namespace "harbor" deleted
release "cp-mariadb" uninstalled
namespace "mariadb" deleted
release "cp-keycloak" uninstalled
namespace "keycloak" deleted
release "cp-portal-api" uninstalled
release "cp-portal-common-api" uninstalled
release "cp-portal-configmap" uninstalled
release "cp-portal-metric-api" uninstalled
release "cp-portal-terraman" uninstalled
release "cp-portal-ui" uninstalled
namespace "cp-portal" deleted
"cp-portal-repository" has been removed from your repositories
Uninstalled plugin: cm-push
    
....       
```

<br>    

## <div id='4'>4. Access container platform portal
The container platform portal can be accessed at the address below.<br>
For the {K8S_MASTER_NODE_IP} value, enter the **Kubernetes Master Node Public IP** value.

- Container platform portal access URI: **http://{K8S_MASTER_NODE_IP}:32703**

<br>

### <div id='4.1'/>4.1. Log in to your Container Platform Portal administrator account
Container Platform Portal Administrator Account Login After checking the initial information, log in to the portal.
- Access http://{K8S_MASTER_NODE_IP}:32703
- Verify initial account information and log in

> Check initial account information using the command below
```
$ kubectl get configmap -n cp-portal cp-portal-configmap -o yaml | grep KEYCLOAK_ADMIN
```
![image 002]

<br>

### <div id='4.2'/>4.2. Log in to your Container Platform Portal user account
#### User registration
- Click the ‘Register’ button at the bottom.
![image 003]

- Enter the user account information to register and click the 'Register' button to sign up for the container platform portal.
![image 004]

- You cannot access the portal immediately after registering as a member. You can use the portal after being assigned a namespace and role for the user to use by the administrator.
Namespace and role assignment is [[4.3. Please refer to the Container Platform User/Operator Portal Usage Guide]](#4.3).
![image 005]

#### User Login
- Log in to the container platform portal with the account registered through membership registration.
![image 006]

<br>

### <div id='4.3'/>4.3. Container Platform Portal User Guide
- For information on how to use the container platform portal, please refer to the user guide below.
   + [Container Platform Portal Use Guide](../../use-guide/portal/container-platform-portal-guide.md)

<br>

## <div id='5'>5. See Container Platform Portal

### <div id='5.1'>5.1. Precautions when creating Kubernetes resources

When using the container platform, be careful not to use the following prefixes when creating resources.

|Resource name|Prefix to be excluded when creating|
|---|---|
|전체 Resource|kube*|
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

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > Standalone deployment portal installation guide

[image 001]:./images-v1.2/cp-001.png
[image 002]:./images-v1.2//cp-002.png
[image 003]:./images-v1.2//cp-003.png
[image 004]:./images-v1.2//cp-004.png
[image 005]:./images-v1.2//cp-005.png
[image 006]:./images-v1.2//cp-006.png
[image 007]:./images-v1.2//cp-007.png
[image 008]:./images-v1.2//cp-008.png
[image 009]:./images-v1.2//cp-009.png
[image 010]:./images-v1.2//cp-010.png