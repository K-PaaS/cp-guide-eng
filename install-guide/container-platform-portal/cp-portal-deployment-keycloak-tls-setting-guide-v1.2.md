### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Keycloak TLS Setting Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
    1.1. [Purpose](#1.1)  

2. [Keycloak TLS Setting](#2)  
    2.1. [TLS Authentication Certificate File Preparation](#2.1)  
    2.2. [Add Authentication Certificate Path within Dockerfile](#2.2)   
    2.3. [Keycloak values.yaml File Modification](#2.3)   
    2.4. [Modify Container Platform Portal Variable Setting](#2.4)   

3. [(Service Deployment) Modify User Authentication Service Configuration](#3)  
    3.1. [Change User Authentication Configuration Variable Value](#3.1)  

<br>

## <div id='1'>1. Document Outline
### <div id='1.1'>1.1. Purpose
This document (Keycloak TLS Setup Guide) describes how to set up Keycloak TLS when installing a Kubernetes Cluster and deploying a container platform portal.
<br><br>

## <div id='2'>2. Keycloak TLS Setting
This guide must be set up before the container platform portal is deployed.
The Container Platform Portal Standalone Deployment Installation Guide and Service Deployment Installation Guide should be done before executing **[3.2.2.2. Defining Container Platform Portal Variables]**.

### <div id='2.1'>2.1. TLS Authenrication File Preparation
TLS certificate file before container platform portal deployment (ex: tls).key, tls.crt) should be prepared ahead.<br>
- Container platform portal Deployment file **keycloak_orig** directory sub-location
- Authentication Certificate file's name needs to be changed to **tls.key**, **tls.crt**
- Authentication Certificate file authorization needs to be changed

> `Example`
```
# Located under the authentication certificate file keycloak_orig directory
ls ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/keycloak_orig/tls-key
tls.crt  tls.key

# Authentication Certificate File Authorization Change
chmod ug+r ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/keycloak_orig/tls-key/*
```


<br>
    
### <div id='2.2'>2.2. Add Authentication Certificate Path within Dockerfile
Adds the TLS certificate file path within the Keycloak Dockerfile.
```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/keycloak_orig
$ vi Dockerfile
```
    
```
# TLS_FILE_PATH : The path to the certificate file in the Deployment file keycloak_orig directory where the TLS certificate file is located
    
...
COPY {TLS_FILE_PATH}/* /etc/x509/https/
...
```
    
> `Example`
```
...
COPY tls-key/* /etc/x509/https/
COPY container-platform/ /opt/jboss/keycloak/themes/container-platform/
...
```    
    
<br>
    
### <div id='2.3'>2.3. Keycloak values.yaml File Modification    
Modify the content below from the Keycloak values.yaml file.

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/values_orig
$ vi paas-ta-container-platform-keycloak-values.yaml
```

```
# Change the service.targetPort value to 8443 (access with https)

...
service:
  type: {SERVICE_TYPE}
  protocol: {SERVICE_PROTOCOL}
  port: 8080
  https:
    port: 8443
  targetPort: 8443 (Modify)
  nodePort: 32710
...
```

<br>
    
### <div id='2.4'>2.4. Modify Container Platform Portal Variable Setting
Modify the contents below in the container platform portal variable file.
```
$ cd ~/workspace/container-platform/paas-ta-container-platform-portal-deployment/script
$ vi container-platform-portal-vars.sh    
```    
```
# Modify the KEYCLOAK_URL value http -> https 
# If you are using nip.io as a Domain, change as shown below
    
....  
# KEYCLOAK    
KEYCLOAK_URL="https:\/\/${K8S_MASTER_NODE_IP}.nip.io:32710"   # Keycloak url (include http:\/\/, if apply TLS, https:\/\/)
....     
```
<br>
    
When the **Keycloak TLS Setting** of the items above is complete, start from **[3.2.2. Defining Container Platform Portal Variable]**  of the Container Platform Portal Standalone Deployment or Service Deployment Installation Guide.
<br>


<br><br> 
    
## <div id='3'>3. (Service Deployment) Modify User Authentication Service Configuration
When deploying a container platform portal as a service, it is necessary to change the value of the user authentication configuration variable when the Keycloak TLS setting is applied.
    
### <div id='3.1'>3.1. Change User Authentication Configuration Variable Value 
 Change the **Keycloak URL** value in the UAA service and Keycloak service authentication configuration file as follows:

```
$ cd ~/workspace/container-platform/paas-ta-container-platform-saml-deployment
$ vi container-platform-saml-vars.sh
```    
```
# Change KEYCLOAK_URL value http -> https 
# If you are using nip.io as a Domain, change as shown below   
    
....     
# KEYCLOAK
KEYCLOAK_URL="https://${K8S_MASTER_NODE_IP}.nip.io:32710"   # Keycloak url (include http://, if apply TLS, https://)  
.... 
```
<br>
    
### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Keycloak TLS Setting Guide