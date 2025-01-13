### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/README.md) > Keycloak TLS Configuration Guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
    1.1. [Purpose](#1.1)  <br>

2. [Keycloak TLS Configuration](#2)<br>
    2.1. [Prepare TLS Certificate Files](#2.1)<br>
    2.2. [Add Certificate File Path in Dockerfile](#2.2)<br>
    2.3. [Modify Keycloak values.yaml File](#2.3)<br>
    2.4. [Modify Container Platform Portal Variable File](#2.4)<br>

3. [(Service Deployment) User Authentication Service Configuration Change](#3)<br>
    3.1. [Change User Authentication Configuration Variables](#3.1)<br>

<br>

## <div id='1'>1. Document Overview
### <div id='1.1'>1.1. Purpose
This document (Keycloak TLS Configuration Guide) describes the method of setting up Keycloak TLS when installing a Kubernetes Cluster and deploying a Container Platform Portal.
<br><br>

## <div id='2'>2. Keycloak TLS Configuration
This guide should be followed before deploying the Container Platform Portal.
Perform the tasks before executing **[3.1.2. Define Container Platform Portal Variables]** in the standalone deployment installation guide or service deployment installation guide.

### <div id='2.1'>2.1. Prepare TLS Certificate Files
Before deploying the Container Platform Portal, TLS certificate files (e.g., tls.key, tls.crt) must be prepared in advance.<br>
- The TLS certificate files must be located under the Deployment file keycloak_orig directory.
- The certificate file names should be changed to **tls.key** and **tls.crt**.
- Change the permissions of the certificate files.


```
# Create a directory for the certificate files
$ mkdir ~/workspace/container-platform/cp-portal-deployment/keycloak_orig/tls-key

# Move the certificate files to the directory and verify
$ ls ~/workspace/container-platform/cp-portal-deployment/keycloak_orig/tls-key
tls.crt tls.key

# Change the permissions of the certificate files
$ chmod ug+r ~/workspace/container-platform/cp-portal-deployment/keycloak_orig/tls-key/*
```



<br>
    
### <div id='2.2'>2.2. Add Certificate File Path in Dockerfile 
Add the TLS certificate file path in the Keycloak Dockerfile.

- > COPY {TLS_FILE_PATH}/* /etc/x509/https/
  + TLS_FILE_PATH: TLS certificate file path within the Deployment file keycloak_orig directory


```
$ cd ~/workspace/container-platform/cp-portal-deployment/keycloak_orig
$ vi Dockerfile
```
```
...
# If you apply TLS, add the TLS certificate file path. (Go to the appropriate annotation location)
COPY tls-key/* /etc/x509/https/  (add)
COPY container-platform/ /opt/jboss/keycloak/themes/container-platform/
...
```    
    
<br>
    
### <div id='2.3'>2.3. Modify Keycloak values.yaml File    
Modify the following content in the Keycloak values.yaml file.

```
$ cd ~/workspace/container-platform/cp-portal-deployment/values_orig
$ vi cp-keycloak.yaml
```

```
# Change service.targetPort value to 8443 (access by https)

...
service:
  type: {SERVICE_TYPE}
  protocol: {SERVICE_PROTOCOL}
  port: 8080
  https:
    port: 8443
  targetPort: 8443 (edit)
  nodePort: 32710
...
```

<br>

<br>
    
### <div id='2.4'>2.4. Modify Container Platform Portal Variable File
Modify the following content in the Container Platform Portal variable file.

```
$ cd ~/workspace/container-platform/cp-portal-deployment/script
$ vi cp-portal-vars.sh    
```    
```
# Change KEYCLOAK_URL value from http to https
# If using nip.io as the domain, change as follows
    
....  
# KEYCLOAK    
KEYCLOAK_URL="https://${K8S_MASTER_NODE_IP}.nip.io:32710"   # keycloak url (if apply TLS, https:// )
....     
```
<br>
    
After completing the above **Keycloak TLS settings**, proceed with **[3.1.2. Define Container Platform Portal Variables]** in the main body of the standalone deployment installation guide or service deployment installation guide.
<br>


<br><br> 
    
## <div id='3'>3. (Service Deployment) Change User Authentication Service Configuration
When deploying the Container Platform Portal as a service, if Keycloak TLS is applied, it is necessary to change the user authentication configuration variable values.
    
### <div id='3.1'>3.1. Change User Authentication Configuration Variables
 Change the **Keycloak URL** value in the user authentication service and Keycloak service configuration variable files as follows.


```
$ cd ~/workspace/container-platform/cp-saml-deployment
$ vi cp-saml-vars.sh
```    
```
# Change KEYCLOAK_URL value from http to https
# If using nip.io as the domain, change as follows
    
....     
# KEYCLOAK
KEYCLOAK_URL="https://${K8S_MASTER_NODE_IP}.nip.io:32710"   # Keycloak url (include http://, if apply TLS, https://)  
.... 
```
<br>
    
### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](/install-guide/README.md) > Keycloak TLS 설정 가이드
