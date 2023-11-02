## Table of Contents

1. [Document Overview](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Scope](#1-2)
2. [Accessing the Container Platform Administrator Portal](#2)
    * [2.1. Signing Up for the Container Platform Administrator Portal](#2-1)
    * [2.2. Logging into the Container Platform Administrator Portal](#2-2)    
3. [Container Platform Administrator Portal Manual](#3)
    * [3.1. Configuration of the Container Platform Administrator Portal Menu](#3-1)
    * [3.2. Explanation of the Container Platform Administrator Portal Menu](#3-2)
    * [3.2.1. Overview](#3-2-1)
    * [3.2.1.1. Overview List View](#3-2-1-1)
    * [3.2.1.2. Changing Overview Namespace](#3-2-1-2)
    * [3.2.1.3. Changing Overview My Info](#3-2-1-3)
    * [3.2.2. Clusters Menu](#3-2-2)
    * [3.2.2.1. Namespaces](#3-2-2-1)
    * [3.2.2.1.1. Namespace List View](#3-2-2-1-1)
    * [3.2.2.1.2. Namespace Details View](#3-2-2-1-2)
    * [3.2.2.1.3. Namespace Creation](#3-2-2-1-3)
    * [3.2.2.1.4. Namespace Modification](#3-2-2-1-4)
    * [3.2.2.1.5. Namespace Deletion](#3-2-2-1-5)
    * [3.2.2.2. Nodes](#3-2-2-2)
    * [3.2.2.2.1. Node List View](#3-2-2-2-1)
    * [3.2.2.2.2. Node Details View](#3-2-2-2-2)
    * [3.2.3. Workloads Menu](#3-2-3)
    * [3.2.3.1. Deployments](#3-2-3-1)
    * [3.2.3.1.1. Deployment List View](#3-2-3-1-1)
    * [3.2.3.1.2. Deployment Details View](#3-2-3-1-2)
    * [3.2.3.1.3. Deployment Creation](#3-2-3-1-3)
    * [3.2.3.1.4. Deployment Modification](#3-2-3-1-4)
    * [3.2.3.1.5. Deployment Deletion](#3-2-3-1-5)
    * [3.2.3.2. Pods](#3-2-3-2)
    * [3.2.3.2.1. Pod List View](#3-2-3-2-1)
    * [3.2.3.2.2. Pod Details View](#3-2-3-2-2)
    * [3.2.3.2.3. Pod Creation](#3-2-3-2-3)
    * [3.2.3.2.4. Pod Modification](#3-2-3-2-4)
    * [3.2.3.2.5. Pod Deletion](#3-2-3-2-5)
    * [3.2.3.3. Replica Sets](#3-2-3-3)
    * [3.2.3.3.1. Replica Set List View](#3-2-3-3-1)
    * [3.2.3.3.2. Replica Set Details View](#3-2-3-3-2)
    * [3.2.3.3.3. Replica Set Creation](#3-2-3-3-3)
    * [3.2.3.3.4. Replica Set Modification](#3-2-3-3-4)
    * [3.2.3.3.5. Replica Set Deletion](#3-2-3-3-5)
    * [3.2.4. Services Menu](#3-2-4)
    * [3.2.4.1. Service List View](#3-2-4-1)
    * [3.2.4.2. Service Details View](#3-2-4-2)
    * [3.2.4.3. Service Creation](#3-2-4-3)
    * [3.2.4.4. Service Modification](#3-2-4-4)
    * [3.2.4.5. Service Deletion](#3-2-4-5)
    * [3.2.5. Storages Menu](#3-2-5)
    * [3.2.5.1. Persistent Volumes](#3-2-5-1)
    * [3.2.5.1.1. Persistent Volume List View](#3-2-5-1-1)
    * [3.2.5.1.2. Persistent Volume Details View](#3-2-5-1-2)
    * [3.2.5.1.3. Persistent Volume Creation](#3-2-5-1-3)
    * [3.2.5.1.4. Persistent Volume Modification](#3-2-5-1-4)
    * [3.2.5.1.5. Persistent Volume Deletion](#3-2-5-1-5)
    * [3.2.5.2. Persistent Volume Claims](#3-2-5-2)
    * [3.2.5.2.1. Persistent Volume Claim List View](#3-2-5-2-1)
    * [3.2.5.2.2. Persistent Volume Claim Details View](#3-2-5-2-2)
    * [3.2.5.2.3. Persistent Volume Claim Creation](#3-2-5-2-3)
    * [3.2.5.2.4. Persistent Volume Claim Modification](#3-2-5-2-4)
    * [3.2.5.2.5. Persistent Volume Claim Deletion](#3-2-5-2-5)
    * [3.2.5.3. Storage Classes](#3-2-5-3)
    * [3.2.5.3.1. Storage Class List View](#3-2-5-3-1)
    * [3.2.5.3.2. Storage Class Details View](#3-2-5-3-2)
    * [3.2.5.3.3. Storage Class Creation](#3-2-5-3-3)
    * [3.2.5.3.4. Storage Class Modification](#3-2-5-3-4)
    * [3.2.5.3.5. Storage Class Deletion](#3-2-5-3-5)
    * [3.2.6. Managements Menu](#3-2-6)
    * [3.2.6.1. Users](#3-2-6-1)
    * [3.2.6.1.1. Cluster Administrator View](#3-2-6-1-1)  
    * [3.2.6.1.2. Cluster Administrator Details View](#3-2-6-1-2)  
    * [3.2.6.1.3. Regular User List View](#3-2-6-1-3)  
    * [3.2.6.1.4. Regular User Details View](#3-2-6-1-4)  
    * [3.2.6.1.5. User Creation](#3-2-6-1-5)  
    * [3.2.6.1.6. User Modification](#3-2-6-1-6)  
    * [3.2.6.1.7. User Deletion](#3-2-6-1-7)  
    * [3.2.6.2. Roles](#3-2-6-2)  
    * [3.2.6.2.1. Role List View](#3-2-6-2-1)
    * [3.2.6.2.2. Role Details View](#3-2-6-2-2)
    * [3.2.6.2.3. Role Creation](#3-2-6-2-3)
    * [3.2.6.2.4. Role Modification](#3-2-6-2-4)
    * [3.2.6.2.5. Role Deletion](#3-2-6-2-5)
    * [3.2.6.3. Resource Quotas](#3-2-6-3)
    * [3.2.6.3.1. Resource Quota List View](#3-2-6-3-1)
    * [3.2.6.3.2. Resource Quota Details View](#3-2-6-3-2)
    * [3.2.6.3.3. Resource Quota Creation](#3-2-6-3-3)
    * [3.2.6.3.4. Resource Quota Modification](#3-2-6-3-4)
    * [3.2.6.3.5. Resource Quota Deletion](#3-2-6-3-5)
    * [3.2.6.4. Limit Ranges](#3-2-6-4)
    * [3.2.6.4.1. Limit Range List View](#3-2-6-4-1)
    * [3.2.6.4.2. Limit Range Details View](#3-2-6-4-2)
    * [3.2.6.4.3. Limit Range Creation](#3-2-6-4-3)
    * [3.2.6.4.4. Limit Range Modification](#3-2-6-4-4)
    * [3.2.6.4.5. Limit Range Deletion](#3-2-6-4-5)



----
# <div id='1'/> 1. Document Overview

## <div id='1-1'/> 1.1. Purpose
This document describes the usage of the Container Platform Operator Portal.

## <div id='1-2'/> 1.2. Scope
This document describes the usage of the Container Platform Operator Portal in a Windows environment.

# <div id='2'/> 2. Accessing the Container Platform Operator Portal

## <div id='2-1'/> 2.1. Registering for the Container Platform Operator Portal
1. Access the Container Platform Operator Portal and click the "Register" button.

![IMG_001]

2. Enter Kubernetes Cluster information, the desired Namespace name, User information, and click the "Register" button to sign up for the Container Platform Operator Portal.
> The images below provide example information for understanding.

![IMG_002]
![IMG_003]

## <div id='2-2'/> 2.2. Logging into the Container Platform Operator Portal
1. Enter the user ID and password, then click the "Login" button to access the Container Platform Operator Portal.

![IMG_004]

# <div id='3'/> 3. Container Platform Operator Portal Manual

## <div id='3-1'/> 3.1. Configuration of the Container Platform Operator Portal Menu
| <center>Menu</center> | <center>Category</center> | <center>Description</center> |
| :--- | :--- | :--- |
|| Overview | Container Platform Dashboard |
| Clusters | Namespaces | Manage Namespaces information |
|| Nodes | Manage Nodes information |
| Workloads | Deployments | Manage Deployments information |
|| Pods | Manage Pods information |
|| Replica Sets | Manage Replica Sets information |
| Services | Services | Manage Services information |
| Storages | Persistent Volumes | Manage Persistent Volumes information |
|| Persistent Volume Claims | Manage Persistent Volume Claims information |
|| Storage Classes | Manage Storage Classes information |
| Managements | Users | User Management |
|| Roles | Roles Management |
|| Resource Quotas | Manage Resource Quotas information |
|| Limit Ranges | Manage Limit Ranges information |

## <div id='3-2'/> 3.2. Description of the Container Platform Operator Portal Menu
This section provides a description of the Container Platform Operator Portal's menu.

### <div id='3-2-1'/> 3.2.1. Overview
#### <div id='3-2-1-1'/> 3.2.1.1. Viewing Overview List
- View the number of Namespaces, Deployments, Pods, and Users, along with charts for Deployments, Pods, and ReplicaSets.

1. After logging in, navigate to the Overview page as the first screen.

![IMG_005]

#### <div id='3-2-1-2'/> 3.2.1.2. Changing Overview Namespace
- Click the Select Box at the top of the screen and choose the desired Namespace from the entire Namespace list.

1. Upon selecting a Namespace in the Select Box, the Overview information for that Namespace is displayed.

![IMG_006]
![IMG_007]

#### <div id='3-2-1-3'/> 3.2.1.3. Changing Overview My Info
- Click the gear icon at the top right of the screen.

1. Modify the Password and E-mail information for the Cluster Administrator account in the My Info popup.

![IMG_008]

### <div id='3-2-2'/> 3.2.2. Clusters Menu
#### <div id='3-2-2-1'/> 3.2.2.1. Namespaces
##### <div id='3-2-2-1-1'/> 3.2.2.1.1. Viewing Namespace List
1. Click on the Namespaces in the Clusters menu to navigate to the Namespace list page.

![IMG_009]

##### <div id='3-2-2-1-2'/> 3.2.2.1.2. Viewing Namespace Details
1. Click on the Namespace name in the Namespace list to view the detailed Namespace page.

#### Namespace Detail Page
![IMG_010]

##### <div id='3-2-2-1-3'/> 3.2.2.1.3. Creating a Namespace
- Specify the Namespace Administrator, Resource Quotas, and Limit Ranges on the Namespace creation page.

1. Click the create button in the Namespace list to navigate to the Namespace creation page.

#### Namespace List Page
![IMG_011]


#### Namespace creation page
![IMG_012]
![IMG_013]
![IMG_014]
![IMG_015]
![IMG_016]

##### <div id='3-2-2-1-4'/> 3.2.2.1.4. Namespace modification
You can modify the Namespace Administrator, Resource Quotas, and Limit Ranges on the Namespace modification page.

1. Click the modify button on the Namespace detail page to navigate to the Namespace modification page.

#### Namespace detail page
![IMG_017]

#### Namespace modification page
![IMG_018]
![IMG_019]

##### <div id='3-2-2-1-5'/> 3.2.2.1.5. Namespace deletion
1. Click the delete button on the Namespace detail to delete the Namespace.

#### Namespace detail page
![IMG_020]

#### Namespace deletion popup
![IMG_021]
![IMG_022]

#### <div id='3-2-2-2'/> 3.2.2.2. Nodes
##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Viewing Node list
1. Click on the Nodes in the Clusters menu to navigate to the Node list page.
![IMG_023]

##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Viewing Node details
1. Click on the Node name in the Node list to view the detailed Node page.

#### Node detail page
![IMG_024]

### <div id='3-2-3'/> 3.2.3. Workloads Menu
#### <div id='3-2-3-1'/> 3.2.3.1 Deployment
##### <div id='3-2-3-1-1'/> 3.2.3.1.1. Viewing Deployment list
1. Click on Deployments in Workloads to navigate to the Deployment list page.

![IMG_025]

##### <div id='3-2-3-1-2'/> 3.2.3.1.2. Viewing Deployment details
1. Click on the Deployment name in the Deployment list to view the detailed Deployment page.

#### Deployment detail page
![IMG_026]

##### <div id='3-2-3-1-3'/> 3.2.3.1.3. Deployment creation
1. Clicking the create button in the Deployment list will prompt the Deployment creation popup.

#### Deployment list page
![IMG_027]

#### Deployment creation popup
![IMG_028]


##### <div id='3-2-3-1-4'/> 3.2.3.1.4. Deployment modification
1. Upon clicking the modify button on the Deployment details page, the Deployment modification popup appears.

#### Deployment detail page
![IMG_029]

#### Deployment modification popup
![IMG_030]

##### <div id='3-2-3-1-5'/> 3.2.3.1.5. Deployment deletion
1. Clicking the delete button on the Deployment details will result in the removal of the Deployment.

#### Deployment detail page
![IMG_031]

#### Deployment deletion popup
![IMG_032]

#### <div id='3-2-3-2'/> 3.2.3.2. Pods
##### <div id='3-2-3-2-1'/> 3.2.3.2.1. Viewing Pod list
1. Click on Pods in Workloads to navigate to the Pod list page.

![IMG_033]

##### <div id='3-2-3-2-2'/> 3.2.3.2.2. Viewing Pod details
1. Click on the Pod name in the Pod list to view the detailed Pod page.

#### Pod detail page
![IMG_034]

##### <div id='3-2-3-2-3'/> 3.2.3.2.3. Pod creation
1. Clicking the create button in the Pod list will prompt the Pod creation popup.

#### Pod list page
![IMG_035]

#### Pod creation popup
![IMG_036]

##### <div id='3-2-3-2-4'/> 3.2.3.2.4. Pod Modification
1. Clicking the modify button on the Pod details page opens the Pod modification pop-up.

#### Pod Details Page
![IMG_037]

#### Pod Modification Pop-up
![IMG_038]

##### <div id='3-2-3-2-5'/> 3.2.3.2.5. Pod Deletion
1. Clicking the delete button on the Pod details page deletes the Pod.

#### Pod Details Page
![IMG_039]

#### Pod Deletion Confirmation Pop-up
![IMG_040]

#### <div id='3-2-3-3'/> 3.2.3.3. Replica Sets
##### <div id='3-2-3-3-1'/> 3.2.3.3.1. View Replica Set List
1. Clicking on 'Replica Sets' in 'Workloads' navigates to the Replica Set list page.

![IMG_041]

##### <div id='3-2-3-3-2'/> 3.2.3.3.2. Replica Set Details View
1. Clicking on the Replica Set name in the list opens the Replica Set details page.

#### Replica Set Details Page
![IMG_042]

##### <div id='3-2-3-3-3'/> 3.2.3.3.3. Replica Set Creation
1. Clicking the create button in the Replica Set list opens the Replica Set creation pop-up.

#### Replica Set List Page
![IMG_043]

#### Replica Set Creation Pop-up
![IMG_044]

##### <div id='3-2-3-3-4'/> 3.2.3.3.4. Replica Set Modification
1. Clicking the modify button in the Replica Set details opens the Replica Set modification pop-up.

#### Replica Set Details Page
![IMG_045]

#### Replica Set Modification Pop-up
![IMG_046]

##### <div id='3-2-3-3-5'/> 3.2.3.3.5. Deleting Replica Sets
1. Clicking the delete button on the Replica Set details page will delete the Replica Set.

#### Replica Set Details Page
![IMG_047]

#### Replica Set Deletion Confirmation Pop-up
![IMG_048]

### <div id='3-2-4'/> 3.2.4. Services Menu
#### <div id='3-2-4-1'/> 3.2.4.1. Viewing Service List
1. Click 'Services' to navigate to the Service list page.

![IMG_049]

#### <div id='3-2-4-2'/> 3.2.4.2. Viewing Service Details
1. Click on the Service name in the list to go to the Service details page.

#### Service Details Page
![IMG_050]

#### <div id='3-2-4-3'/> 3.2.4.3. Creating a Service
1. Clicking the create button in the Service list opens the Service creation pop-up.

#### Service List Page
![IMG_051]

#### Service Creation Pop-up
![IMG_052]

#### <div id='3-2-4-4'/> 3.2.4.4. Modifying a Service
1. Clicking the modify button in the Service details opens the Service modification pop-up.

#### Service Details Page
![IMG_053]

#### Service Modification Pop-up
![IMG_054]

#### <div id='3-2-4-5'/> 3.2.4.5. Deleting a Service
1. Clicking the delete button in the Service details will delete the Service.

#### Service Details Page
![IMG_055]

#### Service Deletion Confirmation Pop-up
![IMG_056]

### <div id='3-2-5'/> 3.2.5. Storages Menu
#### <div id='3-2-5-1'/> 3.2.5.1. Persistent Volumes
##### <div id='3-2-5-1-1'/> 3.2.5.1.1. Viewing Persistent Volume List
1. Click 'Persistent Volumes' in 'Storages' to navigate to the Persistent Volume list page.
![IMG_057]

##### <div id='3-2-5-1-2'/> 3.2.5.1.2. Viewing Persistent Volume Details
1. Click on the Persistent Volume name in the list to go to the Persistent Volume details page.

#### Persistent Volume Details Page
![IMG_058]

##### <div id='3-2-5-1-3'/> 3.2.5.1.3. Creating a Persistent Volume
1. Clicking the create button in the Persistent Volume list opens the Persistent Volume creation pop-up.

#### Persistent Volume List Page
![IMG_059]

#### Persistent Volume Creation Pop-up
![IMG_060]

##### <div id='3-2-5-1-4'/> 3.2.5.1.4. Modifying a Persistent Volume
1. Clicking the modify button in the Persistent Volume details opens the Persistent Volume modification pop-up.

#### Persistent Volume Details Page
![IMG_061]

#### Persistent Volume Modification Pop-up
![IMG_062]

##### <div id='3-2-5-1-5'/> 3.2.5.1.5. Deleting a Persistent Volume
1. Clicking the delete button in the Persistent Volume details will delete the Persistent Volume.

#### Persistent Volume Details Page
![IMG_063]

#### Persistent Volume Deletion Confirmation Pop-up
![IMG_064]

#### <div id='3-2-5-2'/> 3.2.5.2. Persistent Volume Claims
##### <div id='3-2-5-2-1'/> 3.2.5.2.1. Viewing Persistent Volume Claim List
1. Click 'Persistent Volume Claims' in 'Storages' to navigate to the Persistent Volume Claim list page.
![IMG_065]

##### <div id='3-2-5-2-2'/> 3.2.5.2.2. Viewing Persistent Volume Claim Details
1. Click on the Persistent Volume Claim name in the list to go to the Persistent Volume Claim details page.

#### Persistent Volume Claim Details Page
![IMG_066]
##### <div id='3-2-5-2-3'/> 3.2.5.2.3. Creating Persistent Volume Claims
1. Clicking the create button in the Persistent Volume Claim list opens the Persistent Volume Claim creation pop-up.

#### Persistent Volume Claim List Page
![IMG_067]

#### Persistent Volume Claim Creation Pop-up
![IMG_068]

##### <div id='3-2-5-2-4'/> 3.2.5.2.4. Modifying Persistent Volume Claims
1. Clicking the modify button in the Persistent Volume Claim details opens the Persistent Volume Claim modification pop-up.

#### Persistent Volume Claim Details Page
![IMG_069]

#### Persistent Volume Claim Modification Pop-up
![IMG_070]

##### <div id='3-2-5-2-5'/> 3.2.5.2.5. Deleting Persistent Volume Claims
1. Clicking the delete button in the Persistent Volume Claim details will delete the Persistent Volume Claim.

#### Persistent Volume Claim Details Page
![IMG_071]

#### Persistent Volume Claim Deletion Confirmation Pop-up
![IMG_072]

#### <div id='3-2-5-3'/> 3.2.5.3. Storage Classes
##### <div id='3-2-5-3-1'/> 3.2.5.3.1. Viewing Storage Class List
1. Click 'Storage Classes' in 'Storages' to navigate to the Storage Class list page.
![IMG_073]

##### <div id='3-2-5-3-2'/> 3.2.5.3.2. Viewing Storage Class Details
1. Click on the Storage Class name in the list to go to the Storage Class details page.

#### Storage Class Details Page
![IMG_074]

##### <div id='3-2-5-3-3'/> 3.2.5.3.3. Creating Storage Class
1. Clicking the create button in the Storage Class list opens the Storage Class creation pop-up.

#### Storage Class List Page
![IMG_075]

#### Storage Class Creation Pop-up
![IMG_076]

##### <div id='3-2-5-3-4'/> 3.2.5.3.4. Modifying Storage Class
1. Clicking the modify button in the Storage Class details opens the Storage Class modification pop-up.

#### Storage Class Details Page
![IMG_077]

#### Storage Class Modification Pop-up
![IMG_078]

##### <div id='3-2-5-3-5'/> 3.2.5.3.5. Deleting Storage Class
1. Clicking the delete button in the Storage Class details will delete the Storage Class.

#### Storage Class Details Page
![IMG_079]

#### Storage Class Deletion Confirmation Pop-up
![IMG_080]

### <div id='3-2-6'/> 3.2.6. Managements
#### <div id='3-2-6-1'/> 3.2.6.1. Users
##### <div id='3-2-6-1-1'/> 3.2.6.1.1. Viewing Cluster Administrators
1. Select 'Users' in the 'Managements' menu and click on the 'Administrator' tab to view the Cluster Administrators.

![IMG_081]

##### <div id='3-2-6-1-2'/> 3.2.6.1.2. Viewing Cluster Administrator Details
1. Click the Cluster Administrator User ID to navigate to the Cluster Administrator's details page.

#### Cluster Administrator Details Page
![IMG_082]

##### <div id='3-2-6-1-3'/> 3.2.6.1.3. Viewing General User List
1. Select 'Users' in the 'Managements' menu and click on the 'User' tab to view the list of general users.

![IMG_083]

##### <div id='3-2-6-1-4'/> 3.2.6.1.4. Viewing General User Details
1. Click the User ID of a general user to navigate to the details page of that user.

#### General User Details Page
![IMG_084]
##### <div id='3-2-6-1-5'/> 3.2.6.1.5. User Creation
- The user creation page allows the registration of user accounts and the assignment of the Namespace and Role for the respective user.

1. Click the create button on the User list to navigate to the User creation page.

#### User List Page
![IMG_085]

#### User Creation Pop-up
![IMG_086]
![IMG_087]
![IMG_088]

##### <div id='3-2-6-1-6'/> 3.2.6.1.6. User Modification
- The user modification page enables modification of user accounts and their assigned Namespace and Role.

1. Click the modify button on the User details to go to the User modification page.

#### User Details Page
![IMG_089]

#### User Modification Page
![IMG_090]
![IMG_091]
![IMG_092]

##### <div id='3-2-6-1-7'/> 3.2.6.1.7. User Deletion
1. Clicking the delete button on the User details will delete the user.

#### User Details Page
![IMG_093]

#### User Deletion Pop-up
![IMG_094]


#### <div id='3-2-6-2'/> 3.2.6.2. Roles
##### <div id='3-2-6-2-1'/> 3.2.6.2.1. Viewing Role List
1. Click 'Roles' in 'Managements' to navigate to the Role list page.
![IMG_095]

##### <div id='3-2-6-2-2'/> 3.2.6.2.2. Viewing Role Details
1. Click on the Role name in the list to view the Role details page.

#### Role Details Page
![IMG_096]

##### <div id='3-2-6-2-3'/> 3.2.6.2.3. Role Creation
1. Clicking the create button in the Role list will open the Role creation pop-up.

#### Role List Page
![IMG_097]

#### Role Creation Pop-up
![IMG_098]

##### <div id='3-2-6-2-4'/> 3.2.6.2.4. Role Modification
1. Clicking the modify button in the Role details opens the Role modification pop-up.

#### Role Details Page
![IMG_099]

#### Role Modification Pop-up
![IMG_100]

##### <div id='3-2-6-2-5'/> 3.2.6.2.5. Role Deletion
1. Clicking the delete button in the Role details will delete the Role.

#### Role Details Page
![IMG_101]

#### Role Deletion Pop-up
![IMG_102]
#### <div id='3-2-6-3'/> 3.2.6.3. Resource Quotas
##### <div id='3-2-6-3-1'/> 3.2.6.3.1. Viewing Resource Quota List
1. Click 'Resource Quotas' in 'Managements' to navigate to the Resource Quota list page.
![IMG_103]

##### <div id='3-2-6-3-2'/> 3.2.6.3.2. Viewing Resource Quota Details
1. Click on the Resource Quota name in the list to view the Resource Quota details page.

#### Resource Quota Details Page
![IMG_104]

##### <div id='3-2-6-3-3'/> 3.2.6.3.3. Resource Quota Creation
1. Clicking the create button in the Resource Quota list opens the Resource Quota creation pop-up.

#### Resource Quota List Page
![IMG_105]

#### Resource Quota Creation Pop-up
![IMG_106]

##### <div id='3-2-6-3-4'/> 3.2.6.3.4. Resource Quota Modification
1. Clicking the modify button in the Resource Quota details opens the Resource Quota modification pop-up.

#### Resource Quota Details Page
![IMG_107]

#### Resource Quota Modification Pop-up
![IMG_108]

##### <div id='3-2-6-3-5'/> 3.2.6.3.5. Resource Quota Deletion
1. Clicking the delete button in the Resource Quota details will delete the Resource Quota.

#### Resource Quota Details Page
![IMG_109]

#### Resource Quota Deletion Pop-up
![IMG_110]

#### <div id='3-2-6-4'/> 3.2.6.4. Limit Ranges
##### <div id='3-2-6-4-1'/> 3.2.6.4.1. Viewing Limit Range List
1. Click 'Limit Ranges' in 'Managements' to navigate to the Limit Range list page.
![IMG_111]

##### <div id='3-2-6-4-2'/> 3.2.6.4.2. Viewing Limit Range Details
1. Click on the Limit Range name in the list to view the Limit Range details page.

#### Limit Range Details Page
![IMG_112]

##### <div id='3-2-6-4-3'/> 3.2.6.4.3. Limit Range Creation
1. Clicking the create button in the Limit Range list opens the Limit Range creation pop-up.

#### Limit Range List Page
![IMG_113]

#### Limit Range Creation Pop-up
![IMG_114]

##### <div id='3-2-6-4-4'/> 3.2.6.4.4. Limit Range Modification
1. Clicking the modify button in the Limit Range details opens the Limit Range modification pop-up.

#### Limit Range Details Page
![IMG_115]

#### Limit Range Modification Pop-up
![IMG_116]

##### <div id='3-2-6-4-5'/> 3.2.6.4.5. Limit Range Deletion
1. Clicking the delete button in the Limit Range details will delete the Limit Range.

#### Limit Range Details Page
![IMG_117]

#### Limit Range Deletion Pop-up
![IMG_118]

----

[IMG_001]:../images/container-platform/admin-portal/cp-001.png
[IMG_002]:../images/container-platform/admin-portal/cp-002.png
[IMG_003]:../images/container-platform/admin-portal/cp-003.png
[IMG_004]:../images/container-platform/admin-portal/cp-004.png
[IMG_005]:../images/container-platform/admin-portal/cp-005.png
[IMG_006]:../images/container-platform/admin-portal/cp-006.png
[IMG_007]:../images/container-platform/admin-portal/cp-007.png
[IMG_008]:../images/container-platform/admin-portal/cp-008.png
[IMG_009]:../images/container-platform/admin-portal/cp-009.png
[IMG_010]:../images/container-platform/admin-portal/cp-010.png
[IMG_011]:../images/container-platform/admin-portal/cp-011.png
[IMG_012]:../images/container-platform/admin-portal/cp-012.png
[IMG_013]:../images/container-platform/admin-portal/cp-013.png
[IMG_014]:../images/container-platform/admin-portal/cp-014.png
[IMG_015]:../images/container-platform/admin-portal/cp-015.png
[IMG_016]:../images/container-platform/admin-portal/cp-016.png
[IMG_017]:../images/container-platform/admin-portal/cp-017.png
[IMG_018]:../images/container-platform/admin-portal/cp-018.png
[IMG_019]:../images/container-platform/admin-portal/cp-019.png
[IMG_020]:../images/container-platform/admin-portal/cp-020.png
[IMG_021]:../images/container-platform/admin-portal/cp-021.png
[IMG_022]:../images/container-platform/admin-portal/cp-022.png
[IMG_023]:../images/container-platform/admin-portal/cp-023.png
[IMG_024]:../images/container-platform/admin-portal/cp-024.png
[IMG_025]:../images/container-platform/admin-portal/cp-025.png
[IMG_026]:../images/container-platform/admin-portal/cp-026.png
[IMG_027]:../images/container-platform/admin-portal/cp-027.png
[IMG_028]:../images/container-platform/admin-portal/cp-028.png
[IMG_029]:../images/container-platform/admin-portal/cp-029.png
[IMG_030]:../images/container-platform/admin-portal/cp-030.png
[IMG_031]:../images/container-platform/admin-portal/cp-031.png
[IMG_032]:../images/container-platform/admin-portal/cp-032.png
[IMG_033]:../images/container-platform/admin-portal/cp-033.png
[IMG_034]:../images/container-platform/admin-portal/cp-034.png
[IMG_035]:../images/container-platform/admin-portal/cp-035.png
[IMG_036]:../images/container-platform/admin-portal/cp-036.png
[IMG_037]:../images/container-platform/admin-portal/cp-037.png
[IMG_038]:../images/container-platform/admin-portal/cp-038.png
[IMG_039]:../images/container-platform/admin-portal/cp-039.png
[IMG_040]:../images/container-platform/admin-portal/cp-040.png
[IMG_041]:../images/container-platform/admin-portal/cp-041.png
[IMG_042]:../images/container-platform/admin-portal/cp-042.png
[IMG_043]:../images/container-platform/admin-portal/cp-043.png
[IMG_044]:../images/container-platform/admin-portal/cp-044.png
[IMG_045]:../images/container-platform/admin-portal/cp-045.png
[IMG_046]:../images/container-platform/admin-portal/cp-046.png
[IMG_047]:../images/container-platform/admin-portal/cp-047.png
[IMG_048]:../images/container-platform/admin-portal/cp-048.png
[IMG_049]:../images/container-platform/admin-portal/cp-049.png
[IMG_050]:../images/container-platform/admin-portal/cp-050.png
[IMG_051]:../images/container-platform/admin-portal/cp-051.png
[IMG_052]:../images/container-platform/admin-portal/cp-052.png
[IMG_053]:../images/container-platform/admin-portal/cp-053.png
[IMG_054]:../images/container-platform/admin-portal/cp-054.png
[IMG_055]:../images/container-platform/admin-portal/cp-055.png
[IMG_056]:../images/container-platform/admin-portal/cp-056.png
[IMG_057]:../images/container-platform/admin-portal/cp-057.png
[IMG_058]:../images/container-platform/admin-portal/cp-058.png
[IMG_059]:../images/container-platform/admin-portal/cp-059.png
[IMG_060]:../images/container-platform/admin-portal/cp-060.png
[IMG_061]:../images/container-platform/admin-portal/cp-061.png
[IMG_062]:../images/container-platform/admin-portal/cp-062.png
[IMG_063]:../images/container-platform/admin-portal/cp-063.png
[IMG_064]:../images/container-platform/admin-portal/cp-064.png
[IMG_065]:../images/container-platform/admin-portal/cp-065.png
[IMG_066]:../images/container-platform/admin-portal/cp-066.png
[IMG_067]:../images/container-platform/admin-portal/cp-067.png
[IMG_068]:../images/container-platform/admin-portal/cp-068.png
[IMG_069]:../images/container-platform/admin-portal/cp-069.png
[IMG_070]:../images/container-platform/admin-portal/cp-070.png
[IMG_071]:../images/container-platform/admin-portal/cp-071.png
[IMG_072]:../images/container-platform/admin-portal/cp-072.png
[IMG_073]:../images/container-platform/admin-portal/cp-073.png
[IMG_074]:../images/container-platform/admin-portal/cp-074.png
[IMG_075]:../images/container-platform/admin-portal/cp-075.png
[IMG_076]:../images/container-platform/admin-portal/cp-076.png
[IMG_077]:../images/container-platform/admin-portal/cp-077.png
[IMG_078]:../images/container-platform/admin-portal/cp-078.png
[IMG_079]:../images/container-platform/admin-portal/cp-079.png
[IMG_080]:../images/container-platform/admin-portal/cp-080.png
[IMG_081]:../images/container-platform/admin-portal/cp-081.png
[IMG_082]:../images/container-platform/admin-portal/cp-082.png
[IMG_083]:../images/container-platform/admin-portal/cp-083.png
[IMG_084]:../images/container-platform/admin-portal/cp-084.png
[IMG_085]:../images/container-platform/admin-portal/cp-085.png
[IMG_086]:../images/container-platform/admin-portal/cp-086.png
[IMG_087]:../images/container-platform/admin-portal/cp-087.png
[IMG_088]:../images/container-platform/admin-portal/cp-088.png
[IMG_089]:../images/container-platform/admin-portal/cp-089.png
[IMG_090]:../images/container-platform/admin-portal/cp-090.png
[IMG_091]:../images/container-platform/admin-portal/cp-091.png
[IMG_092]:../images/container-platform/admin-portal/cp-092.png
[IMG_093]:../images/container-platform/admin-portal/cp-093.png
[IMG_094]:../images/container-platform/admin-portal/cp-094.png
[IMG_095]:../images/container-platform/admin-portal/cp-095.png
[IMG_096]:../images/container-platform/admin-portal/cp-096.png
[IMG_097]:../images/container-platform/admin-portal/cp-097.png
[IMG_098]:../images/container-platform/admin-portal/cp-098.png
[IMG_099]:../images/container-platform/admin-portal/cp-099.png
[IMG_100]:../images/container-platform/admin-portal/cp-100.png
[IMG_101]:../images/container-platform/admin-portal/cp-101.png
[IMG_102]:../images/container-platform/admin-portal/cp-102.png
[IMG_103]:../images/container-platform/admin-portal/cp-103.png
[IMG_104]:../images/container-platform/admin-portal/cp-104.png
[IMG_105]:../images/container-platform/admin-portal/cp-105.png
[IMG_106]:../images/container-platform/admin-portal/cp-106.png
[IMG_107]:../images/container-platform/admin-portal/cp-107.png
[IMG_108]:../images/container-platform/admin-portal/cp-108.png
[IMG_109]:../images/container-platform/admin-portal/cp-109.png
[IMG_110]:../images/container-platform/admin-portal/cp-110.png
[IMG_111]:../images/container-platform/admin-portal/cp-111.png
[IMG_112]:../images/container-platform/admin-portal/cp-112.png
[IMG_113]:../images/container-platform/admin-portal/cp-113.png
[IMG_114]:../images/container-platform/admin-portal/cp-114.png
[IMG_115]:../images/container-platform/admin-portal/cp-115.png
[IMG_116]:../images/container-platform/admin-portal/cp-116.png
[IMG_117]:../images/container-platform/admin-portal/cp-117.png
[IMG_118]:../images/container-platform/admin-portal/cp-118.png
