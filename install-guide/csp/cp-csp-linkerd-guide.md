### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > CSP 쿠버네티스 서비스 Linkerd 멀티 클러스터 설치 가이드

<br>

## Table of Contents
1. [Document Overview](#1)<br>
   1.1. [Purpose](#1.1)<br>
   1.2. [Scope](#1.2)<br>
   1.3. [System configuration diagram](#1.3)<br>
   1.4. [Reference Material](#1.4)<br>

2. [Prerequisite](#2)<br>
   2.1. [Installation list](#2.1)<br>
   2.2. [About firewalls](#2.2)<br>
   2.3. [Verify CLI installation](#2.3)<br>
   2.4. [Configure multi-cluster access](#2.4)<br>
   2.5. [Setting environment variables](#2.5)<br>
   2.6. [Install Linkerd CLI, step](#2.6)

3. [Installing Linkerd Multi-Cluster](#3)<br>
   3.1. [Generate a certificate](#3.1)<br>
   3.2. [Install linkerd-crds](#3.2)<br>
   3.3. [Install linkerd-control-plane](#3.3)<br>
   3.4. [Install linkerd-multicluster](#3.4)<br>
   3.5. [Install linkerd-smi](#3.5)

4. [Deploy the sample application](#4)<br>
   4.1. [Deploy the sample application to the cluster cluster1](#4.1)<br>
   4.2. [Deploy the sample application on cluster cluster2](#4.2)<br>
   4.3. [Testing multi-cluster communication](#4.3)

<br>

## <span id='1'> 1. Document Overview

### <span id='1.1'> 1.1. Purpose
This document (CSP Kubernetes Service Linkerd Multi-Cluster Installation Guide) describes how to install Linkerd Multi-Cluster on a cloud service provider's Kubernetes service to enable communication between services on two independent Kubernetes clusters.

<br>

### <span id='1.2'> 1.2. Scope
The installation scope was written to configure a "multi-cluster" using Linkerd against a "CSP Kubernetes Service" cluster.

<br>

### <span id='1.3'> 1.3. System configuration diagram
Configure two clusters of the "CSP Kubernetes Service" and install Linkerd to create a "multi-cluster" environment.

<br>

### <span id='1.4'> 1.4. Reference Material
> [[Linkerd Docs]](https://linkerd.io/2.14/overview/) <br>
> [[Linkerd Installing Multi-cluster Components]](https://linkerd.io/2.14/tasks/installing-multicluster/) <br>
> [[Linkerd Multi-cluster communication]](https://linkerd.io/2.14/features/multicluster/)

<br>

## <span id='2'> 2. Prerequisite
- Configure a Linkerd multi-cluster for **two clusters**.
- Each cluster needs to be supported by a service of type **LoadBalancer**.

<br>

### <span id='2.1'>2.1. Installation list
The list of tools that are installed is shown below
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

### <span id='2.2'>2.2. About firewalls
Set the port that should be open for the IaaS Security Group.
|Protocols|Ports|Note|
|---|---|---|
|TCP|4191|Linkerd Gateway Probe|
|TCP|4143|Linkerd Gateway|
|TCP|5000|Linkerd Sample App|

<br>

### <span id='2.3'>2.3. Verify CLI installation
Check whether you have installed the CLIs `kubectl` and `Helm`, which are required for Linkerd multi-cluster installation.
> [Install kubectl](https://kubernetes.io/ko/docs/tasks/tools/#kubectl) <br>
> [Installing Helm](https://helm.sh/ko/docs/intro/install/)
```bash
# Check kubectl version (requires using a kubectl version that is within a minor version difference of the target cluster)
$ kubectl version --client
Client Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.3", ...}

# Check your Helm version
$ helm version
version.BuildInfo{Version:"v3.12.3", ...}
```
<br>

### <span id='2.4'>2.4. Configure multi-cluster access
We need to configure the context so that we can access the clusters cluster1, cluster2 where we will install Linkerd Multi-Cluster. <br>
:loudspeaker: Ensure that the cluster, context, and user names in the two kubeconfig files cluster1, cluster2 do not duplicate. <br>
```bash
# Create a directory .kube
$ mkdir -p ${HOME}/.kube

# cluster1, cluster2 kubeconfig file locations
$ ls ${HOME}/.kube
cluster1-config  cluster2-config

# Set the path to the kubeconfig file
$ export KUBECONFIG="${HOME}/.kube/cluster1-config:${HOME}/.kube/cluster2-config"
```
- Verify context list lookup is working
```bash
$ kubectl config get-contexts
CURRENT   NAME    CLUSTER    AUTHINFO         NAMESPACE
*         ctx-1   cluster1   cluster1-admin
          ctx-2   cluster2   cluster2-admin
```
- Verify cluster access is healthy
```bash
# Get cluster1 nodes
$ kubectl get nodes --context=ctx-1
NAME                                  STATUS   ROLES    AGE     VERSION
k8s-cluster-1-default-worker-node-0   Ready    <none>   5h44m   v1.27.3

# Get cluster2 nodes
$ kubectl get nodes --context=ctx-2
NAME                                  STATUS   ROLES    AGE     VERSION
k8s-cluster-2-default-worker-node-0   Ready    <none>   5h43m   v1.27.3
```

<br>

### <span id='2.5'>2.5. Setting environment variables
Set up environment variables for your Linkerd multi-cluster installation, including cluster information, CLI, etc.
- Set cluster name, context information, and STEP version
```bash
export CLUSTER1_CTX="{cluster1 Context name}"  #(e.g. ctx-1)
export CLUSTER1_NAME="{cluster1 name to use for Linkerd multi-cluster connections}" #(e.g. cluster1)
export CLUSTER2_CTX="{cluster2 Context name}"  #(e.g. ctx-2)
export CLUSTER2_NAME="{cluster2 name to use for Linkerd multi-cluster connections}" #(e.g. cluster2)
export STEP_VERSION="0.24.4"
```

<br>

### <span id='2.6'>2.6. Install Linkerd CLI, step
Install the Linkerd CLI for interacting with Linkerd, and step for certificate generation.
- Installing the Linkerd CLI
```bash
# Installing the Linkerd CLI
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

## <span id='3'>3. Installing Linkerd Multi-Cluster
### <span id='3.1'>3.1. Generate a certificate
To support [mTLS communication](https://linkerd.io/2.14/tasks/generate-certificates/) between service-meshed Pods, Linkerd requires a trust anchor certificate and an issuer certificate with the corresponding key.<br>
Proceed with certificate generation through the pre-installed steps.
```bash
# Create a directory
$ mkdir -p $HOME/linkerd/certs
$ cd $HOME/linkerd/certs

# Generate a trust anchor certificate
$ step certificate create root.linkerd.cluster.local ca.crt ca.key \
--profile root-ca --no-password --insecure

# Generate an issuer certificate and key
$ step certificate create identity.linkerd.cluster.local issuer.crt issuer.key \
--profile intermediate-ca --not-after 8760h --no-password --insecure \
--ca ca.crt --ca-key ca.key

# Confirm certificate and key generation
$ ls
ca.crt  ca.key  issuer.crt  issuer.key
```

<br>

### <span id='3.2'>3.2. Install linkerd-crds
Install `linkerd-crds` via Helm.
```bash
# Registering a linkerd repo
$ helm repo add linkerd https://helm.linkerd.io/stable

$ helm repo list
NAME    URL
linkerd https://helm.linkerd.io/stable (Confirm registration)

# Install linkerd-crds on cluster1
$ helm install linkerd-crds linkerd/linkerd-crds -n linkerd --create-namespace --kube-context="${CLUSTER1_CTX}"

# Install linkerd-crds on cluster2
$ helm install linkerd-crds linkerd/linkerd-crds -n linkerd --create-namespace --kube-context="${CLUSTER2_CTX}"
```

<br> 

### <span id='3.3'>3.3. Install linkerd-control-plane
- Install `linkerd-control-plane` with the certificate and key created in [[3.1. Generate a certificate]](#3.1).

```bash
# Navigate to the certificate, key file location
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

After deployment, verify that the EXTERNAL-IP of the `linkerd-gateway` service in each cluster is assigned.
```bash
# cluster1의 linkerd-gateway에 EXTERNAL-IP 할당 확인 
$ kubectl get svc -n linkerd-multicluster --context="${CLUSTER1_CTX}"
NAME              TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.254.93.121   xxx.xxx.xxx.xxx   4143:30926/TCP,4191:32314/TCP   78s

# cluster2의 linkerd-gateway에 EXTERNAL-IP 할당 확인
$ kubectl get svc -n linkerd-multicluster --context="${CLUSTER2_CTX}"
NAME              TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                         AGE
linkerd-gateway   LoadBalancer   10.254.62.203   xxx.xxx.xxx.xxx   4143:30964/TCP,4191:32367/TCP   51s
```

<br>

#### Set up link so that clusters cluster1 and cluster2 are connected to each other
Linkerd multi-cluster communication works by <b>'mirroring'</b> service information between clusters. By setting up a LINK between clusters, you pass the credentials and service mirror components of those clusters to the
the other cluster's credentials and service mirror components.
```bash
# Extract credentials from cluster1 and create a resource on cluster2
linkerd multicluster link --context="${CLUSTER1_CTX}" --cluster-name "${CLUSTER1_NAME}" \
| kubectl --context="${CLUSTER2_CTX}" apply -f -

# Extract credentials for cluster2 and create resource on cluster1
linkerd multicluster link --context="${CLUSTER2_CTX}" --cluster-name "${CLUSTER2_NAME}" \
| kubectl --context="${CLUSTER1_CTX}" apply -f -
```

```bash
# List of resources generated
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

#### linkerd-multicluster resource deployment status
```bash
# Verify linkerd-multicluster resource deployment on cluster1
$ kubectl get all -n linkerd-multicluster --context="${CLUSTER1_CTX}"
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/linkerd-gateway-58ccc85959-fsgjh                   2/2     Running   0          3m3s
pod/linkerd-service-mirror-cluster2-7ddc698cc8-7szwv   2/2     Running   0          98s

NAME                             TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)                         AGE
service/linkerd-gateway          LoadBalancer   198.19.154.231   xxx.xxx.xxx.xxx   4143:30903/TCP,4191:32408/TCP   3m3s
service/probe-gateway-cluster2   ClusterIP      198.19.240.220   <none>            4191/TCP                        98s

NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/linkerd-gateway                   1/1     1            1           3m3s
deployment.apps/linkerd-service-mirror-cluster2   1/1     1            1           98s

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/linkerd-gateway-58ccc85959                   1         1         1       3m3s
replicaset.apps/linkerd-service-mirror-cluster2-7ddc698cc8   1         1         1       98s
```
```bash
# Verify linkerd-multicluster resource deployment on cluster2
$ kubectl get all -n linkerd-multicluster --context="${CLUSTER2_CTX}"
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/linkerd-gateway-847b97dd4c-fbfwk                   2/2     Running   0          28m
pod/linkerd-service-mirror-cluster1-84cf9df486-zntv9   2/2     Running   0          13m

NAME                             TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)                         AGE
service/linkerd-gateway          LoadBalancer   10.254.159.222   xxx.xxx.xxx.xxx   4143:32668/TCP,4191:32712/TCP   28m
service/probe-gateway-cluster1   ClusterIP      10.254.101.162   <none>            4191/TCP                        13m

NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/linkerd-gateway                   1/1     1            1           28m
deployment.apps/linkerd-service-mirror-cluster1   1/1     1            1           13m

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/linkerd-gateway-847b97dd4c                   1         1         1       28m
replicaset.apps/linkerd-service-mirror-cluster1-84cf9df486   1         1         1       13m
```
<br>

#### Checking the status of multi-cluster connections
In each cluster, check the connection status of the other cluster.
```bash
# Check the status of the cluster1 -> cluster2 connection
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
# Check the status of the cluster2 -> cluster1 connection
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

### <span id='3.5'>3.5. Install linkerd-smi
Install the `linkerd-smi` extension to use Linkerd CRD `TrafficSplit` for traffic splitting.<br>

```bash
#  Registering a linkerd-smi extension repo
$ helm repo add l5d-smi https://linkerd.github.io/linkerd-smi

$ helm repo list
NAME    URL
linkerd https://helm.linkerd.io/stable
l5d-smi https://linkerd.github.io/linkerd-smi (Confirm registration)

# Install linkerd-smi in cluster1
helm install linkerd-smi l5d-smi/linkerd-smi -n linkerd-smi --create-namespace \
--kube-context="${CLUSTER1_CTX}"

# Install linkerd-smi in cluster2
helm install linkerd-smi l5d-smi/linkerd-smi -n linkerd-smi --create-namespace \
--kube-context="${CLUSTER2_CTX}"
```

<br>

## <span id='4'>4. Deploy the sample application
Proceed to deploy the multi-cluster communication sample application on clusters cluster1 and cluster2.

#### Where to deploy applications
| Applications| Cluster1 | Cluster2 | About |
| :---: |:---: | :---: | :---: |  
| helloworld-v1 | :heavy_check_mark: | | Returns the corresponding version <b>(v1)</b> and hostname |
| helloworld-v2 |  | :heavy_check_mark: | Returns the corresponding version <b>(v2)</b> and hostname |
| sleep |  | :heavy_check_mark: | Used as a request source to call other services|


```bash
# Create a directory 
$ mkdir -p $HOME/linkerd/sample
$ cd $HOME/linkerd/sample

# Download the helloworld service deployment script
$ wget https://raw.githubusercontent.com/istio/istio/master/samples/helloworld/gen-helloworld.sh
$ chmod +x gen-helloworld.sh

# Download the SLEEP service deployment YAML
$ wget https://raw.githubusercontent.com/istio/istio/master/samples/sleep/sleep.yaml

# Create a sample namespace and add the linkerd-proxy injection annotation to cluster1
$ kubectl create namespace sample --context="${CLUSTER1_CTX}"
$ kubectl annotate namespace sample "linkerd.io/inject=enabled" --context="${CLUSTER1_CTX}"

# Create a sample namespace and add the linkerd-proxy injection annotation to cluster2
$ kubectl create namespace sample --context="${CLUSTER2_CTX}"
$ kubectl annotate namespace sample "linkerd.io/inject=enabled" --context="${CLUSTER2_CTX}"
```

<br>

### <span id='4.1'>4.1. Deploy the sample application to the cluster cluster1
```bash
# Deploy the helloworld-v1 application on cluster1
$ ./gen-helloworld.sh --version v1 --includeService true --includeDeployment true  | \
    kubectl --context="${CLUSTER1_CTX}" -n sample apply -f -
```
```bash
# Create the helloworld-v1 service resource
service/helloworld created
deployment.apps/helloworld-v1 created
```

<br>

#### Mirroring a service to cluster cluster2
Mirror the service `helloworld` created on cluster cluster1 to cluster cluster2.
> Add the `mirror.linkerd.io/exported=true` label
```bash
$ kubectl label svc helloworld mirror.linkerd.io/exported=true --context="${CLUSTER1_CTX}" -n sample
```

<br>

Verify that the cluster cluster1 service `helloworld` is mirrored on cluster2.
```bash
# Verify that a service is created on cluster2 with `helloworld-{cluster1 people}`
$ kubectl get svc --context="${CLUSTER2_CTX}" -n sample
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
helloworld-cluster1   ClusterIP   10.254.83.67   <none>        5000/TCP   3m11s
```

<br>

### <span id='4.2'>4.2. Deploy the sample application on cluster cluster2
```bash
# Deploy the helloworld-v2 application on cluster2
$ ./gen-helloworld.sh --version v2 --includeService true --includeDeployment true  | \
    kubectl --context="${CLUSTER2_CTX}" -n sample apply -f -

# Deploy a SLEEP application on cluster2
$ kubectl --context="${CLUSTER2_CTX}" -n sample apply -f sleep.yaml
```
```bash
# Create the helloworld-v2 service resource
service/helloworld created
deployment.apps/helloworld-v2 created

# Create a sleep service resource
serviceaccount/sleep created
service/sleep created
deployment.apps/sleep created
```
<br>

#### Deploying `TrafficSplit`
Deploy a resource `TrafficSplit` that allows you to specify the ratio of traffic between services. <br>
Set the service helloworld to route 50% of incoming traffic to `helloworld-cluster1` and 50% to `helloworld`.
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

### <span id='4.3'>4.3. Testing multi-cluster communication
<b>TrafficSplit</b> to see that the traffic is split between `helloworld-v1` on cluster1 and `helloworld-v2` on cluster2.
```bash
# Look up the helloword-v1 pod in cluster1
$ kubectl get pods -l app=helloworld --context="${CLUSTER1_CTX}" -n sample
NAME                             READY   STATUS    RESTARTS   AGE
helloworld-v1-665db7d464-lcl75   2/2     Running   0          3m29s

# Lookup helloworld-v2 pod in cluster2
$ kubectl get pods -l app=helloworld --context="${CLUSTER2_CTX}" -n sample
NAME                            READY   STATUS    RESTARTS   AGE
helloworld-v2-f58d4c8cb-mzpsb   2/2     Running   0          3m3s
```

```bash
# Connect to the sleep pod on cluster2
$ kubectl exec -it -n sample --context="${CLUSTER2_CTX}" \
"$(kubectl get pod -n sample --context="${CLUSTER2_CTX}" -l app=sleep -o jsonpath='{.items[0].metadata.name}')" -c sleep sh

# helloworld service curl communication
~ $  curl helloworld:5000/hello
Hello version: v1, instance: helloworld-v1-665db7d464-lcl75 (version v1, see output of helloword-v1 podname for cluster1)
~ $  curl helloworld:5000/hello
Hello version: v2, instance: helloworld-v2-f58d4c8cb-mzpsb  (version v2, see output of helloword-v2 podname on cluster2)
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

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > CSP 쿠버네티스 서비스 Linkerd 멀티 클러스터 설치 가이드
