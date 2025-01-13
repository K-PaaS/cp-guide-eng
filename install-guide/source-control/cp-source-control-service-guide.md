### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/README.md) > SourceControl Installation Guide

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

4. [Container Platform Source Control Service Broker](#4)<br>
     4.1. [Configuring Container Platform Source Control User Authentication Service](#4.1)<br>
     4.2. [Registering container platform source control service broker](#4.2)<br>
     4.3. [Container platform source control service query settings](#4.3)<br>
     4.4. [Container Platform Source Control Usage Guide](#4.4)<br>



## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. purpose
This document (Container Platform Source Control Service Deployment Installation Guide) describes how to install Kubernetes Cluster and Container Platform Service Deployment Portal and deploy Container Platform Service Deployment Source Control.<br>

<br>

### <div id='1.2'>1.2. range
The installation scope was created based on Kubernetes Cluster deployment.

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
To install the container platform portal, refer to the guide below.
> [K-PaaS container platform portal deployment](../container-platform-portal/cp-portal-deployment-service-guide.md)

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
PROVIDER_TYPE="service"
```

- **K8S_MASTER_NODE_IP** <br>Enter Kubernetes Master Node Public IP<br><br>
- **PROVIDER_TYPE** <br>Enter container platform source control provider type <br>
    + This guide is a service deployment installation guide, so you need to enter the **'service'** value.
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
pod/cp-source-control-api-deployment-588fdfbfd7-pngz6       1/1     Running   0          55s
pod/cp-source-control-broker-deployment-84f687c698-8sgbr    1/1     Running   0          51s
pod/cp-source-control-manager-deployment-69b9b87cfd-6mvkd   1/1     Running   0          53s
pod/cp-source-control-ui-deployment-867557b66d-bg2bg        1/1     Running   0          49s

NAME                                        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/cp-source-control-api-service       NodePort   10.233.43.223   <none>        8091:30091/TCP   55s
service/cp-source-control-broker-service    NodePort   10.233.26.117   <none>        8093:30093/TCP   51s
service/cp-source-control-manager-service   NodePort   10.233.39.233   <none>        8080:30092/TCP   53s
service/cp-source-control-ui-service        NodePort   10.233.62.110   <none>        8094:30094/TCP   49s

NAME                                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cp-source-control-api-deployment       1/1     1            1           55s
deployment.apps/cp-source-control-broker-deployment    1/1     1            1           51s
deployment.apps/cp-source-control-manager-deployment   1/1     1            1           53s
deployment.apps/cp-source-control-ui-deployment        1/1     1            1           49s

NAME                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/cp-source-control-api-deployment-588fdfbfd7       1         1         1       55s
replicaset.apps/cp-source-control-broker-deployment-84f687c698    1         1         1       51s
replicaset.apps/cp-source-control-manager-deployment-69b9b87cfd   1         1         1       53s
replicaset.apps/cp-source-control-ui-deployment-867557b66d        1         1         1       49s
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
  
## <div id='4'>4. Container Platform Source Control Service Broker
When installing container platform K-PaaS as a service-type source control, a broker must be registered to link the container platform source control service deployed in CF and Kubernetes.
If you register and disclose a service through the K-PaaS operator portal, you can apply and use the service through the K-PaaS user portal.
  
### <div id='4.1'>4.1. Configuring the Container Platform Source Control User Authentication Service
In order to use container platform source control as a service, **User Authentication Service** must be configured in advance.<br>
For user authentication service configuration, refer to the guide below.
> [User authentication service configuration](../container-platform-portal/cp-portal-deployment-service-guide.md#4)
When configuring the Container Platform Portal user authentication service, it also applies to source control.

<br>

### <div id='4.2'>4.2. Register Container Platform Source Control Service Broker
:bulb: This content is carried out in **BOSH Inception** where the K-PaaS portal is installed.
When registering a service broker, you must be logged in as a user who can register a service broker on the open cloud platform.

##### Check the list of service brokers.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...

name url
No service brokers found
```
    
    
##### Register the container platform source control service broker.
>`$ cf create-service-broker {service pack name} {service pack user ID} {service pack user password} http://{service pack URL}`

Service pack name: Name displayed on the open cloud platform for service pack management<br>
Service pack user ID/password: User ID/password to access the service pack<br>
Service pack URL: URL where you can use the API provided by the service pack<br>


###### Register Container Platform Source Control Service Broker
>`$ cf create-service-broker cp-source-control-service-broker admin cloudfoundry http://{K8S_MASTER_NODE_IP}:30093`


```
$ cf create-service-broker cp-source-control-service-broker admin cloudfoundry http://xx.xxx.xxx.xx:30093
Creating service broker cp-source-control-service-broker as admin...
OK
```

    
##### Check the registered container platform source control service broker.
>`$ cf service-brokers`
```
$ cf service-brokers
Getting service brokers as admin...
name url
cp-source-control-service-broker http://xx.xxx.xxx.xx:30093
```

    
##### Check the list of accessible services.
>`$ cf service-access`
```
$ cf service-access
Getting service access as admin...
broker: cp-source-control-service-broker
    offering plan access orgs
    scm-manager Shared none
```

        
##### Assign access to the service to a specific organization.

###### Container Platform Source Control Service Access Allow Assignment
>`$ cf enable-service-access scm-manager`

```
$ cf enable-service-access scm-manager
Enabling access to all plans of service offering scm-manager for all orgs as admin...
OK
```
        
##### Check the list of accessible services.
>`$ cf service-access`

```
$ cf service-access
Getting service access as admin...
broker: cp-source-control-service-broker
    offering plan access orgs
    scm-manager Shared all
```

<br>
    
### <div id='4.3'>4.3. Container Platform Source Control Service Lookup Settings
This setting is to enable you to view and apply for the container platform source control service on the K-PaaS portal.

##### Access the K-PaaS operator portal.


##### In the menu [Operation Management]-[Catalog], select the Container Platform Source Control service in the App Service tab to change the settings.
![image](https://user-images.githubusercontent.com/80228983/146296230-2e3a90fa-44ac-4e13-9472-dfb3a1655a98.png)

##### Select the Container Platform Source-control service, change the settings as shown below, and save.
>`'Service' item: Select 'scm-manager'` <br>
>`'Public' item: Check with 'Y'`

![image](https://user-images.githubusercontent.com/80228983/146360677-bd0878f4-85ac-48fc-9e30-6bc49a74381f.png)


##### Access the K-PaaS user portal.

##### Create a service by selecting the Container Platform Source Control service in the service tab in the menu [Catalog]-[Service].
![image](https://user-images.githubusercontent.com/80228983/146360859-7388527a-e570-4985-b4bc-e5b4b3f19c55.png)

<br>

    
### <div id='4.4'/>4.4. Container Platform Source Control User Guide
- For information on how to use container platform source control, refer to the user guide below.
   + [Container Platform Source Control Use Guide](../../use-guide/source-control/cp-source-control-use-guide.md)

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](https://github.com/K-PaaS/container-platform/blob/master/install-guide/Readme.md) > SourceControl installation guide
