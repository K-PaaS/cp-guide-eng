### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > SourceControl Installation Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
    1.1. [Purpose](#1.1)  
    1.2. [Range](#1.2)  
    1.3. [System Configuration Diagram](#1.3)  
    1.4. [References](#1.4)  

2. [Prerequisite](#2)  
    2.1. [NFS Server Installation](#2.1)  
    2.2. [Container Platform Portal Installation](#2.2)  
        
3. [Container Platform SourceControl Deployment](#3)  
    3.1. [CRI-O insecure-registry Setting](#3.1)  
    3.2. [Container Platform SourceControl Deployment](#3.2)  
    3.2.1. [Container Platform SourceControl Deployment File Download](#3.2.1)  
    3.2.2. [Define Container Platform SourceControl Variable](#3.2.2)    
    3.2.3. [Execute Container Platform SourceControl Deployment Script](#3.2.3)    
    3.2.4. [(Refer) Delete Container Platform SourceControl Resource](#3.2.4)    

4. [Container Platform SourceControl Access](#4)      
    4.1. [Container Platform SourceControl Operator Log in](#4.1)      
    4.2. [Container Platform SourceControl User Sign in/ Log in](#4.2)  
    4.3. [Container Platform SourceControl Use Guide](#4.3)           



## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform Source Control Stand-Alone Deployment Installation Guide) describes how source controls are deployed in a stand-alone deployment Kubernetes Cluster environment where container platform portals are deployed.<br>

<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on Kubernetes' stand-alone deployment.

<br>

### <div id='1.3'>1.3. System Configuration Diagram
The system configuration consists of Kubernetes Cluster (Master, Worker) and Cluster Internal (DBMS, HAProxy, Private Repository, Keycloak) environments. <br>
Kubernetes Cluster is installed through Kubespray and middleware environments such as Database and Private Repository are provided as container platform portals to deploy container platform portal environments to Kubernetes Cluster with Docker Image. <br>
For the total required VM environment,  **1 Master VM and 1 or more Worker VMs** are required. This document is about deploying container platform source controls in a Kubernetes cluster.

![image](https://user-images.githubusercontent.com/80228983/146350860-3722c081-7338-438d-b7ec-1fdac09160c4.png)

    
<br>    

### <div id='1.4'>1.4. References
> https://kubernetes.io/ko/docs  

<br>

## <div id='2'>2. Prerequisite
    
### <div id='2.1'>2.1. NFS Server Installation
Installation of storage **NFS Storage Server** to be used by the container platform source control must be performed ahead.<br>
Refer to the guide below for NFS Storage Server installation.  
> [NFS Server Installation](../nfs-server-install-guide.md)      
    
### <div id='2.2'>2.2. Container Platform Portal Installation
Installation of the certificate server **KeyCloak Server**, database **Maria DB**, and repository server **Harbor** must be performed in advance as the infrastructure to be used by the container platform source control.
When deploying the pasta container platform portal, all the infrastructures will be installed.
For installation of Container Platform Infrastructure, refer to the guide below.
> [K-PaaS Container Platform Portal Deployment](../container-platform-portal/cp-portal-deployment-standalone-guide-v1.2.md)     


## <div id='3'>3. Container Platform Source Control Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
When deploying the container platform source control, upload images and package files to the private repository installed in the cluster.
Upload the container platform portal to the deployed Private Repository (Harbor) with images and package files related to the container platform source control. 

Refer to the guide below for the CRI-O insecure-registry settings required for a Private Repository deployment.
> [CRI-O insecure-registry Setting](../container-platform-portal/cp-portal-deployment-standalone-guide-v1.2.md#3.1)      

### <div id='3.2'>3.2. Container Platform Source Control Deployment 
    
#### <div id='3.2.1'>3.2.1. Container Platform Source Control Deployment File Download
Download the container platform source control Deployment file and locate it in the path below for container platform source control deployment.<br>
:bulb: Process this content at the **Master Node** of Kubernetes. 

+ Container Platform Source Control Deployment File Download :  
   [paas-ta-container-platform-source-control-deployment.tar.gz](https://nextcloud.paas-ta.org/index.php/s/6WG9C29tjQGY8We)  

```
# Create Deployment File Download Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Deployment File Download and Check File Path
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/6WG9C29tjQGY8We/download

$ ls ~/workspace/container-platform
  ...
  paas-ta-container-platform-source-control-deployment.tar.gz
  ...

# Unzip Deployment file
$ tar xvfz paas-ta-container-platform-source-control-deployment.tar.gz
```

- Configure Deployment File Directory
```
├── script          # Location of Container Platform Source Control Deployment Related Variables and Script File
├── images          # Location of Container Platform Source Control Image File
├── charts          # Location of Container Platform Source Control Helm Charts File
├── values_orig     # Location of Container Platform Source Control Helm Charts values.yaml Source File 
```

<br>

#### <div id='3.2.2'>3.2.2. Define Container Platform SourceControl Variable
Defining variable values is required before deploying container platform source controls. Set the variable by checking the information required for deployment.

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-source-control-deployment/script
$ vi container-platform-source-control-vars.sh
```

```                                                     
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}"                 # Kubernetes master node public ip
PROVIDER_TYPE="{container platform source control provider type}"        # Container platform source-control provider type (Please enter 'standalone' or 'service')
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"                                       
PROVIDER_TYPE="standalone"           
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Source Control Providing Type <br>
   + This guide is a standalone installation guide that requires **'standalone'** values
<br>    

:bulb: Keycloak default deployment method is **HTTP**, and **HTPS** via certificate is enabled
> [Keycloak TLS Settings](../container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide-v1.2.md)

Modify the contents below in the container platform source control variable file.
```
$ vi container-platform-source-control-vars.sh    
```    
```
# Change KEYCLOAK_URL Value http -> https  
# If you are using nip.io as a Domain, change as shown below:
    
....  
# KEYCLOAK    
KEYCLOAK_URL="https:\/\/${K8S_MASTER_NODE_IP}.nip.io:32710"   # Keycloak url (include http:\/\/, if apply TLS, https:\/\/)
....     
```

#### <div id='3.2.3'>3.2.3. Execute Container Platform SourceControl Deployment Script
Execute the deployment script for deploying the container platform source control.

```
$ chmod +x deploy-container-platform-source-control.sh
$ ./deploy-container-platform-source-control.sh
```

```

...
...
NAME: paas-ta-container-platform-source-control-api
LAST DEPLOYED: Thu Dec 16 10:24:29 2021
NAMESPACE: paas-ta-container-platform-source-control
STATUS: deployed
REVISION: 1
TEST SUITE: None
NAME: paas-ta-container-platform-source-control-manager
LAST DEPLOYED: Thu Dec 16 10:24:30 2021
NAMESPACE: paas-ta-container-platform-source-control
STATUS: deployed
REVISION: 1
TEST SUITE: None
NAME: paas-ta-container-platform-source-control-broker
LAST DEPLOYED: Thu Dec 16 10:24:31 2021
NAMESPACE: paas-ta-container-platform-source-control
STATUS: deployed
REVISION: 1
TEST SUITE: None
NAME: paas-ta-container-platform-source-control-ui
LAST DEPLOYED: Thu Dec 16 10:24:32 2021
NAMESPACE: paas-ta-container-platform-source-control
STATUS: deployed
REVISION: 1
TEST SUITE: None

```

<br>
    
- **Container Platform Source Control**

```
# Check Source Control Resource
$ kubectl get all -n paas-ta-container-platform-source-control
```
```
    
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/container-platform-source-control-api-deployment-5f4b7864fvs8z7   1/1     Running   0          103s
pod/container-platform-source-control-manager-deployment-78d7fq5wnd   1/1     Running   0          103s
pod/container-platform-source-control-ui-deployment-55dc694d8cwn92x   1/1     Running   0          102s

NAME                                                        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-source-control-api-service       NodePort   10.233.40.199   <none>        8091:30091/TCP   103s
service/container-platform-source-control-manager-service   NodePort   10.233.4.243    <none>        8080:30092/TCP   103s
service/container-platform-source-control-ui-service        NodePort   10.233.39.215   <none>        8094:30094/TCP   102s

NAME                                                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-source-control-api-deployment       1/1     1            1           103s
deployment.apps/container-platform-source-control-manager-deployment   1/1     1            1           103s
deployment.apps/container-platform-source-control-ui-deployment        1/1     1            1           102s

NAME                                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-source-control-api-deployment-5f4b7864f6       1         1         1       103s
replicaset.apps/container-platform-source-control-manager-deployment-78d7f9cf75   1         1         1       103s
replicaset.apps/container-platform-source-control-ui-deployment-55dc694d8c        1         1         1       102s



```    

<br>

#### <div id='3.2.4'>3.2.4. (Refer) Delete Container Platform SourceControl Resource
To delete the deployed container platform source control resource, follow the script below.<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-source-control-deployment/script
$ chmod +x uninstall-container-platform-source-control.sh
$ ./uninstall-container-platform-source-control.sh

```
```
...

service "container-platform-source-control-api-service" deleted
service "container-platform-source-control-manager-service" deleted
service "container-platform-source-control-ui-service" deleted
release "paas-ta-container-platform-source-control-api" uninstalled
release "paas-ta-container-platform-source-control-manager" uninstalled
release "paas-ta-container-platform-source-control-ui" uninstalled
...
...
namespace "paas-ta-container-platform-source-control" deleted
...
...
```

## <div id='4'>4. Container Platform SourceControl Access  
The container platform source control page can be accessed through the address below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Source Control standalone Access URI : **http://{K8S_MASTER_NODE_IP}:30094** <br>

<br>
    
### <div id='4.1'/>4.1. Container Platform SourceControl Operator Log in
The initial information for accessing the container platform source control is as follows.
- Access to http://{K8S_MASTER_NODE_IP}:30094.   
- Use username : **admin** / password : **admin** account to log in to Container Platform Source Control.
![image](https://user-images.githubusercontent.com/80228983/146140178-76e85cbb-03a0-4a84-9059-7e5074c1d90e.png)

<br>    


### <div id='4.2'/>4.2. Container Platform SourceControl User Sign in/ Log in
#### User Sign in    
- Access to Keycloak(http://{K8S_MASTER_NODE_IP}:32710).
- Access to the Administration Console. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140243-b01fe7b7-c610-4c74-b520-839b581ca178.png)
<br>

- Use the username : **admin** / password : **admin** account to access. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140270-06c6bc41-94cd-4947-8376-f6ade73b61ac.png)
<br>

- Click 'Users' list from the left buttom of the menu. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140350-c2bed5ab-0683-47cf-838b-6c970e492605.png)
<br>

- Click Add User at the right. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140392-1eb2d2e4-47d7-4fd2-8370-9d3e5b2a9871.png)
<br>

- Enter Username and change the Email Verified switch in to On. Click Save. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140503-3ad5e8f7-613b-4583-83fe-3f6623736286.png)
<br>

- Move to the Credentials tab.
- Enter Password / PassWord Confirmation to set password.
- If the password is not a temporary password, Set the Temporary switch into Off.<br>
    ![image](https://user-images.githubusercontent.com/80228983/146140746-54e57681-584c-43b3-93f1-f172205d34d2.png)
 <br>

####  Login   
- Access to http://{K8S_MASTER_NODE_IP}:30094.
- Log in to the Container Platform Source Control by using the registered account.
    
<br>    

### <div id='4.3'/>4.3. Container Platform SourceControl Use Guide
- For the usage of Container Platform Source Control, refer to the use guide below.  
  + [Container Platform Source Control Use Guide](../../use-guide/source-control/cp-source-control-use-guide.md)   

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > SourceControl Installation Guide
