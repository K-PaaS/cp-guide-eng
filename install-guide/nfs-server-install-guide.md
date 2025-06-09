### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > NFS Server Installation Guide

<br>

# NFS Server Installation Guide

<br>

## Table of Contents

1. [Overview](#1)  
   1.1. [Purpose](#1.1)  
   1.2. [Scope](#1.2)

2. [NFS Server Installation](#2)  
   2.1. [Prerequisite](#2.1)  
   2.2. [Installation](#2.2)   
   2.3. [Verification](#2.3)

3. [NFS Subdir External Provisioner Installation](#3)

<br>

## <div id='1'> 1. Overview

### <div id='1.1'> 1.1. Purpose
This document (NFS Server Installation Guide) describes how to install an NFS Server to configure the storage environment of the K-PaaS container platform, an open PaaS platform supporting planners, developers, and operators.

<br>

### <div id='1.2'> 1.2. Scope
The installation scope is based on configuring the storage environment for the K-PaaS container platform.

<br>

## <div id='2'> 2. NFS Server Installation

### <div id='2.1'> 2.1. Prerequisite
One instance is required to install the NFS Server. The prerequisites for installation are described below.

### OS (***Required***)
The following OS environments are required to install the K-PaaS container platform cluster.

|Supported OS|Version|
|---|---|
|Ubuntu|18.04|
|Ubuntu|20.04|
|Ubuntu|22.04|

<br>

### <div id='2.2'> 2.2. Installation
Update the APT package.
```
$ sudo apt-get update
```

<br>

Install the required APT packages for the NFS Server.
```
$ sudo apt-get install -y nfs-common nfs-kernel-server portmap
```

<br>

Create the directory to be used by the NFS Server and set permissions.
```
$ sudo mkdir -p /home/share/nfs
$ sudo chmod 777 /home/share/nfs
```

<br>

Configure the shared directory.
```
$ sudo vi /etc/exports
```

```
## Format : [/Shared directories] [Access IP] [Options]
## Example : /home/share/nfs 10.0.0.1(rw,no_root_squash,async)
...
/home/share/nfs {{MASTER_NODE_PRIVATE_IP}}(rw,no_root_squash,async)
/home/share/nfs {{WORKER1_NODE_PRIVATE_IP}}(rw,no_root_squash,async)
/home/share/nfs {{WORKER2_NODE_PRIVATE_IP}}(rw,no_root_squash,async)
...
```

> `rw` - Read and write access  
> `no_root_squash` - Allows clients to retain root privileges; files are created with the client's privileges.  
> `async` - Responds to requests before changes are committed to disk, improving performance.

<br>

Restart the NFS Server.
```
$ sudo /etc/init.d/nfs-kernel-server restart
$ sudo systemctl restart portmap
```

<br>

### <div id='2.3'> 2.3. Verification
Verify the NFS export configuration.
```
$ sudo exportfs -v
```

```
/home/share/nfs
                <world>(rw,async,wdelay,no_root_squash,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

<br>

## <div id='3'> 3. NFS Subdir External Provisioner Installation

> **Note:** If NFS is selected as the storage type during K-PaaS cluster installation, NFS Subdir External Provisioner is installed automatically.  
K-PaaS cluster users can skip this manual installation section.  
The following section is intended for cases where the NFS Provisioner needs to be installed manually.

<br>

To enable NFS-based dynamic PersistentVolume provisioning in a Kubernetes cluster, install [`nfs-subdir-external-provisioner`](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner).  
This Provisioner automatically creates a dedicated subdirectory under a specified directory on the NFS server when a PVC is created.

### Prerequisite
- **NFS Server must be installed and configured**  
  The NFS Server that the Provisioner will use must be [installed and configured](#2) beforehand. <br>
  The `/home/share/nfs` directory must be exported on the NFS Server.

<br>

### Writing values.yaml

> **Note:** The following `values.yaml` is the default configuration used during K-PaaS cluster installation.  
You may customize it as needed for your environment.<br>

⚠️ Make sure to replace `nfs.server` with the actual NFS Server IP.

```yaml
nfs:
  server: {NFS Server IP}                     # NFS Server IP address
  path: /home/share/nfs                       # NFS Export path (directory used by the Provisioner)

storageClass:
  provisionerName: cp-nfs-provisioner         # Name of the Provisioner to be used in the StorageClass
  defaultClass: true                          # Whether to set this as the default StorageClass
  name: cp-storageclass                       # Name of the StorageClass to be created
  reclaimPolicy: Retain                       # PV reclaim policy (Retain: Keep PV, Delete: Automatically delete PV)
  allowVolumeExpansion: false                 # Whether to allow PVC expansion (true allows expansion)
  archiveOnDelete: false                      # Whether to archive NFS directories when PVCs are deleted
```

<br>

### Helm Chart Installation

#### 1. Add Helm repo

```bash
$ helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner
$ helm repo update nfs-subdir-external-provisioner
```

#### 2. Install the Provisioner

```bash
$ helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  -f values.yaml --version 4.0.18
```

<br>

### Verification

Verify that the installation was successful.

```bash
$ helm list -n default
NAME                            NAMESPACE       REVISION       STATUS         CHART                                    APP VERSION
nfs-subdir-external-provisioner default         1              deployed       nfs-subdir-external-provisioner-4.0.18   4.0.2
```

```bash
$ kubectl get all -l app=nfs-subdir-external-provisioner
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/nfs-subdir-external-provisioner-5cdc76bcd9-jtjh4   1/1     Running   0          3m20s

NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nfs-subdir-external-provisioner   1/1     1            1           3m20s

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/nfs-subdir-external-provisioner-5cdc76bcd9   1         1         1       3m20s
```

```bash
$ kubectl get storageclass
NAME                        PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
cp-storageclass (default)   cp-nfs-provisioner   Retain          Immediate           false                  3m46s
```

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > NFS Server Installation Guide