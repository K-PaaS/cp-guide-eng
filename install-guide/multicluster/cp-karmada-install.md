### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Karmada Installation Guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
   1.1. [Purpose](#1.1)<br>
   1.2. [Scope](#1.2)<br>
   1.3. [System configuration diagram](#1.3)<br>
   1.4. [Reference Material](#1.4)<br>

2. [Prerequisite](#2)

3. [Install Karmada](#3)<br>
  3.1. [Install Karmada](#3.1)<br>
  3.1.1. [Download the latest release of Karmada](#3.1.1)<br>
  3.1.2. [Karmada Control Plane Installation](#3.1.2)<br>
  3.1.3. [Karmada Context Settings](#3.1.3)<br>
  3.2. [Setting up a Member cluster](#3.2)<br>
  3.2.1. [Config file settings](#3.2.1)<br>
  3.2.2. [Register a Member cluster](#3.2.2)

4. [Deploy the sample application](#4)<br>
  4.1. [Creating and deploying PropagationPolicy policies](#4.1)<br>
  4.2. [Generate sample YAML](#4.2)<br>
  4.3. [Check out the sample application](#4.3)<br>

5. [Deploy a multi-cluster sample application](#5)<br>
  5.1 [Create a namespace](#5.1)<br>
  5.2 [Create Karmada Propagationpolicy](#5.2)<br>
  5.3 [Generate example code](#5.3)<br>
  5.4 [Verify lab application behavior](#5.4)

<br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Karmada Installation Guide) describes how to install Karmada on a cluster of the K-PaaS Container Platform, an open PaaS platform for planners, developers, and operators, to form a `multi-cluster`.

<br>

### <div id='1.2'> 1.2. Scope
The scope of the installation is to install the cluster installation that is the basis of the K-PaaS container platform environment as a `multi-cloud` environment and then configure a `multi-cloud` using Karmada.

<br>

### <div id='1.3'> 1.3. System configuration diagram
The system configuration consists of three Kubernetes `multi-cluster` environments (Control Plane, Worker). (2 multi-cluster, 1 Karmada Host cluster)

Configure a Kubernetes `multi-cluster` with K-PaaS Container Platform Deployment and additionally configure a `single cluster` for the Karmada Host cluster.

<br>

### <div id='1.4'> 1.4. Reference Material
> https://karmada.io/<br>
> https://github.com/karmada-io/karmada

<br>

## <div id='2'> 2. Prerequisite

### K-PaaS Container Platform Clusters
To install a K-PaaS Container Platform cluster, see the guide below.

> [K-PaaS Container Platform Multi-Cluster Installation Guide](https://github.com/K-PaaS/cp-guide-eng/tree/branch/master/install-guide/standalone/cp-cluster-install-multi.md)<br>
> [K-PaaS Container Platform Cluster Installation Guide](https://github.com/K-PaaS/cp-guide-eng/tree/branch/master/install-guide/standalone/cp-cluster-install-single.md)

<br>

## <div id='3'> 3. Install Karmada

### <div id='3.1'> 3.1. Install Karmada

### <div id='3.1.1'> 3.1.1. Download the latest release of Karmada
Download the karmada v1.7.0 release.
```bash
$ curl -s https://raw.githubusercontent.com/karmada-io/karmada/master/hack/install-cli.sh | sudo INSTALL_CLI_VERSION=1.7.0 bash
```

<br>

### <div id='3.1.2'> 3.1.2. Karmada Control Plane Installation
Proceed with the Karmada Control Plane installation.<br>
Karmada's etcd uses the K-PaaS container platform's storage classes to ensure reliable data storage.
```bash
$ sudo karmadactl init --etcd-storage-mode PVC --storage-classes-name {STORAGE_CLASS_NAME} --kubeconfig={KUBE_CONFIG_PATH}

## Example
$ sudo karmadactl init --etcd-storage-mode PVC --storage-classes-name cp-storageclass --kubeconfig=/home/ubuntu/.kube/config
```

```bash
## Karmada Installation Process
I1109 13:37:22.987256  106456 deploy.go:184] kubeconfig file: /home/ubuntu/.kube/config, kubernetes: https://127.0.0.1:6443
I1109 13:37:23.006789  106456 deploy.go:204] karmada apiserver ip: [172.16.11.106]
I1109 13:37:23.355645  106456 cert.go:229] Generate ca certificate success.
I1109 13:37:23.523217  106456 cert.go:229] Generate karmada certificate success.
I1109 13:37:23.711487  106456 cert.go:229] Generate apiserver certificate success.
I1109 13:37:24.461727  106456 cert.go:229] Generate front-proxy-ca certificate success.
I1109 13:37:25.016618  106456 cert.go:229] Generate front-proxy-client certificate success.
I1109 13:37:25.172154  106456 cert.go:229] Generate etcd-ca certificate success.
I1109 13:37:25.706451  106456 cert.go:229] Generate etcd-server certificate success.
I1109 13:37:25.887367  106456 cert.go:229] Generate etcd-client certificate success.
I1109 13:37:25.887728  106456 deploy.go:298] download crds file:https://github.com/karmada-io/karmada/releases/download/v1.7.1/crds.tar.gz
Downloading...[ 100.00% ]
	..
	..
	...skipped
	..
	..
------------------------------------------------------------------------------------------------------
 █████   ████   █████████   ███████████   ██████   ██████   █████████   ██████████     █████████
░░███   ███░   ███░░░░░███ ░░███░░░░░███ ░░██████ ██████   ███░░░░░███ ░░███░░░░███   ███░░░░░███
 ░███  ███    ░███    ░███  ░███    ░███  ░███░█████░███  ░███    ░███  ░███   ░░███ ░███    ░███
 ░███████     ░███████████  ░██████████   ░███░░███ ░███  ░███████████  ░███    ░███ ░███████████
 ░███░░███    ░███░░░░░███  ░███░░░░░███  ░███ ░░░  ░███  ░███░░░░░███  ░███    ░███ ░███░░░░░███
 ░███ ░░███   ░███    ░███  ░███    ░███  ░███      ░███  ░███    ░███  ░███    ███  ░███    ░███
 █████ ░░████ █████   █████ █████   █████ █████     █████ █████   █████ ██████████   █████   █████
░░░░░   ░░░░ ░░░░░   ░░░░░ ░░░░░   ░░░░░ ░░░░░     ░░░░░ ░░░░░   ░░░░░ ░░░░░░░░░░   ░░░░░   ░░░░░
------------------------------------------------------------------------------------------------------
Karmada is installed successfully.

Register Kubernetes cluster to Karmada control plane.

Register cluster with 'Push' mode

Step 1: Use "karmadactl join" command to register the cluster to Karmada control plane. --cluster-kubeconfig is kubeconfig of the member cluster.
(In karmada)~# MEMBER_CLUSTER_NAME=$(cat ~/.kube/config  | grep current-context | sed 's/: /\n/g'| sed '1d')
(In karmada)~# karmadactl --kubeconfig /etc/karmada/karmada-apiserver.config  join ${MEMBER_CLUSTER_NAME} --cluster-kubeconfig=$HOME/.kube/config

Step 2: Show members of karmada
(In karmada)~# kubectl --kubeconfig /etc/karmada/karmada-apiserver.config get clusters


Register cluster with 'Pull' mode

Step 1: Use "karmadactl register" command to register the cluster to Karmada control plane. "--cluster-name" is set to cluster of current-context by default.
(In member cluster)~# karmadactl register 172.16.11.106:32443 --token 1rgb7l.tdk5q1bg077t19z6 --discovery-token-ca-cert-hash sha256:551bab3fcfa57d0917d1b29a2c051c7959c53eb76326ea6f6e2471251ed2ef40

Step 2: Show members of karmada
(In karmada)~# kubectl --kubeconfig /etc/karmada/karmada-apiserver.config get clusters
```

<br>

Check the Karmada-System namespace.
```bash
$ kubectl get all -n karmada-system
NAME                                                READY   STATUS    RESTARTS   AGE
pod/etcd-0                                          1/1     Running   0          3m31s
pod/karmada-aggregated-apiserver-5d7955b64b-4s4xj   1/1     Running   0          2m54s
pod/karmada-apiserver-58f66d6f76-z7sfc              1/1     Running   0          3m25s
pod/karmada-controller-manager-7cdddb5476-hv2mk     1/1     Running   0          2m30s
pod/karmada-scheduler-5555687bb-qqlmn               1/1     Running   0          2m38s
pod/karmada-webhook-8699496649-bcf92                1/1     Running   0          2m22s
pod/kube-controller-manager-675dff8bd8-zmprr        1/1     Running   0          2m43s

NAME                                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
service/etcd                           ClusterIP   None            <none>        2379/TCP,2380/TCP   3m31s
service/karmada-aggregated-apiserver   ClusterIP   10.233.28.84    <none>        443/TCP             2m54s
service/karmada-apiserver              NodePort    10.233.55.116   <none>        5443:32443/TCP      3m25s
service/karmada-webhook                ClusterIP   10.233.51.202   <none>        443/TCP             2m22s
service/kube-controller-manager        ClusterIP   10.233.41.78    <none>        10257/TCP           2m43s

NAME                                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/karmada-aggregated-apiserver   1/1     1            1           2m54s
deployment.apps/karmada-apiserver              1/1     1            1           3m25s
deployment.apps/karmada-controller-manager     1/1     1            1           2m30s
deployment.apps/karmada-scheduler              1/1     1            1           2m38s
deployment.apps/karmada-webhook                1/1     1            1           2m22s
deployment.apps/kube-controller-manager        1/1     1            1           2m43s

NAME                                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/karmada-aggregated-apiserver-5d7955b64b   1         1         1       2m54s
replicaset.apps/karmada-apiserver-58f66d6f76              1         1         1       3m25s
replicaset.apps/karmada-controller-manager-7cdddb5476     1         1         1       2m30s
replicaset.apps/karmada-scheduler-5555687bb               1         1         1       2m38s
replicaset.apps/karmada-webhook-8699496649                1         1         1       2m22s
replicaset.apps/kube-controller-manager-675dff8bd8        1         1         1       2m43s

NAME                    READY   AGE
statefulset.apps/etcd   1/1     3m31s
```

<br>

### <div id='3.1.3'> 3.1.3. Karmada Context Settings
Add and set up a Karmada context in addition to the cluster's default context.
```bash
$ sudo cp /etc/karmada/karmada-apiserver.config ~/.kube
$ export KUBECONFIG=~/.kube/config:~/.kube/karmada-apiserver.config
$ kubectl config get-contexts

CURRENT   NAME                             CLUSTER             AUTHINFO        NAMESPACE
          karmada-apiserver                karmada-apiserver   karmada-admin
*         kubernetes-admin@cluster.local   cluster.local       kubernetes-admin
```

```bash
$ kubectl config use-context karmada-apiserver 
Switched to context "karmada-apiserver".
```

<br>

### <div id='3.2'> 3.2. Setting up a Member cluster

### <div id='3.2.1'> 3.2.1. Config file settings
Place the Kube Config file for your multi-cluster in the following path on your Karmada Host cluster<br>
The Kube Config file for the Member cluster is stored in `$HOME/.kube/config` on each cluster.<br>
```bash
$ mkdir -p $HOME/.kube/member

## Copy and view the Config file
$ ls /home/ubuntu/.kube/member
member1_config member2_config
```

<br>

member1_cofing file example
```bash
$ vi member1_config
```

```
apiVersion: v1
clusters:
- cluster:
  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJWHBOaDJRQVFLVzh3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1Wlh
  .
  .
  ..skipped
  .
  .
  server: https://103.xxx.xxx.xxx:6443
  name: cluster.local
contexts:
- context:
  cluster: cluster.local
  user: kubernetes-admin
  name: kubernetes-admin@cluster.local
current-context: kubernetes-admin@cluster.local
kind: Config
preferences: {}
.
.
...skipped
.
.
```

<br>

Example member2_config file
```bash
$ vi member2_config
```

```
apiVersion: v1
clusters:
- cluster:
  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJVk94MDFEOE5IdHN3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1Wlh
 .
 .
 ...skipped
 .
 .
  server: https://104.xxx.xxx.xxx:6443
  name: cluster.local
contexts:
- context:
  cluster: cluster.local
  user: kubernetes-admin
  name: kubernetes-admin@cluster.local
current-context: kubernetes-admin@cluster.local
kind: Config
.
.
...skipped
.
.
```

<br>

#### <div id='3.2.2'> 3.2.2. Register a Member cluster
Using the copied Kube Config file, register each cluster as a Member cluster in the Karmada Host cluster.

<br>

Register the Member1 cluster.
```bash
$ karmadactl join member1 --cluster-kubeconfig=/home/ubuntu/.kube/member/member1_config
cluster(member1) is joined successfully
```

<br>

Register the Member2 cluster.
```bash
$ karmadactl join member2 --cluster-kubeconfig=/home/ubuntu/.kube/member/member2_config
cluster(member2) is joined successfully
```

<br>

View the Member Clusters you've registered.
```bash
$ kubectl get clusters
NAME      VERSION   MODE   READY   AGE
member1   v1.27.5   Push   True    10m
member2   v1.27.5   Push   True    4m4s
```

<br>

## <div id='4'> 4. Deploy the sample application

### <div id='4.1'> 4.1. Creating and deploying PropagationPolicy policies
Create a PropagationPolicy policy to verify the sample application.

```bash
$ vi propagationpolicy.yaml
```

```yaml
apiVersion: policy.karmada.io/v1alpha1
kind: PropagationPolicy
metadata:
  name: example-policy # The default namespace is `default`.
spec:
  resourceSelectors:
    - apiVersion: apps/v1
      kind: Deployment
      name: nginx # If no namespace is specified, the namespace is inherited from the parent object scope.
  placement:
    clusterAffinity:
      clusterNames:
        - member1
        - member2
```

```bash
$ kubectl apply -f propagationpolicy.yaml
propagationpolicy.policy.karmada.io/example-policy created
```

<br>

### <div id='4.2'> 4.2. Generate sample YAML
Deploy the sample application.
```bash
$ vi nginx.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - image: nginx
          name: nginx
```

```bash
$kubectl apply -f nginx.yaml
deployment.apps/nginx created
```

<br>

### <div id='4.3'> 4.3. Check out the sample application
Verify that the sample application is successfully deployed to the Member cluster.

```bash
$ karmadactl get deployment -l app=nginx
NAME    CLUSTER   READY   UP-TO-DATE   AVAILABLE   AGE   ADOPTION
nginx   member1   2/2     2            2           12s   Y
nginx   member2   2/2     2            2           12s   Y
```

<br>

## <div id='5'> 5. Deploy a multi-cluster sample application
Deploy a sample app to check the multi-cluster communication of cluster member1 and member2 using karmada.<br>
Then go to the Install VMs in the cluster and check the communication between each cluster.<br>
<br>
First, create a path for the lab.<br>
```bash
# Create a lab path
$ mkdir $HOME/samples
$ cd samples
```

### <div id='5.1'> 5.1. Create a namespace
Create a namespace for deploying the sample application.

```bash
$ vi namespace.yaml
```

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: sample
```

```bash
# Create a namespace
$ kubectl apply -f namespace.yaml
```
<br>

### <div id='5.2'> 5.2. Create Karmada Propagationpolicy
Create a PropagationPolicy policy to verify the sample application.<br>

```bash
$ vi propagationpolicy.yaml
```
```yaml
apiVersion: policy.karmada.io/v1alpha1
kind: PropagationPolicy
metadata:
  name: helloworld-v1-policy
spec:
  resourceSelectors:
    - apiVersion: apps/v1
      kind: Deployment
      name: helloworld-v1
  placement:
    clusterAffinity:
      clusterNames:
        - member1
---
apiVersion: policy.karmada.io/v1alpha1
kind: PropagationPolicy
metadata:
  name: helloworld-v2-policy
spec:
  resourceSelectors:
    - apiVersion: apps/v1
      kind: Deployment
      name: helloworld-v2
  placement:
    clusterAffinity:
      clusterNames:
        - member2
---
apiVersion: policy.karmada.io/v1alpha1
kind: PropagationPolicy
metadata:
  name: helloworld-common-policy
spec:
  resourceSelectors:
    - apiVersion: v1
      kind: Namespace
      name: sample
    - apiVersion: v1
      kind: Service
      name: helloworld
    - apiVersion: apps/v1
      kind: Deployment
      name: sleep
    - apiVersion: v1
      kind: ServiceAccount
      name: sleep
    - apiVersion: v1
      kind: Service
      name: sleep
  placement:
    clusterAffinity:
      clusterNames:
        - member1
        - member2
```

```bash
# Deploy to the namespace you created earlier
$ kubectl apply -f propagationpolicy.yaml -n sample
```
<br>

### <div id='5.3'> 5.3. Generate example code
Deploy the helloworld(v1) Application to `member1` and the helloworld(v2) Application to `member2`.<br>

```bash
$ vi helloworld.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: helloworld
  namespace: sample
  labels:
    app: helloworld
    service: helloworld
spec:
  ports:
  - port: 5000
    name: http
  selector:
    app: helloworld
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-v1
  namespace: sample
  labels:
    app: helloworld
    version: v1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: helloworld
      version: v1
  template:
    metadata:
      labels:
        app: helloworld
        version: v1
    spec:
      containers:
      - name: helloworld
        image: docker.io/istio/examples-helloworld-v1
        resources:
          requests:
            cpu: "100m"
        imagePullPolicy: IfNotPresent #Always
        ports:
        - containerPort: 5000
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-v2
  namespace: sample
  labels:
    app: helloworld
    version: v2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: helloworld
      version: v2
  template:
    metadata:
      labels:
        app: helloworld
        version: v2
    spec:
      containers:
      - name: helloworld
        image: docker.io/istio/examples-helloworld-v2
        resources:
          requests:
            cpu: "100m"
        imagePullPolicy: IfNotPresent #Always
        ports:
        - containerPort: 5000
```

```bash
$ kubectl apply -f helloworld.yaml
```
<br>

Deploy the sleep application.



```bash
$ vi sleep.yaml
```

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sleep
  namespace: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sleep
  namespace: sample
  labels:
    app: sleep
    service: sleep
spec:
  ports:
  - port: 80
    name: http
  selector:
    app: sleep
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sleep
  namespace: sample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sleep
  template:
    metadata:
      labels:
        app: sleep
    spec:
      terminationGracePeriodSeconds: 0
      serviceAccountName: sleep
      containers:
      - name: sleep
        image: curlimages/curl
        command: ["/bin/sleep", "infinity"]
        imagePullPolicy: IfNotPresent
        volumeMounts:
        - mountPath: /etc/sleep/tls
          name: secret-volume
      volumes:
      - name: secret-volume
        secret:
          secretName: sleep-secret
          optional: true
```

```bash
$ kubectl apply -f sleep.yaml
```

<br>


### <div id='5.4'> 5.4. Verify lab application behavior
View the resources deployed in the `sample` namespace.<br>
You can see that `member1` is deployed with helloworld(v1) and `member2` is deployed with helloworld(v2).<br>

```bash
$ karmadactl get all -n sample

NAME                                 CLUSTER   READY   STATUS    RESTARTS   AGE
pod/helloworld-v2-79d5467d55-8d6st   member2   2/2     Running   0          19m
pod/sleep-9454cc476-jwm86            member2   2/2     Running   0          19m
pod/helloworld-v1-b6c45f55-gz5qb     member1   2/2     Running   0          19m
pod/sleep-9454cc476-bg9xz            member1   2/2     Running   0          19m

NAME                 CLUSTER   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/helloworld   member1   ClusterIP   10.233.48.160   <none>        5000/TCP   25m
service/sleep        member1   ClusterIP   10.233.31.191   <none>        80/TCP     19m
service/helloworld   member2   ClusterIP   10.233.3.223    <none>        5000/TCP   19m
service/sleep        member2   ClusterIP   10.233.3.198    <none>        80/TCP     19m

NAME                            CLUSTER   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/helloworld-v1   member1   1/1     1            1           19m
deployment.apps/sleep           member1   1/1     1            1           19m
deployment.apps/helloworld-v2   member2   1/1     1            1           19m
deployment.apps/sleep           member2   1/1     1            1           19m

NAME                                       CLUSTER   DESIRED   CURRENT   READY   AGE
replicaset.apps/helloworld-v1-b6c45f55     member1   1         1         1       19m
replicaset.apps/sleep-9454cc476            member1   1         1         1       19m
replicaset.apps/helloworld-v2-79d5467d55   member2   1         1         1       19m
replicaset.apps/sleep-9454cc476            member2   1         1         1       19m
```

### Testing Helloworld communication<br>
:bulb: **For final verification, navigate to the Install VM in the Member1 Cluster**

```bash
# Setting variables on each cluster for communication checks
$ export CTX_CLUSTER1=cluster1
$ export CTX_CLUSTER2=cluster2
```

You can see that the traffic is split and communicated to helloworld-v1 in `cluster1` and hellodworld-v2 in `cluster2` via `TrafficSplit`.
```bash
$ $ kubectl exec --context="${CTX_CLUSTER1}" -n sample -c sleep \
    "$(kubectl get pod --context="${CTX_CLUSTER1}" -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')" \
    -- curl -sS helloworld.sample:5000/hello

Hello version: v2, instance: helloworld-v2-79d5467d55-8d6st
Hello version: v1, instance: helloworld-v1-b6c45f55-gz5qb
Hello version: v1, instance: helloworld-v1-b6c45f55-gz5qb
Hello version: v1, instance: helloworld-v1-b6c45f55-gz5qb
Hello version: v2, instance: helloworld-v2-79d5467d55-8d6st
Hello version: v2, instance: helloworld-v2-79d5467d55-8d6st
Hello version: v1, instance: helloworld-v1-b6c45f55-gz5qb
.
.
.
```


### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Karmada Installation Guide
