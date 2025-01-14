### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > NFS Server Installation Guide

<br>

# Installing an NFS server

<br>

## Table of Contents

1. [Document Outline](#1)<br>  
  1.1. [Purpose](#1.1)<br>  
  1.2. [Scope](#1.2)  

2. [NFS Server Installation](#2)<br>  
  2.1. [Prerequisite](#2.1)<br>  
  2.2. [Installation](#2.2)<br>  
  2.3. [Verify behavior](#2.3)

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (NFS Server Installation Guide) describes how to install NFS Server to configure the storage environment of K-PaaS Container Platform, an open PaaS platform for planners, developers, and operators.

<br>

### <div id='1.2'> 1.2. Scope
The installation scope is based on the K-PaaS Container Platform storage environment configuration.

<br>

## <div id='2'> 2. NFS Server Installation

### <div id='2.1'> 2.1. Prerequisite
One instance of NFS Server is required to install NFS Server, and the prerequisites for installation are described below.

### OS (***Required confirmation***)
The following OS environment information is required to install a K-PaaS Container Platform cluster.

|Supported OS|Version|
|---|---|
|Ubuntu|18.04|
|Ubuntu|20.04|
|Ubuntu|22.04|

<br>

### <div id='2.2'> 2.2. Installation
Proceed with the APT update.
```
$ sudo apt-get update
```

<br>

Proceed to install the APT package for NFS Server.
```
$ sudo apt-get install -y nfs-common nfs-kernel-server portmap
```

<br>

Proceed to create and authorize the directory for use by NFS Server.
```
$ sudo mkdir -p /home/share/nfs
$ sudo chmod 777 /home/share/nfs
```

<br>

Proceed with setting up the shared directory.
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

> `rw - Read Write` <br>
> `no_root_squash - Client can get root privileges, files are created with client privileges when created.`<br>
> `async - Respond to requests before they are changed by the request, for better performance`

<br>

Proceed to restart NFS Server.
```
$ sudo /etc/init.d/nfs-kernel-server restart
$ sudo systemctl restart portmap
```

<br>

### <div id='2.3'> 2.3. Verify behavior
Check the NFS Server settings.
```
$ sudo exportfs -v
```

```
/home/share/nfs
                <world>(rw,async,wdelay,no_root_squash,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > NFS Server Installation Guide

