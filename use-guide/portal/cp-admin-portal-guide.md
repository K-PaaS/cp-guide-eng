### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > Operator Portal Use Guide

<br>

## Table of Contents

1. [Document Outline](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Range](#1-2)
2. [Container Platform Operator Portal Access](#2)
    * [2.1. Container Platform Operator Portal Log in](#2-1)    
3. [Container Platform Operator Portal Manual](#3)
    * [3.1. Container Platform Operator Portal Menu Configuration](#3-1)
    * [3.2. Container Platform Operator Portal Menu Description](#3-2)
    * [3.2.1. Overview](#3-2-1)
    * [3.2.1.1. Overview List Check](#3-2-1-1)
    * [3.2.1.2. Overview Namespace Change](#3-2-1-2)
    * [3.2.2. Clusters Menu](#3-2-2)
    * [3.2.2.1. Namespaces](#3-2-2-1)
    * [3.2.2.1.1. Namespace List Check](#3-2-2-1-1)
    * [3.2.2.1.2. Check Namespace Details](#3-2-2-1-2)
    * [3.2.2.1.3. Create Namespace](#3-2-2-1-3)
    * [3.2.2.1.4. Modify Namespace](#3-2-2-1-4)
    * [3.2.2.1.5. Delete Namespace](#3-2-2-1-5)
    * [3.2.2.2. Nodes](#3-2-2-2)
    * [3.2.2.2.1. Node List Check](#3-2-2-2-1)
    * [3.2.2.2.2. Node Details Check](#3-2-2-2-2)
    * [3.2.3. Workloads Menu](#3-2-3)
    * [3.2.3.1. Deployments](#3-2-3-1)
    * [3.2.3.1.1. Deployment List Check](#3-2-3-1-1)
    * [3.2.3.1.2. Deployment Details Check](#3-2-3-1-2)
    * [3.2.3.1.3. Create Deployment](#3-2-3-1-3)
    * [3.2.3.1.4. Modify Deployment](#3-2-3-1-4)
    * [3.2.3.1.5. Delete Deployment](#3-2-3-1-5)
    * [3.2.3.2. Pods](#3-2-3-2)
    * [3.2.3.2.1. Pod List Check](#3-2-3-2-1)
    * [3.2.3.2.2. Pod Details Check](#3-2-3-2-2)
    * [3.2.3.2.3. Create Pod](#3-2-3-2-3)
    * [3.2.3.2.4. Modify Pod](#3-2-3-2-4)
    * [3.2.3.2.5. Delete Pod](#3-2-3-2-5)
    * [3.2.3.3. Replica Sets](#3-2-3-3)
    * [3.2.3.3.1. Replica Set List Chek](#3-2-3-3-1)
    * [3.2.3.3.2. Replica Set Details Check](#3-2-3-3-2)
    * [3.2.3.3.3. Create Replica Set](#3-2-3-3-3)
    * [3.2.3.3.4. Modify Replica Set](#3-2-3-3-4)
    * [3.2.3.3.5. Delete Replica Set](#3-2-3-3-5)
    * [3.2.4. Services Menu](#3-2-4)
    * [3.2.4.1. Service List Check](#3-2-4-1)
    * [3.2.4.2. Service Details Check](#3-2-4-2)
    * [3.2.4.3. Create Service](#3-2-4-3)
    * [3.2.4.4. Modify Service](#3-2-4-4)
    * [3.2.4.5. Delete Service](#3-2-4-5)
    * [3.2.5. Storages Menu](#3-2-5)
    * [3.2.5.1. Persistent Volumes](#3-2-5-1)
    * [3.2.5.1.1. Persistent Volume List Check](#3-2-5-1-1)
    * [3.2.5.1.2. Persistent Volume Details Check](#3-2-5-1-2)
    * [3.2.5.1.3. Create Persistent Volume](#3-2-5-1-3)
    * [3.2.5.1.4. Modify Persistent Volume](#3-2-5-1-4)
    * [3.2.5.1.5. Delete Persistent Volume](#3-2-5-1-5)
    * [3.2.5.2. Persistent Volume Claims](#3-2-5-2)
    * [3.2.5.2.1. Persistent Volume Claim List Chek](#3-2-5-2-1)
    * [3.2.5.2.2. Persistent Volume Claim Details Check](#3-2-5-2-2)
    * [3.2.5.2.3. Create Persistent Volume Claim](#3-2-5-2-3)
    * [3.2.5.2.4. Modify Persistent Volume Claim](#3-2-5-2-4)
    * [3.2.5.2.5. Delete Persistent Volume Claim](#3-2-5-2-5)
    * [3.2.5.3. Storage Classes](#3-2-5-3)
    * [3.2.5.3.1. Storage Class List Check](#3-2-5-3-1)
    * [3.2.5.3.2. Storage Class Details Check](#3-2-5-3-2)
    * [3.2.5.3.3. Create Storage Class](#3-2-5-3-3)
    * [3.2.5.3.4. Modify Storage Class](#3-2-5-3-4)
    * [3.2.5.3.5. Delete Storage Class](#3-2-5-3-5)
    * [3.2.6. Managements Menu](#3-2-6)
    * [3.2.6.1. Users](#3-2-6-1)
    * [3.2.6.1.1. Cluster Administrator Check](#3-2-6-1-1)  
    * [3.2.6.1.2. Cluster Administrator Details Check](#3-2-6-1-2)  
    * [3.2.6.1.3. Normal User List Check](#3-2-6-1-3)  
    * [3.2.6.1.4. Normal User Details Check](#3-2-6-1-4)  
    * [3.2.6.1.5. Modify User](#3-2-6-1-5)  
    * [3.2.6.2. Roles](#3-2-6-2)  
    * [3.2.6.2.1. Role List Check](#3-2-6-2-1)
    * [3.2.6.2.2. Role Details Check](#3-2-6-2-2)
    * [3.2.6.2.3. Create Role](#3-2-6-2-3)
    * [3.2.6.2.4. Modify Role](#3-2-6-2-4)
    * [3.2.6.2.5. Delete Role](#3-2-6-2-5)
    * [3.2.6.3. Resource Quotas](#3-2-6-3)
    * [3.2.6.3.1. Resource Quota List Check](#3-2-6-3-1)
    * [3.2.6.3.2. Resource Quota Details Check](#3-2-6-3-2)
    * [3.2.6.3.3. Create Resource Quota](#3-2-6-3-3)
    * [3.2.6.3.4. Modify Resource Quota](#3-2-6-3-4)
    * [3.2.6.3.5. Delete Resource Quota](#3-2-6-3-5)
    * [3.2.6.4. Limit Ranges](#3-2-6-4)
    * [3.2.6.4.1. Limit Range List Check](#3-2-6-4-1)
    * [3.2.6.4.2. Limit Range Details Check](#3-2-6-4-2)
    * [3.2.6.4.3. Create Limit Range](#3-2-6-4-3)
    * [3.2.6.4.4. Modify Limit Range](#3-2-6-4-4)
    * [3.2.6.4.5. Delete Limit Range](#3-2-6-4-5)


<br>


# <div id='1'/> 1. Document Outline

## <div id='1-1'/> 1.1. Purpose
This document describes the usage of the container platform operator portal.

## <div id='1-2'/> 1.2. Range
This document describes how to use the container platform operator portal based on the Windows environment.

<br>


# <div id='2'/> 2. Container Platform Operator Portal Access
The container platform operator portal can be accessed at the address below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Operator Portal Access URI : **http://{K8S_MASTER_NODE_IP}:32703** <br>

<br>

## <div id='2-1'/> 2.1. Container Platform Operator Portal Log in
The initial information on accessing the container platform operator portal is as follows.
- Access to http://{K8S_MASTER_NODE_IP}:32703.   
- Log in to the container platform operator's portal by using the username : **admin** / password : **admin** account.

![IMG_004]

<br>

# <div id='3'/> 3. Container Platform Operator Portal Manual


## <div id='3-1'/> 3.1. Container Platform Operator Portal Menu Configuration
| <center>Menu</center> | <center>Classification</center> | <center>Description</center> |
| :--- | :--- | :--- |
|| Overview | Container Platform Dashboard |
| Clusters | Namespaces | Manages Namespaces Information |
|| Nodes | Manages Nodes Information |
| Workloads | Deployments | Manages Deployments Information |
|| Pods | Manages Pods Information |
|| Replica Sets | Manages Replica Sets Information |
| Services | Services | Manages Services Information |
| Storages | Persistent Volumes | Manages Persistent Volumes Information |
|| Persistent Volume Claims | Manages Persistent Volume Claims Information |
|| Storage Classes | Manages Storage Classes Information |
| Managements | Users | Manages User |
|| Roles | Manages Roles |
|| Resource Quotas | Manages Resource Quotas Information |
|| Limit Ranges | Manages Limit Ranges Information |

<br>

## <div id='3-2'/> 3.2. Container Platform Operator Portal Menu Description
This chapter describes the menu of the container platform operator portal.

### <div id='3-2-1'/> 3.2.1. Overview
#### <div id='3-2-1-1'/> 3.2.1.1. Overview List Chek
- Look up the number of Namespace, Deployment, Pod, User, and charts of Deployment, Pod, and ReplicaSet.

1. After logging in, the first screen that appears is the Overview page.

![IMG_005]

#### <div id='3-2-1-2'/> 3.2.1.2. Overview Namespace Change
- Click Select Box at the top of the screen and select the desired Namespace from the full Namespace list.

1. When you select Namespace in the Select Box, the Overview information for the Namespace is queried.

![IMG_006]
![IMG_007]

### <div id='3-2-2'/> 3.2.2. Clusters Menu
#### <div id='3-2-2-1'/> 3.2.2.1. Namespaces
##### <div id='3-2-2-1-1'/> 3.2.2.1.1. Namespace List Check
1. Click Namespace in the Clusters menu to go to the Namespace list page.

![IMG_009]

##### <div id='3-2-2-1-2'/> 3.2.2.1.2. Check Namespace Details 
1. Click the Namespace name in the Namespace list to go to the Namespace detail page.

#### Namespace Details Page
![IMG_010]

##### <div id='3-2-2-1-3'/> 3.2.2.1.3. Create Namespace 
- Can specify the appropriate **Operator of Namespace**, **Resource Quotas**, **Limit Ranges** on the Create Namespace page.

1. Click the Create button in the Namespace list to go to the Namespace creation page.

#### Namespace List Page
![IMG_011]

#### Namespace Creating Page
- **Admin User** allows you to specify a corresponding Namespace administrator.

![IMG_012]
![IMG_013]
![IMG_014]
![IMG_015]
![IMG_016]

##### <div id='3-2-2-1-4'/> 3.2.2.1.4. Modify Namespace
- Can modify the appropriate **Operator of Namespace**, **Resource Quotas**, **Limit Ranges** on the Modify Namespace page.

1. Click the Modify button in Namespace Details to go to the Modify Namespace page.

#### Namespace Details Page
![IMG_017]

#### Namespace Modifying Page
- Can change the corresponding Namespace manager through the **Admin User**.

![IMG_018]
![IMG_019]

##### <div id='3-2-2-1-5'/> 3.2.2.1.5. Delete Namespace
1. Click the Delete button in the Namespace details to delete Namespace.

#### Namespace Details Page
![IMG_020]

#### Namespace Delete Pop-up Screen
![IMG_021]
![IMG_022]

#### <div id='3-2-2-2'/> 3.2.2.2. Nodes
##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Node List Check
1. Click Nodes in the Clusters menu to go to the Node List page.

![IMG_023]

##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Node Details Check
1. Click the Node name in the Node list to go to the Node Details page.
#### Node Details Page

![IMG_024]

### <div id='3-2-3'/> 3.2.3. Workloads Menu
#### <div id='3-2-3-1'/> 3.2.3.1 Deployment
##### <div id='3-2-3-1-1'/> 3.2.3.1.1. Deployment List Check
1. Click Deployments in Workloads to go to the Deployment List page.

![IMG_025]

##### <div id='3-2-3-1-2'/> 3.2.3.1.2. Deployment Details Check
1. In the Deployment list, click the Deployment name to proceed to the Deployment Details page.

#### Deployment Details Page
![IMG_026]

##### <div id='3-2-3-1-3'/> 3.2.3.1.3. Create Deployment
1. When you click the Create button in the Deployment list, the Create Deployment pop-up window appears.

#### Deployment List Page
![IMG_027]

#### Create Deployment Pop-up Screen
![IMG_028]

##### <div id='3-2-3-1-4'/> 3.2.3.1.4. Modify Deployment
1. Click the Modify button in Deployment Details, the Deployment Modify pop-up window appears.

#### Deployment Details page
![IMG_029]

####  Deployment Modifying Pop-up Screen
![IMG_030]

##### <div id='3-2-3-1-5'/> 3.2.3.1.5. Delete Deployment
1. Click the Delete button in Deployment Details, Deployment is deleted.

#### Deployment Details Page
![IMG_031]

#### Deployment Deleting Pop-up Screen
![IMG_032]

#### <div id='3-2-3-2'/> 3.2.3.2. Pods
##### <div id='3-2-3-2-1'/> 3.2.3.2.1. Pod List Check
1.Click Pods in the Workloads to go to the Pod list page.

![IMG_033]

##### <div id='3-2-3-2-2'/> 3.2.3.2.2. Pod Details Check
1. Click the Pod name in the Pod list to go to the Pod detail page.

#### Pod Details Page
![IMG_034]

##### <div id='3-2-3-2-3'/> 3.2.3.2.3. Create Pod 
1. Click the create button from the Pod list and the pop creating pop-up screen appears.

#### Pod List Page
![IMG_035]

#### Pod Creating Pop-up Screen
![IMG_036]

##### <div id='3-2-3-2-4'/> 3.2.3.2.4. Modify Pod 
1. click the Modify button in the Pod details, the Pod Modify pop-up screen appears.

#### Pod Details Page
![IMG_037]

#### Pod Modifying Pop-up Screen
![IMG_038]

##### <div id='3-2-3-2-5'/> 3.2.3.2.5. Delete Pod
1. If you click the Delete button in the Pod details, the Pod deletion is completed.

#### Pod Details Page
![IMG_039]

#### Pod Deletion Pop-up Screen
![IMG_040]


#### <div id='3-2-3-3'/> 3.2.3.3. Replica Sets
##### <div id='3-2-3-3-1'/> 3.2.3.3.1. Replica Set List Check
1. Click Replica Sets in the Workloads to go to the Replica Set list page.

![IMG_041]

##### <div id='3-2-3-3-2'/> 3.2.3.3.2. Replica Set Details Check
1. Click the Replica Set name to go to the Replica Set details page from the Replica Set List.

#### Replica Set Details Page
![IMG_042]

##### <div id='3-2-3-3-3'/> 3.2.3.3.3. Create Replica Set
1. Click the Create button in the Replica Set list and the Replica Set creating pop-up window appears.

#### Replica Set List Page
![IMG_043]

#### Replica Set Creating Pop-up Screen
![IMG_044]

##### <div id='3-2-3-3-4'/> 3.2.3.3.4. Modify Replica Set
1. Click the Modify button in the Replica Set details, a pop-up window for modifying the Replica Set appears.

#### Replica Set Details Page
![IMG_045]

#### Replica Set Modifying Pop-up Screen
![IMG_046]

##### <div id='3-2-3-3-5'/> 3.2.3.3.5. Delete Replica Set
1. Click the Delete button in the Replica Set details to delete the Replica Set.

#### Replica Set Details Page
![IMG_047]

#### Replica Set Deleting Pop-up Screen
![IMG_048]

### <div id='3-2-4'/> 3.2.4. Services Menu
#### <div id='3-2-4-1'/> 3.2.4.1. Service List Check
1. Click Services to proceed to the Service List page.

![IMG_049]

#### <div id='3-2-4-2'/> 3.2.4.2. Service Details Check
1. Click the Service name in the Service list to go to the Service Details page.

#### Service Details Page
![IMG_050]

#### <div id='3-2-4-3'/> 3.2.4.3. Create Service
1. Click the Create button in the Service list, a service creating pop-up window appears.

#### Service List Page
![IMG_051]

#### Service Creating Pop-up Screen
![IMG_052]

#### <div id='3-2-4-4'/> 3.2.4.4. Modify Service
1. Click the Modify button in the Service Details, the Service Modifying pop-up window appears.

#### Service Details Page
![IMG_053]

#### Service Modifying Pop-up Screen
![IMG_054]

#### <div id='3-2-4-5'/> 3.2.4.5. Delete Service

1. Click the Delete button in the Service Details then the service gets deleted.

#### Service Details Page
![IMG_055]

#### Service Deleting Pop-up Screen
![IMG_056]

### <div id='3-2-5'/> 3.2.5. Storages Menu
#### <div id='3-2-5-1'/> 3.2.5.1. Persistent Volumes
##### <div id='3-2-5-1-1'/> 3.2.5.1.1. Persistent Volume List Check
1. Click Persistent Volumes in Storages to proceed to the Persistent Volumes list page.

![IMG_057]

##### <div id='3-2-5-1-2'/> 3.2.5.1.2. Persistent Volume Details Check
1.  Click the Persistent Volume name from the Persistent Volume List to go to the Persistent Volume details page.

#### Persistent Volume Details Page
![IMG_058]

##### <div id='3-2-5-1-3'/> 3.2.5.1.3. Create Persistent Volume
1.  Click the Create button in the Persistent Volume list then the Persistent Volume Creation pop-up window appears.

#### Persistent Volume List Page
![IMG_059]

#### Persistent Volume Creating Pop-up Screen
![IMG_060]


##### <div id='3-2-5-1-4'/> 3.2.5.1.4. Modify Persistent Volume
1. Click the Modify button in the Persistent Volume Details then a Persistent Volume Modifying pop-up window appears.

#### Persistent Volume Details Page
![IMG_061]

#### Persistent Volume Modifying Pop-up Screen
![IMG_062]

##### <div id='3-2-5-1-5'/> 3.2.5.1.5. Delete Persistent Volume
1. Click the Delete button in the Persistent Volume details to delete the Persistent Volume.

#### Persistent Volume Details Page
![IMG_063]

#### Persistent Volume Deleting Pop-up Screen
![IMG_064]

#### <div id='3-2-5-2'/> 3.2.5.2. Persistent Volume Claims
##### <div id='3-2-5-2-1'/> 3.2.5.2.1. Persistent Volume Claim List Check
1. Click Persistent Volume Claims in Storages to proceed to the Persistent Volume Claim list page.

![IMG_065]

##### <div id='3-2-5-2-2'/> 3.2.5.2.2. Persistent Volume Claim Details Check
1. In the Persistent Volume Claim list, click the Persistent Volume Claim name to go to the Persistent Volume Claim details page.

#### Persistent Volume Claim Details Page
![IMG_066]

##### <div id='3-2-5-2-3'/> 3.2.5.2.3. Create Persistent Volumee Claim
1. Click the Create button in the Persistent Volume Claim list then a Persistent Volume Claim Creating pop-up window appears.

#### Persistent Volume Claim List Page
![IMG_067]

#### Persistent Volume Claim Creating Pop-up Screen
![IMG_068]

##### <div id='3-2-5-2-4'/> 3.2.5.2.4. Modify Persistent Volume Claim
1. Click the Modify button in the Persistent Volume Claim Details then  the Persistent Volume Claim Modifying pop-up window appears.

#### Persistent Volume Claim Details Page
![IMG_069]

#### Persistent Volume Claim Modifying Pop-up Screen
![IMG_070]

##### <div id='3-2-5-2-5'/> 3.2.5.2.5. Delete Persistent Volume Claim
1. Click the Delete button in the Persistent Volume Claim details to delete the Persistent Volume Claim.

#### Persistent Volume Claim Details Page
![IMG_071]

#### Persistent Volume Claim Deleting Pop-up Screen
![IMG_072]

#### <div id='3-2-5-3'/> 3.2.5.3. Storage Classes
##### <div id='3-2-5-3-1'/> 3.2.5.3.1. Storage Class List Check
1. Click Storage Classes in Storages to go to the Storage Classes list page.
![IMG_073]

##### <div id='3-2-5-3-2'/> 3.2.5.3.2. Storage Class Details Check
1. In the Storage Class list, click the Storage Class name to go to the Storage Class details page.

#### Storage Class Details Page
![IMG_074]

##### <div id='3-2-5-3-3'/> 3.2.5.3.3. Create Storage Class
1. Click the Create button in the Storage Class list, then a Storage Class Creating pop-up window appears.

#### Storage Class List Page
![IMG_075]

#### Storage Class Creating Pop-up Screen
![IMG_076]

##### <div id='3-2-5-3-4'/> 3.2.5.3.4. Modify Storage Class
1. Click the Modify button in Storage Class Details then a Storage Class Modifying pop-up window appears.

#### Storage Class Details Page
![IMG_077]

#### Storage Class Modifying Pop-up Screen
![IMG_078]

##### <div id='3-2-5-3-5'/> 3.2.5.3.5. Delete Storage Class
1. Click the Delete button in the Storage Class details and the Storage Class gets deleted.

#### Storage Class Details Page
![IMG_079]

#### Storage Class Deleting Pop-up Screen
![IMG_080]

### <div id='3-2-6'/> 3.2.6. Managements
#### <div id='3-2-6-1'/> 3.2.6.1. Users
##### <div id='3-2-6-1-1'/> 3.2.6.1.1. Cluster Administrator Check
1. Select Users from the Management menu and click the Administrator tab to view the cluster administrator.

![IMG_081]

##### <div id='3-2-6-1-2'/> 3.2.6.1.2. Cluster Administrator Details Check
1.  Click the Cluster Administrator User ID to proceed to the Cluster Administrator Details page..

#### Cluster Administrator Details Check Page
![IMG_082]

##### <div id='3-2-6-1-3'/> 3.2.6.1.3.  Normal User List Check
1. Select Users in the Management menu and click the User tab to view the list of users.
  + Active Tab : List of users assigned with Namespace/Role
  + Inactive Tab : List of users unassigned with Namespace/Role

![IMG_083]

##### <div id='3-2-6-1-4'/> 3.2.6.1.4. Normal User Details Check
1.  Click the Normal User ID to prcoeed to the Normal User Detail Check page.

####  Normal User Detals Check Page
![IMG_084]

##### <div id='3-2-6-1-5'/> 3.2.6.1.5. Modify User
- Modify the Namespace and Role that the user will use from the User Modify page.

1. Click the Modify button in the User Details to go to the User Modifying page.

#### User Details Page
![IMG_086]

#### User Modifying Page
![IMG_087]

- Select the Namespace and Role to assign to the user. 

![IMG_088]
![IMG_089]
![IMG_090]

#### <div id='3-2-6-2'/> 3.2.6.2. Roles
##### <div id='3-2-6-2-1'/> 3.2.6.2.1. Role List Check
1. Click the Roles from the Managements Menu and proceed to the Role List page.
![IMG_091]

##### <div id='3-2-6-2-2'/> 3.2.6.2.2. Role Details Check
1. Click the Role Name from Role List to proceed to the Role Details Page.

#### Role Details Page
![IMG_092]

##### <div id='3-2-6-2-3'/> 3.2.6.2.3. Create Role
1. Click the Create button from the Role List and a Role Creating Pop-up screen appears.

#### Role List Page
![IMG_093]

#### Role Creating Pop-up Screen
![IMG_094]

##### <div id='3-2-6-2-4'/> 3.2.6.2.4. Modify Role
1. Click the Modify button in the Role Details, then the Role Modifying pop-up window appears.

#### Role Deatails Page
![IMG_095]

#### Role Modifying Pop-up Screen
![IMG_096]

##### <div id='3-2-6-2-5'/> 3.2.6.2.5. Delete Role
1. Click the Delete button in the Role details to delete the role.

#### Role Details Page
![IMG_097]

#### Role Deleting Pop-up Screen
![IMG_098]

#### <div id='3-2-6-3'/> 3.2.6.3. Resource Quotas
##### <div id='3-2-6-3-1'/> 3.2.6.3.1. Resource Quota List Check
1. Click Resource Quotas in the Management menu to go to the Resource Quota list page.
![IMG_099]

##### <div id='3-2-6-3-2'/> 3.2.6.3.2. Resource Quota Details Check
1. In the Resource Quota list, click the Resource Quota name to go to the Resource Quota Details page.

#### Resource Quota Details Page
![IMG_100]

##### <div id='3-2-6-3-3'/> 3.2.6.3.3. Create Resource Quota
1. Click the Create button in the Resource Quota list, then a Resource Quota Creating pop-up window appears.

#### Resource Quota List Page
![IMG_101]

#### Resource Quota Creating Pop-up Screen
![IMG_102]

##### <div id='3-2-6-3-4'/> 3.2.6.3.4. Modify Resource Quota
1. Click the Modify button in the Resource Quota Details, then a Resource Quota Modifying pop-up window appears.

#### Resource Quota Details Page
![IMG_103]

#### Resource Quota Modifying Pop-up Screen
![IMG_104]

##### <div id='3-2-6-3-5'/> 3.2.6.3.5. Delete Resource Quota
1. Click the Delete button in the Resource Quota Details to delete the Resource Quota.

#### Resource Quota Details Page
![IMG_105]

#### Resource Quota Delete Pop-up Screen
![IMG_106]

#### <div id='3-2-6-4'/> 3.2.6.4. Limit Ranges
##### <div id='3-2-6-4-1'/> 3.2.6.4.1. Limit Range List Check
1. Click Limit Ranges in the Management menu to go to the Limit Range list page.
![IMG_107]

##### <div id='3-2-6-4-2'/> 3.2.6.4.2. Limit Range Details Check
1. Click the Limit Range name from the Limit Range list to go to the Limit Range detail page.

#### Limit Range Details Page
![IMG_108]

##### <div id='3-2-6-4-3'/> 3.2.6.4.3. Create Limit Range
1. Click the Create button in the Limit Range list then a Limit Range creating pop-up window appears.

#### Limit Range List Page
![IMG_109]

#### Limit Range Creating Pop-up Screen
![IMG_110]

##### <div id='3-2-6-4-4'/> 3.2.6.4.4. Modify Limit Range
1. Click the Modify button in the Limit Range details then a Limit Range Modifying pop-up window appears.

#### Limit Range Details Page
![IMG_111]

#### Limit Range Modifying Pop-up Screen
![IMG_112]

##### <div id='3-2-6-4-5'/> 3.2.6.4.5. Delete Limit Range
1. Click the Delete button in the Limit Range details to delete the Limit Range.

#### Limit Range Details Page
![IMG_113]

#### Limit Range Deleting Pop-up Screen
![IMG_114]

<br>

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > Operator Portal Use Guide

----
[IMG_001]:../images/v1.2/container-platform-portal/admin/cp-001.png
[IMG_002]:../images/v1.2/container-platform-portal/admin/cp-002.png
[IMG_003]:../images/v1.2/container-platform-portal/admin/cp-003.png
[IMG_004]:../images/v1.2/container-platform-portal/admin/cp-004.png
[IMG_005]:../images/v1.2/container-platform-portal/admin/cp-005.png
[IMG_006]:../images/v1.2/container-platform-portal/admin/cp-006.png
[IMG_007]:../images/v1.2/container-platform-portal/admin/cp-007.png
[IMG_008]:../images/v1.2/container-platform-portal/admin/cp-008.png
[IMG_009]:../images/v1.2/container-platform-portal/admin/cp-009.png
[IMG_010]:../images/v1.2/container-platform-portal/admin/cp-010.png
[IMG_011]:../images/v1.2/container-platform-portal/admin/cp-011.png
[IMG_012]:../images/v1.2/container-platform-portal/admin/cp-012.png
[IMG_013]:../images/v1.2/container-platform-portal/admin/cp-013.png
[IMG_014]:../images/v1.2/container-platform-portal/admin/cp-014.png
[IMG_015]:../images/v1.2/container-platform-portal/admin/cp-015.png
[IMG_016]:../images/v1.2/container-platform-portal/admin/cp-016.png
[IMG_017]:../images/v1.2/container-platform-portal/admin/cp-017.png
[IMG_018]:../images/v1.2/container-platform-portal/admin/cp-018.png
[IMG_019]:../images/v1.2/container-platform-portal/admin/cp-019.png
[IMG_020]:../images/v1.2/container-platform-portal/admin/cp-020.png
[IMG_021]:../images/v1.2/container-platform-portal/admin/cp-021.png
[IMG_022]:../images/v1.2/container-platform-portal/admin/cp-022.png
[IMG_023]:../images/v1.2/container-platform-portal/admin/cp-023.png
[IMG_024]:../images/v1.2/container-platform-portal/admin/cp-024.png
[IMG_025]:../images/v1.2/container-platform-portal/admin/cp-025.png
[IMG_026]:../images/v1.2/container-platform-portal/admin/cp-026.png
[IMG_027]:../images/v1.2/container-platform-portal/admin/cp-027.png
[IMG_028]:../images/v1.2/container-platform-portal/admin/cp-028.png
[IMG_029]:../images/v1.2/container-platform-portal/admin/cp-029.png
[IMG_030]:../images/v1.2/container-platform-portal/admin/cp-030.png
[IMG_031]:../images/v1.2/container-platform-portal/admin/cp-031.png
[IMG_032]:../images/v1.2/container-platform-portal/admin/cp-032.png
[IMG_033]:../images/v1.2/container-platform-portal/admin/cp-033.png
[IMG_034]:../images/v1.2/container-platform-portal/admin/cp-034.png
[IMG_035]:../images/v1.2/container-platform-portal/admin/cp-035.png
[IMG_036]:../images/v1.2/container-platform-portal/admin/cp-036.png
[IMG_037]:../images/v1.2/container-platform-portal/admin/cp-037.png
[IMG_038]:../images/v1.2/container-platform-portal/admin/cp-038.png
[IMG_039]:../images/v1.2/container-platform-portal/admin/cp-039.png
[IMG_040]:../images/v1.2/container-platform-portal/admin/cp-040.png
[IMG_041]:../images/v1.2/container-platform-portal/admin/cp-041.png
[IMG_042]:../images/v1.2/container-platform-portal/admin/cp-042.png
[IMG_043]:../images/v1.2/container-platform-portal/admin/cp-043.png
[IMG_044]:../images/v1.2/container-platform-portal/admin/cp-044.png
[IMG_045]:../images/v1.2/container-platform-portal/admin/cp-045.png
[IMG_046]:../images/v1.2/container-platform-portal/admin/cp-046.png
[IMG_047]:../images/v1.2/container-platform-portal/admin/cp-047.png
[IMG_048]:../images/v1.2/container-platform-portal/admin/cp-048.png
[IMG_049]:../images/v1.2/container-platform-portal/admin/cp-049.png
[IMG_050]:../images/v1.2/container-platform-portal/admin/cp-050.png
[IMG_051]:../images/v1.2/container-platform-portal/admin/cp-051.png
[IMG_052]:../images/v1.2/container-platform-portal/admin/cp-052.png
[IMG_053]:../images/v1.2/container-platform-portal/admin/cp-053.png
[IMG_054]:../images/v1.2/container-platform-portal/admin/cp-054.png
[IMG_055]:../images/v1.2/container-platform-portal/admin/cp-055.png
[IMG_056]:../images/v1.2/container-platform-portal/admin/cp-056.png
[IMG_057]:../images/v1.2/container-platform-portal/admin/cp-057.png
[IMG_058]:../images/v1.2/container-platform-portal/admin/cp-058.png
[IMG_059]:../images/v1.2/container-platform-portal/admin/cp-059.png
[IMG_060]:../images/v1.2/container-platform-portal/admin/cp-060.png
[IMG_061]:../images/v1.2/container-platform-portal/admin/cp-061.png
[IMG_062]:../images/v1.2/container-platform-portal/admin/cp-062.png
[IMG_063]:../images/v1.2/container-platform-portal/admin/cp-063.png
[IMG_064]:../images/v1.2/container-platform-portal/admin/cp-064.png
[IMG_065]:../images/v1.2/container-platform-portal/admin/cp-065.png
[IMG_066]:../images/v1.2/container-platform-portal/admin/cp-066.png
[IMG_067]:../images/v1.2/container-platform-portal/admin/cp-067.png
[IMG_068]:../images/v1.2/container-platform-portal/admin/cp-068.png
[IMG_069]:../images/v1.2/container-platform-portal/admin/cp-069.png
[IMG_070]:../images/v1.2/container-platform-portal/admin/cp-070.png
[IMG_071]:../images/v1.2/container-platform-portal/admin/cp-071.png
[IMG_072]:../images/v1.2/container-platform-portal/admin/cp-072.png
[IMG_073]:../images/v1.2/container-platform-portal/admin/cp-073.png
[IMG_074]:../images/v1.2/container-platform-portal/admin/cp-074.png
[IMG_075]:../images/v1.2/container-platform-portal/admin/cp-075.png
[IMG_076]:../images/v1.2/container-platform-portal/admin/cp-076.png
[IMG_077]:../images/v1.2/container-platform-portal/admin/cp-077.png
[IMG_078]:../images/v1.2/container-platform-portal/admin/cp-078.png
[IMG_079]:../images/v1.2/container-platform-portal/admin/cp-079.png
[IMG_080]:../images/v1.2/container-platform-portal/admin/cp-080.png
[IMG_081]:../images/v1.2/container-platform-portal/admin/cp-081.png
[IMG_082]:../images/v1.2/container-platform-portal/admin/cp-082.png
[IMG_083]:../images/v1.2/container-platform-portal/admin/cp-083.png
[IMG_084]:../images/v1.2/container-platform-portal/admin/cp-084.png
[IMG_085]:../images/v1.2/container-platform-portal/admin/cp-085.png
[IMG_086]:../images/v1.2/container-platform-portal/admin/cp-086.png
[IMG_087]:../images/v1.2/container-platform-portal/admin/cp-087.png
[IMG_088]:../images/v1.2/container-platform-portal/admin/cp-088.png
[IMG_089]:../images/v1.2/container-platform-portal/admin/cp-089.png
[IMG_090]:../images/v1.2/container-platform-portal/admin/cp-090.png
[IMG_091]:../images/v1.2/container-platform-portal/admin/cp-091.png
[IMG_092]:../images/v1.2/container-platform-portal/admin/cp-092.png
[IMG_093]:../images/v1.2/container-platform-portal/admin/cp-093.png
[IMG_094]:../images/v1.2/container-platform-portal/admin/cp-094.png
[IMG_095]:../images/v1.2/container-platform-portal/admin/cp-095.png
[IMG_096]:../images/v1.2/container-platform-portal/admin/cp-096.png
[IMG_097]:../images/v1.2/container-platform-portal/admin/cp-097.png
[IMG_098]:../images/v1.2/container-platform-portal/admin/cp-098.png
[IMG_099]:../images/v1.2/container-platform-portal/admin/cp-099.png
[IMG_100]:../images/v1.2/container-platform-portal/admin/cp-100.png
[IMG_101]:../images/v1.2/container-platform-portal/admin/cp-101.png
[IMG_102]:../images/v1.2/container-platform-portal/admin/cp-102.png
[IMG_103]:../images/v1.2/container-platform-portal/admin/cp-103.png
[IMG_104]:../images/v1.2/container-platform-portal/admin/cp-104.png
[IMG_105]:../images/v1.2/container-platform-portal/admin/cp-105.png
[IMG_106]:../images/v1.2/container-platform-portal/admin/cp-106.png
[IMG_107]:../images/v1.2/container-platform-portal/admin/cp-107.png
[IMG_108]:../images/v1.2/container-platform-portal/admin/cp-108.png
[IMG_109]:../images/v1.2/container-platform-portal/admin/cp-109.png
[IMG_110]:../images/v1.2/container-platform-portal/admin/cp-110.png
[IMG_111]:../images/v1.2/container-platform-portal/admin/cp-111.png
[IMG_112]:../images/v1.2/container-platform-portal/admin/cp-112.png
[IMG_113]:../images/v1.2/container-platform-portal/admin/cp-113.png
[IMG_114]:../images/v1.2/container-platform-portal/admin/cp-114.png