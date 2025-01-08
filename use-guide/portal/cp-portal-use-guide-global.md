### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) > Global Menu
<br>

## Table of Contents

1. [Global Menu](#1)  
  1.1. [Overview](#1-1)  
   1.1.1. [Get Overview Information](#1-1-1)  
  1.2. [Clusters](#1-2)  
   1.2.1. [Get a list of Clusters](#1-2-1)  
   1.2.2. [Viewing Clusters in Detail](#1-2-2)  
   1.2.3. [Create Clusters](#1-2-3)  
   1.2.4. [Registering Clusters](#1-2-4)  
   1.2.5. [Modify Clusters](#1-2-5)  
  1.3. [Cloud Accounts](#1-3)  
   1.3.1. [Get a list of Cloud Accounts](#1-3-1)  
   1.3.2. [View Cloud Accounts in detail](#1-3-2)  
   1.3.3. [Create a cloud account](#1-3-3)  
   1.3.4. [Modify your cloud account](#1-3-4)  
   1.3.5. [Delete a cloud account](#1-3-5)  
  1.4. [Instance Code Template](#1-4)  
   1.4.1. [Get a list of instance code templates](#1-4-1)  
   1.4.2. [Get Instance Code Template Details](#1-4-2)  
   1.4.3. [Create an instance code template](#1-4-3)  
   1.4.4. [Modify the instance code template](#1-4-4)  
   1.4.5. [Delete the instance code template](#1-4-5)  
  1.5. [SSH Keys](#1-5)  
   1.5.1. [Get a list of SSH Keys](#1-5-1)  
   1.5.2. [Get SSH Keys Details](#1-5-2)  
   1.5.3. [Generate SSH Keys](#1-5-3)  
   1.5.4. [Modify SSH Keys](#1-5-4)  
   1.5.5. [Delete SSH Keys](#1-5-5)

<br>

## <div id='1'/> 1. Global Menu
### <div id='1-1'/> 1.1. Overview
#### <div id='1-1-1'/> 1.1.1. Get Overview Information
- View cluster information and TOP Node (CPU, Memory).
  ![IMG_1_1_1]


<br>

### <div id='1-2'/> 1.2. Clusters
#### <div id='1-2-1'/> 1.2.1. Get a list of Clusters
- Get a list of clusters.
  ![IMG_1_2_1]

<br>

#### <div id='1-2-2'/> 1.2.2. Viewing Clusters in Detail
- View cluster details.
  ![IMG_1_2_2]

<br>

#### <div id='1-2-3'/> 1.2.3. Create Clusters
- To create a cluster, enter the details below and click the Save button to configure the cluster environment.
1. **Cluster Name** : Enter the name of the cluster to create.
2. **Provider** : Select the Provider of the cluster to be created ([1] AWS [2] OPENSTACK [3] NAVER [4] NHN)
3. **Cloud Accounts** : Select the account information for the selected provider.
4. **Template** : Select HCL Template for VM deployment.
5. **Description** : Enter additional information (optional)
6. **Template Detail** : Fill in your own environment information based on the selected HCL Template.
   ![IMG_1_2_3]

<br>

#### <div id='1-2-4'/> 1.2.4. Registering Clusters
- Register the cluster.
1. **Cluster Name** : Enter the name of the cluster to register.
2. **Provider** : Select the Provider of the cluster to register ([1] AWS [2] OPENSTACK [3] NAVER [4] NHN [5] KT)
3. **Cluster API URL** : Enter the target cluster Kubernetes API URL (e.g: https[]()://xxx.xxx.xxx.xxx.xxx:6443)
4. **Description** : Enter additional information (optional)
5. **Cluster Service Token** : Enter the authentication token information of 'cluster-admin' permission to access the target cluster.
   ![IMG_1_2_4]

<br>

#### <div id='1-2-5'/> 1.2.5. Modify Clusters
- Modify cluster information.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After making changes, click the Edit button at the bottom to change the cluster information.
  ![IMG_1_2_5]

<br>

### <div id='1-3'/> 1.3. Cloud Accounts
#### <div id='1-3-1'/> 1.3.1. Get a list of Cloud Accounts
- View the list of Cloud Accounts.
  ![IMG_1_3_1]

<br>

#### <div id='1-3-2'/> 1.3.2. View Cloud Accounts in detail
- View detailed Cloud Accounts information.
  ![IMG_1_3_2]

<br>

#### <div id='1-3-3'/> 1.3.3. Create a cloud account
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

#### <div id='1-3-4'/> 1.3.4. Modify your cloud account
- Modify Cloud Accounts.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the contents, click the Edit button at the bottom to change the Cloud Accounts information.
  ![IMG_1_3_4]

<br>

#### <div id='1-3-5'/> 1.3.5. Delete a cloud account
- Delete Cloud Accounts.
- Click the Delete button at the bottom of the detail screen to delete the cloud account.
  ![IMG_1_3_5]

<br>

### <div id='1-4'/> 1.4. Instance Code Template
Instance Code Template is a template of code that performs automated VM deployment through IaC, and you can register the template in advance to easily deploy subclusters through the K-PaaS container platform, and it provides templates for AWS, OPENSTACK, NAVER, and NHN by default.
#### <div id='1-4-1'/> 1.4.1. Get a list of instance code templates
- Gets a list of Instance Code Templates.
  ![IMG_1_4_1]

<br>

#### <div id='1-4-2'/> 1.4.2. Get Instance Code Template Details
- Get detailed Instance Code Template information.
  ![IMG_1_4_2]

<br>

#### <div id='1-4-3'/> 1.4.3. Create an instance code template
- Create an Instance Code Template.
  ![IMG_1_4_3]

<br>

#### <div id='1-4-4'/> 1.4.4. Modify the instance code template
- Modify the Instance Code Template.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the content, click the Edit button at the bottom to change the template information.
  ![IMG_1_4_4]

<br>

#### <div id='1-4-5'/> 1.4.5. Delete the instance code template
- Delete the Instance Code Template.
- Click the Delete button at the bottom of the detail screen to delete the Instance Code Template.
  ![IMG_1_4_5]

<br>

### <div id='1-5'/> 1.5. SSH Keys
You can register an SSH key to access subcluster instances through the K-PaaS container platform with the SSH key for multicluster creation.
#### <div id='1-1'/> 1.5.1 Get a list of SSH Keys
- Retrieve a list of SSH Keys.
  ![IMG_1_5_1]

<br>

#### <div id='1-5-2'/> 1.5.2. Get SSH Keys Details
- View detailed SSH Keys information.
  ![IMG_1_5_2]

<br>

#### <div id='1-5-3'/> 1.5.3. Generate SSH Keys
- Generate SSH Keys.
  ![IMG_1_5_3]

<br>

#### <div id='1-5-4'/> 1.5.4. Modify SSH Keys
- Modify the SSH Keys.
- If the item is editable, you can edit the value by clicking the Edit button next to the item.
- After changing the contents, click the Edit button at the bottom to change the SSH Key information.
  ![IMG_1_5_4]

<br>

#### <div id='1-5-5'/> 1.5.5. Delete SSH Keys
- Delete SSH Keys.
- Click the Delete button at the bottom of the detail screen to delete the SSH Keys.
  ![IMG_1_5_5]

<br>  

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) > Global Menu

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
