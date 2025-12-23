### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) >  Portal Use Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
   1.1. [Purpose](#1-1)  
   1.2. [Range](#1-2)  
2. [Prerequisite](#2)  
   2.1  [Access the Container Platform Portal](#2-1)  
   2.2. [(Ref) Terraman IaC Settings](#2-2)    
3. [Container Platform Portal Configuration](#3)   
   3.1. [Container Platform Portal user permission types](#3-1)  
   3.2. [Configure the Container Platform Portal menu](#3-2)    


<br>

## <div id='1'/> 1. Documentation Overview

### <div id='1-1'/> 1.1. Purpose
This document describes how to use the Container Platform Portal.

<br>

### <div id='1-2'/> 1.2. Range
This document describes how to use the Container Platform Portal based on the Windows environment.

<br>

## <div id='2'>2. Prerequisite

### <div id='2-1'>2.1. Access the Container Platform Portal
> [[Access the Container Platform Portal]](./cp-portal-use-guide-access.md)

<br>

### <div id='2-2'>2.2. (Ref) Terraman IaC Settings
Creating a subcluster through the Container Platform portal requires additional setup of the target cloud (IaaS). <br>
See the guide below to create a subcluster after completing the setup.
> [[Terraman IaC Scripting Guide]](../../use-guide/terraman/cp-terraman-guide.md)

<br>



## <div id='3'/> 3. Configure the Container Platform Portal
### <div id='3-1'/> 3.1. Container Platform Portal User Permission Types
| <center>User Type</center> | <center>Function</center> |
| :--- | :--- | 
| Super Admin (Full Admin) | Full cluster management privileges |
| Cluster Admin | Assigned cluster administration privileges |
| User (regular user) | Assigned in-cluster namespace management privileges |

<br>

### <div id='3-2'/> 3.2. Configure the Container Platform Portal Menu

<table>
<thead>
  <tr>
    <th>Menu</th>
    <th>Category</th>
    <th>Description</th>
    <th>Access Permissions</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="7">🔗<a href="./cp-portal-use-guide-global.md"><b>Global</b></a></td>
    <td>Overview</td>
    <td>Cluster information dashboard</td>
    <td rowspan="7">Super Admin <br> Cluster Admin (View Permissions)</td>
  </tr>
  <tr>
    <td>Clusters</td>
    <td>Manage cluster information</td>
  </tr>
  <tr>
    <td>Cloud Accounts</td>
    <td>Manage Cloud Accounts information</td>
  </tr>
  <tr>
    <td>Instance Code Template</td>
    <td>Managing Instance Code Template Information</td>
  </tr>
  <tr>
    <td>SSH Keys</td>
    <td>Managing SSH Keys Information</td>
  </tr>  
  <tr>
    <td>Federation</td>
    <td>Manage Federation information</td>
  </tr> 
  <tr>
    <td>Migration</td>
    <td>Manage Migration information</td>
  </tr>   
  <tr>
    <td rowspan="3">🔗<a href="./cp-portal-use-guide-clusters.md"><b>Clusters</b></a></td>
    <td>Overview</td>
    <td>Resources in Clusters Dashboard</td>
    <td>All</td>
  </tr>
  <tr>
    <td>Nodes</td>
    <td>Manage Nodes information</td>
    <td>Super Admin, Cluster Admin</td>
  </tr>
  <tr>
    <td>Namespaces</td>
    <td>Manage Namespaces Information</td>
    <td>All</td>
  </tr>
  <tr>
    <td rowspan="2">🔗<a href="./cp-portal-use-guide-catalogs.md"><b>Catalogs</b></a></td>
    <td>Repositories</td>
    <td>Managing Helm Chart Repository Information</td>
    <td rowspan="2">All</td>
  </tr>
  <tr>
    <td>Releases</td>
    <td>Managing Releases Information</td>
  </tr>  
  <tr>
    <td rowspan="3">🔗<a href="./cp-portal-use-guide-workloads.md"><b>Workloads</b></a></td>
    <td>Deployments</td>
    <td>Manage Deployments information</td>
    <td rowspan="3">All</td>
  </tr>
  <tr>
    <td>Pods</td>
    <td>Manage Pods information</td>
  </tr>
  <tr>
    <td>ReplicaSets</td>
    <td>Managing ReplicaSets Information</td>
  </tr>
  <tr>
    <td rowspan="2">🔗<a href="./cp-portal-use-guide-services.md"><b>Services</b></a></td>
    <td>Services</td>
    <td>Services Information Management</td>
    <td rowspan="2">All</td>
  </tr>
  <tr>
    <td>Ingresses</td>
    <td> Manage Ingresses information</td>
  </tr>
  <tr>
    <td rowspan="3">🔗<a href="./cp-portal-use-guide-storages.md"><b>Storages</b></a></td>
    <td>Persistent Volumes</td>
    <td>Managing Persistent Volumes information</td>
    <td>Super Admin, Cluster Admin</td>
  </tr>
  <tr>
    <td>Persistent Volume Claims</td>
    <td>Manage Persistent Volume Claims information</td>
    <td>All</td>
  </tr>
  <tr>
    <td>Storage Classes</td>
    <td>Managing Storage Classes Information</td>
    <td>Super Admin, Cluster Admin</td>
  </tr>
 <tr>
    <td rowspan="2">🔗<a href="./cp-portal-use-guide-configs.md"><b>Configs</b></a></td>
    <td>ConfigMaps</td>
    <td> Manage ConfigMaps Information</td>
    <td rowspan="2">All</td>
  </tr>
  <tr>
    <td>Secrets</td>
    <td>Manage Secrets Information</td>
  </tr>
  <tr>
    <td rowspan="4">🔗<a href="./cp-portal-use-guide-managements.md"><b>Managements</b></a></td>
    <td>Users</td>
    <td>Manage Users</td>
    <td>Super Admin, Cluster Admin</td>
  </tr>
  <tr>
    <td>Roles</td>
    <td>Manage Roles</td>
    <td rowspan="3">All</td>
  </tr>
  <tr>
    <td>Resource Quotas</td>
    <td>Manage Resource Quotas Information</td>
  </tr>
  <tr>
    <td>Limit Ranges</td>
    <td>Manage Limit Ranges Information</td>
  </tr>
  <tr>
    <td rowspan="2">🔗<a href="./cp-portal-use-guide-chaos.md"><b>Chaos</b></a></td>
    <td>Experiments</td>
    <td>Manage Chaos Experiments Information</td>
    <td rowspan="2">Super Admin</td>
  </tr>
  <tr>
    <td>Events</td>
    <td>Manage Chaos Events Information</td>
  </tr>   
  <tr>
    <td rowspan="2">🔗<a href="./cp-portal-use-guide-info.md"><b>Info</b></a></td>
    <td>Access</td>
    <td>Container Platform Portal <br>Manage configuration information for using the CLI</td>
    <td rowspan="2">All</td>
  </tr>
  <tr>
    <td>Private Repository</td>
    <td>Manage private repository information</td>
  </tr>
</tbody>
</table>

<br>


### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) >  Portal Use Guide











