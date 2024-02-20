### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/Readme.md) > SourceControl installation guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
     1.1. [Purpose](#1.1)<br>
     1.2. [Range](#1.2)<br>
     1.3. [System configuration diagram](#1.3)<br>
     1.4. [Reference Material](#1.4)<br>

2. [Prerequisite](#2)<br>
     2.1. [Installation of Container Platform Portal](#2.1)<br>
        
3. [Container platform source control deployment](#3)<br>
     3.1. [Download container platform source control deployment file](#3.1)<br>
     3.2. [Defining container platform source control variables](#3.2)<br>
     3.3. [Run container platform source control deployment script](#3.3)<br>
     3.4. [(Reference) Delete Container Platform Source Control Resources](#3.4)<br>

4. [Container platform source control access](#4)<br>
     4.1. [Container Platform Source Control Administrator Login](#4.1)<br>
     4.2. [Container platform source control user registration/login](#4.2)<br>
     4.3. [Container Platform Source Control Usage Guide](#4.3)<br>



## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. purpose
This document (Container Platform Source Control Exclusive Deployment Installation Guide) describes how to deploy source control exclusively in an environment where the Container Platform Portal is deployed in a Kubernetes Cluster environment.<br>

<br>

### <div id='1.2'>1.2. range
The installation scope was created based on Kubernetes-only deployment.

<br>

### <div id='1.3'>1.3. System configuration diagram
<p align="center"><img src="https://user-images.githubusercontent.com/33216551/209299431-af201419-9220-425a-8552-d15e379f8ee7.png" width="850" height="530" ">
<br>

The system configuration consists of a Kubernetes Cluster (Master, Worker) environment and a storage server for data management.
Harbor, which manages container platform source control images and Helm Charts, Keycloak, which manages container platform source control user authentication, and MariaDB (RDBMS), which manages container platform source control metadata, run the container platform portal in the Kubernetes Cluster environment installed through Kubespray. It is provided through.
Container platform source control provides SCM-Server, which manages sources, as a container.
The total required VM environment requires at least 1 Master VM and 3 Worker VMs, and this document is about deploying a container platform source control environment in a Kubernetes Cluster.

<br>

### <div id='1.4'>1.4. reference material
> https://kubernetes.io/ko/docs

<br>

## <div id='2'>2. Prerequisite
    
### <div id='2.1'>2.1. Install the Container Platform Portal
As the infrastructure to be used in container platform source control, the authentication server **KeyCloak Server**, database **MariaDB**, and repository server **Harbor** must be installed in advance.
When deploying the K-PaaS container platform portal, install all relevant infrastructure.
For container platform infrastructure installation, refer to the guide below.
> [K-PaaS container platform portal deployment](../container-platform-portal/cp-portal-deployment-standalone-guide.md)

<br>
    
## <div id='3'>3. Container platform source control deployment
    
### <div id='3.1'>3.1. Download container platform source control deployment file
To deploy container platform source control, download the container platform source control deployment file and place it in the path below.<br>
:bulb: This content is carried out in Kubernetes **Master Node**.

+ Download Container Platform Source Control Deployment File:
    [cp-source-control-deployment-v1.4.0.tar.gz](https://nextcloud.k-paas.org/index.php/s/bBKm3JcQFHRw6mB/download)

```
# Create deployment file download path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment file and check file path
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/bBKm3JcQFHRw6mB/download

$ ls ~/workspace/container-platform
   ...
   cp-source-control-deployment-v1.4.0.tar.gz
   ...

# Unzip the Deployment file
$ tar -xvf cp-source-control-deployment-v1.4.0.tar.gz
```

- Deployment file directory configuration
```
├── script # Container platform source control deployment-related variables and script file location
├── images # Container platform source control image file location
├── charts # Container platform source control Helm Charts file location
├── values_orig # Container platform source control Helm Charts values.yaml original file location
```

<br>

### <div id='3.2'>3.2. Define container platform source control variables
Before deploying container platform source control, variable value definition is required. Confirm the information required for distribution and set variables.

```
$ cd ~/workspace/container-platform/cp-source-control-deployment/script
$ vi cp-source-control-vars.sh
```

```
# COMMON VARIABLE
K8S_MASTER_NODE_IP="{k8s master node public ip}" # Kubernetes master node public ip
PROVIDER_TYPE="{container platform source control provider type}" # Container platform source-control provider type (Please enter 'standalone' or 'service')
....
```
```
#Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"
PROVIDER_TYPE="standalone"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter container platform source control provider type <br>
    + This guide is a standalone distribution installation guide and requires entering the **'standalone'** value.
<br>

:bulb: Keycloak's default distribution method is **HTTP** and **HTTPS** via certificate is set.
  > [Keycloak TLS settings](../container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide.md)

Modify the contents below in the container platform source control variable file.
```
$ vi cp-source-control-vars.sh
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
    
### <div id='3.3'>3.3. Run the container platform source control deployment script
Run the deployment script for container platform source control deployment.

```
$ chmod +x deploy-cp-source-control.sh
$ ./deploy-cp-source-control.sh
```
<br>

Check whether container platform source control-related resources are deployed properly.<br>
In the case of resource Pods, it takes several seconds to transition to Running state after binding to Node and creating a container.

- **Container platform source control resource query**
```
$ kubectl get all -n cp-source-control
```

```
NAME                                                        READY   STATUS    RESTARTS   AGE
pod/cp-source-control-api-deployment-588fdfbfd7-727w2       1/1     Running   0          54s
pod/cp-source-control-manager-deployment-69b9b87cfd-kcv8l   1/1     Running   0          53s
pod/cp-source-control-ui-deployment-867557b66d-pq5w9        1/1     Running   0          51s

NAME                                        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-source-control-api-service       NodePort   10.233.21.183   <none>        8091:30091/TCP   54s
service/cp-source-control-manager-service   NodePort   10.233.22.163   <none>        8080:30092/TCP   53s
service/cp-source-control-ui-service        NodePort   10.233.14.98    <none>        8094:30094/TCP   51s

NAME                                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-source-control-api-deployment       1/1     1            1           54s
deployment.apps/cp-source-control-manager-deployment   1/1     1            1           53s
deployment.apps/cp-source-control-ui-deployment        1/1     1            1           51s

NAME                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-source-control-api-deployment-588fdfbfd7       1         1         1       54s
replicaset.apps/cp-source-control-manager-deployment-69b9b87cfd   1         1         1       53s
replicaset.apps/cp-source-control-ui-deployment-867557b66d        1         1         1       51s
```    

<br>

### <div id='3.4'>3.4. (Reference) Deleting Container Platform Source Control Resources
If you want to delete the deployed container platform source control resource, run the script below.<br>

```
$ cd ~/workspace/container-platform/cp-source-control-deployment/script
$ chmod +x uninstall-cp-source-control.sh
$ ./uninstall-cp-source-control.sh
```
```
Are you sure you want to delete the container platform source control? <y/n> # enter y
release "cp-source-control-api" uninstalled
release "cp-source-control-manager" uninstalled
release "cp-source-control-broker" uninstalled
release "cp-source-control-ui" uninstalled
namespace "cp-source-control" deleted
...
...
```

<br>
    
## <div id='4'>4. Container platform source control access
The container platform source control page can be accessed at the address below.<br>
For the {K8S_MASTER_NODE_IP} value, enter the **Kubernetes Master Node Public IP** value.

- Container platform source control standalone access URI: **http://{K8S_MASTER_NODE_IP}:30094** <br>

<br>
    
### <div id='4.1'/>4.1. Container Platform Source Control Administrator Login
Container platform source control connection After checking the initial information, log in to source control.
- Access http://{K8S_MASTER_NODE_IP}:30094
- Verify initial account information and log in

> Check initial account information using the command below
```
$ kubectl get configmap -n cp-portal cp-portal-configmap -o yaml | grep KEYCLOAK_ADMIN
```
![image](https://user-images.githubusercontent.com/80228983/146140178-76e85cbb-03a0-4a84-9059-7e5074c1d90e.png)

<br>


### <div id='4.2'/>4.2. Container platform source control user registration/login
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
- Access http://{K8S_MASTER_NODE_IP}:30094.
- Log in to container platform source control with the account registered through membership registration.
    
<br>

### <div id='4.3'/>4.3. Container Platform Source Control User Guide
- For information on how to use container platform source control, refer to the user guide below.
   + [Container Platform Source Control Use Guide](../../use-guide/source-control/cp-source-control-use-guide.md)

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](https://github.com/PaaS-TA/paas-ta-container-platform/tree/master/install-guide/Readme.md) > SourceControl installation guide
