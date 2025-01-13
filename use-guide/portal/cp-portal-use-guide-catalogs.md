### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../README.md) >  [Portal Use Guide](./cp-portal-use-guide.md) >  Catalogs Menu

<br>

## Table of Contents

1. [Catalogs Menu](#1)  
   1.1. [Repositories](#1-1)  
   1.1.1. [Add default chart repository](#1-1-1)  
   1.1.2. [Add chart repository](#1-1-2)  
   1.1.3. [List chart repositories](#1-1-3)  
   1.1.4. [Clear the chart repositories cache](#1-1-4)  
   1.1.5. [Update chart repository](#1-1-5)  
   1.1.6. [Delete chart repository](#1-1-6)  
   1.1.7. [View chart list](#1-1-7)  
   1.1.8. [Install the chart](#1-1-8)  
   1.2. [Releases](#1-2)   
   1.2.1. [List releases](#1-2-1)  
   1.2.2. [View release details](#1-2-2)  
   1.2.3. [Upgrade release](#1-2-3)  
   1.2.4. [Rollback release](#1-2-4)  
   1.2.5. [Delete release](#1-2-5)

<br>

## <div id='1'/> 1. Catalogs Menu
### <div id='1-1'/> 1.1. Repositories
Manage chart repository information in the Container Platform Portal Menu Catalogs > Repositories. <br>

#### <div id='1-1-1'/> 1.1.1. Add default chart repository
> **(Super Admin Only)** Menu Catalogs > Repositories

**[ChartMuseum](https://github.com/helm/chartmuseum)** is an open source repository that manages Helm charts in the Kubernetes cluster environment. It is included in the Container Platform Portal deployment and is used as the default chart repository for the Container Platform environment.

|Name|URL|
|---|---|
|cp-chart-repository|`https://chartmuseum.${HOST_DOMAIN}`|

<br>

##### Click the Add button to add the default chart repository.
![IMG_9_1_1]

<br>

##### Upload charts to default chart repository
Chart uploads are available through the CLI.
```bash
# Access master node
# list chart repositories (auto-add cp-chart-repository with portal deployment)
$ helm repo list
NAME                    URL
cp-chart-repository     https://chartmuseum.105.xxx.xxx.xxx.nip.io

# Chart to upload
$ ls
nginx-18.2.6.tgz

# Upload chart to repository via helm plugin
$ helm cm-push nginx-18.2.6.tgz cp-chart-repository
Pushing nginx-18.2.6.tgz to cp-chart-repository...
Done.

# Update chart repositories 
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "cp-chart-repository" chart repository
Update Complete. ⎈Happy Helming!⎈

# Check uploaded charts
$ helm search repo nginx
NAME                            CHART VERSION   APP VERSION     DESCRIPTION
cp-chart-repository/nginx       18.2.6          1.27.3          NGINX Open Source is a web server that can be a...
```

##### Update repository from portal UI
![IMG_9_1_2]
![IMG_9_1_3]
![IMG_9_1_4]


<br>


#### <div id='1-1-2'/> 1.1.2. Add chart repository
> Click the [Add] button in the menu Catalogs > Repositories

Add a chart repository.

##### Input values ​​for add
|Configuration|Description|Additional|
|---|---|---|
|Name|**(Required)** Chart repository name to add||
|URL |**(Required)** Chart repository url to add||
|Username|Chart repository username|Click the Authentication checkbox|
|Password|Chart repository password|Click the Authentication checkbox|
|CA Certificate|Certificate if using HTTPS in private chart repository|Click the TLS Verification checkbox|

##### (Example) Add Bitnami
![IMG_9_1_5]
![IMG_9_1_6]

##### (Example) Add private chart repository
![IMG_9_1_7]


<br>


#### <div id='1-1-3'/> 1.1.3. List chart repositories
> Menu Catalogs > Repositories

##### View a list of chart repositories.
![IMG_9_1_8]


<br>

#### <div id='1-1-4'/> 1.1.4. Clear the chart repositories cache
> **(Super Admin Only)** Click the [Clear Cache] button in the menu Catalogs > Repositories
#### Delete cached chart files in storage
After deleting the cached chart files in the storage, all chart repositories are updated with the latest information.
Since the index files of all chart repositories are newly downloaded, the more repositories there are registered, the longer it may take.
Therefore, it is recommended not to perform this task frequently.

![IMG_9_1_9]
![IMG_9_1_10]

<br>

#### <div id='1-1-5'/> 1.1.5. Update chart repository
> Menu Catalogs > Repositories > Click on the chart repository name > Click on the [Update] button
##### Update gets the latest information about charts from the chart repository.
![IMG_9_1_11]
![IMG_9_1_12]

<br>

#### <div id='1-1-6'/> 1.1.6. Delete chart repository
> Menu Catalogs > Repositories > Click on the chart repository name > Click on the [Delete] button
##### Remove the chart repository.
![IMG_9_1_13]
![IMG_9_1_14]


<br>

#### <div id='1-1-7'/> 1.1.7. View chart list
> Click on the menu Catalogs > Repositories > Chart Repository Name

##### View charts in a chart repository.
![IMG_9_1_15]

<br>

#### <div id='1-1-8'/> 1.1.8. Install the chart
> Menu Catalogs > Repositories > chart repository name > Click on the chart name

Install the chart to the cluster.

##### Input values ​​for install the chart
|Configuration|Description|
|---|---|
|Chart Versions|Select the chart version to install|
|Release Name|Release name to install|
|Original Chart Values|(Read Only) Original values.yaml for the chart|
|User-Supplied Values|**(Modify value)** Overrides values ​​based on original values.yaml|

##### How to use KeyMap
|KeyMap|Description|
|---|---|
|Ctrl-F|Start searching|
|Ctrl-G|Find next|
|Shift-Ctrl-G|Find previous|
|Shift-Ctrl-F|Replace|
|Shift-Ctrl-R|Replace all|
|Alt-G|Jump to line|

<br>

#### (Example) Install Bitnami Nginx
- Select the namespace where you want to install the chart
- Select the chart version to install
- Enter the Release Name to install

![IMG_9_1_16]

##### Enter User-Supplied Values
- Add commonLabels : `"app": "mynginx"`
- Specify Service Type: `NodePort`
- Specify Service NodePort: `30000 (http)`

###### Expend the User-Supplied Values View
![IMG_9_1_17]

###### Collapse the User-Supplied Values View
![IMG_9_1_18]


##### Install the chart
![IMG_9_1_19]
![IMG_9_1_20]


<br>


### <div id='1-2'/> 1.2. Releases
Manage release in the Container Platform Portal menu Catalogs > Releases.

#### <div id='1-2-1'/> 1.2.1. List releases
> Menu Catalogs > Releases

##### Get the lists of the releases for a specified namespace.
![IMG_9_2_1]

<br>

#### <div id='1-2-2'/> 1.2.2. View release details
> Menu Catalogs > Releases > Select Release name

##### View detailed information about the release.

|Tab|Description|
|---|---|
|Details|Release details|
|Histories|Revision history information for release|

![IMG_9_2_2]

|Tab|Description|
|---|---|
|Resources Status|Display the resources of the given release|

![IMG_9_2_3]

|Tab|Description|
|---|---|
|Manifests|Display the manifests of the given release|

![IMG_9_2_4]

|Tab|Description|
|---|---|
|Values|Display the values of the given release|

![IMG_9_2_5]


|Tab|Description|
|---|---|
|Notes|Display the notes provided by the chart of a given release|

![IMG_9_2_6]


<br>

#### <div id='1-2-3'/> 1.2.3. Upgrade release
> Menu Catalogs > Releases > Select Release name > Click [Upgrade] button

##### Upgrade the release
- Upgrade chart version
- Update Values

|Configuration|Description|
|---|---|
|Chart Versions|Select the chart version to upgrade|
|Deployed Values|(Read Only) Values ​​of the currently deployed release|
|User-Supplied Values|**(Modify value)** Redefine values ​​based on the deployed values|
|Deployed Manifest|(Read Only) Manifest of the currently deployed release|
|Rendered Manifest|(Read Only) Preview of the rendered manifest with new chart version or redefined values(User-Supplied Values)|

##### How to use KeyMap
|KeyMap|Description|
|---|---|
|Ctrl-F|Start searching|
|Ctrl-G|Find next|
|Shift-Ctrl-G|Find previous|
|Shift-Ctrl-F|Replace|
|Shift-Ctrl-R|Replace all|
|Alt-G|Jump to line|

#### (Example) Upgrade Bitnami Nginx Release
- Select the chart version to upgrade
- Change Service Type: `ClusterIP`
- Enable Ingress
- Specify IngressClassName
- Specify Ingress hostname

![IMG_9_2_7]

##### Update User-Supplied Values
![IMG_9_2_8]

##### Click the [Compare rendered chart templates] button
Renders the Manifest for preview based on the chart version to upgrade or the changed User-Supplied Values.

![IMG_9_2_9]

##### Provides a preview of the rendered Manifest. (click collapse button)
![IMG_9_2_10]

##### Run a release upgrade
![IMG_9_2_11]
![IMG_9_2_12]

##### Check release status after release upgrade
![IMG_9_2_13]

<br>

#### <div id='1-2-4'/> 1.2.4. Rollback release
> Menu Catalogs > Releases > Select Release name > Click [Rollback] button <br>
- *If the maximum number of revision number in Histories is 1, the [Rollback] button is disabled.*

##### Select the revision number to roll back
![IMG_9_2_14]

##### Compare release manifests by revision number (click collapse button)
![IMG_9_2_15]

##### Run a release rollback
![IMG_9_2_16]
![IMG_9_2_17]

##### Check release status after release rollback
![IMG_9_2_18]


 <br>

#### <div id='1-2-5'/> 1.2.5. Delete release
> Menu Catalogs > Releases > Select Release name > Click [Uninstall] button

##### Uninstall the release
![IMG_9_2_19]
![IMG_9_2_20]


<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../README.md) >  [Portal Use Guide](./cp-portal-use-guide.md) >  Catalogs Menu

[IMG_9_1_1]:../images/portal/IMG_9_1_1.png
[IMG_9_1_2]:../images/portal/IMG_9_1_2.png
[IMG_9_1_3]:../images/portal/IMG_9_1_3.png
[IMG_9_1_4]:../images/portal/IMG_9_1_4.png
[IMG_9_1_5]:../images/portal/IMG_9_1_5.png
[IMG_9_1_6]:../images/portal/IMG_9_1_6.png
[IMG_9_1_7]:../images/portal/IMG_9_1_7.png
[IMG_9_1_8]:../images/portal/IMG_9_1_8.png
[IMG_9_1_9]:../images/portal/IMG_9_1_9.png
[IMG_9_1_10]:../images/portal/IMG_9_1_10.png
[IMG_9_1_11]:../images/portal/IMG_9_1_11.png
[IMG_9_1_12]:../images/portal/IMG_9_1_12.png
[IMG_9_1_13]:../images/portal/IMG_9_1_13.png
[IMG_9_1_14]:../images/portal/IMG_9_1_14.png
[IMG_9_1_15]:../images/portal/IMG_9_1_15.png
[IMG_9_1_16]:../images/portal/IMG_9_1_16.png
[IMG_9_1_17]:../images/portal/IMG_9_1_17.png
[IMG_9_1_18]:../images/portal/IMG_9_1_18.png
[IMG_9_1_19]:../images/portal/IMG_9_1_19.png
[IMG_9_1_20]:../images/portal/IMG_9_1_20.png
[IMG_9_2_1]:../images/portal/IMG_9_2_1.png
[IMG_9_2_2]:../images/portal/IMG_9_2_2.png
[IMG_9_2_3]:../images/portal/IMG_9_2_3.png
[IMG_9_2_4]:../images/portal/IMG_9_2_4.png
[IMG_9_2_5]:../images/portal/IMG_9_2_5.png
[IMG_9_2_6]:../images/portal/IMG_9_2_6.png
[IMG_9_2_7]:../images/portal/IMG_9_2_7.png
[IMG_9_2_8]:../images/portal/IMG_9_2_8.png
[IMG_9_2_9]:../images/portal/IMG_9_2_9.png
[IMG_9_2_10]:../images/portal/IMG_9_2_10.png
[IMG_9_2_11]:../images/portal/IMG_9_2_11.png
[IMG_9_2_12]:../images/portal/IMG_9_2_12.png
[IMG_9_2_13]:../images/portal/IMG_9_2_13.png
[IMG_9_2_14]:../images/portal/IMG_9_2_14.png
[IMG_9_2_15]:../images/portal/IMG_9_2_15.png
[IMG_9_2_16]:../images/portal/IMG_9_2_16.png
[IMG_9_2_17]:../images/portal/IMG_9_2_17.png
[IMG_9_2_18]:../images/portal/IMG_9_2_18.png
[IMG_9_2_19]:../images/portal/IMG_9_2_19.png
[IMG_9_2_20]:../images/portal/IMG_9_2_20.png