## Table of Contents

1. [Document Overview](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Scope](#1-2)
2. [Access to Container Platform Operator Portal](#2)
    * [2.1. Registering for Container Platform Operator Portal](#2-1)
    * [2.2. Logging into Container Platform Operator Portal](#2-2)
3. [Container Platform Operator Portal Manual](#3)
    * [3.1. Configuration of Container Platform Operator Portal Menu](#3-1)
    * [3.2. Explanation of Container Platform Operator Portal Menu](#3-2)
        * [3.2.1. Overview](#3-2-1)
            * [3.2.1.1. Overview List View](#3-2-1-1)
            * [3.2.1.2. Overview Namespace Change](#3-2-1-2)
            * [3.2.1.3. Overview My Info Change](#3-2-1-3)
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
        * [3.2.6. Management Menu](#3-2-6)
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




----
# <div id='1'/>1. Document Overview

## <div id='1-1'/>1.1. Purpose
This document describes the usage of the Container Platform Operator Portal.

## <div id='1-2'/>1.2. Scope
This document describes how to use the Container Platform Operator Portal in a Windows environment.

# <div id='2'/>2. Access to the Container Platform Operator Portal

## <div id='2-1'/>2.1. Registering for the Container Platform Operator Portal
1. Access the Container Platform Operator Portal and click on the "Register" button.

   ![IMG_001]

2. Enter Kubernetes Cluster information, the Namespace name to be created, User information, and click the "Register" button to sign up for the Container Platform Operator Portal.
   > The images below are example information to aid in understanding.

   ![IMG_002]
   ![IMG_003]

## <div id='2-2'/>2.2. Logging into the Container Platform Operator Portal
1. Enter the user ID and password, then click the "Login" button to access the Container Platform Operator Portal.

   ![IMG_004]

# <div id='3'/>3. Container Platform Operator Portal Manual

## <div id='3-1'/>3.1. Configuration of the Container Platform Operator Portal Menu

| Menu                     | Category         | Description                   |
| ------------------------ | ---------------- | ----------------------------- |
|                          | Overview         | Container Platform Dashboard  |
| Clusters                 | Namespaces       | Manage Namespaces information  |
|                          | Nodes            | Manage Nodes information       |
| Workloads                | Deployments      | Manage Deployment information  |
|                          | Pods             | Manage Pods information        |
|                          | Replica Sets     | Manage Replica Sets information |
| Services                 | Services         | Manage Services information    |
| Storages                 | Persistent Volumes | Manage Persistent Volumes information |
|                          | Persistent Volume Claims | Manage Persistent Volume Claims information |
|                          | Storage Classes  | Manage Storage Classes information |
| Managements              | Users            | User management                |
|                          | Roles            | Role management                |
|                          | Resource Quotas  | Manage Resource Quotas information |
|                          | Limit Ranges     | Manage Limit Ranges information |



## <div id='3-2'/> 3.2. Description of the Container Platform Operator Portal Menu

In this section, the description of the menu in the Container Platform Operator Portal is detailed.

### <div id='3-2-1'/> 3.2.1. Overview

#### <div id='3-2-1-1'/> 3.2.1.1. Viewing Overview List
- Displays the count of Namespaces, Deployments, Pods, and Users. Also, charts of Deployments, Pods, and ReplicaSets can be viewed.

1. After logging in, go to the Overview page, which is the first screen.

   ![IMG_005]

#### <div id='3-2-1-2'/> 3.2.1.2. Changing Overview Namespace
- Click the Select Box at the top of the screen and choose the desired Namespace from the complete list of Namespaces.

1. Upon selecting a Namespace from the Select Box, the Overview information for that Namespace is displayed.

   ![IMG_006]
   ![IMG_007]

#### <div id='3-2-1-3'/> 3.2.1.3. Changing My Info in Overview
- Click on the gear icon at the top right corner of the screen.

1. In the My Info pop-up window, modify the Password and E-mail information of the Cluster Administrator account.

   ![IMG_008]

### <div id='3-2-2'/> 3.2.2. Clusters Menu

#### <div id='3-2-2-1'/> 3.2.2.1. Namespaces

##### <div id='3-2-2-1-1'/> 3.2.2.1.1. Viewing Namespace List
1. Click on Namespaces in the Clusters menu to access the Namespace list page.

   ![IMG_009]

##### <div id='3-2-2-1-2'/> 3.2.2.1.2. Viewing Namespace Details
1. Click on the Namespace name in the Namespace list to view the detailed Namespace page.

   ![IMG_010]

##### <div id='3-2-2-1-3'/> 3.2.2.1.3. Creating a Namespace
- On the Namespace creation page, you can specify the Namespace administrator, Resource Quotas, and Limit Ranges.

1. Click the Create button in the Namespace list to access the Namespace creation page.

   ![IMG_011]

#### Namespace List Page
   ![IMG_012]

#### Namespace Creation Page
   ![IMG_013]
   ![IMG_014]
   ![IMG_015]
   ![IMG_016]

##### <div id='3-2-2-1-4'/> 3.2.2.1.4. Namespace Modification
In the Namespace modification page, administrators can modify Resource Quotas and Limit Ranges of the respective Namespace.

1. Click the Modify button on the Namespace details to access the Namespace modification page.

#### Namespace Details Page
![IMG_017]

#### Namespace Modification Page
![IMG_018]
![IMG_019]

##### <div id='3-2-2-1-5'/> 3.2.2.1.5. Namespace Deletion
1. Click the Delete button on the Namespace details to remove the Namespace.

#### Namespace Details Page
![IMG_020]

#### Namespace Deletion Popup Window
![IMG_021]
![IMG_022]

#### <div id='3-2-2-2'/> 3.2.2.2. Nodes

##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Viewing Node List
1. Click on Nodes in the Clusters menu to access the Node list page.
![IMG_023]

##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Viewing Node Details
1. Click on the Node name in the Node list to view the detailed Node page.

#### Node Details Page
![IMG_024]

### <div id='3-2-3'/> 3.2.3. Workloads Menu

#### <div id='3-2-3-1'/> 3.2.3.1 Deployment

##### <div id='3-2-3-1-1'/> 3.2.3.1.1. Viewing Deployment List
1. Click on Deployments in the Workloads to access the Deployment list page.

![IMG_025]

##### <div id='3-2-3-1-2'/> 3.2.3.1.2. Viewing Deployment Details
1. Click on the Deployment name in the Deployment list to view the detailed Deployment page.

#### Deployment Details Page
![IMG_026]

##### <div id='3-2-3-1-3'/> 3.2.3.1.3. Deployment Creation
1. Clicking on the create button in the Deployment list will open a Deployment creation popup.

#### Deployment List Page
![IMG_027]

#### Deployment Creation Popup
![IMG_028]

##### <div id='3-2-3-1-4'/> 3.2.3.1.4. Deployment Modification
1. Clicking the modify button in the Deployment details opens a Deployment modification popup.

#### Deployment Details Page
![IMG_029]

#### Deployment Modification Popup
![IMG_030]

##### <div id='3-2-3-1-5'/> 3.2.3.1.5. Deployment Deletion
1. Clicking the delete button in the Deployment details will delete the Deployment.

#### Deployment Details Page
![IMG_031]

#### Deployment Deletion Popup
![IMG_032]

#### <div id='3-2-3-2'/> 3.2.3.2. Pods

##### <div id='3-2-3-2-1'/> 3.2.3.2.1. Viewing Pod List
1. Click on Pods in the Workloads to access the Pod list page.

![IMG_033]

##### <div id='3-2-3-2-2'/> 3.2.3.2.2. Viewing Pod Details
1. Click on the Pod name in the Pod list to view the detailed Pod page.

#### Pod Details Page
![IMG_034]

##### <div id='3-2-3-2-3'/> 3.2.3.2.3. Pod Creation
1. Clicking the create button in the Pod list will open a Pod creation popup.

#### Pod List Page
![IMG_035]

#### Pod Creation Popup
![IMG_036]

##### <div id='3-2-3-2-4'/> 3.2.3.2.4. Pod Modification
1. Clicking the modify button in the Pod details will open a Pod modification popup.

#### Pod Details Page
![IMG_037]

#### Pod Modification Popup
![IMG_038]

##### <div id='3-2-3-2-5'/> 3.2.3.2.5. Pod Deletion
1. Clicking the delete button in the Pod details will delete the Pod.

#### Pod Details Page
![IMG_039]

#### Pod Deletion Popup
![IMG_040]

#### <div id='3-2-3-3'/> 3.2.3.3. Replica Sets
##### <div id='3-2-3-3-1'/> 3.2.3.3.1. Viewing Replica Set List
1. Click on 'Replica Sets' in 'Workloads' to access the list of Replica Sets.

![IMG_041]

##### <div id='3-2-3-3-2'/> 3.2.3.3.2. Viewing Replica Set Details
1. Click on a specific Replica Set in the list to view its detailed information.

#### Replica Set Details Page
![IMG_042]

##### <div id='3-2-3-3-3'/> 3.2.3.3.3. Creating a Replica Set
1. Click the create button on the Replica Set list page to initiate the creation of a new Replica Set.

#### Replica Set List Page
![IMG_043]

#### Replica Set Creation Popup
![IMG_044]

##### <div id='3-2-3-3-4'/> 3.2.3.3.4. Modifying a Replica Set
1. Click the modify button on the Replica Set details page to modify the existing Replica Set.

#### Replica Set Details Page
![IMG_045]

#### Replica Set Modification Popup
![IMG_046]

##### <div id='3-2-3-3-5'/> 3.2.3.3.5. Deleting a Replica Set
1. Click the delete button on the Replica Set details page to remove the respective Replica Set.

#### Replica Set Details Page
![IMG_047]

#### Replica Set Deletion Popup
![IMG_048]

### <div id='3-2-4'/> 3.2.4. Services Menu
#### <div id='3-2-4-1'/> 3.2.4.1. Viewing Service List
1. Click on 'Services' to access the list of available services.

![IMG_049]

#### <div id='3-2-4-2'/> 3.2.4.2. Viewing Service Details
1. Click on a specific service in the list to view its detailed information.

#### Service Details Page
![IMG_050]

#### <div id='3-2-4-3'/> 3.2.4.3. Creating a Service
1. Click the create button on the Service list page to create a new service.

#### Service List Page
![IMG_051]

#### Service Creation Popup
![IMG_052]

#### <div id='3-2-4-4'/> 3.2.4.4. Modifying a Service
1. Click the modify button on the Service details page to modify an existing service.

#### Service Details Page
![IMG_053]

#### Service Modification Popup
![IMG_054]

#### <div id='3-2-4-5'/> 3.2.4.5. Deleting a Service
1. Click the delete button on the Service details page to remove the respective service.

#### Service Details Page
![IMG_055]

#### Service Deletion Popup
![IMG_056]

### <div id='3-2-5'/> 3.2.5. Storage Menu
#### <div id='3-2-5-1'/> 3.2.5.1. Persistent Volumes
##### <div id='3-2-5-1-1'/> 3.2.5.1.1. Viewing Persistent Volume List
1. Click on 'Persistent Volumes' in 'Storages' to access the list of Persistent Volumes.

![IMG_057]

##### <div id='3-2-5-1-2'/> 3.2.5.1.2. Viewing Persistent Volume Details
1. Click on a specific Persistent Volume in the list to view its detailed information.

#### Persistent Volume Details Page
![IMG_058]

##### <div id='3-2-5-1-3'/> 3.2.5.1.3. Creating a Persistent Volume
1. Click the create button on the Persistent Volume list to initiate the creation of a new Persistent Volume.

#### Persistent Volume List Page
![IMG_059]

#### Persistent Volume Creation Popup
![IMG_060]

##### <div id='3-2-5-1-4'/> 3.2.5.1.4. Modifying a Persistent Volume
1. Click the modify button on the Persistent Volume details page to modify the existing Persistent Volume.

#### Persistent Volume Details Page
![IMG_061]

#### Persistent Volume Modification Popup
![IMG_062]

##### <div id='3-2-5-1-5'/> 3.2.5.1.5. Deleting a Persistent Volume
1. Click the delete button on the Persistent Volume details page to remove the respective Persistent Volume.

#### Persistent Volume Details Page
![IMG_063]

#### Persistent Volume Deletion Popup
![IMG_064]

#### <div id='3-2-5-2'/> 3.2.5.2. Persistent Volume Claims
##### <div id='3-2-5-2-1'/> 3.2.5.2.1. Viewing Persistent Volume Claim List
1. Click on 'Persistent Volume Claims' in 'Storages' to access the list of Persistent Volume Claims.

![IMG_065]

##### <div id='3-2-5-2-2'/> 3.2.5.2.2. Viewing Persistent Volume Claim Details
1. Click on a specific Persistent Volume Claim in the list to view its detailed information.

#### Persistent Volume Claim Details Page
![IMG_066]

##### <div id='3-2-5-2-3'/> 3.2.5.2.3. Creating a Persistent Volume Claim
1. Click the create button on the Persistent Volume Claim list to initiate the creation of a new Persistent Volume Claim.

#### Persistent Volume Claim List Page
![IMG_067]

#### Persistent Volume Claim Creation Popup
![IMG_068]

##### <div id='3-2-5-2-4'/> 3.2.5.2.4. Modifying a Persistent Volume Claim
1. Click the modify button on the Persistent Volume Claim details page to modify the existing Persistent Volume Claim.

#### Persistent Volume Claim Details Page
![IMG_069]

#### Persistent Volume Claim Modification Popup
![IMG_070]

##### <div id='3-2-5-2-5'/> 3.2.5.2.5. Deleting a Persistent Volume Claim
1. Click the delete button on the Persistent Volume Claim details page to remove the respective Persistent Volume Claim.

#### Persistent Volume Claim Details Page
![IMG_071]

#### Persistent Volume Claim Deletion Popup
![IMG_072]

#### <div id='3-2-5-3'/> 3.2.5.3. Storage Classes
##### <div id='3-2-5-3-1'/> 3.2.5.3.1. Viewing Storage Class List
1. Click on 'Storage Classes' in 'Storages' to access the Storage Class list.
![IMG_073]

##### <div id='3-2-5-3-2'/> 3.2.5.3.2. Viewing Storage Class Details
1. Click on a specific Storage Class in the list to view its detailed information.

#### Storage Class Details Page
![IMG_074]

##### <div id='3-2-5-3-3'/> 3.2.5.3.3. Creating a Storage Class
1. Click the create button on the Storage Class list to create a new Storage Class.

#### Storage Class List Page
![IMG_075]

#### Storage Class Creation Popup
![IMG_076]

##### <div id='3-2-5-3-4'/> 3.2.5.3.4. Modifying a Storage Class
1. Click the modify button on the Storage Class details page to edit the existing Storage Class.

#### Storage Class Details Page
![IMG_077]

#### Storage Class Modification Popup
![IMG_078]

##### <div id='3-2-5-3-5'/> 3.2.5.3.5. Deleting a Storage Class
1. Click the delete button on the Storage Class details page to remove the respective Storage Class.

#### Storage Class Details Page
![IMG_079]

#### Storage Class Deletion Popup
![IMG_080]

### <div id='3-2-6'/> 3.2.6. Managements
#### <div id='3-2-6-1'/> 3.2.6.1. Users
##### <div id='3-2-6-1-1'/> 3.2.6.1.1. Viewing Cluster Administrators
1. Select 'Users' under the 'Managements' menu, and click on the 'Administrator' tab to view the cluster administrators.

![IMG_081]

##### <div id='3-2-6-1-2'/> 3.2.6.1.2. Viewing Cluster Administrator Details
1. Click on the Cluster Administrator User ID to navigate to the detailed information page of the cluster administrator.

#### Cluster Administrator Details Page
![IMG_082]

##### <div id='3-2-6-1-3'/> 3.2.6.1.3. Viewing Regular User List
1. Select 'Users' under the 'Managements' menu, and click on the 'User' tab to view the list of regular users.

![IMG_083]

##### <div id='3-2-6-1-4'/> 3.2.6.1.4. Viewing Regular User Details
1. Click on the Regular User User ID to navigate to the detailed information page of the regular user.

#### Regular User Details Page
![IMG_084]

##### <div id='3-2-6-1-5'/> 3.2.6.1.5. User Creation
- On the User creation page, you can register user accounts and specify the Namespace and Role for the respective user to utilize.

1. Click the create button on the User list to navigate to the User creation page.

#### User List Page
![IMG_085]

#### User Creation Popup
![IMG_086]
![IMG_087]
![IMG_088]

##### <div id='3-2-6-1-6'/> 3.2.6.1.6. User Modification
- On the User modification page, you can modify user account details and adjust the Namespace and Role for the respective user.

1. Click the modify button on the User details page to navigate to the User modification page.

#### User Details Page
![IMG_089]

#### User Modification Page
![IMG_090]
![IMG_091]
![IMG_092]

##### <div id='3-2-6-1-7'/> 3.2.6.1.7. User Deletion
1. Click the delete button on the User details page to delete the user.

#### User Details Page
![IMG_093]

#### User Deletion Popup
![IMG_094]


#### <div id='3-2-6-2'/> 3.2.6.2. Roles
##### <div id='3-2-6-2-1'/> 3.2.6.2.1. Viewing Role List
1. Click on 'Roles' under the 'Managements' menu to access the Role list page.
![IMG_095]

##### <div id='3-2-6-2-2'/> 3.2.6.2.2. Viewing Role Details
1. Click on a specific Role in the list to view its detailed information.

#### Role Details Page
![IMG_096]

##### <div id='3-2-6-2-3'/> 3.2.6.2.3. Creating a Role
1. Click the create button on the Role list page to create a new Role.

#### Role List Page
![IMG_097]

#### Role Creation Popup
![IMG_098]

##### <div id='3-2-6-2-4'/> 3.2.6.2.4. Modifying a Role
1. Click the modify button on the Role details page to edit the existing Role.

#### Role Details Page
![IMG_099]

#### Role Modification Popup
![IMG_100]
#### <div id='3-2-6-2-5'/> 3.2.6.2.5. Role Deletion
1. Clicking the delete button on the Role details page will delete the Role.

#### Role Details Page
![IMG_101]

#### Role Deletion Popup
![IMG_102]

#### <div id='3-2-6-3'/> 3.2.6.3. Resource Quotas
##### <div id='3-2-6-3-1'/> 3.2.6.3.1. Viewing Resource Quota List
1. Click on 'Resource Quotas' under the 'Managements' menu to access the Resource Quota list page.
![IMG_103]

##### <div id='3-2-6-3-2'/> 3.2.6.3.2. Viewing Resource Quota Details
1. Click on a specific Resource Quota in the list to view its detailed information.

#### Resource Quota Details Page
![IMG_104]

##### <div id='3-2-6-3-3'/> 3.2.6.3.3. Creating a Resource Quota
1. Click the create button on the Resource Quota list page to create a new Resource Quota.

#### Resource Quota List Page
![IMG_105]

#### Resource Quota Creation Popup
![IMG_106]

##### <div id='3-2-6-3-4'/> 3.2.6.3.4. Modifying a Resource Quota
1. Click the modify button on the Resource Quota details page to edit the existing Resource Quota.

#### Resource Quota Details Page
![IMG_107]

#### Resource Quota Modification Popup
![IMG_108]

##### <div id='3-2-6-3-5'/> 3.2.6.3.5. Resource Quota Deletion
1. Clicking the delete button on the Resource Quota details page will delete the Resource Quota.

#### Resource Quota Details Page
![IMG_109]

#### Resource Quota Deletion Popup
![IMG_110]

#### <div id='3-2-6-4'/> 3.2.6.4. Limit Ranges
##### <div id='3-2-6-4-1'/> 3.2.6.4.1. Viewing Limit Range List
1. Click on 'Limit Ranges' under the 'Managements' menu to access the Limit Range list page.
![IMG_111]

##### <div id='3-2-6-4-2'/> 3.2.6.4.2. Viewing Limit Range Details
1. Click on a specific Limit Range in the list to view its detailed information.

#### Limit Range Details Page
![IMG_112]

##### <div id='3-2-6-4-3'/> 3.2.6.4.3. Creating a Limit Range
1. Click the create button on the Limit Range list page to create a new Limit Range.

#### Limit Range List Page
![IMG_113]

#### Limit Range Creation Popup
![IMG_114]

##### <div id='3-2-6-4-4'/> 3.2.6.4.4. Modifying a Limit Range
1. Click the modify button on the Limit Range details page to edit the existing Limit Range.

#### Limit Range Details Page
![IMG_115]

#### Limit Range Modification Popup
![IMG_116]

##### <div id='3-2-6-4-5'/> 3.2.6.4.5. Limit Range Deletion
1. Clicking the delete button on the Limit Range details page will delete the Limit Range.

#### Limit Range Details Page
![IMG_117]

#### Limit Range Deletion Popup
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
