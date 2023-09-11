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
        
3. [Container Platform Source Control Deployment](#3)  
    3.1. [CRI-O insecure-registry Setting](#3.1)  
    3.2. [Container Platform Source Control Deployment](#3.2)  
    3.2.1. [Container Platform Source Control Deployment File Download](#3.2.1)  
    3.2.2. [Define Container Platform Source Control Variable](#3.2.2)    
    3.2.3. [Execute Container Platform Source Control Deployment Script](#3.2.3)    
    3.2.4. [(Refer) Delete Container Platform Source Control Resource](#3.2.4)    

4. [Container Platform Source Control Service Broker](#4)   
    4.1. [Container Platform Source Control User Authentication Service Configuration](#4.1)   
    4.2. [Container Platform Source Control Service Broker Registration](#4.2)  
    4.3. [Container Platform Source Control Service Lookup Setting](#4.3)    
    4.4. [Container Platform Source Control Use Guide](#4.4)       



## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform Source Control Service Deployment Installation Guide) installs the Kubernetes Cluster and Container Platform Service Deployment Portal and describes how to deploy Container Platform Service Deployment Source Control.<br>

<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration Diagram
![image](https://user-images.githubusercontent.com/80228983/146350860-3722c081-7338-438d-b7ec-1fdac09160c4.png)
<br>    
The system configuration consists of a Kubernetes Cluster (Master, Worker) environment and a Network File System (NFS) storage server for data management. 
Harbor, which manages container platform source control images and HelmCharts, Keycloak, which manages container platform source control user authentication, and MariaDB (RDBMS), which manages container platform source control metadata, are provided through the container platform portal.
 The Container Platform Source Control provides an SCM-Server that manages the source as a container. 
The total VM environment required is one Master Node VM, one or more Worker Node VMs, and one NFS Server, and this document is about deploying a container platform source control environment in a Kubernetes cluster. Network File System (NFS) is the storage provided by the container platform and can use various types of storage depending on the user environment. 


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
Refer to the guide below for installing the container platform portal.
> [K-PaaS Container Platform Portal Deployment](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md)     

  
## <div id='3'>3. Container Platform Source Control Deployment

### <div id='3.1'>3.1. CRI-O insecure-registry Setting
When deploying the container platform source control, upload images and package files to the private repository installed in the cluster.
Upload the container platform portal to the deployed Private Repository (Harbor) with images and package files related to the container platform source control. 

Refer to the guide below for the CRI-O insecure-registry settings required for a Private Repository deployment.
> [CRI-O insecure-registry Setting](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md#3.1)      

### <div id='3.2'>3.2. Container Platform Source Control Deployment
    
#### <div id='3.2.1'>3.2.1. Container Platform Source Control Deployment File Download
Download the container platform source control Deployment file and locate it in the path below for container platform source control deployment.<br>
:bulb: Process this content at the **Master Node** of Kubernetes. 

+ Container Platform Source Control Deployment File Download :  
   [paas-ta-container-platform-source-control-deployment.tar](https://nextcloud.paas-ta.org/index.php/s/6WG9C29tjQGY8We)  

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
# Unzip Deployment File
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
PROVIDER_TYPE="service"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Source Control Providing Type <br>
   + This guide is a service deployment installation guide that requires **'service'** values
<br>    

:bulb: Keycloak default deployment method is **HTTP**, and **HTPS** via certificate is set
> [Keycloak TLS Setting](../container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide-v1.2.md)

Modify the contents below in the container platform source control variable file.
```
$ vi container-platform-source-control-vars.sh    
```    
```
# Change KEYCLOAK_URL value http -> https 
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
pod/container-platform-source-control-api-deployment-597fc499bc9dpp   1/1     Running   0          63s
pod/container-platform-source-control-broker-deployment-68bbdcq2grf   1/1     Running   0          62s
pod/container-platform-source-control-manager-deployment-6b548xkw7p   1/1     Running   0          63s
pod/container-platform-source-control-ui-deployment-85f754cfc6k28hj   1/1     Running   0          61s

NAME                                                        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-source-control-api-service       NodePort   10.233.46.158   <none>        8091:30091/TCP   63s
service/container-platform-source-control-broker-service    NodePort   10.233.38.22    <none>        8093:30093/TCP   62s
service/container-platform-source-control-manager-service   NodePort   10.233.32.242   <none>        8080:30092/TCP   63s
service/container-platform-source-control-ui-service        NodePort   10.233.29.174   <none>        8094:30094/TCP   61s

NAME                                                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-source-control-api-deployment       1/1     1            1           63s
deployment.apps/container-platform-source-control-broker-deployment    1/1     1            1           62s
deployment.apps/container-platform-source-control-manager-deployment   1/1     1            1           63s
deployment.apps/container-platform-source-control-ui-deployment        1/1     1            1           61s

NAME                                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-source-control-api-deployment-597fc499bb       1         1         1       63s
replicaset.apps/container-platform-source-control-broker-deployment-68bbdcb55     1         1         1       62s
replicaset.apps/container-platform-source-control-manager-deployment-6b54859fbf   1         1         1       63s
replicaset.apps/container-platform-source-control-ui-deployment-85f754cfc6        1         1         1       61s

```    

<br>

#### <div id='3.2.4'>3.2.4. (Refer) Delete Container Platform Source Control Resource
To delete the deployed container platform source control resource, follow the script shown below.<br>

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
  
## <div id='4'>4. Container Platform Source Control Service Broker
When installing as a container platform K-PaaS service-type source control, brokers must be registered for the container platform source control service integration deployed to CF and Kubernetes.
If you register and disclose the service through the K-PaaS operator portal, you can apply for and use the service through the K-PaaS user portal.
  
## <div id='4.1'>4.1. Container Platform Source Control User Authentication Service Configuration
To use the container platform source control as a service, the **user authentication service** configuration must be performed in advance.<br>
For user authentication service configuration, refer to the guide below.
> [User Authentication Service Configuration](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md#4)      
When configuring the container platform portal user authentication service, it also applies to source control.

### <div id='4.2'>4.2. Container Platform Source Control Service Broker Registration
:bulb: This will be done at **BOSH Inception** with K-PaaS Portal installed.
When registering a service broker, you must be logged in as a user who can register a service broker on an open cloud platform.

##### Check Service Broker List.
>`$ cf service-brokers` 
```
$ cf service-brokers
Getting service brokers as admin...

name   url
No service brokers found
```
    
    
##### Register Container Platform Source Control Service Broker.
>`$ cf create-service-broker {Servicepack Name} {Servicepack User ID} {Servicepack User Password} http://{Servicepack URL}`

Servicepack Name: Name shown at the Open Cloud Platform for Servicepack management<br>
Servicepack User ID/PW : User ID/PW to access to the servicepack<br>
Servicepack URL: URL that can use the API provided by the servicepack<br>


###### Register Container Platfrom Source Control Service Broker 
>`$ cf create-service-broker container-platform-source-control-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:30093`   


```
$ cf create-service-broker container-platform-source-control-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:30093
Creating service broker container-platform-source-control-service-broker as admin...
OK
```    

    
##### Check the registered Container Platform Source Control Service Broker.
>`$ cf service-brokers` 
```
$ cf service-brokers 
Getting service brokers as admin... 
name                                         url 
container-platform-source-control-service-broker   http://xx.xxx.xxx.xx:30093
```

    
##### Check the accessible service list.
>`$ cf service-access`     
```
$ cf service-access 
Getting service access as admin... 
broker: container-platform-source-control-service-broker
   offering      plan     access   orgs
   scm-manager   Shared   none

```

        
##### Assign access permission of the service to a specific organization.

###### Assign access to Container Platform Source Contril Service  
>`$ cf enable-service-access scm-manager`  

```
$ cf enable-service-access scm-manager
Enabling access to all plans of service offering scm-manager for all orgs as admin...
OK
```
        
##### Check accessible service list.
>`$ cf service-access` 

```
$ cf service-access 
Getting service access as admin... 
broker: container-platform-source-control-service-broker
   offering      plan     access   orgs
   scm-manager   Shared   all
```

<br>
    
### <div id='4.3'>4.3. Container Platform Source Control Service Lookup Setting
A setting for viewing and applying for the container platform source control service on the K-PaaS portal.

##### Access to the K-PaaS Operator Portal.


##### In the [OperationManagement]-[Catalog] menu, select Container Platform Source Control Service from the App service tab and change settings.
![image](https://user-images.githubusercontent.com/80228983/146296230-2e3a90fa-44ac-4e13-9472-dfb3a1655a98.png)

##### Select Container Platform Source-control Service, set as shown below and save.
>`'Service' Catalog : Select 'scm-manager'` <br>
>`'Public' Catalog : Check as 'Y'`    

![image](https://user-images.githubusercontent.com/80228983/146360677-bd0878f4-85ac-48fc-9e30-6bc49a74381f.png)


##### Access to K-PaaS User Portal.

##### In the [Catalog]-[Service] menu, select Container Platform Source Control Service from the service tab and create Service.
![image](https://user-images.githubusercontent.com/80228983/146360859-7388527a-e570-4985-b4bc-e5b4b3f19c55.png)

<br>

    
### <div id='4.4'/>4.4. Container Platform Source Control Use Guide
- For the usage of Container Platform Source Control, refer to the use guide below.  
  + [Container Platform Source Control Use Guide](../../use-guide/source-control/cp-source-control-use-guide.md)   

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > SourceControl Installation Guide
