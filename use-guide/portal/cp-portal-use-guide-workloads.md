### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) >  Workloads Menu

<br>

## Table of Contents

1. [Workloads Menu](#1)  
  1.1. [Deployment](#1-1)  
   1.1.1. [Get a list of deployments](#1-1-1)    
   1.1.2. [Get Deployment Details](#1-1-2)  
   1.1.3. [Create Deployment](#1-1-3)  
   1.1.4. [Modify the Deployment](#1-1-4)  
   1.1.5. [Delete a deployment](#1-1-5)   
  1.2. [Pods](#1-2)      
   1.2.1. [Get a list of pods](#1-2-1)  
   1.2.2. [View Pod Details](#1-2-2)  
   1.2.3. [Create a Pod](#1-2-3)  
   1.2.4. [Modify the Pod](#1-2-4)  
   1.2.5. [Delete Pod](#1-2-5)  
  1.3. [ReplicaSets](#1-3)  
   1.3.1. [Get a list of ReplicaSets](#1-3-1)  
   1.3.2. [Get ReplicaSet Details](#1-3-2)  
   1.3.3. [Create a ReplicaSet](#1-3-3)  
   1.3.4. [Modify the ReplicaSet](#1-3-4)  
   1.3.5. [Delete the ReplicaSet](#1-3-5)

<br>

## <div id='1'/> 1. Workloads Menu
K-PaaS standard model’s single-cloud deployment (default installation) comes with a default Kyverno-based network isolation policy applied.
According to this policy, Pod-to-Pod traffic between namespaces is blocked by default. However, if inter-service communication across namespaces is required, an additional configuration must be applied to allow it.

#### How to Allow Cross-Namespace Communication
By default, the network policy blocks traffic between namespaces.
However, communication can be allowed if a specific label is set on the Pod.
- `cp-role=shared`
    + This label must be set for the Pod to communicate with other namespaces
    + If the label is not present, the default policy blocks the traffic

#### Setting the Label via Portal UI
When creating a Deployment, ReplicaSet, or Pod in the Workloads menu, you can use the following option to automatically add the required label.

##### Steps
1. In the resource list view, click the [Create] button
2. Check the option “Allow traffic between namespaces”
3. Upon creation, the Pod will automatically have the `cp-role=shared` label attached

![IMG_3_4_1]

##### How to Verify
After the resource is created, you can verify the label from the Labels section in the Pod detail view.

![IMG_3_4_2]

<br>

### <div id='1-1'/> 1.1. Deployments
#### <div id='1-1-1'/> 1.1.1. Get a list of deployments
- Click Deployments in Workloads to go to the Deployments list page.
  ![IMG_3_1_1]

<br>

#### <div id='1-1-2'/> 1.1.2. Get Deployment Details
- Click the name of the deployment in the Deployments list to go to the deployment details page.
  ![IMG_3_1_2]

<br>

#### <div id='1-1-3'/> 1.1.3. Create Deployment
- When you click the Create button in the Deployment list, the Create Deployment popup appears.
  ![IMG_3_1_3]

<br>

#### <div id='1-1-4'/> 1.1.4. Modify the Deployment
- When you click the Edit button in the Deployment details, the Edit Deployment popup appears.
  ![IMG_3_1_4]

<br>

#### <div id='1-1-5'/> 1.1.5. Delete a deployment
- Clicking the Delete button in the Deployment details will delete the Deployment.
  ![IMG_3_1_5]

<br>


### <div id='1-2'/> 1.2. Pods
#### <div id='1-2-1'/> 1.2.1. Get a list of pods
- Click Pods in Workloads to go to the Pods list page.
  ![IMG_3_2_1]

<br>

#### <div id='1-2-2'/> 1.2.2. View Pod Details
- Click a pod name in the Pods list to go to the pod details page.
  ![IMG_3_2_2]

<br>

#### <div id='1-2-3'/> 1.2.3. Create a Pod
- When you click the Create button in the Pods list, the Create Pod popup appears.
  ![IMG_3_2_3]

<br>

#### <div id='1-2-4'/> 1.2.4. Modify the Pod
- When you click the Edit button in the pod details, the Edit Pod popup appears.
  ![IMG_3_2_4]

<br>

#### <div id='1-2-5'/> 1.2.5. Delete Pod
- Clicking the Delete button in the pod details completes the deletion of the pod.
  ![IMG_3_2_5]

<br>

### <div id='1-3'/> 1.3. ReplicaSets
#### <div id='1-3-1'/> 1.3.1. Get a list of ReplicaSets
- Click ReplicaSets in Workloads to go to the ReplicaSet list page.
  ![IMG_3_3_1]

<br>

#### <div id='1-3-2'/> 1.3.2. Get ReplicaSet Details
- In the ReplicaSet list, click the name of the ReplicaSet to go to the ReplicaSet details page.
  ![IMG_3_3_2]

<br>

#### <div id='1-3-3'/> 1.3.3. Create a ReplicaSet
- When you click the Create button in the ReplicaSet list, the Create ReplicaSet pop-up window appears.
  ![IMG_3_3_3]

<br>

#### <div id='1-3-4'/> 1.3.4. Modify the ReplicaSet
- When you click the Edit button in the ReplicaSet details, the Edit ReplicaSet popup window appears.
  ![IMG_3_3_4]

<br>

#### <div id='1-3-5'/> 1.3.5. Delete the ReplicaSet
- Clicking the Delete button in the ReplicaSet details completes the deletion of the ReplicaSet.
  ![IMG_3_3_5]

<br>


### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) >  Workloads Menu

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
[IMG_3_4_1]:../images/portal/IMG_3_4_1.png
[IMG_3_4_2]:../images/portal/IMG_3_4_2.png
