### [Index](https://github.com/K-PaaS/container-platform/blob/master/README.md) > [CP Install](/install-guide/Readme.md)  > K-PaaS Container Platform Linkerd Multi-Cluster Installation Guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
   1.1. [Purpose](#1.1)<br>
   1.2. [Range](#1.2)<br>
   1.3. [System Configuration](#1.3)<br>
   1.4. [Reference](#1.4)

2. [Prerequisite](#2)<br>
   2.1. [Installation List](#2.1)<br>
   2.2. [Firewall Information](#2.2)<br>
   2.3. [Configure Contexts](#2.3)<br>
   2.4. [Check Helm Installation](#2.4)<br>
   2.5. [Check MetalLB Installation](#2.5)<br>
   2.6. [Set Environment Variables](#2.6)<br>
   2.7. [Install Linkerd CLI, step](#2.7)

3. [Install Linkerd Multi-Cluster](#3)<br>
   3.1. [Create Certificate](#3.1)<br>
   3.2. [Install linkerd-crds](#3.2)<br>
   3.3. [Install linkerd-control-plane](#3.3)<br>
   3.4. [Install linkerd-multicluster](#3.4)<br>
   3.5. [Add Iptables Policy](#3.5)<br>
   3.6. [Install linkerd-smi](#3.6)

4. [Sample Application](#4)<br>
   4.1. [Deploy the sample application to cluster cluster1](#4.1)<br>
   4.2. [Deploy the sample application to cluster cluster2](#4.2)<br>
   4.3. [Verifying Cross-Cluster Traffic](#4.3)


<br>

## <span id='1'> 1. Document Overview

### <span id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Linkerd Multi-Cluster Installation Guide) installs Linkerd, a service mesh tool for Kubernetes, on a cluster on the K-PaaS container platform, an open PaaS platform with a support environment for planners, developers, and operators, to enable communication between services in the two clusters.

<br>

### <span id='1.2'> 1.2. Range
The installation scope was written to install two clusters of the K-PaaS container platform environment based on a `single cloud` and then configure a `multi-cluster` using Linkerd.

<br>

### <span id='1.3'> 1.3. System Configuration
Configure two Kubernetes `single clusters` through K-PaaS container platform Deployment and install Linkerd to configure a `multi-cluster` environment.

<br>

### <span id='1.4'> 1.4. Reference
> [[Linkerd Docs]](https://linkerd.io/2.14/overview/) <br>
> [[Linkerd Installing Multi-cluster Components]](https://linkerd.io/2.14/tasks/installing-multicluster/) <br>
> [[Linkerd Multi-cluster communication]](https://linkerd.io/2.14/features/multicluster/)

<br>

## <span id='2'> 2. Prerequisite
- Configure Linked Multi cluster for two single container platform clusters.
  + [[K-PaaS Container Platform Cluster Installation Guide]](/install-guide/standalone/cp-cluster-install-single.md)
- The task instance for Linkerd multi-cluster configuration is run on the **Master Node** of `cluster1`.

<br>

### <span id='2.1'>2.1. Installation List
The list of installed tools is as follows.
#### Linkerd
> [Linkerd Releases and Versions](https://linkerd.io/releases/) <br>
> [Linkerd Extension List](https://linkerd.io/2.14/reference/extension-list/)

|NAME|NAMESPACE|Extensions|
|:---|:---|:---|
|<b>[linkerd-crds](https://artifacthub.io/packages/helm/linkerd2/linkerd-crds)</b>|-|-|
|<b>[linkerd-control-plane](https://artifacthub.io/packages/helm/linkerd2/linkerd-control-plane)</b>|linkerd|-|
|<b>[linkerd-multicluster](https://artifacthub.io/packages/helm/linkerd2/linkerd-multicluster)</b>|linkerd-multicluster|O|
|<b>[linkerd-smi](https://github.com/linkerd/linkerd-smi)</b>|linkerd-smi|O|

#### step ([`v0.24.4`](https://github.com/smallstep/cli/releases/tag/v0.24.4))
> [step CLI](https://smallstep.com/docs/step-cli/index.html)


<br>

### <span id='2.2'>2.2. Firewall Information
Set the port that should be opened for the IaaS Security Group.
|Protocol|Port|Description|
|---|---|---|
|TCP|4191|Linkerd Gateway Probe|
|TCP|4143|Linkerd Gateway|
|TCP|5000|Linkerd Sample App|

<br>

### <span id='2.3'>2.3. Configure Contexts
> The task instance for Linkerd multi-cluster configuration is run on the **master node** in `cluster1`.

Context configuration is required to access cluster 1 and cluster 2 where Linkerd multi-clusters will be installed. <br>
:loudspeaker: Ensure that cluster, context, and user are not duplicated in each cluster kubeconfig file. <br>
```bash
# Create and Move .kube Dir
$ cd ${HOME}/.kube

# Place each cluster kubeconfig file
$ ls ${HOME}/.kube
cluster1-config  cluster2-config ...

# Set the kubeconfig file path
$ export KUBECONFIG="${HOME}/.kube/cluster1-config:${HOME}/.kube/cluster2-config"
```
-  Check the output of 'NAME', 'CLUSTER', and 'AUTHINFO' values normally
```bash
$ kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         cluster1   cluster1   cluster1
          cluster2   cluster2   cluster2
```
- Verify the get cluster resource
```bash
# Get nodes in cluster1
$ kubectl get nodes --context=cluster1
NAME              STATUS   ROLES           AGE     VERSION
cluster1-node-1   Ready    control-plane   4h25m   v1.27.5
cluster1-node-2   Ready    <none>          4h24m   v1.27.5
cluster1-node-3   Ready    <none>          4h24m   v1.27.5

# Get nodes in cluster2
$ kubectl get nodes --context=cluster2
NAME              STATUS   ROLES           AGE    VERSION
cluster2-node-1   Ready    control-plane   3h6m   v1.27.5
cluster2-node-2   Ready    <none>          3h5m   v1.27.5
cluster2-node-3   Ready    <none>          3h5m   v1.27.5
```

<br>

### <span id='2.4'>2.4. Check Helm Installation
K-PaaS container platform cluster provides Helm installation by default.<br> Make sure the Helm CLI is working properly.
```bash
$ helm version
version.BuildInfo{Version:"v3.12.3", ...}
```

<br>

### <span id='2.5'> 2.5. Check MetalLB Installation
K-PaaS container platform cluster provides MetalLB installation by default.<br> Check the installation of MetalLB in cluster1, cluster2.
```bash
# Check the pods of MetalLB in cluster1
$ kubectl get pods -n metallb-system --context=cluster1
NAME                          READY   STATUS    RESTARTS   AGE
controller-58cd4b5d45-xgbs6   1/1     Running   0          4h8m
speaker-26qjf                 1/1     Running   0          4h28m
speaker-69nm5                 1/1     Running   0          4h28m
speaker-7wxrx                 1/1     Running   0          4h5m

# Check the pods of MetalLB in cluster2
$ kubectl get pods -n metallb-system  --context=cluster2
NAME                          READY   STATUS    RESTARTS   AGE
controller-58cd4b5d45-rqdzh   1/1     Running   0          3h7m
speaker-d5kdz                 1/1     Running   0          3h7m
speaker-s62z9                 1/1     Running   0          3h7m
speaker-sswws                 1/1     Running   0          3h7m
```

<br>

### <span id='2.6'>2.6. Set Environment Variables
Set environmental variables such as cluster information and CLI for Linkerd multi-cluster installation.
- Set the cluster context, name, and STEP CLI version
```bash
export CLUSTER1_CTX="{cluster1 Context Name}"  #(e.g. cluster1)
export CLUSTER1_NAME="{cluster1 name to use when connecting to Linked Multicluster}" #(e.g. cluster1)
export CLUSTER2_CTX="{cluster2 Context Name}"  #(e.g. cluster2)
export CLUSTER2_NAME="{cluster2 name to use when connecting to Linked Multicluster}" #(e.g. cluster2)
export STEP_VERSION="0.24.4"
```

<br>

### <span id='2.7'>2.7. Install Linkerd CLI, step
Install the Linkerd CLI to interact with Linkerd and the steps to generate certificates.
- Install Linkerd CLI
```bash
# Install Linkerd CLI
$ curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install | sh -

Download complete!

Validating checksum...
Checksum valid.

Linkerd stable-2.x was successfully installed 🎉
...
```
```bash
$ sudo mv $HOME/.linkerd2/bin/linkerd /usr/local/bin/linkerd
$ linkerd version --client
Client version: stable-2.x
```
- Install step
```bash
# Install step
$ wget https://dl.smallstep.com/gh-release/cli/docs-cli-install/v${STEP_VERSION}/step-cli_${STEP_VERSION}_amd64.deb
$ sudo dpkg -i step-cli_${STEP_VERSION}_amd64.deb
$ step version
Smallstep CLI/0.24.4 (linux/amd64)
Release Date: 2023-05-11T19:52:34Z
```
<br>

## <span id='3'>3. Install Linkerd Multi-Cluster
### <span id='3.1'>3.1. Create Certificate
To support [mTLS connections](https://linkerd.io/2.14/tasks/generate-certificates/) between service mesh Pods, Linkerd requires a trust anchor certificate and an issuer certificate containing the corresponding key.
Generate the certificate through the pre-installed step.
```bash
# Create dir
$ mkdir -p $HOME/linkerd/certs
$ cd $HOME/linkerd/certs

# Create a trust anchor certificate
$ step certificate create root.linkerd.cluster.local ca.crt ca.key \
--profile root-ca --no-password --insecure

# Create issuer certificate and key
$ step certificate create identity.linkerd.cluster.local issuer.crt issuer.key \
--profile intermediate-ca --not-after 8760h --no-password --insecure \
--ca ca.crt --ca-key ca.key

# Verify certificate and key creation
$ ls
ca.crt  ca.key  issuer.crt  issuer.key
```
<br>

### <span id='3.2'>3.2. Install linkerd-crds
Install `linkerd-crds` via Helm.
```bash
# Add linkerd repo
$ helm repo add linkerd https://helm.linkerd.io/stable

$ helm repo list
NAME    URL
linkerd https://helm.linkerd.io/stable (Check add)

# Install linkerd-crds on cluster1
$ helm install linkerd-crds linkerd/linkerd-crds -n linkerd --create-namespace --kube-context="${CLUSTER1_CTX}"

# Install linkerd-crds on cluster2
$ helm install linkerd-crds linkerd/linkerd-crds -n linkerd --create-namespace --kube-context="${CLUSTER2_CTX}"
```

<br> 

### <span id='3.3'>3.3. Install linkerd-control-plane
- Install linkerd-control-plane along with the certificate and key created in [3.1. Create Certificate](#3.1).

```bash
# Go to the certificate/key file location
$ cd $HOME/linkerd/certs

# Install linkerd-control-plane on cluster1
$ helm install linkerd-control-plane linkerd/linkerd-control-plane -n linkerd \
  --set-file identityTrustAnchorsPEM=ca.crt \
  --set-file identity.issuer.tls.crtPEM=issuer.crt \
  --set-file identity.issuer.tls.keyPEM=issuer.key \
  --kube-context="${CLUSTER1_CTX}"

# Install linkerd-control-plane on cluster2
$ helm install linkerd-control-plane linkerd/linkerd-control-plane -n linkerd \
  --set-file identityTrustAnchorsPEM=ca.crt \
  --set-file identity.issuer.tls.crtPEM=issuer.crt \
  --set-file identity.issuer.tls.keyPEM=issuer.key \
  --kube-context="${CLUSTER2_CTX}"
```

<br>

### <span id='3.4'>3.4. Install linkerd-multicluster
Install `linkerd-multicluster`.
```bash
# Install linkerd-multicluster on cluster1
$ helm install linkerd-multicluster linkerd/linkerd-multicluster -n linkerd-multicluster --create-namespace \
--kube-context="${CLUSTER1_CTX}"

# Install linkerd-multicluster on cluster2
$ helm install linkerd-multicluster linkerd/linkerd-multicluster -n linkerd-multicluster --create-namespace \
--kube-context="${CLUSTER2_CTX}"
```
<br>

After deployment, check if the EXTERNAL-IP of the `linkerd-gateway` service in each cluster is assigned.
```bash
# Verify linkerd-gateway EXTERNAL-IP assignment of cluster1
$ kubectl get svc linkerd-gateway -n linkerd-multicluster --context="${CLUSTER1_CTX}"
NAME              TYPE           CLUSTER-IP     EXTERNAL-IP             PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.233.17.96   192.xx.xx.xx (Check)    4143:31134/TCP,4191:32460/TCP   6m

# Verify linkerd-gateway EXTERNAL-IP assignment of cluster2
$ kubectl get svc linkerd-gateway -n linkerd-multicluster --context="${CLUSTER2_CTX}"
NAME              TYPE           CLUSTER-IP    EXTERNAL-IP            PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.233.7.15   172.xx.xx.xx (Check)   4143:31114/TCP,4191:31573/TCP   6m
```

<br>

#### Set up link for cluster 1 and cluster 2 to connect to each other
Linkerd multi-cluster communication works by <b>'mirroring'</b> service information between clusters. By establishing a link between clusters, the credentials and service mirror components of that cluster are configured on the other cluster.
```bash
# After extracting credentials from cluster1, create resources in cluster2.
linkerd multicluster link --context="${CLUSTER1_CTX}" --cluster-name "${CLUSTER1_NAME}" \
| kubectl --context="${CLUSTER2_CTX}" apply -f -

# After extracting credentials from cluster2, create resources in cluster1
linkerd multicluster link --context="${CLUSTER2_CTX}" --cluster-name "${CLUSTER2_NAME}" \
| kubectl --context="${CLUSTER1_CTX}" apply -f -
```

```bash
# List of resources created
secret/cluster-credentials-cluster1 created (linkerd)
secret/cluster-credentials-cluster1 created (linkerd-multicluster)
link.multicluster.linkerd.io/cluster1 created
clusterrole.rbac.authorization.k8s.io/linkerd-service-mirror-access-local-resources-cluster1 created
clusterrolebinding.rbac.authorization.k8s.io/linkerd-service-mirror-access-local-resources-cluster1 created
role.rbac.authorization.k8s.io/linkerd-service-mirror-read-remote-creds-cluster1 created
rolebinding.rbac.authorization.k8s.io/linkerd-service-mirror-read-remote-creds-cluster1 created
serviceaccount/linkerd-service-mirror-cluster1 created
deployment.apps/linkerd-service-mirror-cluster1 created
service/probe-gateway-cluster1 created
```
<br>

#### linkerd-multicluster resource status
```bash
# Check the linkerd-multicluster resource status of cluster1
$ kubectl get all -n linkerd-multicluster --context="${CLUSTER1_CTX}"
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/linkerd-gateway-847b97dd4c-djmx2                   2/2     Running   0          12m
pod/linkerd-service-mirror-cluster2-846b6cbc66-7kkql   2/2     Running   0          51s

NAME                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                         AGE
service/linkerd-gateway          LoadBalancer   10.233.17.96   192.xx.xx.xx   4143:31134/TCP,4191:32460/TCP   12m
service/probe-gateway-cluster2   ClusterIP      10.233.28.44   <none>         4191/TCP                        51s

NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/linkerd-gateway                   1/1     1            1           12m
deployment.apps/linkerd-service-mirror-cluster2   1/1     1            1           51s

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/linkerd-gateway-847b97dd4c                   1         1         1       12m
replicaset.apps/linkerd-service-mirror-cluster2-846b6cbc66   1         1         1       51s
```
```bash
# Check the linkerd-multicluster resource status of cluster2
$ kubectl get all -n linkerd-multicluster --context="${CLUSTER2_CTX}"
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/linkerd-gateway-847b97dd4c-m5zw7                   2/2     Running   0          12m
pod/linkerd-service-mirror-cluster1-84cf9df486-hpnhc   2/2     Running   0          4m39s

NAME                             TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                         AGE
service/linkerd-gateway          LoadBalancer   10.233.7.15   172.xx.xx.xx   4143:31114/TCP,4191:31573/TCP   12m
service/probe-gateway-cluster1   ClusterIP      10.233.9.15   <none>         4191/TCP                        4m40s

NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/linkerd-gateway                   1/1     1            1           12m
deployment.apps/linkerd-service-mirror-cluster1   1/1     1            1           4m40s

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/linkerd-gateway-847b97dd4c                   1         1         1       12m
replicaset.apps/linkerd-service-mirror-cluster1-84cf9df486   1         1         1       4m40s
```
<br>

### <span id='3.5'>3.5. Add Iptables Policy
The EXTERNAL-IP of the service `linkerd-gateway` assigned through MetalLB is the network subnet band Private IP of each cluster node, and it is necessary to add an iptables policy to enable service communication between two independent clusters.

```bash
# Check the linkerd-gateway EXTERNAL-IP of cluster1
$ kubectl get svc linkerd-gateway -n linkerd-multicluster --context="${CLUSTER1_CTX}"
NAME              TYPE           CLUSTER-IP     EXTERNAL-IP            PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.233.17.96   192.xx.xx.xx (Check)   4143:31134/TCP,4191:32460/TCP   16m

# Check the linkerd-gateway EXTERNAL-IP of cluster2
$ kubectl get svc linkerd-gateway -n linkerd-multicluster --context="${CLUSTER2_CTX}"
NAME              TYPE           CLUSTER-IP    EXTERNAL-IP           PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.233.7.15   172.xx.xx.xx (Check)  4143:31114/TCP,4191:31573/TCP   16m
```

#### Create interface and assign floating IP
Refer to the guide below to create a new interface for the EXTERNAL-IP of `linkerd-gateway` of each cluster and connect it to the floating IP.
> [[Set Kubernetes Service External IP]](/install-guide/standalone/cp-cluster-install-single.md#2.1.6)

<br>

#### Add Iptables Policy
**PREROUTING** is set up so that the `linked-gateway` EXTERNAL-IP of the counterpart cluster is changed to the floating IP connected to all nodes of each cluster so that it can be communicated.
```bash
# * Proceed on all nodes of cluster1
## Add Interface PREROUTING Settings for Cluster2
$ sudo iptables -t nat -I PREROUTING -d {CLUSTER2_LINKERD_GATEWAY_EXTERNAL_PRIVATE_IP} -j DNAT --to-destination {CLUSTER2_EXTERNAL_PUBLIC_IP}

## Check PREROUTING settings
$ sudo iptables -nL PREROUTING -t nat
Chain PREROUTING (policy ACCEPT)
target     prot     opt                  source               destination
DNAT       all  --  0.0.0.0/0            172.xx.xx.xx         to:103.xxx.xxx.xxx
```

```bash
# * Proceed on all nodes of cluster2
## Add Interface PREROUTING Settings for Cluster1
$ sudo iptables -t nat -I PREROUTING -d {CLUSTER1_LINKERD_GATEWAY_EXTERNAL_PRIVATE_IP} -j DNAT --to-destination {CLUSTER1_EXTERNAL_PUBLIC_IP}

## Check PREROUTING settings
$ sudo iptables -nL PREROUTING -t nat
Chain PREROUTING (policy ACCEPT)
target     prot     opt                  source               destination
DNAT       all  --  0.0.0.0/0            192.xx.xx.xx         to:180.xxx.xxx.xxx
```

<br>

#### Check multi-cluster connection status
In each cluster, check the connection status with the other cluster.
```bash
# Check cluster1 -> cluster2 connection status
$ linkerd multicluster check --context="${CLUSTER1_CTX}"
linkerd-multicluster
--------------------
√ Link CRD exists
√ Link resources are valid
        * cluster2
√ remote cluster access credentials are valid
        * cluster2
√ clusters share trust anchors
        * cluster2
√ service mirror controller has required permissions
        * cluster2
√ service mirror controllers are running
        * cluster2
√ probe services able to communicate with all gateway mirrors
        * cluster2
√ multicluster extension proxies are healthy
√ multicluster extension proxies are up-to-date
√ multicluster extension proxies and cli versions match

Status check results are √
```
```bash
# Check cluster2 -> cluster1 connection status
$ linkerd multicluster check --context="${CLUSTER2_CTX}"
linkerd-multicluster
--------------------
√ Link CRD exists
√ Link resources are valid
        * cluster1
√ remote cluster access credentials are valid
        * cluster1
√ clusters share trust anchors
        * cluster1
√ service mirror controller has required permissions
        * cluster1
√ service mirror controllers are running
        * cluster1
√ probe services able to communicate with all gateway mirrors
        * cluster1
√ multicluster extension proxies are healthy
√ multicluster extension proxies are up-to-date
√ multicluster extension proxies and cli versions match

Status check results are √
```
<br>

### <span id='3.6'>3.6. Install linkerd-smi
Install the `linkerd-smi` extension to use the Linkerd CRD `TrafficSplit` for traffic splitting. <br>

```bash
# Add linkerd-smi extension repo
$ helm repo add l5d-smi https://linkerd.github.io/linkerd-smi

$ helm repo list
NAME    URL
linkerd https://helm.linkerd.io/stable
l5d-smi https://linkerd.github.io/linkerd-smi (Check Add)

# Install linkerd-smi on cluster1
helm install linkerd-smi l5d-smi/linkerd-smi -n linkerd-smi --create-namespace \
--kube-context="${CLUSTER1_CTX}"

# Install linkerd-smi on cluster2
helm install linkerd-smi l5d-smi/linkerd-smi -n linkerd-smi --create-namespace \
--kube-context="${CLUSTER2_CTX}"
```

<br>

## <span id='4'>4. Sample Application
Deploy the multi-cluster communication sample application to clusters cluster1 and cluster2.

#### Where to deploy the application
| Application| Cluster1 | Cluster2 |Information |
| :---: |:---: | :---: | :---: |  
| helloworld-v1 | :heavy_check_mark: | | Returns its version (v1) and hostname |
| helloworld-v2 |  | :heavy_check_mark: |Returns its version (v2) and hostname |
| sleep |  | :heavy_check_mark: | Use as a request source to call other services|


```bash
# Create sample dir
$ mkdir -p $HOME/linkerd/sample
$ cd $HOME/linkerd/sample

# Download gen-helloworld.sh
$ wget https://raw.githubusercontent.com/istio/istio/master/samples/helloworld/gen-helloworld.sh
$ chmod +x gen-helloworld.sh

# Download sleep.yaml
$ wget https://raw.githubusercontent.com/istio/istio/master/samples/sleep/sleep.yaml

# Create sample namespace in cluster1 and add linkerd-proxy injection annotation
$ kubectl create namespace sample --context="${CLUSTER1_CTX}"
$ kubectl annotate namespace sample "linkerd.io/inject=enabled" --context="${CLUSTER1_CTX}"

# Create sample namespace in cluster2 and add linkerd-proxy injection annotation
$ kubectl create namespace sample --context="${CLUSTER2_CTX}"
$ kubectl annotate namespace sample "linkerd.io/inject=enabled" --context="${CLUSTER2_CTX}"
```

<br>

### <span id='4.1'>4.1. Deploy the sample application to cluster cluster1
```bash
# Deploy helloworld-v1 application to cluster1
$ ./gen-helloworld.sh --version v1 --includeService true --includeDeployment true  | \
    kubectl --context="${CLUSTER1_CTX}" -n sample apply -f -
```
```bash
# Created helloworld-v1's resource
service/helloworld created
deployment.apps/helloworld-v1 created
```

<br>

#### Mirroring services to cluster cluster2
Mirror the service `helloworld` created in cluster cluster1 to cluster cluster2.
> Add label `mirror.linkerd.io/exported=true`
```bash
$ kubectl label svc helloworld mirror.linkerd.io/exported=true --context="${CLUSTER1_CTX}" -n sample
```

<br>

Verify that the cluster cluster1 service `helloworld` is mirrored on cluster cluster2.
```bash
# Check if the service `helloworld-{cluster1 name}` is created in cluster2
$ kubectl get svc --context="${CLUSTER2_CTX}" -n sample
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
helloworld-cluster1   ClusterIP   10.254.83.67   <none>        5000/TCP   3m11s
```

<br>

### <span id='4.2'>4.2. Deploy the sample application to cluster cluster2
```bash
# Deploy helloworld-v2 application to cluster2
$ ./gen-helloworld.sh --version v2 --includeService true --includeDeployment true  | \
    kubectl --context="${CLUSTER2_CTX}" -n sample apply -f -

# Deploy sleep application to cluster2
$ kubectl --context="${CLUSTER2_CTX}" -n sample apply -f sleep.yaml
```
```bash
# Created helloworld-v2's resource
service/helloworld created
deployment.apps/helloworld-v2 created

# Created sleep's resource
serviceaccount/sleep created
service/sleep created
deployment.apps/sleep created
```
<br>

#### Deploy `TrafficSplit`
Deploy a resource `TrafficSplit` that allows you to specify the traffic ratio between services. <br>
Set up routing of 50% of traffic coming into service helloworld to `helloworld-cluster1` and 50% to `helloworld`.
```yaml
cat << EOF | kubectl apply --context="${CLUSTER2_CTX}" -n sample -f -
apiVersion: split.smi-spec.io/v1alpha1
kind: TrafficSplit
metadata:
  name: helloworld
spec:
  service: helloworld
  backends:
  - service: helloworld
    weight: 50
  - service: helloworld-cluster1
    weight: 50
EOF
```

<br>

### <span id='4.3'>4.3. Verifying Cross-Cluster Traffic
Through <b>TrafficSplit</b>, you can see that traffic is split and communicated between cluster1's `helloworld-v1` and cluster2's `helloworld-v2`.
```bash
# Check helloword-v1 pod in cluster1
$ kubectl get pods -l app=helloworld --context="${CLUSTER1_CTX}" -n sample
NAME                             READY   STATUS    RESTARTS   AGE
helloworld-v1-665db7d464-lcl75   2/2     Running   0          3m29s

# Check helloworld-v2 pod in cluster2
$ kubectl get pods -l app=helloworld --context="${CLUSTER2_CTX}" -n sample
NAME                            READY   STATUS    RESTARTS   AGE
helloworld-v2-f58d4c8cb-mzpsb   2/2     Running   0          3m3s
```

```bash
# Access sleep pod of cluster2
$ kubectl exec -it -n sample --context="${CLUSTER2_CTX}" \
"$(kubectl get pod -n sample --context="${CLUSTER2_CTX}" -l app=sleep -o jsonpath='{.items[0].metadata.name}')" -c sleep sh

# curl helloworld service
~ $  curl helloworld:5000/hello
Hello version: v1, instance: helloworld-v1-665db7d464-lcl75 (version v1, helloword-v1 pod name of cluster1)
~ $  curl helloworld:5000/hello
Hello version: v2, instance: helloworld-v2-f58d4c8cb-mzpsb  (version v2, helloword-v2 pod name of cluster2)
~ $  curl helloworld:5000/hello
Hello version: v1, instance: helloworld-v1-665db7d464-lcl75
~ $  curl helloworld:5000/hello
Hello version: v1, instance: helloworld-v1-665db7d464-lcl75
~ $  curl helloworld:5000/hello
Hello version: v2, instance: helloworld-v2-f58d4c8cb-mzpsb
~ $  curl helloworld:5000/hello
Hello version: v1, instance: helloworld-v1-665db7d464-lcl75
~ $  curl helloworld:5000/hello
Hello version: v2, instance: helloworld-v2-f58d4c8cb-mzpsb
~ $  curl helloworld:5000/hello
Hello version: v2, instance: helloworld-v2-f58d4c8cb-mzpsb
```

<br>

### [Index](https://github.com/K-PaaS/container-platform/blob/master/README.md) > [CP Install](/install-guide/Readme.md)  > K-PaaS Container Platform Linkerd Multi-Cluster Installation Guide
