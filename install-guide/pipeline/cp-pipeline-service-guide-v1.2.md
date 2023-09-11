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
    3.2.1. [Download Container Platform Pipeline Deployment File](#3.2.1)  
    3.2.2. [Define Container Platform Pipeline Variable](#3.2.2)    
    3.2.3. [Execute Container Platform Pipeline Deployment Script](#3.2.3)    
    3.2.4. [(Refer) Delete Container Platform Pipeline Resource](#3.2.4)    

4. [Container Platform Pipeline Service Broker](#4)   
    4.1. [Container Platform Pipeline User Authentication Service Configuration](#4.1)   
    4.2. [Container Platform Pipeline Service Broker Registration](#4.2)  
    4.3. [Container Platform Pipeline Service Lookup Settings](#4.3)    
    4.4. [Container Platform Pipeline Use Guide](#4.4)       



## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Container Platform Pipeline Service Deployment Installation Guide) installs the Kubernetes Cluster and Container Platform Service Deployment Portal and describes how to deploy the Container Platform Service Deployment Pipeline.<br>

<br>

### <div id='1.2'>1.2. Range
The installation range was prepared based on the Kubernetes Cluster deployment.

<br>

### <div id='1.3'>1.3. System Configuration Diagram
![image](https://user-images.githubusercontent.com/80228983/146350860-3722c081-7338-438d-b7ec-1fdac09160c4.png)<br>
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
> [K-PaaS Container Platform Portal Deployment](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md)     


### <div id='2.3'>2.3. Cluster Environment
Install in an environment that allows **External Network Communication** because images such as sonarqube and postgresql are downloaded from the public network for Container Platform Deployment. <br>
When the container platform pipeline installation is completed, the resource used in the idle state is as follows.
```
NAME                                                              CPU(cores)   MEMORY(bytes)
container-platform-pipeline-api-deployment-67bc5b4d9b-s6455       1m           168Mi
container-platform-pipeline-broker-deployment-c7d5cc56-xgsf2      2m           181Mi
container-platform-pipeline-common-api-deployment-6677c494bfjjj   2m           242Mi
container-platform-pipeline-config-server-deployment-58fc95prx2   2m           178Mi
container-platform-pipeline-inspection-api-deployment-6f7c6sfbp   1m           141Mi
container-platform-pipeline-jenkins-deployment-5595ff67b5-7snrn   2m           905Mi
container-platform-pipeline-ui-deployment-c88df8f74-jc9wv         1m           173Mi
paas-ta-container-platform-postgresql-postgresql-0                4m           41Mi
paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97-pltdn   13m          1566Mi

```
For cluster environments where container platform pipelines are to be installed, a minimum of **4Gi** total free memory is recommended..<br>

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
> [CRI-O insecure-registry Setting](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md#3.1)     

### <div id='3.2'>3.2. Container Platform Pipeline Deployment
    
#### <div id='3.2.1'>3.2.1.  Container Platform Pipeline Deployment File Download
For container platform pipeline deployment, download the container platform pipeline deployment file and locate it in the path below.<br>
:bulb: This content will be conducted at Kubernetes **Master Node**.

+  Container Platform Pipeline Deployment File Download :  
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
CF_API_URL="https:\/\/{paas-ta-api-domain}"                      # e.g) In case of https:\/\/api.10.0.0.120.nip.io,  PaaS-TA API Domain, enter PROVIDER_TYPE=service 
....    
```
```    
# Example
K8S_MASTER_NODE_IP="xx.xxx.xxx.xx"                                       
PROVIDER_TYPE="service"
CF_API_URL="https:\/\/api.xx.xxx.xxx.xx.nip.io"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter Container Platform Pipeline Providing Type <br>
   + This guide is a service deployment installation guide that requires **'service'** values<br><br>
- **CF_API_URL** <br>Enter api domain to connect to K-PaaS to interwork the service <br>
<br>    

:bulb: Keycloak's default deployment method is **HTTP** and **HTTPS** via certificate is enabled
> [Keycloak TLS Setting](../container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide-v1.2.md)

Modify the contents under the container platform pipeline variable file.
```
$ vi container-platform-pipeline-vars.sh    
```    
```
# Change KEYCLOAK_URL Value http -> https 
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
pod/container-platform-pipeline-api-deployment-67bc5b4d9b-s6455       1/1     Running   0          119s
pod/container-platform-pipeline-broker-deployment-c7d5cc56-xgsf2      1/1     Running   1          118s
pod/container-platform-pipeline-common-api-deployment-6677c494bfjjj   1/1     Running   0          118s
pod/container-platform-pipeline-config-server-deployment-58fc95prx2   1/1     Running   0          113s
pod/container-platform-pipeline-inspection-api-deployment-6f7c6sfbp   1/1     Running   0          116s
pod/container-platform-pipeline-jenkins-deployment-5595ff67b5-7snrn   1/1     Running   0          115s
pod/container-platform-pipeline-ui-deployment-c88df8f74-jc9wv         1/1     Running   0          117s
pod/paas-ta-container-platform-postgresql-postgresql-0                1/1     Running   0          112s
pod/paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97-pltdn   1/1     Running   0          111s

NAME                                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/container-platform-pipeline-api-service              NodePort    10.233.10.130   <none>        8082:30082/TCP   119s
service/container-platform-pipeline-broker-service           NodePort    10.233.6.98     <none>        8083:30083/TCP   118s
service/container-platform-pipeline-common-api-service       NodePort    10.233.47.49    <none>        8081:30081/TCP   118s
service/container-platform-pipeline-config-server-service    NodePort    10.233.51.40    <none>        8080:30088/TCP   114s
service/container-platform-pipeline-inspection-api-service   NodePort    10.233.55.123   <none>        8085:30085/TCP   116s
service/container-platform-pipeline-jenkins-service          NodePort    10.233.18.195   <none>        8080:30086/TCP   115s
service/container-platform-pipeline-ui-service               NodePort    10.233.49.219   <none>        8084:30084/TCP   117s
service/paas-ta-container-platform-postgresql                ClusterIP   10.233.46.68    <none>        5432/TCP         112s
service/paas-ta-container-platform-postgresql-headless       ClusterIP   None            <none>        5432/TCP         112s
service/paas-ta-container-platform-sonarqube-sonarqube       NodePort    10.233.47.187   <none>        9000:30087/TCP   111s

NAME                                                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/container-platform-pipeline-api-deployment              1/1     1            1           119s
deployment.apps/container-platform-pipeline-broker-deployment           1/1     1            1           118s
deployment.apps/container-platform-pipeline-common-api-deployment       1/1     1            1           118s
deployment.apps/container-platform-pipeline-config-server-deployment    1/1     1            1           114s
deployment.apps/container-platform-pipeline-inspection-api-deployment   1/1     1            1           116s
deployment.apps/container-platform-pipeline-jenkins-deployment          1/1     1            1           115s
deployment.apps/container-platform-pipeline-ui-deployment               1/1     1            1           117s
deployment.apps/paas-ta-container-platform-sonarqube-sonarqube          1/1     1            1           111s

NAME                                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/container-platform-pipeline-api-deployment-67bc5b4d9b             1         1         1       119s
replicaset.apps/container-platform-pipeline-broker-deployment-c7d5cc56            1         1         1       118s
replicaset.apps/container-platform-pipeline-common-api-deployment-6677c494b       1         1         1       118s
replicaset.apps/container-platform-pipeline-config-server-deployment-58fc9bb8b4   1         1         1       113s
replicaset.apps/container-platform-pipeline-inspection-api-deployment-6f7cbc558   1         1         1       116s
replicaset.apps/container-platform-pipeline-jenkins-deployment-5595ff67b5         1         1         1       115s
replicaset.apps/container-platform-pipeline-ui-deployment-c88df8f74               1         1         1       117s
replicaset.apps/paas-ta-container-platform-sonarqube-sonarqube-5c799d8c97         1         1         1       111s

NAME                                                                READY   AGE
statefulset.apps/paas-ta-container-platform-postgresql-postgresql   1/1     112s


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
  
## <div id='4'>4. Container Platform Pipeline Service Broker
When installing as a container platform K-PaaS service pipeline, brokers must be registered to interwork the container platform pipeline service deployed to CF and Kubernetes.
If you register and disclose the service through the K-PaaS operator portal, you can apply for and use the service through the K-PaaS user portal.
  
## <div id='4.1'>4.1. Container Platform Pipeline User Authentication Service Configuration
To use the container platform pipeline as a service, the **user authentication service** configuration must be performed in advance.<br>
Refer to the guide below for configuring the user authentication service.
> [User Authentication Service Configuration](../container-platform-portal/cp-portal-deployment-service-guide-v1.2.md#4)      
When configuring the container platform portal user authentication service, it also gets applied to the pipeline.

### <div id='4.2'>4.2. Container Platform Pipeline Service Broker Registration
:bulb: This content will be conducted at the **BOSH Inception** where K-PaaS Portal is installed.
When registering a service broker, you must be logged in as a user who can register a service broker on an open cloud platform.

##### Check Service Broker List.
>`$ cf service-brokers` 
```
$ cf service-brokers
Getting service brokers as admin...

name   url
No service brokers found
```
    
    
##### Register Container Platform Pipeline Service Broker.
>`$ cf create-service-broker {Servicepack Name} {Servicepack USer ID} {Servicepack User Password} http://{Servicepack URL}`

Servicepack Name: Name shown at the Open Cloud Platform for Servicepack Management<br>
Servicepack User ID/PW : User ID/ Password for servicepack access<br>
Servicepack URL: URL that can use the API provided by the servicepack<br>


###### Container Platform Pipeline Service Broker Registration 
>`$ cf create-service-broker container-platform-pipeline-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:30083`   


```
$ cf create-service-broker container-platform-pipeline-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:30083
Creating service broker container-platform-pipeline-service-broker as admin...
OK
```    

    
##### Check the registered Container Platform Pipeline Service Broker.
>`$ cf service-brokers` 
```
$ cf service-brokers 
Getting service brokers as admin... 
name                                         url 
container-platform-pipeline-service-broker   http://xx.xxx.xxx.xx:30083
```

    
##### Check the accessible service list.
>`$ cf service-access`     
```
$ cf service-access 
Getting service access as admin... 
broker: container-platform-pipeline-service-broker 
   offering                      plan                                 access   orgs 
   container-platform-pipeline   container-platform-pipeline-shared   none

```

        
##### Assign authentication to access the service to a specific organization.

###### Assign access to Container Platform Pipeline Service  
>`$ cf enable-service-access container-platform-pipeline`   

```
$ cf enable-service-access container-platform-pipeline 
Enabling access to all plans of service offering container-platform-pipeline for all orgs as admin... 
OK
```
        
##### Check the accessible service list.
>`$ cf service-access` 

```
$ cf service-access 
Getting service access as admin... 
broker: container-platform-pipeline-service-broker 
   offering                      plan                                 access   orgs 
   container-platform-pipeline   container-platform-pipeline-shared   all
```

<br>
    
### <div id='4.3'>4.3. Container Platform Pipeline Service Lookup Settings
A setting for retrieving and applying for container platform pipeline service on K-PaaS Portal.

##### Access to K-PaaS Opertator Portal.


##### In [Operation Management]-[Catalog] menu, select the Container Platform Pipeline service in the App Service tab and change the settings.
![image](https://user-images.githubusercontent.com/80228983/146296230-2e3a90fa-44ac-4e13-9472-dfb3a1655a98.png)

##### Select the Container Platform Pipeline service and change the setting as follows and save it.
>`'Service' Catalog : Select 'container-platform-pipeline'` <br>
>`' Public' Catalog: Check as 'Y'`    

![image](https://user-images.githubusercontent.com/80228983/146296316-3bbb70d4-ce31-42f6-9ec0-019c0f12d774.png)

##### Access to K-PaaS User Portal.

##### In the [Catalog]-[Service] menu, select the container platform pipeline service in the service tab to create a service.
![image](https://user-images.githubusercontent.com/80228983/146296949-fceac26c-86b6-40fb-b005-dcc84b3f081c.png)

<br>

    
### <div id='4.4'/>4.4. Container Platform Pipeline Use Guide
- Refer to the use guide below for container platform pipeline usage.  
  + [Container Platform Pipeline Use Guide](../../use-guide/pipeline/cp-pipeline-use-guide.md)   

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/tree/master) > [CP Install](../Readme.md) > Pipeline Installation Guide
