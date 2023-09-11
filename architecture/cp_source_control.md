### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Architecture](./README.md) > Source Control

## Purpose
This document provides an architecture of K-PaaS Container Platform SourceControl.
<br><br>

## Container Configuration Diagram
![image](https://user-images.githubusercontent.com/80228983/146350860-3722c081-7338-438d-b7ec-1fdac09160c4.png)



| Container Name  | Role |
|-------|-----|
| SC-API | Provides the REST API required for source control service control |
| SC-UI | Source Control Service Web UI |
| SC-Broker | Application for relay role between K-PaaS and source control services |
| SCM-Server | SCM Server |



## Description
The K-PaaS container platform source control service provides a UI for managing Git and Svn Repository.   


### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Architecture](./README.md) > Source Control
