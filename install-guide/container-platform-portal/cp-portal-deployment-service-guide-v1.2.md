### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Service Deployment Portal Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
    1.1. [Purpose](#1.1)  
    1.2. [Range](#1.2)  
    1.3. [System Configuration Diagram](#1.3)  
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
    3.2.4. [(Refer)Delete Container Platform Portal Resource](#3.2.4)

4. [Configuring Container Platform Portal User Authentication Service](#4)      
    4.1. [Download Container Platform Portal User Authentication Configuration Deployment](#4.1)      
    4.2. [Defining Container Platform Portal User Authentication Configurations](#4.2)      
    4.3. [Execute Container Platform Portal User Authentication Configuration Script](#4.3)          
    4.4. [(Refer) Unconfiguring Container Platform Portal User Authentication](#4.4)    

5. [Container Platform Portal Service Broker](#5)       
    5.1. [Container Platform Portal Service Broker Registration](#5.1)  
    5.2. [Container Platform Portal Service Lookup Settings](#5.2)    
    5.3. [Container Platform User/ Operator Portal Use Guide](#5.3)

6. [Container Platform Portal Reference](#6)  
    6.1. [Create Operator Cluster Role Token](#6.1)    
    6.2. [Precautions for Creating Kubernetes Resources](#6.2)      


## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform K-PaaS Service Deployment Portal Installation Guide) describes how to install the Kubernetes Cluster and deploy the Container Platform K-PaaS Service Deployment Portal.<br>
<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration Diagram
<p align="center"><img src="images-v1.2/cp-001.png"></p>    

System configuration consists of **Kubernetes Cluster(Master, Worker)** environment and **Network File System(NFS)** ) storage server for data management. It provides containerized middleware environments such as **Harbor** managing container platform portal images and HelmCharts, **Keycloak** managing container platform portal user authentication, and **MariaDB (RDBMS)** managing container platform portal metadata. The total required VM environment, **1 Master Node VM, 1 or more Worker Node VM, NFS Server: 1**. This document is about deploying a container platform portal environment in a Kubernetes cluster. **Network File System (NFS)** is the built-in storage provided by the container platform, and various types of storage can be used depending on the user environment.  

<br>    

### <div id='1.4'>1.4. References
> https://kubernetes.io/ko/docs<br>
> https://goharbor.io/docs<br>
> https://www.keycloak.org/documentation

<br>

## <div id='2'>2. Prerequisite
This installation guide is prepared based on installation in the **Ubuntu 18.04** environment.

### <div id='2.1'>2.1. Firewall Information
Set the port that should be opened for the IaaS security group.

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
For NFS Storage Server installation, refer to the guide below.  
> [NFS Server Installation](../nfs-server-install-guide.md)      

<br>

## <div id='3'>3. Container Platform Portal Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
The container platform portal deployment includes a private repository (Harbor) deployment. To upload images and package files related to the container platform portal to the Private Repository and to set up HTTP connections, install podman in Kubernetes **Master Node, Worker Node**, and set 'insure-registries' in the config file before deployment.

- **:bulb:Master Node, Worker Node needs to set add all**
- **For {K8S_MASTER_NODE_IP} value, enter Kubernetes Master Node Public IP**

#### 1. podman Installation

```
$ sudo apt-get update
$ sudo apt-get install -y podman
```

#### 2. Set 'insecure-registries' inside crio.conf  
```
$ sudo vi /etc/crio/crio.conf
```

```
# Add "{K8S_MASTER_NODE_IP}:30002" to the 'insecure_registries' category
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

#  Unzip Deployment File
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
> [Keycloak TLS Setting](paas-ta-container-platform-portal-deployment-keycloak-tls-setting-guide-v1.2.md#2-keycloak-tls-설정)       

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
PROVIDER_TYPE="service"           
```

- **K8S_MASTER_NODE_IP** <br>Enter `Kubernetes Master Node Public IP<br><br>
- **K8S_AUTH_BEARER_TOKEN** <br>Enter Kubernetes Bearer Token<br>
   + Refer to [[6.1. Create Operator Cluster Role Token]](#6.1) to create and enter Token value<br><br>
- **NFS_SERVER_IP** <br>Enter NFS Server Private IP<br>
   + Enter the NFS Server Private IP installed through [[NFS Server Installation](../nfs-server-install-guide.md)] guide<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Portal Providing Type <br>
   + This guide is a portal K-PaaS service deployment installation guide that requires **'service'** value

<br>    

#### <div id='3.2.3'>3.2.3. Execute Container Platform Portal Deployment Script
Execute a deployment script for deploying a container platform portal.

```
$ chmod +x deploy-container-platform-portal.sh
$ ./deploy-container-platform-portal.sh
```
<br>

Verify if the resources related to the container platform portal is deployed normally.<br>
In the case of the resource Pod, it takes several seconds to enter the Running state after binding and container creation at the Node.

- **Retrieve NFS Resource**
>`$ kubectl get all -n nfs-storageclass`  
```
$ kubectl get all -n nfs-storageclass
NAME                                       READY   STATUS    RESTARTS   AGE
pod/nfs-pod-provisioner-7fff84f48f-ltd2w   1/1     Running   0          3m24s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nfs-pod-provisioner   1/1     1            1           3m24s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/nfs-pod-provisioner-7fff84f48f   1         1         1       3m24s
```

- **Retrieve Harbor Resource**
>`$ kubectl get all -n harbor`   
```
$ kubectl get all -n harbor
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-harbor-chartmuseum-859f844877-npzw9    1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-core-5f8cbdfc4b-vnbh5           1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-database-0                      1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-jobservice-c8cb6fd44-gc5vn      1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-nginx-755cf6876f-znmnn          1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-notary-server-9bb4d9774-rjd72   1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-notary-signer-7fd8996c68jfb7v   1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-portal-c79c78f5b-4w5qz          1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-redis-0                         1/1     Running   0          3m59s
pod/paas-ta-container-platform-harbor-registry-5d478577d7-hv76l       2/2     Running   0          3m59s
pod/paas-ta-container-platform-harbor-trivy-0                         1/1     Running   0          3m59s

NAME                                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                       AGE
service/harbor                                            NodePort    10.233.4.37     <none>        80:30002/TCP,4443:30004/TCP   3m59s
service/paas-ta-container-platform-harbor-chartmuseum     ClusterIP   10.233.1.163    <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-core            ClusterIP   10.233.4.62     <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-database        ClusterIP   10.233.45.86    <none>        5432/TCP                      3m59s
service/paas-ta-container-platform-harbor-jobservice      ClusterIP   10.233.54.157   <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-notary-server   ClusterIP   10.233.11.94    <none>        4443/TCP                      3m59s
service/paas-ta-container-platform-harbor-notary-signer   ClusterIP   10.233.56.213   <none>        7899/TCP                      3m59s
service/paas-ta-container-platform-harbor-portal          ClusterIP   10.233.27.172   <none>        80/TCP                        3m59s
service/paas-ta-container-platform-harbor-redis           ClusterIP   10.233.44.10    <none>        6379/TCP                      3m59s
service/paas-ta-container-platform-harbor-registry        ClusterIP   10.233.54.76    <none>        5000/TCP,8080/TCP             3m59s
service/paas-ta-container-platform-harbor-trivy           ClusterIP   10.233.3.176    <none>        8080/TCP                      3m59s

NAME                                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/paas-ta-container-platform-harbor-chartmuseum     1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-core            1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-jobservice      1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-nginx           1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-notary-server   1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-notary-signer   1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-portal          1/1     1            1           3m59s
deployment.apps/paas-ta-container-platform-harbor-registry        1/1     1            1           3m59s

NAME                                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/paas-ta-container-platform-harbor-chartmuseum-859f844877     1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-core-5f8cbdfc4b            1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-jobservice-c8cb6fd44       1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-nginx-755cf6876f           1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-notary-server-9bb4d9774    1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-notary-signer-7fd8996c68   1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-portal-c79c78f5b           1         1         1       3m59s
replicaset.apps/paas-ta-container-platform-harbor-registry-5d478577d7        1         1         1       3m59s

NAME                                                          READY   AGE
statefulset.apps/paas-ta-container-platform-harbor-database   1/1     3m59s
statefulset.apps/paas-ta-container-platform-harbor-redis      1/1     3m59s
statefulset.apps/paas-ta-container-platform-harbor-trivy      1/1     3m59s
```

- **Retrieve MariaDB Resource**
>`$ kubectl get all -n mariadb`  
```
$ kubectl get all -n mariadb
NAME                                       READY   STATUS    RESTARTS   AGE
pod/paas-ta-container-platform-mariadb-0   1/1     Running   0          2m13s

NAME                                         TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/paas-ta-container-platform-mariadb   NodePort   10.233.24.16   <none>        3306:31306/TCP   2m13s

NAME                                                  READY   AGE
statefulset.apps/paas-ta-container-platform-mariadb   1/1     2m13s
```    

- **Retrieve Keycloak Resource**
>`$ kubectl get all -n keycloak`  
```
$ kubectl get all -n keycloak
NAME                            READY   STATUS    RESTARTS   AGE
pod/keycloak-5bdb65fcb5-25cmn   1/1     Running   0          3m10s
pod/keycloak-5bdb65fcb5-ndxtm   1/1     Running   0          3m10s

NAME                       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/keycloak           NodePort    10.233.56.97   <none>        8080:32710/TCP   3m10s
service/keycloak-cluster   ClusterIP   None           <none>        8080/TCP         3m10s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/keycloak   2/2     2            2           3m10s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/keycloak-5bdb65fcb5   2         2         2       3m10s
```

- **Retrieve Container Platform Portal Resource**
>`$ kubectl get all -n paas-ta-container-platform-portal`   
```
$ kubectl get all -n paas-ta-container-platform-portal
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/container-platform-admin-service-broker-deployment-54cdcc7qzxxn   1/1     Running   0          3m23s
pod/container-platform-api-deployment-64d487c88-2c9zw                 1/1     Running   0          3m26s
pod/container-platform-common-api-deployment-645c8486dd-lf94j         1/1     Running   0          3m25s
pod/container-platform-user-service-broker-deployment-547fdb7c45mkd   1/1     Running   0          3m23s
pod/container-platform-webadmin-deployment-587645b5c-bvpfr            1/1     Running   0          3m25s
pod/container-platform-webuser-deployment-58b6b79669-jg8vf            1/1     Running   0          3m24s

NAME                                                      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-admin-service-broker-service   NodePort   10.233.1.48     <none>        3330:32704/TCP   3m23s
service/container-platform-api-service                    NodePort   10.233.47.173   <none>        3333:32701/TCP   3m26s
service/container-platform-common-api-service             NodePort   10.233.46.171   <none>        3334:32700/TCP   3m25s
service/container-platform-user-service-broker-service    NodePort   10.233.54.150   <none>        3331:32705/TCP   3m23s
service/container-platform-webadmin-service               NodePort   10.233.57.102   <none>        8090:32703/TCP   3m25s
service/container-platform-webuser-service                NodePort   10.233.0.236    <none>        8091:32702/TCP   3m24s

NAME                                                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-admin-service-broker-deployment   1/1     1            1           3m23s
deployment.apps/container-platform-api-deployment                    1/1     1            1           3m26s
deployment.apps/container-platform-common-api-deployment             1/1     1            1           3m25s
deployment.apps/container-platform-user-service-broker-deployment    1/1     1            1           3m23s
deployment.apps/container-platform-webadmin-deployment               1/1     1            1           3m25s
deployment.apps/container-platform-webuser-deployment                1/1     1            1           3m24s

NAME                                                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-admin-service-broker-deployment-54cdcc7b76   1         1         1       3m23s
replicaset.apps/container-platform-api-deployment-64d487c88                     1         1         1       3m26s
replicaset.apps/container-platform-common-api-deployment-645c8486dd             1         1         1       3m25s
replicaset.apps/container-platform-user-service-broker-deployment-547fdb7cb5    1         1         1       3m23s
replicaset.apps/container-platform-webadmin-deployment-587645b5c                1         1         1       3m25s
replicaset.apps/container-platform-webuser-deployment-58b6b79669                1         1         1       3m24s
```    

<br>

#### <div id='3.2.4'>3.2.4. (Refer)Delete Container Platform Portal Resource
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
release "paas-ta-container-platform-admin-service-broker" uninstalled
release "paas-ta-container-platform-user-service-broker" uninstalled
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

## <div id='4'>4. Configuring Container Platform Portal User Authentication Service
Container platform portal user authentication is managed through the Keycloak service. To access the container platform portal with the user account of the K-PaaS portal user authentication service UAA, a step is required to configure the UAA service as an identity provider and the Keycloak service as a service provider.

#### <div id='4.1'>4.1. Download Container Platform Portal User Authentication Configuration Deployment
Download the Deployment file for configuring UAA service and Keycloak service authentication and locate it in the path below.<br>
:bulb: It will be done at the **BOSH Inception** where K-PaaS Portal is installed.

+ Download Container Platform Portal User Authentication Configuration Deployment :  
   [paas-ta-container-platform-saml-deployment.tar.gz](https://nextcloud.paas-ta.org/index.php/s/iJYjroasEA9BJgs/download)  

```
# Create Deployment File Download Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment File and Check File Path
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/iJYjroasEA9BJgs/download

$ ls ~/workspace/container-platform
  paas-ta-container-platform-saml-deployment.tar.gz

# Unzip Deployment File
$ tar -xvf paas-ta-container-platform-saml-deployment.tar.gz
```
<br>

#### <div id='4.2'>4.2. Defining Container Platform Portal User Authentication Configurations
Defining variable values for configuring UAA service and Keycloak service authentication is required. Set the variable by checking the information required for the configuration.

:bulb: If **Keycloak TLS HTTPS** setting is applied, the Keycloak URL variable value needs to be changed. <br>
Refer to the guide below to change the variable value.
> [(Service Deployment) Change User Authentication Service Configuration](cp-portal-deployment-keycloak-tls-setting-guide-v1.2#3-service-deployment-modify-user-authentication-service-configuration)       

<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-saml-deployment
$ vi container-platform-saml-vars.sh
```

```                                                     
# COMMON VARIABLE
PAASTA_SYSTEM_DOMAIN="xx.xxx.xx.xxx.nip.io"                       # PaaS-TA System Domain
K8S_MASTER_NODE_IP="xx.xxx.xx.xxx"                                # Kubernetes Master Node Public Ip
UAA_CLIENT_ADMIN_ID="admin"                                       # UAA Admin Client ID (e.g. admin)
UAA_CLIENT_ADMIN_SECRET="admin-secret"                            # UAA Admin Client Secret (e.g. admin-secret)
....    
```


- **PAASTA_SYSTEM_DOMAIN** <br> Enter the System Domain name that was specified during PaaS-TA deployment<br><br>
- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **UAA_CLIENT_ADMIN_ID** <br>Enter UAAC Admin Client Admin ID (Default value : admin)<br><br>
- **UAA_CLIENT_ADMIN_SECRET** <br>Secret variable to access to UAAC Admin Client (Default value : admin-secret)<br><br>

<br>

#### <div id='4.3'>4.3. Execute Container Platform Portal User Authentication Configuration Script
Execute script for configuring UAA service and Keycloak service authentication.

```
$ chmod +x create-service-provider.sh
$ ./create-service-provider.sh
```

<br>

Verify if the configuration is processed normally. (**Check result of RESPONSE BODY**)
- Check UAAC Service Providers   
>`$ uaac curl /saml/service-providers --insecure`     
```    
$ uaac curl /saml/service-providers --insecure
GET https://uaa.xx.xxx.xxx.xx.nip.io/saml/service-providers

200 OK
RESPONSE HEADERS:
  Cache-Control: no-cache, no-store, max-age=0, must-revalidate
  Content-Type: application/json
  Date: Tue, 14 Dec 2021 02:08:06 GMT
  Expires: 0
  Pragma: no-cache
  Strict-Transport-Security: max-age=31536000 ; includeSubDomains
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-Vcap-Request-Id: e6934abf-d9b8-41ae-7e3f-bebfa688d198
  X-Xss-Protection: 1; mode=block
  Content-Length: 1768
  Connection: close
RESPONSE BODY:
[
  {
    "config": "{\"metaDataLocation\": .... }",
    "id": "0679dca3-0461-4af9-b513-7c114b6f9110",
    "entityId": "http://xx.xxx.xxx.xx:32710/auth/realms/container-platform-realm",
    "name": "paas-ta-container-platform-saml-sp",
    "version": 0,
    "created": 1639447555314,
    "lastModified": 1639447555314,
    "active": true,
    "identityZoneId": "uaa"
  }
....    
]
```    

<br>

#### <div id='4.4'>4.4. (Refer) Unconfiguring Container Platform Portal User Authentication
To unconfigure the UAA service and Keycloak service authentication, run the script below.<br>
:loudspeaker: (Note) When executing the script while the container platform portal is operating, caution is needed because user authentication cannot be configured.<br>


##### Look up for Service Provider ID to remove
After retrieving UAAC Service Providers, Look for the **Service Provider ID**with the following conditions in the results in **RESPONSE BODY**.
- `entityId : http://{K8S_MASTER_NODE_IP}:32710/auth/realms/container-platform-realm` <br>
- `name : paas-ta-container-platform-saml-sp` <br>

```  
$ uaac curl /saml/service-providers --insecure

....
RESPONSE BODY:
[
  {
    "config": "{\"metaDataLocation\": .... }",
    "id": "0679dca3-0461-4af9-b513-7c114b6f9110",   # Service Provider ID to remove
    "entityId": "http://xx.xxx.xxx.xx:32710/auth/realms/container-platform-realm",
    "name": "paas-ta-container-platform-saml-sp",
    "version": 0,
    "created": 1639447555314,
    "lastModified": 1639447555314,
    "active": true,
    "identityZoneId": "uaa"
  }
....    
]
```    

<br>

After retrieving the **Service Provider ID** to remove, execute the unconfigure authentication script.

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-saml-deployment
$ chmod +x uninstall-service-provider.sh
$ ./uninstall-service-provider.sh {Service_Provider_ID}
```

```    
$ ./uninstall-service-provider.sh 0679dca3-0461-4af9-b513-7c114b6f9110
....  
Are you sure you want to delete this service provider? <y/n> y
DELETE https://uaa.13.125.147.203.nip.io/saml/service-providers/0679dca3-0461-4af9-b513-7c114b6f9110
....    
```

<br>

## <div id='5'>5. Container Platform Portal Service Broker
When installing as a container platform K-PaaS service-type portal, brokers must be registered to interwork the container platform portal service deployed to CF and Kubernetes.
If you register and disclose the service through the K-PaaS operator portal, you can apply for and use the service through the K-PaaS user portal.

### <div id='5.1'>5.1. Container Platform Portal Service Broker Registration
When registering a service broker, you must be logged in as a user who can register a service broker on an open cloud platform.

##### Check Service Broker List.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...

name   url
No service brokers found
```


##### Register Container Platform Portal Service Broker.
>`$ cf create-service-broker {Servicepack Name} {Servicepack User ID} {Servicepack User Password} http://{Servicepack URL}`

Servicepack Name: Names seen on open cloud platforms for service pack management<br>
Servicepack User ID/PW: User ID/password to access the service pack<br>
Servicepack URL: URL where the API provided by the service pack is available<br>


###### Register Container Platform Portal Service Broker
>`$ cf create-service-broker container-platform-admin-portal-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:32704`   
###### Register Container Platform Portal Service Broker     
>`$ cf create-service-broker container-platform-user-portal-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:32705`

```
$ cf create-service-broker container-platform-admin-portal-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:32704
Creating service broker container-platform-admin-portal-service-broker as admin...
OK

$ cf create-service-broker container-platform-user-portal-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:32705
Creating service broker container-platform-user-portal-service-broker as admin...
OK
```    


##### Check the registered container platform portal service broker.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...
name                                             url
container-platform-admin-portal-service-broker   http://xx.xxx.xxx.xx:32704
container-platform-user-portal-service-broker    http://xx.xxx.xxx.xx:32705
```


##### Check the accessible service list.
>`$ cf service-access`     
```
$ cf service-access
Getting service access as admin...

broker: container-platform-admin-portal-service-broker
   offering                                         plan       access   orgs
   container-platform-admin-portal-service-broker   Advenced   none

broker: container-platform-user-portal-service-broker
   offering                                        plan       access   orgs
   container-platform-user-portal-service-broker   Advenced   none
   container-platform-user-portal-service-broker   Micro      none
   container-platform-user-portal-service-broker   Small      none
```


##### Assign authorization to access a specific organization.

###### Assign Access to Container Platform Operator Portal Services  
>`$ cf enable-service-access container-platform-admin-portal-service-broker`   
###### Assign Access to Container Platform User Portal Services     
>`$ cf enable-service-access container-platform-user-portal-service-broker`

```
$ cf enable-service-access container-platform-admin-portal-service-broker
Enabling access to all plans of service offering container-platform-admin-portal-service-broker for all orgs as admin...
OK

$ cf enable-service-access container-platform-user-portal-service-broker
Enabling access to all plans of service offering container-platform-user-portal-service-broker for all orgs as admin...
OK
```


##### Check accessible service list.
>`$ cf service-access`

```
$ cf service-access
Getting service access as admin...

broker: container-platform-admin-portal-service-broker
   offering                                         plan       access   orgs
   container-platform-admin-portal-service-broker   Advanced   all

broker: container-platform-user-portal-service-broker
   offering                                        plan       access   orgs
   container-platform-user-portal-service-broker   Advenced   all
   container-platform-user-portal-service-broker   Micro      all
   container-platform-user-portal-service-broker   Small      all
```

<br>

### <div id='5.2'>5.2. Container Platform Portal Service Lookup Settings
This setting is to enable the container platform portal service to be inquired and applied from the K-PaaS portal.

##### Access to the K-PaaS Operator Portal.
![image 007]


##### In [Operation Management]-[Catalog] menu,  select Container Platform Admin Portal and Container Platform User Portal services in the App Services tab and change the settings.
![image 008]

##### Select the Container Platform Admin Portal service and change the setting as shown below and save it.
>`'Service' List : Select 'container-platform-admin-portal-service-broker'` <br>
>`' Public' Catalog: Check as 'Y'`

![image 009]

##### Select the Container Platform User Portal service and change the setting as follows and save it.
>`'Service' Catalog : Select 'container-platform-user-portal-service-broker'` <br>
>`' Public' Catalog: Check as 'Y'`

![image 010]    

<br>

#### :bulb: Precautions when applying for container platform operator portal service
- In the case of container platform operator portal service, the application can only be made from the organization name **'portal'**.
- The container platform operator portal service is available only from one organization within the entire organization, and it is designated as an organization **'portal'** that is created by default when the K-PaaS Portal is deployed.
- If there is no organization **'portal'** when applying for the service, it is necessary to create the organization with **'portal'** and apply for the service.

<br>

>'When applying for the container platform operator portal service, you need to check the organization name 'portal' before applying.'     

![image 011]       

<br>

### <div id='5.3'/>5.3. Container Platform User/ Operator Portal Use Guide
- Refer to the use guide below for container platform portal usage.  
  + [Container Platform Operator Portal Use Guide](../../use-guide/portal/container-platform-admin-portal-guide.md)    
  + [Container Platform User Portal Use Guide](../../use-guide/portal/container-platform-user-portal-guide.md)


<br>

## <div id='6'>6. Container Platform Portal Reference

### <div id='6.1'>6.1. Create Operator Cluster Role Token
Create a service account of an operator with a cluster role and obtain the token value of the service account..<br>
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

- Check the Mountable secrets value of the Service Account.
```
$ kubectl describe serviceaccount {SERVICE_ACCOUNT} -n kube-system
(ex. kubectl describe serviceaccount k8sadmin -n kube-system)

...

Mountable secrets:   k8sadmin-token-xxxx
```

- Aquire Token of Service Account.

```
## {SECRET_NAME} : Enter Mountable secrets Value

$ kubectl describe secret {SECRET_NAME} -n kube-system | grep -E '^token' | cut -f2 -d':' | tr -d " "
```

<br>

### <div id='6.2'>6.2. Precautions for Creating Kubernetes Resources

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

### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Service Deployment Portal Installation Guide

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
