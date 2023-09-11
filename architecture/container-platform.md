### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Architecture](./README.md) > K-PaaS CP

## Purpose
This document provides an architecture of the K-PaaS Container Platform (CP).
<br><br>

## System Configuration
![image](https://user-images.githubusercontent.com/67575226/147038676-2ef2e8a6-217d-41ff-95b0-0280a1584885.png)



| Classification  | Number of Instances| Qualification |
|-------|----|-----|
| master | 1 or 3 | 2vCPU / 4GB RAM |
| worker | N | 2vCPU / 4GB RAM |
| nfs-server | N | 1vCPU / 2GB RAM / 100GB Additional Disk |


## Description
K-PaaS CPis a container-based development and deployment management platform using Kubernetes, which supports container orchestration. 
K-PaaS CP enables container-based applications to be deployed, tested, and expanded faster and easier.


### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Architecture](./README.md) > K-PaaS CP
