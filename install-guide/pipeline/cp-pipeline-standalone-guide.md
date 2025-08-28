### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Container Platform Pipeline Installation Guide

<br>

## Table of Contents

1. [Document Overview](#1)  
   1.1. [Purpose](#1.1)  
   1.2. [Range](#1.2)  
   1.3. [System Configuration](#1.3)  
   1.4. [Reference](#1.4)

2. [Prerequisite](#2)  
   2.1. [Install Container Platform Portal](#2.1)  
   2.2. [Cluster Environments](#2.2)

3. [Deploy the Container Platform Pipeline](#3)      
   3.1. [Download the Container Platform Pipeline Deployment](#3.1)    
   3.2. [Define Container Platform Pipeline Variables](#3.2)    
   3.3. [Run the Container Platform Pipeline Deployment Script](#3.3)    
   3.4. [(Reference) Delete Container Platform Pipeline Resources](#3.4)

4. [Access the Container Platform Pipeline](#4)      
   4.1. [Login to the Container Platform Pipeline Administrator Account](#4.1)      
   4.2. [Login to the Container Platform Pipeline User Account](#4.2)  
   4.3. [Container Platform Pipeline Use Guide](#4.3)


<br>

## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. Purpose
This document (Container Platform Pipeline Deployment Guide) describes how to install a Kubernetes cluster and the Container Platform portal and deploy the pipeline.

<br>

### <div id='1.2'>1.2. Range
The installation scope is based on a Kubernetes cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration
<p align="center"><img src="../images/portal/cp-001.png" width="850" height="530"></p>
<br>

The system configuration consists of a **Kubernetes cluster (master, worker)** environment and a storage server for data management. **Harbor** to manage container platform pipeline images and Helm charts,
**Keycloak** to manage container platform pipeline user authentication, and **MariaDB (RDBMS)** to manage container platform pipeline metadata are provided through the container platform portal in the Kubernetes Cluster environment installed via Kubespray.
The container platform pipeline provides the environment necessary for pipeline operation, such as **Jenkins Server (Ci-Server)** to manage continuous integration and deployment functions, **Sonarqube (Inspection-Server)** for static analysis, and
**Spring Config Server (Config-Server)** to manage the configuration of deployed applications. The total required VM environment is at least **1 master VM and 3 worker VMs**, and this document is about deploying a container platform pipeline environment on a Kubernetes cluster.

<br>

### <div id='1.4'>1.4. Reference
> https://kubernetes.io

<br>

## <div id='2'>2. Prerequisite

### <div id='2.1'>2.1. Install Container Platform Portal
The infrastructure to be used in the container platform pipeline requires the installation of the authentication server **KeyCloak**, the database **MariaDB**, and the repository server **Harbor** in advance. When deploying the K-PaaS container platform portal,
all the relevant infrastructure is installed, so refer to the guide below to proceed with the pre-installation.

##### Single Cluster Environments
> [[Single Cluster Container Platform Portal Deployment Guide]](../portal/cp-portal-standalone-guide.md)
##### Multi Cluster environments
> [[Multi Cluster Container Platform Portal Deployment Guide]](../portal/cp-portal-standalone-guide-mc.md)


<br>

### <div id='2.2'>2.2. Cluster Environments
In order to deploy the container platform pipeline, images such as sonarqube and postgresql are downloaded from the public network, so they must be installed in an environment where **external network communication** is possible.
When the container platform pipeline installation is complete, the resources used in the idle state are as follows.

```bash
NAME                                                     CPU(cores)   MEMORY(bytes)
cp-pipeline-api-deployment-6d99f4c7d7-88h64              1m           224Mi
cp-pipeline-common-api-deployment-7488bc6d5d-q5wvt       2m           255Mi
cp-pipeline-config-server-deployment-5f676b858c-vxd6d    2m           170Mi
cp-pipeline-inspection-api-deployment-f5bfbdccc-pk5bk    1m           158Mi
cp-pipeline-jenkins-deployment-5f75b74b6d-vc25p          3m           1319Mi
cp-pipeline-postgresql-postgresql-0                      4m           58Mi
cp-pipeline-sonarqube-sonarqube-5f6c94bdbc-rqrf8         7m           1744Mi
cp-pipeline-ui-deployment-845bb6999f-kr78p               1m           337Mi
```
We recommend that the cluster environment where the container platform pipeline will be installed have at least **4Gi** of free memory total for the cluster.

When the container platform pipeline installation is complete, the Persistent Volume usage resources are as follows.
```bash
NAME                                       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS
cp-pipeline-jenkins-pvc                    Bound    pvc-4bf64900-d25c-482f-9aa3-baa07c11cdd1   20Gi       RWO            cp-storageclass
data-cp-pipeline-postgresql-postgresql-0   Bound    pvc-f61096ac-5e2b-4105-9ed3-04a9a7d999cb   8Gi        RWX            cp-storageclass
```
We recommend **28Gi** of free NFS storage capacity for the cluster environment where the container platform pipeline will be installed.

<br>    

## <div id='3'>3. Deploy the Container Platform Pipeline

### <div id='3.1'>3.1. Download the Container Platform Pipeline Deployment
For container platform pipeline deployment, download the container platform pipeline deployment file and place it in the path below. <br>
:bulb: This will be done on the **Master Node**.

> In the case of a multi-cluster environment, proceed with the cluster that was designated as **Cluster1** when deploying the container platform portal.

+  Download the Container Platform Pipeline Deployment file :
   [cp-pipeline-deployment-v1.6.2.tar.gz](https://nextcloud.k-paas.org/index.php/s/D7fz23QcDrT4Afs/download)

```bash
# Create Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment File and Verify File
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/D7fz23QcDrT4Afs/download

$ ls ~/workspace/container-platform
  cp-pipeline-deployment-v1.6.2.tar.gz ...
  
# Decompress Deployment Files
$ tar -xvf cp-pipeline-deployment-v1.6.2.tar.gz
```

- Configure the Deployment File Directory
```bash
cp-pipeline-deployment
 ├── script        # Variables and script files for pipeline deployment
 └── values_orig   # Helm chart values file
```

<br>

### <div id='3.2'>3.2. Define Container Platform Pipeline Variables
Define the variable values required for container platform pipeline deployment.
```bash
$ cd ~/workspace/container-platform/cp-pipeline-deployment/script
$ vi cp-pipeline-vars.sh
```
```bash                                                  
# COMMON VARIABLE (Please change the value of the variables below.)
HOST_DOMAIN="{host domain}"                           # Host Domain (e.g. xx.xxx.xxx.xx.nip.io)
K8S_STORAGECLASS="cp-storageclass"                    # Kubernetes StorageClass Name (e.g. cp-storageclass)
IS_MULTI_CLUSTER="N"                                  # Please enter "Y" if deploy in a multi-cluster environment
```
```bash
# (Example input values)
HOST_DOMAIN="105.xxx.xxx.xxx.nip.io"
K8S_STORAGECLASS="cp-storageclass"
IS_MULTI_CLUSTER="N"
```

|Variable|Description|Additional|
|---|---|---|
|**HOST_DOMAIN**|Enter Host Domain|Enter the `HOST_DOMAIN` value defined in <br>[[3.2. Define Container Platform Portal Variables]](../portal/cp-portal-standalone-guide.md#3.2)|
|**K8S_STORAGECLASS**|Enter StorageClass Name|The cluster deployed through the container platform is <br> `cp-storageclass` by default. <br> When using another StorageClass, enter the resource name.|
|**IS_MULTI_CLUSTER**|Whether it is a multi-cluster environment|Enter "Y" when deploying to a multi-cluster environment|

<br>

### <div id='3.3'>3.3. Run the Container Platform Pipeline Deployment Script
Run the script for deploying the container platform pipeline.

```bash
$ chmod +x deploy-cp-pipeline.sh
$ ./deploy-cp-pipeline.sh
```

<br>

Verify that the container platform pipeline related resources have been deployed normally.<br>
For Resource Pod, it takes a few seconds to bind to Node and create a container before it transitions to Running state.

#### List Container Platform Pipeline Resource

```bash
$ kubectl get all -n cp-pipeline
```

```bash
NAME                                                        READY   STATUS    RESTARTS      AGE
pod/cp-pipeline-api-deployment-6d99f4c7d7-88h64             1/1     Running   0             100s
pod/cp-pipeline-common-api-deployment-7488bc6d5d-q5wvt      1/1     Running   0             100s
pod/cp-pipeline-config-server-deployment-5f676b858c-vxd6d   1/1     Running   0             100s
pod/cp-pipeline-inspection-api-deployment-f5bfbdccc-pk5bk   1/1     Running   0             100s
pod/cp-pipeline-jenkins-deployment-5f75b74b6d-vc25p         1/1     Running   0             103s
pod/cp-pipeline-postgresql-postgresql-0                     1/1     Running   0             103s
pod/cp-pipeline-sonarqube-sonarqube-5f6c94bdbc-rqrf8        1/1     Running   0             101s
pod/cp-pipeline-ui-deployment-845bb6999f-kr78p              1/1     Running   0             100s

NAME                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/cp-pipeline-api-service              ClusterIP   10.233.27.151   <none>        8082/TCP   100s
service/cp-pipeline-common-api-service       ClusterIP   10.233.11.81    <none>        8081/TCP   100s
service/cp-pipeline-config-server-service    ClusterIP   10.233.50.72    <none>        8080/TCP   100s
service/cp-pipeline-inspection-api-service   ClusterIP   10.233.44.110   <none>        8085/TCP   100s
service/cp-pipeline-jenkins-service          ClusterIP   10.233.33.33    <none>        8080/TCP   103s
service/cp-pipeline-postgresql               ClusterIP   10.233.19.130   <none>        5432/TCP   103s
service/cp-pipeline-postgresql-headless      ClusterIP   None            <none>        5432/TCP   103s
service/cp-pipeline-sonarqube-sonarqube      ClusterIP   10.233.8.200    <none>        9000/TCP   101s
service/cp-pipeline-ui-service               ClusterIP   10.233.8.123    <none>        8084/TCP   100s

NAME                                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-pipeline-api-deployment              1/1     1            1           100s
deployment.apps/cp-pipeline-common-api-deployment       1/1     1            1           100s
deployment.apps/cp-pipeline-config-server-deployment    1/1     1            1           100s
deployment.apps/cp-pipeline-inspection-api-deployment   1/1     1            1           100s
deployment.apps/cp-pipeline-jenkins-deployment          1/1     1            1           103s
deployment.apps/cp-pipeline-sonarqube-sonarqube         1/1     1            1           101s
deployment.apps/cp-pipeline-ui-deployment               1/1     1            1           100s

NAME                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-pipeline-api-deployment-6d99f4c7d7             1         1         1       100s
replicaset.apps/cp-pipeline-common-api-deployment-7488bc6d5d      1         1         1       100s
replicaset.apps/cp-pipeline-config-server-deployment-5f676b858c   1         1         1       100s
replicaset.apps/cp-pipeline-inspection-api-deployment-f5bfbdccc   1         1         1       100s
replicaset.apps/cp-pipeline-jenkins-deployment-5f75b74b6d         1         1         1       103s
replicaset.apps/cp-pipeline-sonarqube-sonarqube-5f6c94bdbc        1         1         1       101s
replicaset.apps/cp-pipeline-ui-deployment-845bb6999f              1         1         1       100s

NAME                                                 READY   AGE
statefulset.apps/cp-pipeline-postgresql-postgresql   1/1     103s
```    
```bash
# List Pods when deployed in a multi-cluster environment
NAME                                                     READY   STATUS    RESTARTS      AGE
cp-pipeline-api-deployment-6978c48fbb-wkhmk              1/1     Running   0             3m45s
cp-pipeline-common-api-deployment-558bb4cbdc-wxf2h       2/2     Running   0             3m43s
cp-pipeline-config-server-deployment-68674cf765-kcwls    2/2     Running   0             3m41s
cp-pipeline-inspection-api-deployment-75c58bcffd-szflk   1/1     Running   0             3m45s
cp-pipeline-jenkins-deployment-64ff4ccd64-bvf6r          1/1     Running   0             3m40s
cp-pipeline-postgresql-postgresql-0                      1/1     Running   0             3m39s
cp-pipeline-sonarqube-sonarqube-5b8d4b4856-297qn         1/1     Running   0             3m37s
cp-pipeline-ui-deployment-58c5ffdc6c-2lgq9               2/2     Running   0             3m42s
```

<br>


### <div id='3.4'>3.4. (Reference) Delete Container Platform Pipeline Resources
If you want to delete a deployed container platform pipeline resource, run the script below.<br>
:loudspeaker: (Caution) If you run the script while the container platform pipeline is running, **all resources will be deleted**, so be careful.<br>
> If the StorageClass type of the cluster installed through the container platform is NFS, the reclaim policy is Retain. <br>
> The `Retain` policy requires manual data cleanup because data still exists on the storage NFS server even if the Persistent Volume is deleted.

```bash
$ cd ~/workspace/container-platform/cp-pipeline-deployment/script
$ chmod +x uninstall-cp-pipeline.sh
$ ./uninstall-cp-pipeline.sh
Are you sure you want to delete the container platform pipeline? <y/n> y # Enter 'Y'
```

<br>

## <div id='4'>4. Access the Container Platform Pipeline
Access the Container Platform Pipeline. <br><br>
**Container Platform Pipeline URL** : `https://pipeline.${HOST_DOMAIN}`
+ Enter the `HOST_DOMAIN` values defined in [[3.2. Define Container Platform Pipeline Variables]](#3.2)

<br>

### <div id='4.1'/>4.1. Login to the Container Platform Pipeline Administrator Account
Login to the pipeline UI with the Container Platform Portal administrator account.
> [[Login to the Container Platform Portal Administrator Account]](../portal/cp-portal-standalone-guide.md#4.1)
+ **Username** : `admin`
+ **Password** : `Enter new password`

![image 002]

<br>    


### <div id='4.2'/>4.2. Login to the Container Platform Pipeline User Account
#### User Registration
- Click the 'Register' button at the bottom.

![image 003]

- Enter the user account information to register and click the 'Register' button to sign up for the container platform pipeline.

![image 004]

####  User Login
- Login to the container platform pipeline with the account registered through user registration.

![image 006]

<br>    

### <div id='4.3'/>4.3. Container Platform Pipeline Use Guide
- For more information on how to use the container platform pipeline, please refer to the user guide below.
   + [Container Platform Pipeline Use Guide](../../use-guide/pipeline/cp-pipeline-use-guide.md)

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Container Platform Pipeline Installation Guide


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