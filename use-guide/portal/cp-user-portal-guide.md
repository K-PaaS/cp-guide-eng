### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > User Portal Use Guide

<br>

## Table of Contents

1. [Document Outline](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Range](#1-2)
2. [Container Platform Platform User Portal Access](#2)
    * [2.1. Container Platform User Portal Sign in](#2-1)
    * [2.2. Container Platform User Portal Log in](#2-2)    
3. [Container Platform User Portal Manual](#3)
    * [3.1. Container Platform User Portal Menu Configuration](#3-1)
    * [3.2. Container Platform User Portal Menu Description](#3-2)
    * [3.2.1. Intro](#3-2-1)
    * [3.2.1.1. Overview](#3-2-1-1)
    * [3.2.1.2. Access](#3-2-1-2)
    * [3.2.1.3. Private Repository](#3-2-1-3)
    * [3.2.2. Namespaces](#3-2-1-3)
    * [3.2.2.1. Namespace Details Check](#3-2-2-1)
    * [3.2.3. Nodes](#3-2-3)
    * [3.2.3.1. Node Details Check](#3-2-3-1)
    * [3.2.4. Users](#3-2-2)
    * [3.2.4.1. User List Check](#3-2-4-1)
    * [3.2.4.2. User Role Change](#3-2-4-2)
    * [3.2.5. Roles](#3-2-5)
    * [3.2.5.1 Role List Check](#3-2-5-1)
    * [3.2.5.2 Role Details Check](#3-2-5-2)
    * [3.2.6. Workloads](#3-2-6)
    * [3.2.6.1. Deployments](#3-2-6-1)
    * [3.2.6.1.1. Deployment List Check](#3-2-6-1-1)
    * [3.2.6.1.2. Deployment Details Check](#3-2-6-1-2)
    * [3.2.6.1.3. Create Deployment](#3-2-6-1-3)
    * [3.2.6.1.4. Modify Deployment](#3-2-6-1-4)
    * [3.2.6.1.5. Delete Deployment](#3-2-6-1-5)
    * [3.2.6.2. Pods](#3-2-6-2)
    * [3.2.6.2.1. Pod List Check](#3-2-6-2-1)
    * [3.2.6.2.2. Pod Details Check](#3-2-6-2-2)
    * [3.2.6.2.3. Create Pod](#3-2-6-2-3)
    * [3.2.6.2.4. Modify Pod](#3-2-6-2-4)
    * [3.2.6.2.5. Delete Pod](#3-2-6-2-5)
    * [3.2.6.3. Replica Sets](#3-2-6-3)
    * [3.2.6.3.1. Replica Set List Check](#3-2-6-3-1)
    * [3.2.6.3.2. Replica Set Details Check](#3-2-6-3-2)
    * [3.2.6.3.3. Create Replica Set](#3-2-6-3-3)
    * [3.2.6.3.4. Modify Replica Set](#3-2-6-3-4)
    * [3.2.6.3.5. Delete Replica Set](#3-2-6-3-5)
    * [3.2.7. Services](#3-2-7)
    * [3.2.7.1. Service List Check](#3-2-7-1)
    * [3.2.7.2. Service Details Check](#3-2-7-2)
    * [3.2.7.3. Create Service](#3-2-7-3)
    * [3.2.7.4. Modify Service](#3-2-7-4)
    * [3.2.7.5. Delete Service](#3-2-7-5)
    * [3.2.8. Storages](#3-2-8)
    * [3.2.8.1. Persistent Volume Claim List Check](#3-2-8-1)
    * [3.2.8.2. Persistent Volume Claim Details Check](#3-2-8-2)
    * [3.2.8.3. Create Persistent Volume Claim](#3-2-8-3)
    * [3.2.8.4. Modify Persistent Volume Claim](#3-2-8-4)
    * [3.2.8.5. Delete Persistent Volume Claim](#3-2-8-5)


----

# <div id='1'/> 1. Document Outline

## <div id='1-1'/> 1.1. Purpose
This document describes how to use the container platform user portal.

## <div id='1-2'/> 1.2. Range
This document describes how to use the container platform user portal based on the Windows environment.

<br>


# <div id='2'/> 2. Container Platform Platform User Portal Access
The container platform user portal can be accessed at the address below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Platform User Portal Access URI : **http://{K8S_MASTER_NODE_IP}:32702** <br>

<br>

### <div id='2-1'/> 2.1. Container Platform User Portal Sign in

#### User Sign in    
- Access to http://{K8S_MASTER_NODE_IP}:32702.
- Click 'Register' button below.

![IMG_001]


- After entering the user account information to be registered, click the 'Register' button to register as a member of the container platform user portal.

![IMG_002]

- The container platform user portal is not available immediately after members sign in. The portal can be used after receiving the Namespace and Role to be used by the user from the Cluster Manager or Namespace manager.   

![IMG_003]


### <div id='2-2'/> 2.2. Container Platform User Portal Log in
- Access to http://{K8S_MASTER_NODE_IP}:32702.
- Log in to the container platform user portal with a registered account through a member sign in.

![IMG_108]

<br>

## <div id='3'/> 3. Container Platform User Portal Manual


## <div id='3-1'/> 3.1. Container Platform User Portal Menu Configuration
| <center>Menu</center> | <center>Classification</center> | <center>Description</center> |
| :--- | :--- | :--- |
| Intro | Overview | Container Platform User Portal Dashboard |
|| Access | Managing configuration information for using the Container Platform User Portal CLI |
|| Private Repository | Manage setup information for deploying and using Private Repository on containers |
| Namespaces | Namespaces | Namespaces Check |
| Nodes | Nodes | Nodes check |
| Users | Users | Manage User |
| Roles | Roles | Roles check |
| Workloads | Overview | Workloads Dashboard |
|| Deployments | Deployments information management |
|| Pods | Pods information management |
|| Replica Sets | Replica Sets information management |
| Services | Services | Services information management |
| Storages | Persistent Volume Claims | Persistent Volume Claims information management |



## <div id='3-2'/> 3.2. Container Platform User Portal Menu Description
This chapter describes the menu of the container platform user portal.

### <div id='3-2-1'/> 3.2.1. Intro

#### <div id='3-2-1-1'/> 3.2.1.1. Overview
- Check the Namespace information, Resource Quota information, Limit Range information.

1. Proceed to the Overview page by clicking the Overview tab of the Intro.

![IMG_004]


#### <div id='3-2-1-2'/> 3.2.1.2. Access
- The configuration setting information for using the CLI of the container platform is checked.

1. Click the Access tab of the Intro to go to the Access page.

![IMG_005]


#### <div id='3-2-1-3'/> 3.2.1.3. Private Repository
- Includes information and access links from the Private Repository.

1. Click the Intro's Private Repository tab to go to the Private Repository page.

![IMG_006]

### <div id='3-2-2'/> 3.2.2. Namespaces
#### <div id='3-2-2-1'/> 3.2.2.1. Namespace Details Check

- The Namespace detail page consists of the Details, Events tab.

1. Click Namespace Name to go to the Namespace detail page from the Overview tab of the Intro.

#### Overview Tab of Intro
![IMG_007]

#### Namespace Details Page
![IMG_009]
![IMG_010]

### <div id='3-2-3'/> 3.2.3. Nodes
#### <div id='3-2-3-1'/> 3.2.3.1. Node Details Check

- The Node detail page consists of Summary, Details, and Events tabs.

1. Click the Node name in the Pods list to go to the Node Details page.
#### Pod List
![IMG_011]

#### Node Details Page
![IMG_012]
![IMG_013]
![IMG_014]


### <div id='3-2-4'/> 3.2.4. Users
-  User account information and Role using the user portal are managed.

| <center>User Type</center> | <center>Function</center> |
| :--- | :--- |
| Cluster Operator | Access to operator portal, assign user Namespace/Role  |
| Namespace Operator |Access to operator portal, assign user Namespace/Role  |
| Normal User | Access to operator portal|

- Cluster administrators can assign Namespace/Role to users. The task is available on the operator portal.
- The Namespace administrator can change the role of the Namespace user he manages and assign access to users who do not use the Namespace.

#### <div id='3-2-4-1'/> 3.2.4.1. User List Check
1. Click the upper right icon and access the menu 'Users' to view the list of users in the corresponding Namespace.

![IMG_015]
![IMG_017]


#### <div id='3-2-4-2'/> 3.2.4.2. User Role Change
1. Changing of the User's Role can be done only by the **Namespace operator**.

#### User List Page
![IMG_018]

#### User Role Setting Page
![IMG_019]

-  Change the Role of the corresponding Namespace user or select the checkbox to the left of the corresponding Namespace non-user User ID (assign access) and click the Save button.

![IMG_020]
![IMG_021]

### <div id='3-2-5'/> 3.2.5. Roles
#### <div id='3-2-5-1'/> 3.2.5.1. Role List Check

1.  Check the Role List.

![IMG_022]


#### <div id='3-2-5-2'/> 3.2.5.2. Role Details Check

- The Role detail page consists of Details, Events, and YAML tabs.

1. Click the Role name in the Role list to go to the Role Details page.

#### Role List Page
![IMG_023]

#### Role Details Page
![IMG_024]
![IMG_025]
![IMG_026]

### <div id='3-2-6'/> 3.2.6. Workloads
-  Check the Overview, Deployments, Pods, and Replica Sets List.

![IMG_027]

#### <div id='3-2-6-1'/> 3.2.6.1. Deployments

##### <div id='3-2-6-1-1'/> 3.2.6.1.1. Deployment List Check
1. Click the Deployment tab of Workloads to go to the Deployment list page.

![IMG_028]

##### <div id='3-2-6-1-2'/> 3.2.6.1.2. Deployment Details Check
- The Deployment Details page consists of the Details, Events, and YAML tabs.


1. Click the Deployment name in the Deployment list to go to the Deployment Details page.

#### Deployment List Page
![IMG_029]

#### Deployment Details Page
![IMG_030]
![IMG_031]
![IMG_032]

##### <div id='3-2-6-1-3'/> 3.2.6.1.3. Create Deployment
1. Click the Create button in the Deployment list to go to the Create Deployment page.

#### Deployment List Page
![IMG_033]

#### Deployment Creating Page
![IMG_034]
![IMG_035]
![IMG_036]


##### <div id='3-2-6-1-4'/> 3.2.6.1.4. Modify Deployment
1. Click the Modify button in Deployment Details to go to the Modify Deployment page.

#### Deployment Details Page
![IMG_037]

#### Deployment Modifying Page
![IMG_038]
![IMG_039]
![IMG_040]

##### <div id='3-2-6-1-5'/> 3.2.6.1.5. Delete Deployment
1. When you click the Delete button in Deployment Details, the Delete Deployment pop-up window appears.

#### Deployment Details Page
![IMG_041]

#### Deployment Delete Pop-up Screen
![IMG_042]
![IMG_043]

#### <div id='3-2-6-2'/> 3.2.6.2. Pods

##### <div id='3-2-6-2-1'/> 3.2.6.2.1. Pod List Check
1. Click the Pods tab of the Workloads to go to the Pod list page.

![IMG_044]

##### <div id='3-2-6-2-2'/> 3.2.6.2.2. Pod Details Check
- The Pod detail page consists of Details, Events, and YAML tabs.

1. Click the Pod name in the Pod list to go to the Pod details page.

#### Pod List Page
![IMG_045]

#### Pod Details Page
![IMG_046]
![IMG_047]
![IMG_048]

##### <div id='3-2-6-2-3'/> 3.2.6.2.3. Create Pod
1. Click the Create button in the Pod list to go to the Create Pod page.

#### Pod List Page
![IMG_049]

#### Pod Creating Page
![IMG_050]
![IMG_051]
![IMG_052]

##### <div id='3-2-6-2-4'/> 3.2.6.2.4. Modify Pod
1. Click the Modify button in Pod Details to go to the Modify Pod page.

#### Pod Details Page
![IMG_053]

#### Pod Modifying Page
![IMG_054]
![IMG_055]
![IMG_056]

##### <div id='3-2-6-2-5'/> 3.2.6.2.5. Delete Pod
1. Click the Delete button in Pod Details, then a Pod Delete pop-up window appears.

#### Pod Details Page
![IMG_057]

#### Pod Delete Pop-up Screen
![IMG_058]
![IMG_059]


#### <div id='3-2-6-3'/> 3.2.6.3. Replica Sets

##### <div id='3-2-6-3-1'/> 3.2.6.3.1. Replica Set List Check
1. Click the Replica Sets tab in Workloads to go to the Replica Set list page.

![IMG_060]

##### <div id='3-2-6-3-2'/> 3.2.6.3.2. Replica Set Details Check
- The Replica Set detail page consists of Details, Events, and YAML tabs.

1. Click the Replica Set name in the Replica Set list to go to the Replica Set detail page.

#### Replica Set List Page
![IMG_061]

#### Replica Set Details Page
![IMG_062]
![IMG_063]
![IMG_064]

##### <div id='3-2-6-3-3'/> 3.2.6.3.3. Create Replica Set
1. Click the Create button in the Replica Set list to go to the Create Replica Set page.

#### Replica Set List Page
![IMG_065]

#### Replica Set Details Page
![IMG_066]
![IMG_067]
![IMG_068]

##### <div id='3-2-6-3-4'/> 3.2.6.3.4. Modify Replica Set
1. Click the Modify button in Replica Set Details to go to the Modify Replica Set page.

#### Replica Set Details Page
![IMG_069]

#### Replica Set Modifying Page
![IMG_070]
![IMG_071]
![IMG_072]

##### <div id='3-2-6-3-5'/> 3.2.6.3.5. Delete Replica Set
1. Click the Delete button in Replica Set Details, then the Delete Replica Set pop-up window appears.

#### Replica Set Details Page
![IMG_073]

#### Replica Set Delete Pop-up Screen
![IMG_074]
![IMG_075]


### <div id='3-2-7'/> 3.2.7. Services Menu

#### <div id='3-2-7-1'/> 3.2.7.1. Service List Check
1. Click the Services menu to go to the Service List page.

![IMG_076]


#### <div id='3-2-7-2'/> 3.2.7.2. Service Details Check
- The Service Details page consists of Details, Events, and YAML tabs.

1. Click the service name in the Service list to go to the Service Details page.

#### Service List Page
![IMG_077]

#### Service Details Page
![IMG_078]
![IMG_079]
![IMG_080]

##### <div id='3-2-7-3'/> 3.2.7.3. Create Service
1. Click the Create button in the Service list to go to the Create Service page.

#### Service List Page
![IMG_081]

#### Service Creating Page
![IMG_082]
![IMG_083]
![IMG_084]

##### <div id='3-2-7-4'/> 3.2.7.4. Modify Service
1. Click the Modify button in Service Details to go to the Modify Service page.

#### Service Details Page
![IMG_085]

#### Service Modifying Page
![IMG_086]
![IMG_087]
![IMG_088]

##### <div id='3-2-7-5'/> 3.2.7.5. Delete Service
1. Click the Delete button in Service Details, then a pop-up window to Delete Service appears.

#### Service Details Page
![IMG_089]

#### Service Delete Pop-up screen
![IMG_090]
![IMG_091]


### <div id='3-2-8'/> 3.2.8. Storages menu

#### <div id='3-2-8-1'/> 3.2.8.1. Persistent Volume Claim List Check
1. Click the Storages menu to go to the Persistent Volume Claim list page.

![IMG_092]

#### <div id='3-2-8-2'/> 3.2.8.2. Persistent Volume Claim Details Check
- The Persistent Volume Claim detail page consists of Details, Events, and YAML tabs.

1.  Click the Persistent Volume Claim name to go to the Persistent Volume Claim detail page from the Persistent Volume Claim list.

#### Persistent Volume Claim List Page
![IMG_093]

#### Persistent Volume Claim Details Page
![IMG_094]
![IMG_095]
![IMG_096]

##### <div id='3-2-8-3'/> 3.2.8.3. Create Persistent Volume Claim
1. Click the Create button in the Persistent Volume Claim list to go to the Create Persistent Volume Claim page.

#### Persistent Volume Claim List Page
![IMG_097]

#### Persistent Volume Claim Creating Page
![IMG_098]
![IMG_099]
![IMG_100]


##### <div id='3-2-8-4'/> 3.2.8.4. Modify Persistent Volume Claim
1. Click the Modify button in the Persistent Volume Claim details to go to the Correct Persistent Volume Claim page.

#### Persistent Volume Claim Details Page
![IMG_101]

#### Persistent Volume Claim Modifying Page
![IMG_102]
![IMG_103]
![IMG_104]

##### <div id='3-2-8-5'/> 3.2.8.5. Delete Persistent Volume Claim
1. Click the Delete button in Persistent Volume Claim details, then a Delete Persistent Volume Claim pop-up window appears.

#### Persistent Volume Claim Details Page
![IMG_105]

#### Persistent Volume Claim Delete Pop-up Screen
![IMG_106]
![IMG_107]

<br>

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > User Portal Use Guide

----


[IMG_001]:../images/v1.2/container-platform-portal/user/cp-001.png
[IMG_002]:../images/v1.2/container-platform-portal/user/cp-002.png
[IMG_003]:../images/v1.2/container-platform-portal/user/cp-003.png
[IMG_004]:../images/v1.2/container-platform-portal/user/cp-004.png
[IMG_005]:../images/v1.2/container-platform-portal/user/cp-005.png
[IMG_006]:../images/v1.2/container-platform-portal/user/cp-006.png
[IMG_007]:../images/v1.2/container-platform-portal/user/cp-007.png
[IMG_008]:../images/v1.2/container-platform-portal/user/cp-008.png
[IMG_009]:../images/v1.2/container-platform-portal/user/cp-009.png
[IMG_010]:../images/v1.2/container-platform-portal/user/cp-010.png
[IMG_011]:../images/v1.2/container-platform-portal/user/cp-011.png
[IMG_012]:../images/v1.2/container-platform-portal/user/cp-012.png
[IMG_013]:../images/v1.2/container-platform-portal/user/cp-013.png
[IMG_014]:../images/v1.2/container-platform-portal/user/cp-014.png
[IMG_015]:../images/v1.2/container-platform-portal/user/cp-015.png
[IMG_016]:../images/v1.2/container-platform-portal/user/cp-016.png
[IMG_017]:../images/v1.2/container-platform-portal/user/cp-017.png
[IMG_018]:../images/v1.2/container-platform-portal/user/cp-018.png
[IMG_019]:../images/v1.2/container-platform-portal/user/cp-019.png
[IMG_020]:../images/v1.2/container-platform-portal/user/cp-020.png
[IMG_021]:../images/v1.2/container-platform-portal/user/cp-021.png
[IMG_022]:../images/v1.2/container-platform-portal/user/cp-022.png
[IMG_023]:../images/v1.2/container-platform-portal/user/cp-023.png
[IMG_024]:../images/v1.2/container-platform-portal/user/cp-024.png
[IMG_025]:../images/v1.2/container-platform-portal/user/cp-025.png
[IMG_026]:../images/v1.2/container-platform-portal/user/cp-026.png
[IMG_027]:../images/v1.2/container-platform-portal/user/cp-027.png
[IMG_028]:../images/v1.2/container-platform-portal/user/cp-028.png
[IMG_029]:../images/v1.2/container-platform-portal/user/cp-029.png
[IMG_030]:../images/v1.2/container-platform-portal/user/cp-030.png
[IMG_031]:../images/v1.2/container-platform-portal/user/cp-031.png
[IMG_032]:../images/v1.2/container-platform-portal/user/cp-032.png
[IMG_033]:../images/v1.2/container-platform-portal/user/cp-033.png
[IMG_034]:../images/v1.2/container-platform-portal/user/cp-034.png
[IMG_035]:../images/v1.2/container-platform-portal/user/cp-035.png
[IMG_036]:../images/v1.2/container-platform-portal/user/cp-036.png
[IMG_037]:../images/v1.2/container-platform-portal/user/cp-037.png
[IMG_038]:../images/v1.2/container-platform-portal/user/cp-038.png
[IMG_039]:../images/v1.2/container-platform-portal/user/cp-039.png
[IMG_040]:../images/v1.2/container-platform-portal/user/cp-040.png
[IMG_041]:../images/v1.2/container-platform-portal/user/cp-041.png
[IMG_042]:../images/v1.2/container-platform-portal/user/cp-042.png
[IMG_043]:../images/v1.2/container-platform-portal/user/cp-043.png
[IMG_044]:../images/v1.2/container-platform-portal/user/cp-044.png
[IMG_045]:../images/v1.2/container-platform-portal/user/cp-045.png
[IMG_046]:../images/v1.2/container-platform-portal/user/cp-046.png
[IMG_047]:../images/v1.2/container-platform-portal/user/cp-047.png
[IMG_048]:../images/v1.2/container-platform-portal/user/cp-048.png
[IMG_049]:../images/v1.2/container-platform-portal/user/cp-049.png
[IMG_050]:../images/v1.2/container-platform-portal/user/cp-050.png
[IMG_051]:../images/v1.2/container-platform-portal/user/cp-051.png
[IMG_052]:../images/v1.2/container-platform-portal/user/cp-052.png
[IMG_053]:../images/v1.2/container-platform-portal/user/cp-053.png
[IMG_054]:../images/v1.2/container-platform-portal/user/cp-054.png
[IMG_055]:../images/v1.2/container-platform-portal/user/cp-055.png
[IMG_056]:../images/v1.2/container-platform-portal/user/cp-056.png
[IMG_057]:../images/v1.2/container-platform-portal/user/cp-057.png
[IMG_058]:../images/v1.2/container-platform-portal/user/cp-058.png
[IMG_059]:../images/v1.2/container-platform-portal/user/cp-059.png
[IMG_060]:../images/v1.2/container-platform-portal/user/cp-060.png
[IMG_061]:../images/v1.2/container-platform-portal/user/cp-061.png
[IMG_062]:../images/v1.2/container-platform-portal/user/cp-062.png
[IMG_063]:../images/v1.2/container-platform-portal/user/cp-063.png
[IMG_064]:../images/v1.2/container-platform-portal/user/cp-064.png
[IMG_065]:../images/v1.2/container-platform-portal/user/cp-065.png
[IMG_066]:../images/v1.2/container-platform-portal/user/cp-066.png
[IMG_067]:../images/v1.2/container-platform-portal/user/cp-067.png
[IMG_068]:../images/v1.2/container-platform-portal/user/cp-068.png
[IMG_069]:../images/v1.2/container-platform-portal/user/cp-069.png
[IMG_070]:../images/v1.2/container-platform-portal/user/cp-070.png
[IMG_071]:../images/v1.2/container-platform-portal/user/cp-071.png
[IMG_072]:../images/v1.2/container-platform-portal/user/cp-072.png
[IMG_073]:../images/v1.2/container-platform-portal/user/cp-073.png
[IMG_074]:../images/v1.2/container-platform-portal/user/cp-074.png
[IMG_075]:../images/v1.2/container-platform-portal/user/cp-075.png
[IMG_076]:../images/v1.2/container-platform-portal/user/cp-076.png
[IMG_077]:../images/v1.2/container-platform-portal/user/cp-077.png
[IMG_078]:../images/v1.2/container-platform-portal/user/cp-078.png
[IMG_079]:../images/v1.2/container-platform-portal/user/cp-079.png
[IMG_080]:../images/v1.2/container-platform-portal/user/cp-080.png
[IMG_081]:../images/v1.2/container-platform-portal/user/cp-081.png
[IMG_082]:../images/v1.2/container-platform-portal/user/cp-082.png
[IMG_083]:../images/v1.2/container-platform-portal/user/cp-083.png
[IMG_084]:../images/v1.2/container-platform-portal/user/cp-084.png
[IMG_085]:../images/v1.2/container-platform-portal/user/cp-085.png
[IMG_086]:../images/v1.2/container-platform-portal/user/cp-086.png
[IMG_087]:../images/v1.2/container-platform-portal/user/cp-087.png
[IMG_088]:../images/v1.2/container-platform-portal/user/cp-088.png
[IMG_089]:../images/v1.2/container-platform-portal/user/cp-089.png
[IMG_090]:../images/v1.2/container-platform-portal/user/cp-090.png
[IMG_091]:../images/v1.2/container-platform-portal/user/cp-091.png
[IMG_092]:../images/v1.2/container-platform-portal/user/cp-092.png
[IMG_093]:../images/v1.2/container-platform-portal/user/cp-093.png
[IMG_094]:../images/v1.2/container-platform-portal/user/cp-094.png
[IMG_095]:../images/v1.2/container-platform-portal/user/cp-095.png
[IMG_096]:../images/v1.2/container-platform-portal/user/cp-096.png
[IMG_097]:../images/v1.2/container-platform-portal/user/cp-097.png
[IMG_098]:../images/v1.2/container-platform-portal/user/cp-098.png
[IMG_099]:../images/v1.2/container-platform-portal/user/cp-099.png
[IMG_100]:../images/v1.2/container-platform-portal/user/cp-100.png
[IMG_101]:../images/v1.2/container-platform-portal/user/cp-101.png
[IMG_102]:../images/v1.2/container-platform-portal/user/cp-102.png
[IMG_103]:../images/v1.2/container-platform-portal/user/cp-103.png
[IMG_104]:../images/v1.2/container-platform-portal/user/cp-104.png
[IMG_105]:../images/v1.2/container-platform-portal/user/cp-105.png
[IMG_106]:../images/v1.2/container-platform-portal/user/cp-106.png
[IMG_107]:../images/v1.2/container-platform-portal/user/cp-107.png
[IMG_108]:../images/v1.2/container-platform-portal/user/cp-108.png