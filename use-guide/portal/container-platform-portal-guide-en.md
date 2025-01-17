### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Portal Use Guide

<br>

## Table of Contents

1. [Document Outline](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Range](#1-2)
2. [Prerequisite](#2)
    * [2.1. Terraman IaC Settings](#2-1)
3. [Access the Container Platform Portal](#3)
    * [3.1. Log in to the Container Platform Portal administrator account](#3-1)
    * [3.2. Log in to the Container Platform Portal user account](#3-2)
4. [Container Platform Portal Configuration](#4)
    * [4.1. Container Platform Portal user permission types](#4-1)
    * [4.2. Configure the Container Platform Portal menu](#4-2)
5. [Container Platform Portal menu description](#5)
    * [5.1. Global menu](#5-1)
    * [5.1.1. Overview](#5-1-1)
    * [5.1.2. Clusters](#5-1-2)
    * [5.1.3. Cloud Accounts](#5-1-3)
    * [5.1.4. Instance Code Template](#5-1-4)
    * [5.1.5. SSH Keys](#5-1-5)
    * [5.2. Clusters Menu](#5-2)
    * [5.2.1. Overview](#5-2-1)
    * [5.2.2. Nodes](#5-2-2)
    * [5.2.3. Namespaces](#5-2-3)
    * [5.3. Workloads Menu](#5-3)
    * [5.3.1. Deployment](#5-3-1)
    * [5.3.2. Pods](#5-3-2)
    * [5.3.3. ReplicaSets](#5-3-3)
    * [5.4. Services Menu](#5-4)
    * [5.4.1. Services](#5-4-1)
    * [5.4.2. Ingresses](#5-4-2)
    * [5.5. Storages menu](#5-5)
    * [5.5.1. Persistent Volumes](#5-5-1)
    * [5.5.2. Persistent Volume Claims](#5-5-2)
    * [5.5.3. Storage Classes](#5-5-3)
    * [5.6. ConfigMaps Menu](#5-6)
    * [5.6.1. ConfigMaps](#5-6-1)
    * [5.7. Managements menu](#5-7)
    * [5.7.1. Users](#5-7-1)
    * [5.7.2. Roles](#5-7-2)
    * [5.7.3. Resource Quotas](#5-7-3)
    * [5.7.4. Limit Ranges](#5-7-4)
    * [5.8. Info Menu](#5-8)
    * [5.8.1. Access](#5-8-1)
    * [5.8.2. Private Repository](#5-8-2)



<br>


# <div id='1'/> 1. Documentation Overview

## <div id='1-1'/> 1.1. Purpose
This document describes how to use the Container Platform Portal.

<br>

## <div id='1-2'/> 1.2. Range
This document describes how to use the Container Platform Portal based on the Windows environment.

<br>

# <div id='2'>2. Prerequisite

## <div id='2-1'>2.1. Setting up Terraman IaC
Creating a subcluster through the Container Platform portal requires additional setup of the target cloud (IaaS). <br>
See the guide below to create a subcluster after completing the setup.
> [Terraman IaC Scripting Guide](../../use-guide/terraman/cp-terraman-guide.md)

<br>

## <div id='3'>3. Access the Container Platform Portal
Access the Container Platform portal.<br><br>
**Container Platform Portal URL** : `http://portal.${HOST_DOMAIN}`
+ Enter the value of `HOST_DOMAIN` as defined in [[3.1.2. Defining Container Platform Portal variables]] (..//portal/cp-portal-standalone-guide.md#3.1.2)

<br>

### <div id='3-1'/>3.1. Log in to your Container Platform Portal administrator account
Administrator accounts require a password reset setting, so preprocess them as described below.
<details>
<summary><h4> :key: Set Container Platform Portal administrator account password </h4></summary>

<h1></h1>  

### 1. Get Keycloak Admin account information
To check the Keycloak Admin account information, execute the command below.
```bash
# Get Keycloak Admin account
$ kubectl get cm cp-portal-configmap -n cp-portal -o yaml | grep KEYCLOAK_ADMIN
KEYCLOAK_ADMIN_USERNAME: ********* (Username)
KEYCLOAK_ADMIN_PASSWORD: ********* (Password)
```

<br>

### 2. Access and login to the Keycloak Admin Console
Access the Keycloak Admin Console and log in to the Keycloak Admin account you found.<br><br>

**Keycloak Admin Console URL** : `http://keycloak.${HOST_DOMAIN}/auth/admin`
+ Connect as `https` when Keycloak TLS is applied
+ Enter the value of `HOST_DOMAIN` as defined in [[3.1.2. Defining Container Platform Portal variables]] (..//portal/cp-portal-standalone-guide.md#3.1.2)

![image 011]

<br>

### 3. Reset the container platform administrator account password
- Change the Realm information in the top left corner to `Cp-realm`.

![image 012]

- Select the menu [Users] and click on the account with the username `admin`.

![image 013]

- Select the menu [Credentials] and click the button [Reset password] to reset the password.
    + **Password** : `Enter a password value to initialize`
    + **Temporary** : Select `Off
        - If 'On' is selected, the user will need to change the password again at the next login.

![image 014]
![image 015]

<h1></h1>

</details>

- Log in to the Container Platform portal with an administrator account.
    + **Username** : `admin`
    + **Password** : `initialized password value`

![image 002]

<br>

### <div id='3-2'/>3.2. Container Platform Portal User Account Login
#### Register as a user at
- Click the 'Register' button at the bottom.

![image 003]

- Enter the user account information to register, and click 'Register' button to sign up for the Container Platform Portal.

![image 004]

- You cannot access the portal immediately after registering, but you can use the portal after the administrator assigns a Namespace and Role to the user.
  To assign Namespace and Role to a user, see [[5.7.1.5. Modify User]](#5-7-1-5).


#### User login
- Log in to the Container Platform portal with the account registered through registration.

![image 006]

<br>    

# <div id='4'/> 4. Configure the Container Platform Portal
## <div id='4-1'/> 4.1. Container Platform Portal User Permission Types
| <center>User Type</center> | <center>Function</center> |
| :--- | :--- | 
| Super Admin (Full Admin) | Full cluster management privileges |
| Cluster Admin | Assigned cluster administration privileges |
| User (regular user) | Assigned in-cluster namespace management privileges |

<br>

## <div id='4-2'/> 4.2. Configure the Container Platform Portal Menu
<table style="table-layout: fixed; width: 816px;>
<colgroup>
<col style="width: 126px">
<col style="width: 174px">
<col style="width: 307px">
<col style="width: 209px">
</colgroup>
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
    <td rowspan="4">Global</td>
    <td>Overview</td>
    <td>Cluster information dashboard</td>
    <td rowspan="4">Super Admin <br> Cluster Admin (View Permissions)</td>
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
    <td rowspan="3">Clusters</td>
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
    <td rowspan="3">Workloads</td>
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
    <td rowspan="2">Services</td>
    <td>Services</td>
    <td>Services Information Management</td>
    <td rowspan="2">All</td>
  </tr>
  <tr>
    <td>Ingresses</td>
    <td> Manage Ingresses information</td>
  </tr>
  <tr>
    <td rowspan="3">Storages</td>
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
    <td>ConfigMaps</td>
    <td>ConfigMaps</td>
    <td> Manage ConfigMaps Information</td>
    <td>All</td>
  </tr>
  <tr>
    <td rowspan="4">Managements</td>
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
    <td rowspan="2">Info</td>
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

# <div id='5'/> 5. Container Platform Portal Menu Description
This chapter describes the menus in the Container Platform portal.

<br>

## <div id='5-1'/> 5.1. Global Menu
### <div id='5-1-1'/> 5.1.1. Overview
#### <div id='5-1-1-1'/> 5.1.1.1. Get Overview Information
- View cluster information and TOP Node (CPU, Memory).
  ![IMG_1_1_1]


<br>

### <div id='5-1-2'/> 5.1.2. Clusters
#### <div id='5-1-2-1'/> 5.1.2.1. Get a list of Clusters
- Get a list of clusters.
  ![IMG_1_2_1]

<br>

#### <div id='5-1-2-2'/> 5.1.2.2. Viewing Clusters in Detail
- View cluster details.
  ![IMG_1_2_2]

<br>

#### <div id='5-1-2-3'/> 5.1.2.3. Create Clusters
- To create a cluster, enter the details below and click the Save button to configure the cluster environment.
1. **Cluster Name** : Enter the name of the cluster to create.
2. **Provider** : Select the Provider of the cluster to be created ([1] AWS [2] OPENSTACK [3] NAVER [4] NHN)
3. **Cloud Accounts** : Select the account information for the selected provider.
4. **Template** : Select HCL Template for VM deployment.
5. **Description** : Enter additional information (optional)
6. **Template Detail** : Fill in your own environment information based on the selected HCL Template.
   ![IMG_1_2_3]

<br>

#### <div id='5-1-2-4'/> 5.1.2.4. Registering Clusters
- Register the cluster.
1. **Cluster Name** : Enter the name of the cluster to register.
2. **Provider** : Select the Provider of the cluster to register ([1] AWS [2] OPENSTACK [3] NAVER [4] NHN [5] KT)
3. **Cluster API URL** : Enter the target cluster Kubernetes API URL (e.g: https[]()://xxx.xxx.xxx.xxx.xxx:6443)
4. **Description** : Enter additional information (optional)
5. **Cluster Service Token** : Enter the authentication token information of 'cluster-admin' permission to access the target cluster.
   ![IMG_1_2_4]

<br>

#### <div id='5-1-2-5'/> 5.1.2.5. Modify Clusters
- Modify cluster information.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After making changes, click the Edit button at the bottom to change the cluster information.
  ![IMG_1_2_5]

<br>

### <div id='5-1-3'/> 5.1.3. Cloud Accounts
#### <div id='5-1-3-1'/> 5.1.3.1. Get a list of Cloud Accounts
- View the list of Cloud Accounts.
  ![IMG_1_3_1]

<br>

#### <div id='5-1-3-2'/> 5.1.3.2. View Cloud Accounts in detail
- View detailed Cloud Accounts information.
  ![IMG_1_3_2]

<br>

#### <div id='5-1-3-3'/> 5.1.3.3. Create a cloud account
- Create Cloud Accounts.
- Used for credentials required for automated VM creation via IaC, and stored securely via Vault.
- The inputs vary depending on the provider.

|  **AWS**  | **OPENSTACK** | **NAVER** |  **NHN**  |
|:---------:|:-------------:|:---------:|:---------:|
| accessKey |   auth_url    | accessKey | auth_url  |
| secretKey |   password    | secretKey | password  |
|  region   |   user_name   |   site    | user_name |
|           |    project    | accessKey |  project  |
|           |    region     |  region   |  region   |

![IMG_1_3_3_1]

<br>

#### <div id='5-1-3-4'/> 5.1.3.4. Modify your cloud account
- Modify Cloud Accounts.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the contents, click the Edit button at the bottom to change the Cloud Accounts information.
  ![IMG_1_3_4]

<br>

#### <div id='5-1-3-5'/> 5.1.3.5. Delete a cloud account
- Delete Cloud Accounts.
- Click the Delete button at the bottom of the detail screen to delete the cloud account.
  ![IMG_1_3_5]

<br>

### <div id='5-1-4'/> 5.1.4. Instance Code Template
Instance Code Template is a template of code that performs automated VM deployment through IaC, and you can register the template in advance to easily deploy subclusters through the K-PaaS container platform, and it provides templates for AWS, OPENSTACK, NAVER, and NHN by default.
#### <div id='5-1-4-1'/> 5.1.4.1. Get a list of instance code templates
- Gets a list of Instance Code Templates.
  ![IMG_1_4_1]

<br>

#### <div id='5-1-4-2'/> 5.1.4.2. Get Instance Code Template Details
- Get detailed Instance Code Template information.
  ![IMG_1_4_2]

<br>

#### <div id='5-1-4-3'/> 5.1.4.3. Create an instance code template
- Create an Instance Code Template.
  ![IMG_1_4_3]

<br>

#### <div id='5-1-4-4'/> 5.1.4.4. Modify the instance code template
- Modify the Instance Code Template.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the content, click the Edit button at the bottom to change the template information.
  ![IMG_1_4_4]

<br>

#### <div id='5-1-4-5'/> 5.1.4.5. Delete the instance code template
- Delete the Instance Code Template.
- Click the Delete button at the bottom of the detail screen to delete the Instance Code Template.
  ![IMG_1_4_5]

<br>

### <div id='5-1-5'/> 5.1.5. SSH Keys
You can register an SSH key to access subcluster instances through the K-PaaS container platform with the SSH key for multicluster creation.
#### <div id='5-1-5-1'/> 5.1.5.1. Get a list of SSH Keys
- Retrieve a list of SSH Keys.
  ![IMG_1_5_1]

<br>

#### <div id='5-1-5-2'/> 5.1.5.2. Get SSH Keys Details
- View detailed SSH Keys information.
  ![IMG_1_5_2]

<br>

#### <div id='5-1-5-3'/> 5.1.5.3. Generate SSH Keys
- Generate SSH Keys.
  ![IMG_1_5_3]

<br>

#### <div id='5-1-5-4'/> 5.1.5.4. Modify SSH Keys
- Modify the SSH Keys.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the contents, click the Edit button at the bottom to change the SSH Key information.
  ![IMG_1_5_4]

<br>

#### <div id='5-1-5-5'/> 5.1.5.5. Delete SSH Keys
- Delete SSH Keys.
- Click the Delete button at the bottom of the detail screen to delete the SSH Keys.
  ![IMG_1_5_5]

<br>  

## <div id='5-2'/> 5.2. Cluster Menu
### <div id='5-2-1'/> 5.2.1. Overview
#### <div id='5-2-1-1'/> 5.2.1.1. Get Overview Information
- Get the number of Namespaces, Deployments, Pods, and Users, as well as a chart of Deployments, Pods, and ReplicaSets.
  ![IMG_2_1_1]

<br>

#### <div id='5-2-1-2'/> 5.2.1.2. Changing the Overview Cluster
- Selecting a cluster in the Select Box will retrieve information about that cluster.
- After selecting a cluster, selecting a Namespace from the Select Box will retrieve information about that Namespace.
- For Namespace **'ALL'**, information about the entire Namespace is retrieved.
  ![IMG_2_1_2_1]
  ![IMG_2_1_2_2]

<br>

### <div id='5-2-2'/> 5.2.2. Nodes
#### <div id='5-2-2-1'/> 5.2.2.1. Get a list of Nodes
- Click Nodes in the Clusters menu to go to the Node list page.
  ![IMG_2_2_1]

<br>

#### <div id='5-2-2-2'/> 5.2.2.2. Get Node Details
- Click the name of a node in the Nodes list to go to the node details page.
  ![IMG_2_2_2]

<br>

### <div id='5-2-3'/> 5.2.3. Namespaces
#### <div id='5-2-3-1'/> 5.2.3.1. Get a list of Namespaces
- Click Namespaces in the Clusters menu to go to the Namespace list page.
  ![IMG_2_3_1]

<br>

#### <div id='5-2-3-2'/> 5.2.3.2. Get Namespace Details
- Click the name of a namespace in the Namespaces list to go to the namespace details page.
  ![IMG_2_3_2]

<br>

#### <div id='5-2-3-3'/> 5.2.3.3. Create a Namespace
- Click the Create button in the Namespace list to go to the Create Namespace page.
- On the Create Namespace page, you can specify **Resource Quotas**, **Limit Ranges**.
- Click the Save button at the bottom of the Create Namespace page to create the namespace.
  ![IMG_2_3_3_1]
  ![IMG_2_3_3_2]
  ![IMG_2_3_3_3]

<br>

#### <div id='5-2-3-4'/> 5.2.3.4. Modify the Namespace
- Under Namespace details, click the Edit button to go to the Edit Namespace page.
- You can modify **Resource Quotas**, **Limit Ranges** on the Edit Namespace page.
- Click the Save button at the bottom of the Edit Namespace page to modify the namespace.
  ![IMG_2_3_4]

<br>

#### <div id='5-2-3-5'/> 5.2.3.5. Delete a Namespace
- In the Namespace details, click the Delete button to delete the namespace.
  ![IMG_2_3_5]

<br>

## <div id='5-3'/> 5.3. Workload Menu
### <div id='5-3-1'/> 5.3.1. Deployments
#### <div id='5-3-1-1'/> 5.3.1.1. Get a list of deployments
- Click Deployments in Workloads to go to the Deployments list page.
  ![IMG_3_1_1]

<br>

#### <div id='5-3-1-2'/> 5.3.1.2. Get Deployment Details
- Click the name of the deployment in the Deployments list to go to the deployment details page.
  ![IMG_3_1_2]

<br>

#### <div id='5-3-1-3'/> 5.3.1.3. Create Deployment
- When you click the Create button in the Deployment list, the Create Deployment popup appears.
  ![IMG_3_1_3]

<br>

#### <div id='5-3-1-4'/> 5.3.1.4. Modify the Deployment
- When you click the Edit button in the Deployment details, the Edit Deployment popup appears.
  ![IMG_3_1_4]

<br>

#### <div id='5-3-1-5'/> 5.3.1.5. Delete a deployment
- Clicking the Delete button in the Deployment details will delete the Deployment.
  ![IMG_3_1_5]

<br>


### <div id='5-3-2'/> 5.3.2. Pods
#### <div id='5-3-2-1'/> 5.3.2.1. Get a list of pods
- Click Pods in Workloads to go to the Pods list page.
  ![IMG_3_2_1]

<br>

#### <div id='5-3-2-2'/> 5.3.2.2. View Pod Details
- Click a pod name in the Pods list to go to the pod details page.
  ![IMG_3_2_2]

<br>

#### <div id='5-3-2-3'/> 5.3.2.3. Create a Pod
- When you click the Create button in the Pods list, the Create Pod popup appears.
  ![IMG_3_2_3]

<br>

#### <div id='5-3-2-4'/> 5.3.2.4. Modify the Pod
- When you click the Edit button in the pod details, the Edit Pod popup appears.
  ![IMG_3_2_4]

<br>

#### <div id='5-3-2-5'/> 5.3.2.5. Pod
- Clicking the Delete button in the pod details completes the deletion of the pod.
  ![IMG_3_2_5]

<br>

### <div id='5-3-3'/> 5.3.3. ReplicaSets
#### <div id='5-3-3-1'/> 5.3.3.1. Get a list of ReplicaSets
- Click ReplicaSets in Workloads to go to the ReplicaSet list page.
  ![IMG_3_3_1]

<br>

#### <div id='5-3-3-2'/> 5.3.3.2. Get ReplicaSet Details
- In the ReplicaSet list, click the name of the ReplicaSet to go to the ReplicaSet details page.
  ![IMG_3_3_2]

<br>

#### <div id='5-3-3-3'/> 5.3.3.3. Create a ReplicaSet
- When you click the Create button in the ReplicaSet list, the Create ReplicaSet pop-up window appears.
  ![IMG_3_3_3]

<br>

#### <div id='5-3-3-4'/> 5.3.3.4. Modify the ReplicaSet
- When you click the Edit button in the ReplicaSet details, the Edit ReplicaSet popup window appears.
  ![IMG_3_3_4]

<br>

#### <div id='5-3-3-5'/> 5.3.3.5. Delete the ReplicaSet
- Clicking the Delete button in the ReplicaSet details completes the deletion of the ReplicaSet.
  ![IMG_3_3_5]

<br>

## <div id='5-4'/> 5.4. Service Menu
### <div id='5-4-1'/> 5.4.1. Services
#### <div id='5-4-1-1'/> 5.4.1.1. Get the list of services
- Click Services in Services to go to the Services list page.
  ![IMG_4_1_1]

<br>

#### <div id='5-4-1-2'/> 5.4.1.2. Get Service Details
- Click the service name in the Services list to go to the service details page.
  ![IMG_4_1_2]

<br>

#### <div id='5-4-1-3'/> 5.4.1.3. Create a Service
- When you click the Create button in the Service list, the Create Service popup appears.
  ![IMG_4_1_3]

<br>

#### <div id='5-4-1-4'/> 5.4.1.4. Modify the Service
- When you click the Edit button in the service details, the Edit Service popup appears.
  ![IMG_4_1_4]

<br>

#### <div id='5-4-1-5'/> 5.4.1.5. Delete the Service
- Clicking the Delete button in the service details completes the deletion of the service.
  ![IMG_4_1_5]

<br>

### <div id='5-4-2'/> 5.4.2. Ingresses
#### <div id='5-4-2-1'/> 5.4.2.1. Get the Ingress list
- Click Ingresses in Services to go to the Ingresses list page.
  ![IMG_4_2_1]

<br>

#### <div id='5-4-2-2'/> 5.4.2.2. Ingress Detail Lookup
- Click the name of an ingress in the Ingress list to go to the ingress details page.
  ![IMG_4_2_2]

<br>

#### <div id='5-4-2-3'/> 5.4.2.3. Create an Ingress
- Clicking the Create button in the Ingress list will take you to the Create Ingress page.
- Enter the **Rule** (Host, Path, Target) information.
  ![IMG_4_2_3]

<br>

#### <div id='5-4-2-4'/> 5.4.2.4. Fix Ingress
- If an item is editable, you can edit the value by clicking the icon next to the item.
- After changing the content, click the Edit button at the bottom to change the Ingress information.
  ![IMG_4_2_4]

<br>

#### <div id='5-4-2-5'/> 5.4.2.5. Delete Ingress
- Clicking the Delete button in the Ingress details completes the deletion of the Ingress.
  ![IMG_4_2_5]

<br>

## <div id='5-5'/> 5.5. Repository Menu
### <div id='5-5-1'/> 5.5.1. Persistent Volumes
#### <div id='5-5-1-1'/> 5.5.1.1. Get a list of Persistent Volumes
- Click Persistent Volumes in Storage to go to the Persistent Volumes list page.
  ![IMG_5_1_1]

<br>

#### <div id='5-5-1-2'/> 5.5.1.2. Get Persistent Volume Details
- In the Persistent Volumes list, click the name of the Persistent Volume to go to the Persistent Volume details page.
  ![IMG_5_1_2]

<br>

#### <div id='5-5-1-3'/> 5.5.1.3. Create a Persistent Volume
- When you click the Create button in the Persistent Volume list, the Create Persistent Volume pop-up window appears.
  ![IMG_5_1_3]

<br>

#### <div id='5-5-1-4'/> 5.5.1.4. Modify a Persistent Volume
- When you click the Modify button in the Persistent Volume details, the Modify Persistent Volume pop-up window appears.
  ![IMG_5_1_4]

<br>

#### <div id='5-5-1-5'/> 5.5.1.5. Delete a Persistent Volume
- Clicking the Delete button in the Persistent Volume details completes the deletion of the Persistent Volume.
  ![IMG_5_1_5]

<br>

### <div id='5-5-2'/> 5.5.2. Persistent Volume Claims
#### <div id='5-5-2-1'/> 5.5.2.1. Get a list of persistent volume claims
- Click Persistent Volume Claims in Storage to go to the Persistent Volume Claims list page.
  ![IMG_5_2_1]

<br>

#### <div id='5-5-2-2'/> 5.5.2.2. Get a detailed view of persistent volume claims
- In the Persistent Volume Claim list, click the name of the Persistent Volume Claim to go to the Persistent Volume Claim details page.
  ![IMG_5_2_2]

<br>

#### <div id='5-5-2-3'/> 5.5.2.3. Create a persistent volume claim
- When you click the Create button in the Persistent Volume Claim list, the Create Persistent Volume Claim pop-up window appears.
  ![IMG_5_2_3]

<br>

#### <div id='5-5-2-4'/> 5.5.2.4. Modify the Persistent Volume Claim
- When you click the Edit button in the Persistent Volume Claim details, the Edit Persistent Volume Claim pop-up window appears.
  ![IMG_5_2_4]

<br>

#### <div id='5-5-2-5'/> 5.5.2.5. Delete a persistent volume claim
- Clicking the Delete button in the Persistent Volume Claim details completes the deletion of the Persistent Volume Claim.
  ![IMG_5_2_5]

<br>

### <div id='5-5-3'/> 5.5.3. Storage Classes
#### <div id='5-5-3-1'/> 5.5.3.1. Get a list of Storage Classes
- Click Storage Classes in Storages to go to the Storage Classes list page.
  ![IMG_5_3_1]

<br>

#### <div id='5-5-3-2'/> 5.5.3.2. Get Storage Class Details
- Click the name of the storage class in the Storage Class list to go to the storage class details page.
  ![IMG_5_3_2]

<br>

#### <div id='5-5-3-3'/> 5.5.3.3. Create a Storage Class
- When you click the Create button in the Storage Class list, the Create Storage Class pop-up window appears.
  ![IMG_5_3_3]

<br>

#### <div id='5-5-3-4'/> 5.5.3.4. Modify the Storage Class
- When you click the Edit button in the Storage Class details, the Edit Storage Class pop-up window appears.
  ![IMG_5_3_4]

<br>

#### <div id='5-5-3-5'/> 5.5.3.5. Delete the Storage Class
- Clicking the Delete button in the Storage Class details completes the deletion of the storage class.
  ![IMG_5_3_5]

<br>

## <div id='5-6'/> 5.6. ConfigMap Menu
### <div id='5-6-1'/> 5.6.1. ConfigMaps
#### <div id='5-6-1-1'/> 5.6.1.1. Get the ConfigMap list
- Click ConfigMaps in ConfigMaps to go to the ConfigMap list page.
  ![IMG_6_1_1]

<br>

#### <div id='5-6-1-2'/> 5.6.1.2. View ConfigMap Details
- Click the name of a ConfigMap in the ConfigMap list to go to the ConfigMap details page.
  ![IMG_6_1_2]

<br>

#### <div id='5-6-1-3'/> 5.6.1.3. Create a ConfigMap
- When you click the Create button in the ConfigMap list, the Create ConfigMap popup window appears.
  ![IMG_6_1_3]

<br>

#### <div id='5-6-1-4'/> 5.6.1.4. Modify the ConfigMap
- When you click the Edit button in the ConfigMap details, the Edit ConfigMap popup window appears.
  ![IMG_6_1_4]

<br>

#### <div id='5-6-1-5'/> 5.6.1.5. Delete a ConfigMap
- Clicking the Delete button in the ConfigMap details completes the deletion of the ConfigMap.
  ![IMG_6_1_5]

<br>

## <div id='5-7'/> 5.7. Managements
### <div id='5-7-1'/> 5.7.1. Users
#### <div id='5-7-1-1'/> 5.7.1.1. Lookup the Cluster Administrator
- Select Users from the Managements menu and click the Administrator tab to view the cluster administrators.
- More than one person can be a cluster administrator.
  ![IMG_7_1_1]

<br>

#### <div id='5-7-1-2'/> 5.7.1.2. View Cluster Manager Details
- Click the cluster administrator User ID to go to the cluster administrator details page.
  ![IMG_7_1_2]

<br>

#### <div id='5-7-1-3'/> 5.7.1.3. Get a list of common users
- Select Users from the Managements menu and click the Users tab to view the list of users.
    + Active tab: List of users assigned Namespace/Role in this cluster
  + Inactive tab: list of users in the cluster who are not assigned a Namespace/Role
      ![IMG_7_1_3_1]
      ![IMG_7_1_3_2]

<br>

#### <div id='5-7-1-4'/> 5.7.1.4. Get general user details
- Click the general user User ID to go to the general user detail lookup page.
  ![IMG_7_1_4]

<br>

#### <div id='5-7-1-5'/> 5.7.1.5. Modify User
- Under User details, click the Edit button to go to the Edit User page.
- On the Edit User page, you can assign the user either Cluster Admin or User permissions.

<br>

**Set the Cluster Admin privileges**.
>'Authority' item: select as 'Cluster Admin'

![IMG_7_1_5_1]

<br>

**Set general User permissions**.
>'Authority' item: select as 'User'
> 'Namespaces ⁄ Roles' item: click the Select button to select the Namespace/Role to assign to the user'

![IMG_7_1_5_2]
![IMG_7_1_5_3]
![IMG_7_1_5_4]

<br>

##### Click the Edit button at the bottom of to reflect the changed User permissions.
![IMG_7_1_5_5]

<br>

### <div id='5-7-2'/> 5.7.2. Roles
#### <div id='5-7-2-1'/> 5.7.2.1. Get a list of roles
- Click Roles in the Managements menu to go to the Role list page.
  ![IMG_7_2_1]

<br>

#### <div id='5-7-2-2'/> 5.7.2.2. View Role Details
- Click the name of a role in the Role list to go to the role details page.
  ![IMG_7_2_2]

<br>

#### <div id='5-7-2-3'/> 5.7.2.3. Create a Role
- When you click the Create button in the Role list, the Create role popup appears.
  ![IMG_7_2_3]

<br>

#### <div id='5-7-2-4'/> 5.7.2.4. Modify the Role
c- When you click the Edit button in the role details, the Edit role popup appears.
![IMG_7_2_4]

<br>

#### <div id='5-7-2-5'/> 5.7.2.5. Delete a Role
- Clicking the Delete button in the role details will delete the role.
  ![IMG_7_2_5]

<br>

### <div id='5-7-3'/> 5.7.3. Resource Quotas
#### <div id='5-7-3-1'/> 5.7.3.1. Get a list of Resource Quotas
- Click Resource Quotas on the Managements menu to go to the Resource Quotas list page.
  ![IMG_7_3_1]

<br>

#### <div id='5-7-3-2'/> 5.7.3.2. View Resource Quota Details
- Click the name of a resource quota in the Resource Quota list to go to the Resource Quota details page.
  ![IMG_7_3_2]

<br>

#### <div id='5-7-3-3'/> 5.7.3.3. Create a Resource Quota
- When you click the Create button in the Resource Quota list, the Create Resource Quota pop-up window appears.
  ![IMG_7_3_3]

<br>

#### <div id='5-7-3-4'/> 5.7.3.4. Modify Resource Quota
- When you click the Edit button in the Resource Quota details, the Edit Resource Quota pop-up window appears.
  ![IMG_7_3_4]

<br>

#### <div id='5-7-3-5'/> 5.7.3.5. Delete a Resource Quota
- Clicking the Delete button in the Resource Quota details will delete the Resource Quota.
  ![IMG_7_3_5]

<br>

### <div id='5-7-4'/> 5.7.4. Limit Ranges
#### <div id='5-7-4-1'/> 5.7.4.1. Get a list of Limit Ranges
- Click Limit Ranges on the Managements menu to go to the Limit Range list page.
  ![IMG_7_4_1]

<br>

#### <div id='5-7-4-2'/> 5.7.4.2. Get Limit Range Details
- Click the name of the limit range in the Limit Range list to go to the Limit Range details page.
  ![IMG_7_4_2]

<br>

#### <div id='5-7-4-3'/> 5.7.4.3. Create a Limit Range
- When you click the Create button in the Limit Range list, the Create Limit Range pop-up window appears.
  ![IMG_7_4_3]

<br>

#### <div id='5-7-4-4'/> 5.7.4.4. Modify the Limit Range
- When you click the Edit button in the Limit Range details, the Edit Limit Range pop-up window appears.
  ![IMG_7_4_4]

<br>

#### <div id='5-7-4-5'/> 5.7.4.5. Delete the Limit Range
- Clicking the Delete button in the Limit Range details will delete the Limit Range.
  ![IMG_7_4_5]

<br>

## <div id='5-8'/> 5.8. Info Menu
### <div id='5-8-1'/> 5.8.1. Access
#### <div id='5-8-1-1'/> 5.8.1.1. Get Access Information
- Gets the User Info information.
- Get environment and configuration information for using the container platform's CLI.
  ![IMG_8_1_1]
  ![IMG_8_1_2]

<br>

### <div id='5-8-2'/> 5.8.2. Private Repository
#### <div id='5-8-2-1'/> 5.8.2.1. Get Private Repository Information
- View private repository information.
  ![IMG_8_2_1]

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Portal Use Guide


----
[image 002]:..//images/portal/cp-002.png
[image 003]:..//images/portal/cp-003.png
[image 004]:..//images/portal/cp-004.png
[image 005]:..//images/portal/cp-005.png
[image 006]:..//images/portal/cp-006.png
[image 011]:..//images/portal/cp-011.png
[image 012]:..//images/portal/cp-012.png
[image 013]:..//images/portal/cp-013.png
[image 014]:..//images/portal/cp-014.png
[image 015]:..//images/portal/cp-015.png
[IMG_1_1_1]:../images/portal/IMG_1_1_1.png
[IMG_1_2_1]:../images/portal/IMG_1_2_1.png
[IMG_1_2_2]:../images/portal/IMG_1_2_2.png
[IMG_1_2_3]:../images/portal/IMG_1_2_3.png
[IMG_1_2_4]:../images/portal/IMG_1_2_4.png
[IMG_1_2_5]:../images/portal/IMG_1_2_5.png
[IMG_1_3_1]:../images/portal/IMG_1_3_1.png
[IMG_1_3_2]:../images/portal/IMG_1_3_2.png
[IMG_1_3_3_1]:../images/portal/IMG_1_3_3_1.png
[IMG_1_3_3_2]:../images/portal/IMG_1_3_3_2.png
[IMG_1_3_4]:../images/portal/IMG_1_3_4.png
[IMG_1_3_5]:../images/portal/IMG_1_3_5.png
[IMG_1_4_1]:../images/portal/IMG_1_4_1.png
[IMG_1_4_2]:../images/portal/IMG_1_4_2.png
[IMG_1_4_3]:../images/portal/IMG_1_4_3.png
[IMG_1_4_4]:../images/portal/IMG_1_4_4.png
[IMG_1_4_5]:../images/portal/IMG_1_4_5.png
[IMG_1_5_1]:../images/portal/IMG_1_5_1.png
[IMG_1_5_2]:../images/portal/IMG_1_5_2.png
[IMG_1_5_3]:../images/portal/IMG_1_5_3.png
[IMG_1_5_4]:../images/portal/IMG_1_5_4.png
[IMG_1_5_5]:../images/portal/IMG_1_5_5.png
[IMG_2_1_1]:../images/portal/IMG_2_1_1.png
[IMG_2_1_2_1]:../images/portal/IMG_2_1_2_1.png
[IMG_2_1_2_2]:../images/portal/IMG_2_1_2_2.png
[IMG_2_2_1]:../images/portal/IMG_2_2_1.png
[IMG_2_2_2]:../images/portal/IMG_2_2_2.png
[IMG_2_3_1]:../images/portal/IMG_2_3_1.png
[IMG_2_3_2]:../images/portal/IMG_2_3_2.png
[IMG_2_3_3_1]:../images/portal/IMG_2_3_3_1.png
[IMG_2_3_3_2]:../images/portal/IMG_2_3_3_2.png
[IMG_2_3_3_3]:../images/portal/IMG_2_3_3_3.png
[IMG_2_3_4]:../images/portal/IMG_2_3_4.png
[IMG_2_3_5]:../images/portal/IMG_2_3_5.png
[IMG_3_1_1]:../images/portal/IMG_3_1_1.png
[IMG_3_1_2]:../images/portal/IMG_3_1_2.png
[IMG_3_1_3]:../images/portal/IMG_3_1_3.png
[IMG_3_1_4]:../images/portal/IMG_3_1_4.png
[IMG_3_1_5]:../images/portal/IMG_3_1_5.png
[IMG_3_2_1]:../images/portal/IMG_3_2_1.png
[IMG_3_2_2]:../images/portal/IMG_3_2_2.png
[IMG_3_2_3]:../images/portal/IMG_3_2_3.png
[IMG_3_2_4]:../images/portal/IMG_3_2_4.png
[IMG_3_2_5]:../images/portal/IMG_3_2_5.png
[IMG_3_3_1]:../images/portal/IMG_3_3_1.png
[IMG_3_3_2]:../images/portal/IMG_3_3_2.png
[IMG_3_3_3]:../images/portal/IMG_3_3_3.png
[IMG_3_3_4]:../images/portal/IMG_3_3_4.png
[IMG_3_3_5]:../images/portal/IMG_3_3_5.png
[IMG_4_1_1]:../images/portal/IMG_4_1_1.png
[IMG_4_1_2]:../images/portal/IMG_4_1_2.png
[IMG_4_1_3]:../images/portal/IMG_4_1_3.png
[IMG_4_1_4]:../images/portal/IMG_4_1_4.png
[IMG_4_1_5]:../images/portal/IMG_4_1_5.png
[IMG_4_2_1]:../images/portal/IMG_4_2_1.png
[IMG_4_2_2]:../images/portal/IMG_4_2_2.png
[IMG_4_2_3]:../images/portal/IMG_4_2_3.png
[IMG_4_2_4]:../images/portal/IMG_4_2_4.png
[IMG_4_2_5]:../images/portal/IMG_4_2_5.png
[IMG_5_1_1]:../images/portal/IMG_5_1_1.png
[IMG_5_1_2]:../images/portal/IMG_5_1_2.png
[IMG_5_1_3]:../images/portal/IMG_5_1_3.png
[IMG_5_1_4]:../images/portal/IMG_5_1_4.png
[IMG_5_1_5]:../images/portal/IMG_5_1_5.png
[IMG_5_2_1]:../images/portal/IMG_5_2_1.png
[IMG_5_2_2]:../images/portal/IMG_5_2_2.png
[IMG_5_2_3]:../images/portal/IMG_5_2_3.png
[IMG_5_2_4]:../images/portal/IMG_5_2_4.png
[IMG_5_2_5]:../images/portal/IMG_5_2_5.png
[IMG_5_3_1]:../images/portal/IMG_5_3_1.png
[IMG_5_3_2]:../images/portal/IMG_5_3_2.png
[IMG_5_3_3]:../images/portal/IMG_5_3_3.png
[IMG_5_3_4]:../images/portal/IMG_5_3_4.png
[IMG_5_3_5]:../images/portal/IMG_5_3_5.png
[IMG_6_1_1]:../images/portal/IMG_6_1_1.png
[IMG_6_1_2]:../images/portal/IMG_6_1_2.png
[IMG_6_1_3]:../images/portal/IMG_6_1_3.png
[IMG_6_1_4]:../images/portal/IMG_6_1_4.png
[IMG_6_1_5]:../images/portal/IMG_6_1_5.png
[IMG_7_1_1]:../images/portal/IMG_7_1_1.png
[IMG_7_1_2]:../images/portal/IMG_7_1_2.png
[IMG_7_1_3_1]:../images/portal/IMG_7_1_3_1.png
[IMG_7_1_3_2]:../images/portal/IMG_7_1_3_2.png
[IMG_7_1_4]:../images/portal/IMG_7_1_4.png
[IMG_7_1_5_1]:../images/portal/IMG_7_1_5_1.png
[IMG_7_1_5_2]:../images/portal/IMG_7_1_5_2.png
[IMG_7_1_5_3]:../images/portal/IMG_7_1_5_3.png
[IMG_7_1_5_4]:../images/portal/IMG_7_1_5_4.png
[IMG_7_1_5_5]:../images/portal/IMG_7_1_5_5.png
[IMG_7_2_1]:../images/portal/IMG_7_2_1.png
[IMG_7_2_2]:../images/portal/IMG_7_2_2.png
[IMG_7_2_3]:../images/portal/IMG_7_2_3.png
[IMG_7_2_4]:../images/portal/IMG_7_2_4.png
[IMG_7_2_5]:../images/portal/IMG_7_2_5.png
[IMG_7_3_1]:../images/portal/IMG_7_3_1.png
[IMG_7_3_2]:../images/portal/IMG_7_3_2.png
[IMG_7_3_3]:../images/portal/IMG_7_3_3.png
[IMG_7_3_4]:../images/portal/IMG_7_3_4.png
[IMG_7_3_5]:../images/portal/IMG_7_3_5.png
[IMG_7_4_1]:../images/portal/IMG_7_4_1.png
[IMG_7_4_2]:../images/portal/IMG_7_4_2.png
[IMG_7_4_3]:../images/portal/IMG_7_4_3.png
[IMG_7_4_4]:../images/portal/IMG_7_4_4.png
[IMG_7_4_5]:../images/portal/IMG_7_4_5.png
[IMG_8_1_1]:../images/portal/IMG_8_1_1.png
[IMG_8_1_2]:../images/portal/IMG_8_1_2.png
[IMG_8_2_1]:../images/portal/IMG_8_2_1.png