### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > Pipeline installation guide

<br>
## Table of Contents

1. [Document Overview](#1)<br>
     1.1. [Purpose](#1.1)<br>
     1.2. [Range](#1.2)<br>
     1.3. [System configuration diagram](#1.3)<br>
     1.4. [Reference Material](#1.4)<br>

2. [Prerequisite](#2)<br>
     2.1. [Installation of Container Platform Portal](#2.1)<br>
     2.2. [Cluster environment](#2.2)<br>
        
3. [Container platform pipeline deployment](#3)<br>
     3.1. [Download Container Platform Pipeline Deployment File](#3.1)<br>
     3.2. [Container platform pipeline variable definition](#3.2)<br>
     3.3. [Run container platform pipeline deployment script](#3.3)<br>
     3.4. [(Reference) Delete Container Platform Pipeline Resource](#3.4)<br>

4. [Container platform pipeline access](#4)<br>
     4.1. [Container Platform Pipeline Manager Login](#4.1)<br>
     4.2. [Container platform pipeline user registration/login](#4.2)<br>
     4.3. [Container Platform Pipeline Usage Guide](#4.3)<br>



## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. purpose
This document (Container Platform Pipeline-only deployment installation guide) describes how to install a Kubernetes Cluster and a container platform-only deployment portal and deploy a container platform-only deployment pipeline.<br>

<br>

### <div id='1.2'>1.2. range
The installation scope was created based on Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System configuration diagram
<p align="center"><img src="https://user-images.githubusercontent.com/33216551/209299431-af201419-9220-425a-8552-d15e379f8ee7.png" width="850" height="530" ">
<br>

The system configuration consists of a Kubernetes Cluster (Master, Worker) environment and a storage server for data management.
Harbor, which manages container platform pipeline images and Helm Charts, Keycloak, which manages container platform pipeline user authentication, and MariaDB (RDBMS), which manages container platform pipeline metadata, are installed in the Kubernetes Cluster environment installed through Kubespray to create the container platform portal. It is provided through.
In the container platform pipeline, pipes include Jenkins server (Ci-Server) that manages continuous integration and deployment functions, Sonarqube (Inspection-Server) for static analysis, and Spring Config Server (Config-Server) that manages the configuration of the deployed application. The environment required for line operation is provided as a container.
The total required VM environment requires at least 1 Master VM and 3 Worker VMs, and this document is about deploying a container platform pipeline environment in a Kubernetes Cluster.

<br>

### <div id='1.4'>1.4. reference material
> https://kubernetes.io/ko/docs

<br>

## <div id='2'>2. Prerequisite
    
### <div id='2.1'>2.1. Install the Container Platform Portal
As infrastructure to be used in the container platform pipeline, the authentication server **KeyCloak Server**, database **MariaDB**, and repository server **Harbor** must be installed in advance.
When deploying the K-PaaS container platform portal, install all relevant infrastructure.
For container platform infrastructure installation, refer to the guide below.
> [K-PaaS container platform portal deployment](../container-platform-portal/cp-portal-deployment-standalone-guide.md)

<br>
    
### <div id='2.2'>2.2. Cluster environment
To deploy a container platform, images such as sonarqube and postgresql are downloaded from the public network, so they must be installed in an environment where **external network communication** is possible. <br>
The resources used in idle state upon completion of container platform pipeline installation are as follows.
```
NAME CPU(cores) MEMORY(bytes)
cp-pipeline-api-deployment-bb6f7bd46-nl4j4 1m 224Mi
cp-pipeline-common-api-deployment-54c646c95c-87k5g 2m 255Mi
cp-pipeline-config-server-deployment-78675b565d-8kxf9 2m 170Mi
cp-pipeline-inspection-api-deployment-6bf9c4479d-d6xgb 1m 158Mi
cp-pipeline-jenkins-deployment-779c6d7bc9-pn782 3m 1319Mi
cp-pipeline-postgresql-postgresql-0 4m 58Mi
cp-pipeline-sonarqube-sonarqube-6d9c6b579f-2xkkw 7m 1744Mi
cp-pipeline-ui-deployment-5db955b77b-snkpl 1m 337Mi
```
For the cluster environment where the container platform pipeline will be installed, a total of at least **4Gi** of free memory in the cluster is recommended.

When the container platform pipeline installation is completed, the Persistent Volume usage resources are as follows.
```
NAME STATUS VOLUME CAPACITY ACCESS MODES STORAGE CLASS
cp-pipeline-jenkins-pv Bound pvc-4bf64900-d25c-482f-9aa3-baa07c11cdd1 20Gi RWO kpaas-cp-storageclass
data-cp-pipeline-postgresql-postgresql-0 Bound pvc-f61096ac-5e2b-4105-9ed3-04a9a7d999cb 8Gi RWX kpaas-cp-storageclass
```
A spare capacity of NFS storage capacity of **28Gi** is recommended for the cluster environment where the container platform pipeline will be installed.<br>

<br>

## <div id='3'>3. Container Platform Pipeline Deployment
    
### <div id='3.1'>3.1. Download Container Platform Pipeline Deployment File
To deploy the container platform pipeline, download the container platform pipeline deployment file and place it in the path below.<br>
:bulb: This content is carried out in Kubernetes **Master Node**.

+ Download Container Platform Pipeline Deployment file:
    [cp-pipeline-deployment-v1.4.0.tar.gz](https://nextcloud.k-paas.org/index.php/s/wjiQ5dScS6pPQkp/download)

```
# Create deployment file download path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment file and check file path
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/wjiQ5dScS6pPQkp/download

$ ls ~/workspace/container-platform
   ...
   cp-pipeline-deployment-v1.4.0.tar.gz
   ...
  
# Unzip the Deployment file
$ tar -xvf cp-pipeline-deployment-v1.4.0.tar.gz
```

- Deployment file directory configuration
```
├── script # Location of variables and script files related to container platform pipeline deployment
├── images # Container platform pipeline image file location
├── charts # Container platform pipeline Helm Charts file location
├── values_orig # Container Platform Pipeline Helm Charts values.yaml original file location
```

<br>

### <div id='3.2'>3.2. Container platform pipeline variable definitions
Before deploying the container platform pipeline, variable values need to be defined. Confirm the information required for distribution and set variables.

```
$ cd ~/workspace/container-platform/cp-pipeline-deployment/script
$ vi cp-pipeline-vars.sh
```

```
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}" # Kubernetes master node public ip
PROVIDER_TYPE="{container platform pipeline provider type}" # Container platform pipeline provider type (Please enter 'standalone' or 'service')
....
```
```
#Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"
PROVIDER_TYPE="standalone"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter container platform pipeline provision type <br>
    + This guide is a standalone distribution installation guide and requires entering the **'standalone'** value<br><br>
- **CF_API_URL** <br>No need to enter in standalone distribution version <br>

<br>

:bulb: Keycloak's default distribution method is **HTTP** and **HTTPS** via certificate is set.
> [Keycloak TLS settings](../container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide.md)

Modify the following contents in the container platform pipeline variable file.
```
$ vi cp-pipeline-vars.sh
```
```
# Change KEYCLOAK_URL value from http to https
# If nip.io is used as the domain, change as follows
    
....
#KEYCLOAK
KEYCLOAK_URL="https://${K8S_MASTER_NODE_IP}.nip.io:32710" #if apply TLS, https://
....
```
<br>
    
### <div id='3.3'>3.3. Run the Container Platform Pipeline Deployment Script
Run the deployment script for deploying the container platform pipeline.

```
$ chmod +x deploy-cp-pipeline.sh
$ ./deploy-cp-pipeline.sh
```

<br>

Check whether container platform pipeline-related resources are deployed properly.<br>
In the case of resource Pods, it takes several seconds to transition to Running state after binding to Node and creating a container.

- **View container platform pipeline resources**

```
$ kubectl get all -n cp-pipeline
```

```
NAME                                                         READY   STATUS    RESTARTS      AGE
pod/cp-pipeline-api-deployment-bb6f7bd46-xmtxx               1/1     Running   0             71s
pod/cp-pipeline-common-api-deployment-54c646c95c-pr8d4       1/1     Running   0             71s
pod/cp-pipeline-config-server-deployment-78675b565d-jsg6z    1/1     Running   0             68s
pod/cp-pipeline-inspection-api-deployment-6bf9c4479d-qvt2s   1/1     Running   0             69s
pod/cp-pipeline-jenkins-deployment-779c6d7bc9-fphrv          1/1     Running   0             69s
pod/cp-pipeline-postgresql-postgresql-0                      1/1     Running   0             65s
pod/cp-pipeline-sonarqube-sonarqube-6d9c6b579f-ndjmj         0/1     Running   0             65s
pod/cp-pipeline-ui-deployment-5db955b77b-6zgtg               1/1     Running   0             70s

NAME                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-pipeline-api-service              NodePort    10.233.33.178   <none>        8082:30082/TCP   71s
service/cp-pipeline-common-api-service       NodePort    10.233.42.55    <none>        8081:30081/TCP   71s
service/cp-pipeline-config-server-service    NodePort    10.233.29.96    <none>        8080:30088/TCP   68s
service/cp-pipeline-inspection-api-service   NodePort    10.233.18.126   <none>        8085:30085/TCP   69s
service/cp-pipeline-jenkins-service          NodePort    10.233.43.30    <none>        8080:30086/TCP   69s
service/cp-pipeline-postgresql               ClusterIP   10.233.27.243   <none>        5432/TCP         67s
service/cp-pipeline-postgresql-headless      ClusterIP   None            <none>        5432/TCP         67s
service/cp-pipeline-sonarqube-sonarqube      NodePort    10.233.52.4     <none>        9000:30087/TCP   66s
service/cp-pipeline-ui-service               NodePort    10.233.57.132   <none>        8084:30084/TCP   70s

NAME                                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-pipeline-api-deployment              1/1     1            1           71s
deployment.apps/cp-pipeline-common-api-deployment       1/1     1            1           71s
deployment.apps/cp-pipeline-config-server-deployment    1/1     1            1           68s
deployment.apps/cp-pipeline-inspection-api-deployment   1/1     1            1           69s
deployment.apps/cp-pipeline-jenkins-deployment          1/1     1            1           69s
deployment.apps/cp-pipeline-sonarqube-sonarqube         0/1     1            0           65s
deployment.apps/cp-pipeline-ui-deployment               1/1     1            1           70s

NAME                                                               DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-pipeline-api-deployment-bb6f7bd46               1         1         1       71s
replicaset.apps/cp-pipeline-common-api-deployment-54c646c95c       1         1         1       71s
replicaset.apps/cp-pipeline-config-server-deployment-78675b565d    1         1         1       68s
replicaset.apps/cp-pipeline-inspection-api-deployment-6bf9c4479d   1         1         1       69s
replicaset.apps/cp-pipeline-jenkins-deployment-779c6d7bc9          1         1         1       69s
replicaset.apps/cp-pipeline-sonarqube-sonarqube-6d9c6b579f         1         1         0       65s
replicaset.apps/cp-pipeline-ui-deployment-5db955b77b               1         1         1       70s

NAME                                                 READY   AGE
statefulset.apps/cp-pipeline-postgresql-postgresql   1/1     66s
```    
<br>

### <div id='3.4'>3.4. (Reference) Deleting Container Platform Pipeline Resources
If you want to delete the deployed container platform pipeline resource, run the script below.<br>

```
$ cd ~/workspace/container-platform/cp-pipeline-deployment/script
$ chmod +x uninstall-cp-pipeline.sh
$ ./uninstall-cp-pipeline.sh
Are you sure you want to delete the container platform pipeline? <y/n> # enter y
```
    
```
release "cp-pipeline-api" uninstalled
release "cp-pipeline-common-api" uninstalled
release "cp-pipeline-ui" uninstalled
release "cp-pipeline-inspection-api" uninstalled
release "cp-pipeline-jenkins" uninstalled
release "cp-pipeline-config-server" uninstalled
release "cp-pipeline-postgresql" uninstalled
release "cp-pipeline-sonarqube" uninstalled
namespace "cp-pipeline" deleted
...
...
```
<br>

## <div id='4'>4. Container platform pipeline access
The container platform pipeline page can be accessed at the address below.<br>
For the {K8S_MASTER_NODE_IP} value, enter the **Kubernetes Master Node Public IP** value.

- Container platform pipeline standalone connection URI: **http://{K8S_MASTER_NODE_IP}:30084** <br>

<br>
    
### <div id='4.1'/>4.1. Container Platform Pipeline Manager Login
After checking the initial information for connecting to the container platform pipeline, log in to the pipeline.
- Access http://{K8S_MASTER_NODE_IP}:30084
- Verify initial account information and log in

> Check initial account information using the command below
```
$ kubectl get configmap -n cp-portal cp-portal-configmap -o yaml | grep KEYCLOAK_ADMIN
```

![image](https://user-images.githubusercontent.com/80228983/146140178-76e85cbb-03a0-4a84-9059-7e5074c1d90e.png)

<br>


### <div id='4.2'/>4.2. Container platform pipeline user registration/login
#### User registration
- Connect to Keycloak (http://{K8S_MASTER_NODE_IP}:32710).
- Access the Administration Console (administrator page). <br>
     ![image](https://user-images.githubusercontent.com/80228983/146140243-b01fe7b7-c610-4c74-b520-839b581ca178.png)
<br>

- Connect with the administrator account. <br>
     ![image](https://user-images.githubusercontent.com/80228983/146140270-06c6bc41-94cd-4947-8376-f6ade73b61ac.png)

<br>

- Click the ‘Users’ list at the bottom of the left menu. <br>
     ![image](https://user-images.githubusercontent.com/80228983/146140350-c2bed5ab-0683-47cf-838b-6c970e492605.png)

<br>

- Click Add User on the right. <br>
     ![image](https://user-images.githubusercontent.com/80228983/146140392-1eb2d2e4-47d7-4fd2-8370-9d3e5b2a9871.png)

<br>

- Enter the Username and change the Email Verified switch to On. Then press Save. <br>
     ![image](https://user-images.githubusercontent.com/80228983/146140503-3ad5e8f7-613b-4583-83fe-3f6623736286.png)

<br>

- Go to the Credentials tab.
- Enter Password / PassWord Confirmation to set the password.
- If it is not a temporary number, set the Temporary switch to Off.<br>
     ![image](https://user-images.githubusercontent.com/80228983/146140746-54e57681-584c-43b3-93f1-f172205d34d2.png)
  <br>

####  log in
- Access http://{K8S_MASTER_NODE_IP}:30084.
- Log in to the container platform pipeline with the account registered through membership registration.
    
<br>

### <div id='4.3'/>4.3. Container Platform Pipeline User Guide
- For information on how to use the container platform pipeline, refer to the user guide below.
   + [Container Platform Pipeline Use Guide](../../use-guide/pipeline/cp-pipeline-use-guide.md)
<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > Pipeline installation guide
