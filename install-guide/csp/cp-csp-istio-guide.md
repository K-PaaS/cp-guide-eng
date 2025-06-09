### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > CSP Kubernetes Service Istio Multi Cluster Installation Guide

<br>

## Table of Contents
1. [Document Overview](#1)    
   1.1. [Purpose](#1.1)  
   1.2. [Range](#1.2)  
   1.3. [System Configuration](#1.3)  
   1.4. [Reference](#1.4)

2. [Information](#2)  
   2.1. [Prerequisite](#2.1)  
   2.2. [Installation List](#2.2)  
   2.3. [Firewall Information](#2.3)  
   2.4. [Demo Cluster Environment](#2.4)  
   
3. [Install Istio Multi Cluster](#3)  
   3.1. [Download the Deployment](#3.1)   
   3.2. [Install CLI](#3.2)  
   3.3. [Configure Multi Cluster Access](#3.3)       
   3.4. [Install Istio Multi Cluster](#3.4)   

4. [Sample Application](#4)  
   4.1. [Deploy Multi Cluster Sample Application](#4.1)     
   4.2. [Verifying Cross-Cluster Traffic](#4.2)    

5. [Delete Istio Multi Cluster](#5)  

6. [Presets when deploying the Container Platform Portal](#6)    
   6.1. [Check StorageClass](#6.1)  
   6.2. [Install Metrics Server](#6.2)  
   6.3. [Modify the Configuration of a cluster using Cilium CNI](#6.3)    
  
7. [Reference](#7)  
   7.1. [Manually configure Istio Ingressgateway EXTERNAL-IP](#7.1)  
   7.2. [Troubleshooting Horizontal Pod Autoscaling unknown](#7.2)
    

<br>

## <span id='1'> 1. Document Overview
### <span id='1.1'> 1.1. Purpose
This document (CSP Kubernetes Service Istio Multi Cluster Installation Guide) describes how to install Istio Multi Cluster on a cloud service provider's Kubernetes service, enabling each cluster to reach from another cluster.

<br>

### <span id='1.2'>1.2. Range
It was written to install multi cluster using Istio for `CSP Kubernetes Service` clusters.

<br>

### <span id='1.3'>1.3. System Configuration
Configure Istio Mesh by installing the Istio Control Plane on N Kubernetes clusters across different networks. <br>
The default network for cluster N is set to network N, and the cluster communicates through the Istio Gateway without direct communication between cluster Pods.

> *Image Source* <br>
> [Install Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/)<br>

<img src="https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/arch.svg" width="800">


<br>

### <span id='1.4'>1.4. Reference
> [Istio Install Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/)<br>

<br>

## <span id='2'> 2. Information

### <span id='2.1'>2.1. Prerequisite
- This installation guide is based on **Ubuntu 22.04** installation.
- Each cluster requires support for a **LoadBalancer** type of service.

<br>

### <span id='2.2'>2.2. Installation List
The list of installed tools is as follows.
| Tool | Version |
| :---: | :---: |  
| kubectl | v1.32.3 |
| Helm | v3.16.4 |
| step | 0.24.4 |
| Podman | - |
| ca-certificates | - |
| Istio | 1.26.0 |

| Istio Version | [Supported Kubernetes Versions](https://istio.io/latest/docs/releases/supported-releases/#support-status-of-istio-releases)|  
| :---: | :---: |  
|  `1.26`  |1.29, 1.30, 1.31, 1.32|  

<br>

### <span id='2.3'>2.3. Firewall Information
Set the port that should be opened for the IaaS Security Group.
|Protocol|Port|Description|
| :---: | :---: | --- | 
|TCP|15021|Istio ingressgateway status|
|TCP|80|Istio ingressgateway http|
|TCP|443|Istio ingressgateway https|
|TCP|31400|Istio ingressgateway tcp|
|TCP|15443|Istio ingressgateway mtls|
|TCP|15012|Istio ingressgateway istiod|
|TCP|15017|Istio ingressgateway webhook|


<br>
<br>

### <span id='2.4'>2.4. Demo Cluster Environment
We provide an example of configuring a multi cluster environment based on three clusters using Istio.
> This guide targets the Kubernetes cluster services of three domestic CSP.

#### CSP Kubernetes cluster environment
| Kubernetes Service                    | CNI     |
|---------------------------------------|---------| 
| KT Kubernetes Service (K2P Standard)  | Calico  |
| Ncloud Kubernetes Service (NKS)       | Cilium  |
| NHN kubernetes Service (NKS)          | Calico  | 


<br>

## <span id='3'>3. Install Istio Multi Cluster
> It provides an example of configuring a multi cluster environment based on three clusters using Istio.

### <span id='3.1'>3.1. Download the Deployment
To install Istio Multi Cluster, download the Container Platform Portal Deployment and locate it at the path below.<br>

+ Download the Container Platform Portal Deployment file :
  [cp-portal-deployment-v1.6.1.1.tar.gz](https://nextcloud.k-paas.org/index.php/s/jyjGsowwx3AHNPk/download)

```bash
# Create Path
$ mkdir -p ~/workspace/container-platform
$ cd ~/workspace/container-platform

# Download Deployment File and Verify File
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/jyjGsowwx3AHNPk/download

$ ls ~/workspace/container-platform
  cp-portal-deployment-v1.6.1.1.tar.gz

# Decompress Deployment Files
$ tar -xvf cp-portal-deployment-v1.6.1.1.tar.gz
```

<br>

### <span id='3.2'>3.2. Install CLI
> [[2.1. Installation List]](#2.1) <br>

For Istio multi cluster installation, you need to pre-install the necessary tools, such as the command line.<br>
For `kubectl`, it is necessary to use the version that is within the minor version difference of the cluster, so check the cluster version to modify the file below if any changes are needed.
```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/istio_mc
$ vi istio-vars-mc.sh
```
```bash
# Change to the version to install
# command line tool
KUBECTL_VERSION="1.32.3"
HELM_VERSION="3.16.4"
STEP_VERSION="0.24.4"

# Istio
ISTIO_VERSION="1.26.0"
...
```

Run the tool installation script.
```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/istio_mc
$ chmod +x install_tools.sh
$ ./install_tools.sh
```

<br>

### <span id='3.3'>3.3. Configure Multi Cluster Access
The Istio multi cluster installation is based on the context list output by the `kubectl config get-contexts` command.<br>
Therefore, we need to pre-configure the context to access each cluster. <br>
:loudspeaker: Ensure that cluster, context, and user are not duplicated in each cluster kubeconfig file.
```bash
# Create and Move .kube Dir
$ mkdir -p ${HOME}/.kube
$ cd ${HOME}/.kube

# Place each cluster kubeconfig file
$ ls ${HOME}/.kube
cluster1-config  cluster2-config  cluster3-config

# Set the kubeconfig file path
$ export KUBECONFIG="${HOME}/.kube/cluster1-config:${HOME}/.kube/cluster2-config:${HOME}/.kube/cluster3-config"
```
##### :bulb: Check the output of 'NAME', 'CLUSTER', and 'AUTHINFO' values normally
```bash
$ kubectl config get-contexts
CURRENT   NAME     CLUSTER      AUTHINFO     NAMESPACE
*         kt       kt-k2p       kt-k2p
          ncloud   ncloud-nks   ncloud-nks
          nhn      nhn-nks      nhn-nks
```
##### Verify the get cluster resource
```bash
# Get nodes in cluster1 (kt) 
$ kubectl get nodes --context=kt
NAME                       STATUS   ROLES           AGE    VERSION
kt-k2p-standard.master01   Ready    control-plane   5d6h   v1.30.4
kt-k2p-standard.worker01   Ready    <none>          5d6h   v1.30.4
kt-k2p-standard.worker02   Ready    <none>          5d6h   v1.30.4

# Get nodes in cluster2 (ncloud)
$ kubectl get nodes --context=ncloud
NAME                    STATUS   ROLES    AGE    VERSION
ncloud-nks-w-2lmr       Ready    <none>   5d6h   v1.28.10
ncloud-nks-w-2lms       Ready    <none>   5d6h   v1.28.10

# Get nodes in cluster3 (nhn)
$ kubectl get nodes --context=nhn
NAME                                STATUS   ROLES    AGE    VERSION
nhn-nks-default-worker-node-0       Ready    <none>   5d6h   v1.30.3
nhn-nks-default-worker-node-1       Ready    <none>   5d6h   v1.30.3
```

<br>


### <span id='3.4'>3.4. Install Istio Multi Cluster
Run the script for the Istio multi-cluster installation.
```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/istio_mc
$ chmod +x deploy-istio-mc.sh
$ ./deploy-istio-mc.sh
```
```bash
# Below is the log of the Istio installation script run against three clusters.
[Generate Root CA]...
Your certificate has been saved in certs/root-cert.pem.
Your private key has been saved in certs/root-ca.key.
[Install Istio in cluster1]...
Your certificate has been saved in certs/kt-k2p/ca-cert.pem.
Your private key has been saved in certs/kt-k2p/ca-key.pem.
namespace/istio-system created
secret/cacerts created
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Istio core installed ⛵️
✔ Istiod installed 🧠
✔ Installation complete
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Ingress gateways installed 🛬
✔ Installation complete
gateway.networking.istio.io/cross-network-gateway created
customresourcedefinition.apiextensions.k8s.io/gatewayclasses.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/gateways.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/httproutes.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/referencegrants.gateway.networking.k8s.io created
[Install Istio in cluster2]...
Your certificate has been saved in certs/ncloud-nks/ca-cert.pem.
Your private key has been saved in certs/ncloud-nks/ca-key.pem.
namespace/istio-system created
secret/cacerts created
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Istio core installed ⛵️
✔ Istiod installed 🧠
✔ Installation complete
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Ingress gateways installed 🛬
✔ Installation complete
gateway.networking.istio.io/cross-network-gateway created
customresourcedefinition.apiextensions.k8s.io/gatewayclasses.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/gateways.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/httproutes.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/referencegrants.gateway.networking.k8s.io created
[Install Istio in cluster3]...
Your certificate has been saved in certs/nhn-nks/ca-cert.pem.
Your private key has been saved in certs/nhn-nks/ca-key.pem.
namespace/istio-system created
secret/cacerts created
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Istio core installed ⛵️
✔ Istiod installed 🧠
✔ Installation complete
        |\
        | \
        |  \
        |   \
      /||    \
     / ||     \
    /  ||      \
   /   ||       \
  /    ||        \
 /     ||         \
/______||__________\
____________________
  \__       _____/
     \_____/

✔ Ingress gateways installed 🛬
✔ Installation complete
gateway.networking.istio.io/cross-network-gateway created
customresourcedefinition.apiextensions.k8s.io/gatewayclasses.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/gateways.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/httproutes.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/referencegrants.gateway.networking.k8s.io created
secret/istio-remote-secret-kt-k2p created
secret/istio-remote-secret-kt-k2p created
secret/istio-remote-secret-ncloud-nks created
secret/istio-remote-secret-ncloud-nks created
secret/istio-remote-secret-nhn-nks created
secret/istio-remote-secret-nhn-nks created

--------------------------------------------------------------
[cluster1 (kt)] $ istioctl remote-clusters
--------------------------------------------------------------
NAME           SECRET                                          STATUS     ISTIOD
kt-k2p                                                         synced     istiod-69f99b4667-2rbmd
ncloud-nks     istio-system/istio-remote-secret-ncloud-nks     synced     istiod-69f99b4667-2rbmd
nhn-nks        istio-system/istio-remote-secret-nhn-nks        synced     istiod-69f99b4667-2rbmd

--------------------------------------------------------------
[cluster2 (ncloud)] $ istioctl remote-clusters
--------------------------------------------------------------
NAME           SECRET                                       STATUS     ISTIOD
ncloud-nks                                                  synced     istiod-5c6c96f9c4-xg66w
kt-k2p         istio-system/istio-remote-secret-kt-k2p      synced     istiod-5c6c96f9c4-xg66w
nhn-nks        istio-system/istio-remote-secret-nhn-nks     synced     istiod-5c6c96f9c4-xg66w

--------------------------------------------------------------
[cluster3 (nhn)] $ istioctl remote-clusters
--------------------------------------------------------------
NAME           SECRET                                          STATUS     ISTIOD
nhn-nks                                                        synced     istiod-5bdd85f847-swxk4
kt-k2p         istio-system/istio-remote-secret-kt-k2p         synced     istiod-5bdd85f847-swxk4
ncloud-nks     istio-system/istio-remote-secret-ncloud-nks     synced     istiod-5bdd85f847-swxk4
```
<br>

#### Istio Resource Status
> Set cluster context name environment variable
```bash
$ source ~/workspace/container-platform/cp-portal-deployment/istio_mc/istio-vars-mc.sh
```
#### Check cluster1's (KT K2P) Istio Resource Status
**KT K2P Standard(K8S)** does not support the LoadBalancer service type (automatically generated), so the EXTERNAL-IP of istio-ingressgateway is in `Pending` status.
Please refer to the guide below to create a LoadBalancer and assign the EXTERNAL-IP to the istio-ingressgateway service. <br>
Also, if the HorizontalPodAutoscaler Targets value is `unknown`, please refer to the guide below. 
> [[7.1. Manually configure Istio Ingressgateway EXTERNAL-IP]](#7.1) <br>
> [[7.2. Troubleshooting Horizontal Pod Autoscaling unknown]](#7.2) 

```bash
$ kubectl get all -n ${ISTIO_NAMESPACE} --context=${CLUSTER1_CONFIG[CTX]}
NAME                                       READY   STATUS    RESTARTS   AGE
pod/istio-ingressgateway-fd6887d6f-6kzzn   1/1     Running   0          8m46s
pod/istiod-69f99b4667-2rbmd                1/1     Running   0          8m51s

NAME                           TYPE           CLUSTER-IP       EXTERNAL-IP        PORT(S)                                                                                                      AGE
service/istio-ingressgateway   LoadBalancer   10.103.54.27     210.xxx.xxx.xxx    15021:32654/TCP,80:31323/TCP,443:30658/TCP,31400:32070/TCP,15443:31125/TCP,15012:30960/TCP,15017:31546/TCP   8m46s
service/istiod                 ClusterIP      10.100.252.167   <none>             15010/TCP,15012/TCP,443/TCP,15014/TCP                                                                        8m51s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/istio-ingressgateway   1/1     1            1           8m46s
deployment.apps/istiod                 1/1     1            1           8m51s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/istio-ingressgateway-fd6887d6f   1         1         1       8m46s
replicaset.apps/istiod-69f99b4667                1         1         1       8m51s

NAME                                                       REFERENCE                         TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/istio-ingressgateway   Deployment/istio-ingressgateway   cpu: 3%/80%   1         5         1          8m46s

```

#### Check cluster2's (Ncloud NKS) Istio Resource Status
**Ncloud Kubernetes Service** provides `Cilium` as the default CNI. <br>
See Using Istio in Kubernetes clusters using Cilium.
> [[Cilium’s integration with Istio]](https://docs.cilium.io/en/stable/network/servicemesh/istio) <br>
> [(Reference) [6.3. Modify the Configuration of a cluster using Cilium CNI]](#6.3) 


```bash
$ kubectl get all -n ${ISTIO_NAMESPACE} --context=${CLUSTER2_CONFIG[CTX]}
NAME                                        READY   STATUS    RESTARTS   AGE
pod/istio-ingressgateway-7596586df5-77kwt   1/1     Running   0          9m14s
pod/istiod-5c6c96f9c4-xg66w                 1/1     Running   0          9m22s

NAME                           TYPE           CLUSTER-IP       EXTERNAL-IP                         PORT(S)                                                                                                      AGE
service/istio-ingressgateway   LoadBalancer   198.19.236.169   istio-syste-istio-ingres-xxx.com    15021:30852/TCP,80:30966/TCP,443:30811/TCP,31400:30812/TCP,15443:31121/TCP,15012:31116/TCP,15017:30571/TCP   9m14s
service/istiod                 ClusterIP      198.19.150.8     <none>                              15010/TCP,15012/TCP,443/TCP,15014/TCP                                                                        9m22s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/istio-ingressgateway   1/1     1            1           9m15s
deployment.apps/istiod                 1/1     1            1           9m23s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/istio-ingressgateway-7596586df5   1         1         1       9m16s
replicaset.apps/istiod-5c6c96f9c4                 1         1         1       9m24s

NAME                                                       REFERENCE                         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/istio-ingressgateway   Deployment/istio-ingressgateway   3%/80%    1         5         1          9m16s
```

#### Check cluster3's (NHN NKS) Istio Resource Status
```
$ kubectl get all -n ${ISTIO_NAMESPACE} --context=${CLUSTER3_CONFIG[CTX]}
NAME                                        READY   STATUS    RESTARTS   AGE
pod/istio-ingressgateway-5df95c4895-kcrrv   1/1     Running   0          10m
pod/istiod-5bdd85f847-swxk4                 1/1     Running   0          10m

NAME                           TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                                                                                                      AGE
service/istio-ingressgateway   LoadBalancer   10.254.216.17   133.xxx.xxx.xxx   15021:32380/TCP,80:32215/TCP,443:31685/TCP,31400:31715/TCP,15443:31549/TCP,15012:32498/TCP,15017:31299/TCP   10m
service/istiod                 ClusterIP      10.254.166.12   <none>            15010/TCP,15012/TCP,443/TCP,15014/TCP                                                                        10m

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/istio-ingressgateway   1/1     1            1           10m
deployment.apps/istiod                 1/1     1            1           10m

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/istio-ingressgateway-5df95c4895   1         1         1       10m
replicaset.apps/istiod-5bdd85f847                 1         1         1       10m

NAME                                                       REFERENCE                         TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/istio-ingressgateway   Deployment/istio-ingressgateway   cpu: 2%/80%   1         5         1          10m
```
<br> 


## <span id='4'>4. Sample Application
Deploy a sample application to clusters cluster1, cluster2, and cluster3 to test multi cluster communication.
> Deploy some applications in [Istio Bookinfo Application](https://istio.io/latest/docs/examples/bookinfo/)
#### Where to deploy the application
| Application | cluster1 | cluster2 | cluster3 | Information |
| :---: |:---: | :---: | :---: | :---: | 
| reviews-v1 | :heavy_check_mark: | | | Returns its version (v1) and hostname | 
| reviews-v2 | |:heavy_check_mark: | | Returns its version (v2) and hostname | 
| reviews-v3 | | |:heavy_check_mark: | Returns its version (v3) and hostname | 
| sleep | | | :heavy_check_mark: |Use as a request source to call other services| 


```bash
# Set cluster context name variable
$ source ~/workspace/container-platform/cp-portal-deployment/istio_mc/istio-vars-mc.sh

# Create sample dir
$ mkdir -p ~/workspace/container-platform/cp-portal-deployment/istio_mc/sample
$ cd ~/workspace/container-platform/cp-portal-deployment/istio_mc/sample

# Download bookinfo.yaml
$ wget https://raw.githubusercontent.com/istio/istio/release-1.24/samples/bookinfo/platform/kube/bookinfo.yaml
# Download sleep.yaml
$ wget https://raw.githubusercontent.com/istio/istio/master/samples/sleep/sleep.yaml
```

<br>

### <span id='4.1'>4.1. Deploy Multi Cluster Sample Application
```bash
# Create sample namespace for each cluster and set the istio-injection=enabled label on a namespace
# Deploy reviews-N application to cluster N
for IDX in $(seq 1 "$CLUSTER_CNT"); do CTX="CLUSTER${IDX}_CONFIG[CTX]"; \
  echo "context: ${!CTX}"
  kubectl --context=${!CTX} create ns sample; \
  kubectl --context=${!CTX} label namespace sample istio-injection=enabled; \
  kubectl --context=${!CTX} -n sample apply -f bookinfo.yaml -l service=reviews; \
  kubectl --context=${!CTX} -n sample apply -f bookinfo.yaml -l account=reviews; \
  kubectl --context=${!CTX} -n sample apply -f bookinfo.yaml -l app=reviews,version=v${IDX}; \
done
```
```bash
# Deploy sleep application to cluster3
$ kubectl --context="${CLUSTER3_CONFIG[CTX]}" -n sample apply -f sleep.yaml
```

<br>


### <span id='4.2'>4.2. Verifying Cross-Cluster Traffic
```bash
# Check reviews-v1 pod in cluster1
$ kubectl get pods -l app=reviews --context="${CLUSTER1_CONFIG[CTX]}" -n sample
NAME                          READY   STATUS    RESTARTS   AGE
reviews-v1-67896867f4-2rtf4   2/2     Running   0          2m17s

# Check reviews-v2 pod in cluster2
$ kubectl get pods -l app=reviews --context="${CLUSTER2_CONFIG[CTX]}" -n sample
NAME                          READY   STATUS    RESTARTS   AGE
reviews-v2-65c9797659-zp6mf   2/2     Running   0          2m42s

# Check reviews-v3, sleep pod in cluster3
$ kubectl get pods -l app=reviews --context="${CLUSTER3_CONFIG[CTX]}" -n sample
NAME                          READY   STATUS    RESTARTS   AGE
reviews-v3-77947c4c78-7vvkf   2/2     Running   0          3m9s

$ kubectl get pods -l app=sleep --context="${CLUSTER3_CONFIG[CTX]}" -n sample
NAME                     READY   STATUS    RESTARTS   AGE
sleep-5577c64d7c-9zg7w   2/2     Running   0          74s
```

```bash
# Access to cluster3's sleep pod 
$ kubectl exec -it -n sample --context="${CLUSTER3_CONFIG[CTX]}" \
"$(kubectl get pod -n sample --context="${CLUSTER3_CONFIG[CTX]}" -l app=sleep -o jsonpath='{.items[0].metadata.name}')" -c sleep -- sh

# curl reviews service
~ $ curl reviews:9080/reviews/1
{"id": "1","podname": "reviews-v3-77947c4c78-7vvkf","clustername": "null","reviews":...  # cluster3's reviews-v3 
~ $ curl reviews:9080/reviews/1
{"id": "1","podname": "reviews-v2-65c9797659-zp6mf","clustername": "null","reviews":...  # cluster2's reviews-v2 
~ $ curl reviews:9080/reviews/1
{"id": "1","podname": "reviews-v2-65c9797659-zp6mf","clustername": "null","reviews":...  # cluster2's reviews-v2 
~ $ curl reviews:9080/reviews/1
{"id": "1","podname": "reviews-v1-67896867f4-2rtf4","clustername": "null","reviews":...  # cluster1's reviews-v1 
```

<br>

##### :wastebasket: Cleanup Sample Application
When the sample application test is finished, run the following script to clean up your resources.
```bash
# Delete sample namespace
for IDX in $(seq 1 "$CLUSTER_CNT"); do CTX="CLUSTER${IDX}_CONFIG[CTX]"; \
  echo "context: ${!CTX}"; kubectl --context=${!CTX} delete ns sample; \
done
```

<br>

## <span id='5'>5. Delete Istio Multi Cluster
If you want to delete the Istio multi cluster configuration, run the script below. <br>
:loudspeaker: (Caution) When you run this script, it will remove all Istio multicluster resources. <br>

```bash
$ cd ~/workspace/container-platform/cp-portal-deployment/istio_mc
$ chmod +x uninstall-istio-mc.sh
$ ./uninstall-istio-mc.sh
Are you sure you want to uninstall istio in all clusters? <y/n> y # Enter 'Y'
```

<br>

## <span id='6'>6. Presets when deploying the Container Platform Portal
If you are deploying the Container Platform Portal in an Istio multi-cluster environment configured through this guide, you will need the following presets.

### <span id='6.1'>6.1. Check StorageClass
Make sure each cluster has a resource `StorageClass`.
> If StorageClass is not provided, separate installation is required. <br>
> [NFS Server Installation](../nfs-server-install-guide.md)
```bash
# Check cluster1's StorageClass (separate installation)
$ kubectl get storageclass --context=${CLUSTER1_CONFIG[CTX]}
NAME              PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
cp-storageclass   cp-nfs-provisioner   Delete          Immediate           false                  2m46s

# Check cluster2's StorageClass 
$ kubectl get storageclass --context=${CLUSTER2_CONFIG[CTX]}
NAME                          PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
nks-block-storage (default)   blk.csi.ncloud.com   Delete          WaitForFirstConsumer   true                   5h14m
```

<br>

### <span id='6.2'>6.2. Install Metrics Server
Metrics Server installation is required to collect metrics data from the container platform management cluster.
> [Install Metrics Server](#Install-Metrics-Server) 

<br>

### <span id='6.3'>6.3. Modify the Configuration of a cluster using Cilium CNI
When using Istio in a cluster that uses `Cilium` as CNI, the following configuration changes are required.
> [Cilium’s integration with Istio](https://docs.cilium.io/en/stable/network/servicemesh/istio) 

#### Modify Cilium Configmap
```bash
$ kubectl edit configmaps cilium-config -n kube-system
```
```yaml
# Add
bpf-lb-sock-hostns-only: "true"
cni-exclusive: "false"

# Modify
enable-l7-proxy: "false" 
```

#### Restart Cilium DaemonSet
```bash
# Restart cilium daemonset
$ kubectl rollout restart daemonset cilium -n kube-system
daemonset.apps/cilium restarted

# Check cilium pod status 'running'
$ kubectl get pods -n kube-system -l k8s-app=cilium
NAME           READY   STATUS    RESTARTS   AGE
cilium-b4l8k   1/1     Running   0          4m32s
cilium-sqps5   1/1     Running   0          4m32s
```

<br> 

## <span id='7'>7. Reference

### <span id='7.1'>7.1. Manually configure Istio Ingressgateway EXTERNAL-IP
> KT K2P Standard (K8S) service is an example.

After installing Istio, the initial state of the service istio-ingressgateway is as follows.
- EXTERNAL-IP `pending` status
- Auto-Allocate NodePort
```bash
$ kubectl get svc -n istio-system
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                                                                                                      AGE
istio-ingressgateway   LoadBalancer   10.103.54.27     <pending>     15021:32654/TCP,80:31323/TCP,443:30658/TCP,31400:32070/TCP,15443:31125/TCP,15012:30960/TCP,15017:31546/TCP   14m
istiod                 ClusterIP      10.100.252.167   <none>        15010/TCP,15012/TCP,443/TCP,15014/TCP                                                                        14m   
```

#### <span id='7.1.1'>7.1.1 Create LoadBalancer
Create a LoadBalancer by checking the port and NodPort of the service istio-ingressgateway. <br>
> Access the kt cloud console > Load Balancer > Load Balancer Management > Create Load Balancer

<table>
<thead>
  <tr>
    <th>No</th>
    <th>Service Port</th>
    <th>Service IP</th>
    <th>Zone</th>
    <th>Tier</th>
    <th>Service Type</th>
    <th>Service Options</th>
    <th>Health Check</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td align="center">⑴</td>
    <td align="center">15021</td>
    <td align="center">New IP Allocation</td>
    <td align="center" rowspan="7">Select the same network zone as the cluster node (same concept as VPC)</td>
    <td align="center" rowspan="7">Select the same network tier as the cluster node (same concept as Subnet)</td>
    <td align="center" rowspan="7">TCP</td>
    <td align="center" rowspan="7">Round Robin</td>
    <td align="center" rowspan="7">TCP</td>
  </tr>
  <tr>
    <td align="center">⑵</td>
    <td align="center">80</td>
    <td align="center" rowspan="6">Select the IP generated through ⑴ </td>
  </tr>
  <tr>
    <td align="center">⑶</td>
    <td align="center">443</td>
  </tr>
  <tr>
    <td align="center">⑷</td>
    <td align="center">31400</td>
  </tr>
  <tr>
    <td align="center">⑸</td>
    <td align="center">15443</td>
  </tr>
  <tr>
    <td align="center">⑹</td>
    <td align="center">15012</td>
  </tr>
  <tr>
    <td align="center">⑺</td>
    <td align="center">15017</td>
  </tr>
</tbody>
</table>

###### (Example) Create a LoadBalancer on port `15021`
![image 001]

###### (Example) Create a LoadBalancer on port `80`
![image 003]

###### (Example) List of LoadBalancers that you created
![image 004]

####  Attaching VM
Attach all cluster nodes sequentially to the LoadBalancer you created.
> Access the kt cloud console  > Load Balancer > Load Balancer Management >  `⑴ 15021 ~ ⑺ 15017 All in progress` Select LoadBalancer > Click Connect/Disconnect VM

|Configuration|Explanation|
|---|---|
|Tier|Choose the same network tier as the cluster node|
|VM|Sequentially select cluster nodes|
|Public Port|Enter the NodePort assigned to the port|

![image 002]



<br>

#### <span id='7.1.2'>7.1.2 Create Public IP
Create a Public IP to be assigned to the EXTERNAL-IP of the service istio-ingressgateway.<br>
> Access the kt cloud console  > Servers > Networking > Click the `Create IP` button to create a Public IP

![image 005]

##### Select the created Public IP > Click the 'Static NAT' button > Click the Resource 'Load-Balancer' tab > Select one of the created LoadBalancers
![image 006]

##### Verify LoadBalancer assignment to Static NAT of Public IP
![image 007]

##### Select Public IP > Click 'Firewall Settings' button > Register Firewall Rule
![image 008]


<br>

#### <span id='7.1.3'>7.1.3 Install MetalLB
> **[[MetalLB Installation]](https://metallb.universe.tf/installation/)** load-balancer implementation for bare metal Kubernetes clusters <br>

Install MetalLB for cluster where the EXTERNAL-IP of the service istio-ingressgateway is pending or not Attached as a public IP.
```bash
$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```

##### Create IPAddressPool
> Enter the spec.addresses with the created [Public IP](#7.1.2)
```yaml
cat <<EOF | kubectl apply -f -
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  annotations:
  name: istio-ingressgateway
  namespace: metallb-system
spec:
  addresses:
  - 210.xxx.xxx.xxx/32 # (Replace IP)
  autoAssign: false
  avoidBuggyIPs: false
EOF
```
##### Patch istio-ingressgateway `loadBalancerIP` value
```bash
$ kubectl patch svc istio-ingressgateway -n istio-system -p '{"spec":{"loadBalancerIP":"210.xxx.xxx.xxx"}}' # (Replace IP)
```
```bash
$ kubectl get svc -n istio-system
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP
istio-ingressgateway   LoadBalancer   10.101.94.112    210.xxx.xxx.xxx # (Check Public IP Allocation) ...
```

<br>

### <span id='7.2'>7.2. Troubleshooting Horizontal Pod Autoscaling unknown
If the HorizontalPodAutoscaler Targets value is `unknown` after installing Istio, you need to install Metrics Server.
```bash
$ kubectl get horizontalpodautoscaler.autoscaling -n istio-system
NAME                   REFERENCE                         TARGETS              MINPODS   MAXPODS   REPLICAS
istio-ingressgateway   Deployment/istio-ingressgateway   cpu: <unknown>/80%   1         5         1       
```

#### Install Metrics Server
> [Kubernetes Metrics Server](https://github.com/kubernetes-sigs/metrics-server)
```bash
$ wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
##### Add args `--kubelet-insecure-tls`
```yaml
···
114 apiVersion: apps/v1
115 kind: Deployment
···
132     spec:
133       containers:
134       - args:
            - --kubelet-insecure-tls # (Add)
135         - --cert-dir=/tmp
136         - --secure-port=10250
···
```
##### Deploy Metrics Server
```bash
$ kubectl apply -f components.yaml
```

##### Check Istio Ingressgateway Horizontal Pod Autoscaling
```bash
# Check metric-server pod is running 
$ kubectl get pods -n kube-system -l k8s-app=metrics-server
NAME                            READY   STATUS    RESTARTS   AGE
metrics-server-544865cd-n4cdp   1/1     Running   0          2m55s

# Check kubectl top command
$ kubectl top nodes
NAME                       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
kt-k2p-standard.master01   132m         6%     1833Mi          48%
kt-k2p-standard.worker01   38m          0%     968Mi           6%
kt-k2p-standard.worker02   51m          1%     1062Mi          6%

# Check horizontalpodautoscaler targets values
$ kubectl get horizontalpodautoscaler.autoscaling -n istio-system
NAME                   REFERENCE                         TARGETS               MINPODS   MAXPODS   REPLICAS
istio-ingressgateway   Deployment/istio-ingressgateway   cpu: 4%/80% (Check)   1         5         1        
```

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > CSP Kubernetes Service Istio Multi Cluster Installation Guide


[image 001]:../images/csp/csp-001.png
[image 002]:../images/csp/csp-002.png
[image 003]:../images/csp/csp-003.png
[image 004]:../images/csp/csp-004.png
[image 005]:../images/csp/csp-005.png
[image 006]:../images/csp/csp-006.png
[image 007]:../images/csp/csp-007.png
[image 008]:../images/csp/csp-008.png
