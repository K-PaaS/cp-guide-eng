### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../README.md) > NFS Server Installation


# NFS Server Installation

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Range](#1.2)  

2. [NFS Server Installation](#2)  
  2.1. [Prerequisite](#2.1)  
  2.2. [Installation](#2.2)  
  2.3. [Operation Check](#2.3)

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (Kubespray Installation Guide) describes how to upgrade the open PaaS platform and install NFS Storage Server for all environments for the container platform deployed in Open PaaS based on developer-enabled environments.

Starting with PaaS-TA 6.0, storage to be used as a Persistence Volume is necessary if you want to install services for container platforms in a basic cluster deployed by Kuberpray.

<br>

### <div id='1.2'> 1.2. Range
The installation range was prepared based on the basic installation of Kubespray to verify Kubernetes Native.

<br>

## <div id='2'> 2. NFS Server Installation

<br>

### <div id='2.1'> 2.1. Prerequisite
This installation guide is based on installation in **Ubuntu 18.04** environment. Install it on a separate VM for Storage because it is for Storage to be used by clusters deployed in Kubespray.


### <div id='2.2'> 2.2. Installation
- Package Update
```
$ sudo apt-get update
```

- Install package for NFS Server

```
$ sudo apt-get install nfs-common nfs-kernel-server portmap
```

- Create the directory to be used at NFS and give authority
```
$ sudo mkdir -p /home/share/nfs
$ sudo chmod 777 /home/share/nfs
```

- Share dirctory settings
```
$ sudo vi /etc/exports
## Type : [/share directory] [Access IP] [Option]
/home/share/nfs *(rw,no_root_squash,async)
```
>`rw - readwrite` <br>
>       `no_root_squash - Client can acquire root privileges, created with client privileges when creating files.`<br>
>       `async - 요청에 의해 변경되기 전에 요청에 응답, 성능 향상용`


- nfs Server Restart
```
$ sudo /etc/init.d/nfs-kernel-server restart
$ sudo systemctl restart portmap
```


### <div id='2.2'> 2.2. Operation Check

- Check settings
```
$ sudo exportfs -v
```

- a normal result
```
/home/share/nfs
                <world>(rw,async,wdelay,no_root_squash,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Install](https://github.com/PaaS-TA/paas-ta-container-platform-guide-eng/tree/master/install-guide) > > NFS Server Installation
