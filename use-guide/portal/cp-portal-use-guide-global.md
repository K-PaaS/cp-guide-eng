### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Global Menu
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
   1.6. [Federation](#1-6)  
   1.6.1. [Get Overview Information](#1-6-1)   
   1.6.2. [Get a list of Clusters](#1-6-2)   
   1.6.3. [Join Clusters](#1-6-3)   
   1.6.4. [View Cluster Details](#1-6-4)   
   1.6.5. [Unjoin Clusters](#1-6-5)   
   1.6.6. [Sync Clusters](#1-6-6)   
   1.6.7. [View Policy List](#1-6-7)   
   1.6.8. [Create Policy](#1-6-8)   
   1.6.9. [View Policy Details](#1-6-9)   
   1.6.10. [Edit Policy](#1-6-10)   
   1.6.11. [Delete Policy](#1-6-11)   
   1.6.12. [View Namespace List](#1-6-12)   
   1.6.13. [Create Namespace](#1-6-13)   
   1.6.14. [View Namespace Details](#1-6-14)   
   1.6.15. [Delete Namespace](#1-6-15)     
   1.6.16. [View Workload List](#1-6-16)   
   1.6.17. [Create Workload](#1-6-17)   
   1.6.18. [View Workload Details](#1-6-18)   
   1.6.19. [Edit Workload](#1-6-19)   
   1.6.20. [Delete Workload](#1-6-20)   
   1.6.21. [View ConfigMaps & Secrets List](#1-6-21)   
   1.6.22. [Create ConfigMaps & Secrets](#1-6-22)   
   1.6.23. [View ConfigMaps & Secrets Details](#1-6-23)   
   1.6.24. [Edit ConfigMaps & Secrets](#1-6-24)   
   1.6.25. [Delete ConfigMaps & Secrets](#1-6-25)  
  1.7. [Migration](#1-7)  
  1.7.1. [View Storage Account List](#1-7-1)  
  1.7.2. [View Storage Account Details](#1-7-2)  
  1.7.3. [Create Storage Account](#1-7-3)  
  1.7.4. [Delete Storage Account](#1-7-4)  
  1.7.5. [View Migration Bucket List](#1-7-5)  
  1.7.6. [Migration Copy](#1-7-6)  
  1.7.7. [Migration Sync](#1-7-7)

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

### <div id='1-6'/> 1.6. Federation

Federation is a service that enables consistent operation and deployment of multiple clusters from a single control plane. Through Federation, you can manage multi-cluster integration, deploy multi-cluster applications, automate policy-based operations, and monitor the status, resource usage, and workload status of multiple clusters in an integrated manner.

#### <div id='1-6-1'/> 1.6.1. Get Overview Information

- On the Overview tab of the Federation menu, you can view federation resource information, as well as the CPU, Memory Requests and usage, number of nodes, and status of the host and member clusters.
- If a member cluster does not have metrics, the CPU and Memory usage for that cluster may not be displayed.
  ![IMG_1_6_1]

<br>

#### <div id='1-6-2'/> 1.6.2. Get a list of Clusters

- On the Clusters tab, you can view a list of member clusters joined to the Federation.
- If a member cluster does not have metrics, the CPU and Memory usage for that cluster may not be displayed.
  ![IMG_1_6_2]

<br>

#### <div id='1-6-3'/> 1.6.3. Join Clusters

- Click the + Join button on the Clusters tab.
- A list of clusters available to join as member clusters in the Federation is displayed, and multiple clusters can be selected.
- The list of available clusters consists of those created in Global > Clusters that are not yet registered as Federation member clusters.
  ![IMG_1_6_3]

<br>

#### <div id='1-6-4'/> 1.6.4. View Cluster Details

- Click the View button of the cluster you wish to view in detail.
- You can view member cluster information in the form of a YAML editor.
  ![IMG_1_6_4]

<br>

#### <div id='1-6-5'/> 1.6.5. Unjoin Clusters

- Click the Exclude button for the cluster you wish to remove from the member clusters.
- The Exclude button does not delete the cluster itself; it simply removes it from being a member cluster affected by the Federation.
- To completely disconnect a cluster from K-PaaS, you must additionally proceed with the removal in the Global > Clusters menu.
  ![IMG_1_6_5]

<br>

#### <div id='1-6-6'/> 1.6.6. Sync Clusters

- Resources in member clusters joined to the Federation can be synchronized to the Federation.
- Click the Sync button of the member cluster you want to sync with the Federation.
- Select the resources in the member cluster you want to sync and click the Apply button.
- Resources already present in the Federation do not need to be synced, so their checkboxes will be disabled.
- Resources in namespaces starting with 'kube-' or 'karamada-' cannot be synced.
  ![IMG_1_6_6]

<br>

#### <div id='1-6-7'/> 1.6.7. View Policy List

- You can view the list of policies on the Policies tab.
- Policies are divided into Namespace level and Cluster level.
- At the Namespace level, you can view lists organized by namespace.
  ![IMG_1_6_7]

<br>

#### <div id='1-6-8'/> 1.6.8. Create Policy

- You can create a policy by clicking the +Add button on the Policies tab.
- There are three steps in total, the first of which is Metadata.
- Level, Namespace, and Name are required fields.
- 'Preserve Resource On Deletion' controls whether resources in member clusters should be preserved when the resource template is deleted.
  The default is false. If set to true, when a resource selected in the next step (Resource Selector) is deleted from the Federation, the propagated resources in the member clusters will be preserved instead of deleted. If a deletion is subsequently attempted on the member cluster, the resource will not be recreated after deletion.
  ![IMG_1_6_8_1]

<br>

- Click the Next button to proceed to the Resource Selectors step.
- Click the + button next to Resource Selectors to add the resources to be propagated. You can add up to 20 resources.
  ![IMG_1_6_8_2]

<br>

- Please note that if you only select the 'Kind' when adding resources to propagate, all resources of that Kind existing in the Federation will be propagated.
- You can select up to 20 LabelSelectors.
  ![IMG_1_6_8_3]

<br>

- Click the Next button to proceed to the Placement step.
- In ClusterNames, you must select at least one candidate member cluster to which the resource will be propagated.
  ![IMG_1_6_8_4]

<br>

- The option areas are as follows:

- `Replica Scheduling`: This is the scheduling method for resource replicas, and the default is 'Duplicated'.
  - `Duplicated`: When propagating resources, the number of replicas added in the previous step is applied identically to each candidate member cluster. For example, if you add Deployment A with 5 replicas and designate member1 and member2 as candidates, Deployment A with 5 replicas will be propagated to both member1 and member2.
  - `Divided`: When propagating resources, the number of replicas added in the Resource Selectors step is divided among the candidate member clusters based on the selected 'Division Preference' value.
- `Division Preference`: Options to choose how replicas are divided and propagated.
  - `Aggregated`: Divides and propagates replicas into as few clusters as possible.
  - `Weighted`: Determines the weight of replicas according to Weight Preference.
    ![IMG_1_6_8_5]

<br>

#### <div id='1-6-9'/> 1.6.9. View Policy Details

- Clicking the View button of a policy allows you to see the policy's information in YAML format.
  ![IMG_1_6_9]

<br>

#### <div id='1-6-10'/> 1.6.10. Edit Policy

- You can edit a policy by clicking its View button, modifying the YAML information, and then clicking the Edit button at the bottom.
  ![IMG_1_6_10]

<br>

#### <div id='1-6-11'/> 1.6.11. Delete Policy

- You can delete a policy by clicking its Delete button.
  ![IMG_1_6_11]

<br>

#### <div id='1-6-12'/> 1.6.12. View Namespace List

- Clicking the Namespaces tab in the Federation menu allows you to view the list of namespaces.
  ![IMG_1_6_12]

<br>

#### <div id='1-6-13'/> 1.6.13. Create Namespace

- You can create a namespace by clicking the +Add button.
- Name is a required field.
  ![IMG_1_6_13]

<br>

#### <div id='1-6-14'/> 1.6.14. View Namespace Details

- Clicking the View button of a namespace allows you to see the namespace's information in YAML format.
  ![IMG_1_6_14]

<br>

#### <div id='1-6-15'/> 1.6.15. Delete Namespace

- You can delete a namespace by clicking its Delete button.
  ![IMG_1_6_15]

<br>

#### <div id='1-6-16'/> 1.6.16. View Workload List

- Clicking the Workloads tab in the Federation menu allows you to view the list of workloads.
  ![IMG_1_6_16]

<br>

#### <div id='1-6-17'/> 1.6.17. Create Workload

- You can create a workload by clicking the +Add button.
  ![IMG_1_6_17]

<br>

#### <div id='1-6-18'/> 1.6.18. View Workload Details

- Clicking the View button of a workload allows you to see the workload's information in YAML format.
  ![IMG_1_6_18]

<br>

#### <div id='1-6-19'/> 1.6.19. Edit Workload

- You can edit a workload by clicking its View button, modifying the YAML information, and then clicking the Edit button at the bottom.
  ![IMG_1_6_19]

<br>

#### <div id='1-6-20'/> 1.6.20. Delete Workload

- You can delete a workload by clicking its Delete button.
  ![IMG_1_6_20]

<br>

#### <div id='1-6-21'/> 1.6.21. View ConfigMaps & Secrets List

- Clicking the ConfigMaps & Secrets tab in the Federation menu allows you to view the lists of ConfigMaps and Secrets respectively.
  ![IMG_1_6_21]

<br>

#### <div id='1-6-22'/> 1.6.22. Create ConfigMaps & Secrets

- You can create a ConfigMap or Secret by clicking the +Add button.
  ![IMG_1_6_22]

<br>

#### <div id='1-6-23'/> 1.6.23. View ConfigMaps & Secrets Details

- Clicking the View button of a ConfigMap or Secret allows you to see its information in YAML format.
  ![IMG_1_6_23]

<br>

#### <div id='1-6-24'/> 1.6.24. Edit ConfigMaps & Secrets

- You can edit a ConfigMap or Secret by clicking its View button, modifying the YAML information, and then clicking the Edit button at the bottom.
  ![IMG_1_6_24]

<br>

#### <div id='1-6-25'/> 1.6.25. Delete ConfigMaps & Secrets

- You can delete a ConfigMap or Secret by clicking its Delete button.
  ![IMG_1_6_25]

<br>


### <div id='1-7'/> 1.7. Migration
You can perform migration between S3-based storages through the K-PaaS container platform.

#### <div id='1-7-1'/> 1.7.1. View Storage Account List
- View the list of storage accounts.
  ![IMG_1_7_1]

<br>

#### <div id='1-7-2'/> 1.7.2. View Storage Account Details
- View detailed information about a storage account.
  ![IMG_1_7_2]

<br>

#### <div id='1-7-3'/> 1.7.3. Create Storage Account
- Create a storage account.
1. **Name**: Enter the name of the storage to be connected.
2. **Storage Type**: Select the Storage Type of the storage to be connected (S3).
3. **Endpoint**: Enter the endpoint information of the storage.
4. **Access Key Id**: Enter the Access Key Id of the storage.
5. **Secret Access Key**: Enter the Secret Access Key of the storage.
  ![IMG_1_7_3]

<br>

#### <div id='1-7-4'/> 1.7.4. Delete Storage Account
- Delete a storage account.
- Clicking the Delete button at the bottom of the details screen will delete the corresponding storage account.
  ![IMG_1_7_4]

<br>

#### <div id='1-7-5'/> 1.7.5. View Migration Bucket List
- Select the Source and Destination storage buckets.
- Select the storage (Source) to perform the migration from and the bucket belonging to that storage.
- Select the storage (Destination) to receive the migration and the bucket belonging to that storage.
  ![IMG_1_7_5]
  ![IMG_1_7_6]
  ![IMG_1_7_7]
  ![IMG_1_7_8]

<br>

#### <div id='1-7-6'/> 1.7.6. Migration Copy
- Copies all contents of the Source storage bucket to the Destination storage bucket.
- Clicking the Copy button at the bottom of the details screen will replicate all contents of the Source bucket to the Destination bucket.
  ![IMG_1_7_9]

<br>

#### <div id='1-7-7'/> 1.7.7. Migration Sync
- Synchronizes all contents of the Source storage bucket with the Destination storage bucket.
- Clicking the Sync button at the bottom of the details screen will make the Destination bucket identical to the Source bucket.
- **Caution**: Files that do not exist in the Source will be removed from the Destination, so please be careful to avoid losing important data.
  ![IMG_1_7_10]

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Global Menu

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
[IMG_1_6_1]: ../images/portal/IMG_1_6_1.png
[IMG_1_6_2]: ../images/portal/IMG_1_6_2.png
[IMG_1_6_3]: ../images/portal/IMG_1_6_3.png
[IMG_1_6_4]: ../images/portal/IMG_1_6_4.png
[IMG_1_6_5]: ../images/portal/IMG_1_6_5.png
[IMG_1_6_6]: ../images/portal/IMG_1_6_6.png
[IMG_1_6_7]: ../images/portal/IMG_1_6_7.png
[IMG_1_6_8_1]: ../images/portal/IMG_1_6_8_1.png
[IMG_1_6_8_2]: ../images/portal/IMG_1_6_8_2.png
[IMG_1_6_8_3]: ../images/portal/IMG_1_6_8_3.png
[IMG_1_6_8_4]: ../images/portal/IMG_1_6_8_4.png
[IMG_1_6_8_5]: ../images/portal/IMG_1_6_8_5.png
[IMG_1_6_9]: ../images/portal/IMG_1_6_9.png
[IMG_1_6_10]: ../images/portal/IMG_1_6_10.png
[IMG_1_6_11]: ../images/portal/IMG_1_6_11.png
[IMG_1_6_12]: ../images/portal/IMG_1_6_12.png
[IMG_1_6_13]: ../images/portal/IMG_1_6_13.png
[IMG_1_6_14]: ../images/portal/IMG_1_6_14.png
[IMG_1_6_15]: ../images/portal/IMG_1_6_15.png
[IMG_1_6_16]: ../images/portal/IMG_1_6_16.png
[IMG_1_6_17]: ../images/portal/IMG_1_6_17.png
[IMG_1_6_18]: ../images/portal/IMG_1_6_18.png
[IMG_1_6_19]: ../images/portal/IMG_1_6_19.png
[IMG_1_6_20]: ../images/portal/IMG_1_6_20.png
[IMG_1_6_21]: ../images/portal/IMG_1_6_21.png
[IMG_1_6_22]: ../images/portal/IMG_1_6_22.png
[IMG_1_6_23]: ../images/portal/IMG_1_6_23.png
[IMG_1_6_24]: ../images/portal/IMG_1_6_24.png
[IMG_1_6_25]: ../images/portal/IMG_1_6_25.png
[IMG_1_7_1]:../images/portal/IMG_1_7_1.png
[IMG_1_7_2]:../images/portal/IMG_1_7_2.png
[IMG_1_7_3]:../images/portal/IMG_1_7_3.png
[IMG_1_7_4]:../images/portal/IMG_1_7_4.png
[IMG_1_7_5]:../images/portal/IMG_1_7_5.png
[IMG_1_7_6]:../images/portal/IMG_1_7_6.png
[IMG_1_7_7]:../images/portal/IMG_1_7_7.png
[IMG_1_7_8]:../images/portal/IMG_1_7_8.png
[IMG_1_7_9]:../images/portal/IMG_1_7_9.png
[IMG_1_7_10]:../images/portal/IMG_1_7_10.png

