### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Pipeline Installation Guide

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
    2.3. [Cluster Environment](#2.3)
        
3. [Container Platform Pipeline Deployment](#3)  
    3.1. [CRI-O insecure-registry Setting](#3.1)  
    3.2. [Container Platform Pipeline Deployment](#3.2)  
    3.2.1. [Container Platform Pipeline Deployment File Download](#3.2.1)  
    3.2.2. [Define Container Platform Pipeline Variable](#3.2.2)    
    3.2.3. [Execute Container Platform Pipeline Deployment Script](#3.2.3)    
    3.2.4. [(Refer) Delete Container Platform Pipeline Resource](#3.2.4)    

4. [Container Platform Pipeline Access](#4)      
    4.1. [Container Platform Pipeline Operator Login](#4.1)      
    4.2. [Container Platform Pipeline User Sign in/ Log in](#4.2)  
    4.3. [Container Platform Pipeline Use Guide](#4.3)           



## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform Pipeline Stand-Alone Deployment Installation Guide) installs the Kubernetes Cluster and Container Platform Stand-Alone Deployment Portal and describes how to deploy the Container Platform Stand-Alone Deployment Pipeline.<br>.<br>

<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration Diagram
![image](https://user-images.githubusercontent.com/80228983/146350860-3722c081-7338-438d-b7ec-1fdac09160c4.png)
<br>
The system configuration consists of a Kubernetes Cluster (Master, Worker) environment and a Network File System (NFS) storage server for data management. 
Kubespray-installed Kubernetes Cluster environment provided through the container platform portal which are
Harbor manages container platform pipeline images and Helm charts, Keycloak manages container platform pipeline user authentication, and MariaDB (RDBMS) manages container platform pipeline metadata.
 The container platform pipeline provides the environment required for pipeline operation, such as Jenkins Server (Ci-Server), which manages continuous integration and deployment functions, Sonarqube (Inspection-Server) for static analysis, and Spring Config Server (Config-Server), which manages the configuration of the deployed application. 
The total VM environment required is one Master Node VM, one or more Worker Node VMs, and one NFS Server, and this document is about deploying a container platform pipeline environment in a Kubernetes cluster. Network File System (NFS) is the storage provided by the container platform and can use various types of storage depending on the user environment.

### <div id='1.4'>1.4. References
> https://kubernetes.io/ko/docs  

<br>

## <div id='2'>2. Prerequisite
    
### <div id='2.1'>2.1. NFS Server Installation
Installation of storage **NFS Storage Server** to be used in the container platform pipeline must be performed ahead.<br>
For NFS Storage Server installation, refer to the guide below.  
> [NFS Server Installation](../nfs-server-install-guide.md)      
    
### <div id='2.2'>2.2. Container Platform Portal Installation
The infrastructure to be used in the container platform pipeline must be pre-installed with the authenticating server **KeyCloak Server**, database **Maria DB**, and repository server **Harbor**.
When deploying the K-PaaS container platform portal, install all the infrastructure.
Refer to the guide below for Container Platform infra installation.
> [K-PaaS Container Platform Portal Deployment](../container-platform-portal/cp-portal-deployment-standalone-guide-v1.2.md)     


### <div id='2.3'>2.3. Cluster Environment
Install in an environment that allows **External Network Communication** because images such as sonarqube and postgresql are downloaded from the public network for Container Platform Deployment. <br>
When the container platform pipeline installation is completed, the resource used in the idle state is as follows.
```
NAME                                                              CPU(cores)   MEMORY(bytes)
container-platform-pipeline-api-deployment-5dcc5bd8c5-99vgp       2m           191Mi
container-platform-pipeline-common-api-deployment-fbf44dc9tsrb8   2m           239Mi
container-platform-pipeline-config-server-deployment-5555dhbtrn   2m           164Mi
container-platform-pipeline-inspection-api-deployment-65f5gbrvw   2m           156Mi
container-platform-pipeline-jenkins-deployment-66845767f9-gl8v6   2m           777Mi
container-platform-pipeline-ui-deployment-5b68b494bf-8dbjf        1m           155Mi
paas-ta-container-platform-postgresql-postgresql-0                5m           54Mi
paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97-2d6xg   10m          1577Mi
```
For cluster environments where container platform pipelines are to be installed, a minimum of **4Gi** total free memory is recommended.

When the container platform pipeline installation is completed, the resource for using Persistent Volume is as follows.    
```
NAME                                                      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                                  AGE
container-platform-pipeline-jenkins-pv                    Bound    pvc-9faf103f-5462-4a76-9f4b-f9bd81e1471b   20Gi        RWO            paas-ta-container-platform-nfs-storageclass   30h
data-paas-ta-container-platform-postgresql-postgresql-0   Bound    pvc-327312f3-35b9-4e4e-aa2a-0f094f5fdd0a   8Gi        RWX            paas-ta-container-platform-nfs-storageclass   30h

```
NFS storage capacity **28Gi** is recommended for cluster environments where container platform pipelines will be installed.<br>        
    
## <div id='3'>3. Container Platform Pipeline Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
When deploying the container platform pipeline, upload the image and package file to the private repository installed in the cluster.
Upload images and package files related to the container platform pipeline to the Private Repository (Harbor) deployed through the container platform portal. 

Refer to the guide below for CRI-O insecure-registry settings needed for the Private Repository.
> [CRI-O insecure-registry Setting](../container-platform-portal/cp-portal-deployment-standalone-guide-v1.2.md#3.1)      

### <div id='3.2'>3.2. Container Platform Pipeline Deployment
    
#### <div id='3.2.1'>3.2.1. Container Platform Pipeline Deployment File Download
For container platform pipeline deployment, download the container platform pipeline deployment file and locate it in the path below.<br>
:bulb: This content will be conducted at Kubernetes **Master Node**.

+ Container Platform Pipeline Deployment File Download :  
   [paas-ta-container-platform-pipeline-deployment.tar](https://nextcloud.paas-ta.org/index.php/s/6BDzar68ck5jryq)  

```
# Create Deployment File Download Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Deployment File Download and Check File Path
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/6BDzar68ck5jryq/download

$ ls ~/workspace/container-platform
  ...
  paas-ta-container-platform-pipeline-deployment.tar.gz
  ...
# Unzip Deployment File
$ tar xvfz paas-ta-container-platform-pipeline-deployment.tar.gz
```

- Configure Deployment File Directory
```
├── script          # Container Platform Pipeline Deployment Variables and Script File Locations
├── images          # Container Platform Pipeline Image File Location
├── charts          # Container Platform Pipeline Helm Charts File Location
├── values_orig     # Container Platform Pipeline Helm Charts values.yaml Source File Location 
```

<br>

#### <div id='3.2.2'>3.2.2. Define Container Platform Pipeline Variable
Defining variable values is necessary before deploying a container platform pipeline. Set the variable by checking the information required for deployment.

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-pipeline-deployment/script
$ vi container-platform-pipeline-vars.sh
```

```                                                     
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}"                 # Kubernetes master node public ip
PROVIDER_TYPE="{container platform pipeline provider type}"        # Container platform pipeline provider type (Please enter 'standalone' or 'service')
CF_API_URL="https:\/\/{paas-ta-api-domain}"                      # e.g) In case of https:\/\/api.10.0.0.120.nip.io, PaaS-TA API Domain, PROVIDER_TYPE=service
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"                                       
PROVIDER_TYPE="standalone"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Pipeline Providing Type <br>
   + This guide is a standalone installation guide that requires **'standalone'** values<br><br>
- **CF_API_URL** <br>No need to enter in stand-alone deployment version <br>    

<br>

:bulb: Keycloak's default deployment method is **HTTP** and **HTTPS** via certificate is enabled
> [Keycloak TLS Settig](../container-platform-portal/paas-ta-container-platform-portal-deployment-keycloak-tls-setting-guide-v1.2.md)

Modified the contents under the container platform pipeline variable file.
```
$ vi container-platform-pipeline-vars.sh    
```    
```
# Change KEYCLOAK_URL value http -> https 
# If nip.io is being used with Domain, set as shown below
    
....  
# KEYCLOAK    
KEYCLOAK_URL="https:\/\/${K8S_MASTER_NODE_IP}.nip.io:32710"   # Keycloak url (include http:\/\/, if apply TLS, https:\/\/)
....     
```

#### <div id='3.2.3'>3.2.3. Execute Container Platform Pipeline Deployment Script
Execute a deployment script for deploying a container platform pipeline.

```
$ chmod +x deploy-container-platform-pipeline.sh
$ ./deploy-container-platform-pipeline.sh
```

```

...
...
NAME: paas-ta-container-platform-pipeline-api
LAST DEPLOYED: Tue Dec 14 04:23:06 2021
NAMESPACE: paas-ta-container-platform-pipeline
STATUS: deployed
REVISION: 1
TEST SUITE: None
...
...
NAME: paas-ta-container-platform-sonarqube
LAST DEPLOYED: Tue Dec 14 04:23:12 2021
NAMESPACE: paas-ta-container-platfrom-pipeline
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace paas-ta-container-platfrom-pipeline -o jsonpath="{.spec.ports[0].nodePort}" services paas-ta-container-platform-sonarqube-sonarqube)
  export NODE_IP=$(kubectl get nodes --namespace paas-ta-container-platfrom-pipeline -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT

```


<br>

- **Container Platform Pipeline**

```
# Check Pipeline Resource
$ kubectl get all -n paas-ta-container-platform-pipeline
```

```
NAME                                                                  READY   STATUS    RESTARTS   AGE
pod/container-platform-pipeline-api-deployment-6c5cdd777f-lsb5h       1/1     Running   1          1h
pod/container-platform-pipeline-common-api-deployment-589768b97xxv7   1/1     Running   1          1h
pod/container-platform-pipeline-config-server-deployment-5555d4w5xl   1/1     Running   1          1h
pod/container-platform-pipeline-inspection-api-deployment-64fcp6kbp   1/1     Running   1          1h
pod/container-platform-pipeline-jenkins-deployment-5d9d9d4567-r2jlj   1/1     Running   1          1h
pod/container-platform-pipeline-ui-deployment-d96754495-6j69f         1/1     Running   1          1h
pod/paas-ta-container-platform-postgresql-postgresql-0                1/1     Running   1          1h
pod/paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97-5n2ts   1/1     Running   1          1h

NAME                                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-pipeline-api-service              NodePort    10.233.19.233   <none>        8082:30082/TCP   1h
service/container-platform-pipeline-common-api-service       NodePort    10.233.37.186   <none>        8081:30081/TCP   1h
service/container-platform-pipeline-config-server-service    NodePort    10.233.58.13    <none>        8080:30088/TCP   1h
service/container-platform-pipeline-inspection-api-service   NodePort    10.233.62.95    <none>        8085:30085/TCP   1h
service/container-platform-pipeline-jenkins-service          NodePort    10.233.43.109   <none>        8080:30086/TCP   1h
service/container-platform-pipeline-ui-service               NodePort    10.233.33.112   <none>        8084:30084/TCP   1h
service/paas-ta-container-platform-postgresql                ClusterIP   10.233.9.51     <none>        5432/TCP         1h
service/paas-ta-container-platform-postgresql-headless       ClusterIP   None            <none>        5432/TCP         1h
service/paas-ta-container-platform-sonarqube-sonarqube       NodePort    10.233.6.103    <none>        9000:30087/TCP   1h

NAME                                                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-pipeline-api-deployment              1/1     1            1           1h
deployment.apps/container-platform-pipeline-common-api-deployment       1/1     1            1           1h
deployment.apps/container-platform-pipeline-config-server-deployment    1/1     1            1           1h
deployment.apps/container-platform-pipeline-inspection-api-deployment   1/1     1            1           1h
deployment.apps/container-platform-pipeline-jenkins-deployment          1/1     1            1           1h
deployment.apps/container-platform-pipeline-ui-deployment               1/1     1            1           1h
deployment.apps/paas-ta-container-platform-sonarqube-sonarqube          1/1     1            1           1h

NAME                                                                               DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-pipeline-api-deployment-6c5cdd777f              1         1         1       1h
replicaset.apps/container-platform-pipeline-common-api-deployment-589768b984       1         1         1       1h
replicaset.apps/container-platform-pipeline-config-server-deployment-5555dff6b4    1         1         1       1h
replicaset.apps/container-platform-pipeline-inspection-api-deployment-64fc6484bf   1         1         1       1h
replicaset.apps/container-platform-pipeline-jenkins-deployment-5d9d9d4567          1         1         1       1h
replicaset.apps/container-platform-pipeline-ui-deployment-d96754495                1         1         1       1h
replicaset.apps/paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97          1         1         1       1h

NAME                                                                READY   AGE
statefulset.apps/paas-ta-container-platform-postgresql-postgresql   1/1     1h

```    

#### <div id='3.2.4'>3.2.4. (Refer) Delete Container Platform Pipeline Resource
To delete the deployed container platform pipeline resources, run the script below.<br>

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-pipeline-deployment/script
$ chmod +x uninstall-container-platform-pipeline.sh
$ ./uninstall-container-platform-pipeline.sh

```
```
...

replicaset.apps "container-platform-pipeline-common-api-deployment-5d8c66f94c" deleted
replicaset.apps "container-platform-pipeline-config-server-deployment-859fc5fd85" deleted
replicaset.apps "container-platform-pipeline-inspection-api-deployment-56945ffc96" deleted
replicaset.apps "container-platform-pipeline-jenkins-deployment-7466bbf878" deleted
replicaset.apps "container-platform-pipeline-ui-deployment-7698cc79c8" deleted
...
...
namespace "paas-ta-container-platform-pipeline" deleted
...
...
```

<br>

## <div id='4'>4. Container Platform Pipeline Access 
The container platform pipeline page can be accessed with the address below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Pipeline standalone Access URI : **http://{K8S_MASTER_NODE_IP}:30084** <br>

<br>
    
### <div id='4.1'/>4.1. Container Platform Pipeline Operator Login
The initial information for accessing the container platform pipeline is as follows.
- Access to http://{K8S_MASTER_NODE_IP}:30084.   
- Log in to the container platform pipeline with the username : **admin** / password : **admin** account.
![image](https://user-images.githubusercontent.com/80228983/146140178-76e85cbb-03a0-4a84-9059-7e5074c1d90e.png)

<br>    


### <div id='4.2'/>4.2. Container Platform Pipeline User Sign in/ Log in
#### User Sign in    
- Access to Keycloak(http://{K8S_MASTER_NODE_IP}:32710).
- Access to Administration Console. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140243-b01fe7b7-c610-4c74-b520-839b581ca178.png)  
<br>

- Access with username : **admin** / password : **admin** account. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140270-06c6bc41-94cd-4947-8376-f6ade73b61ac.png)

<br>

- Click 'Users' list from the buttom part of the menu on left. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140350-c2bed5ab-0683-47cf-838b-6c970e492605.png)   

<br>

- Click Add User at the right. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140392-1eb2d2e4-47d7-4fd2-8370-9d3e5b2a9871.png)  

<br>

- Enter the Username and change the Email Verified switch to On. Click Save. <br>
    ![image](https://user-images.githubusercontent.com/80228983/146140503-3ad5e8f7-613b-4583-83fe-3f6623736286.png)

<br>

- Proceed to Credentials tab.
- Enter Password / PassWord Confirmation to set password.
- If it is not a temporary number, set the Temporary switch to Off.<br>
    ![image](https://user-images.githubusercontent.com/80228983/146140746-54e57681-584c-43b3-93f1-f172205d34d2.png)
 <br>

####  Log in   
- Access to http://{K8S_MASTER_NODE_IP}:30084.
- Log in to the container platform pipeline with an account registered through sign-in.
    
<br>    

### <div id='4.3'/>4.3. Container Platform Pipeline Use Guide
- Refer to the use guide below for usage of container platform pipeline.  
  + [Container Platform Pipeline Use Guide](../../use-guide/pipeline/cp-pipeline-use-guide.md)    

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Pipeline Installation Guide
