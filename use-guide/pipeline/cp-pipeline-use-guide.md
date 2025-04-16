### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Pipeline Service Use Guide

<br>

# [K-PaaS Container Platform Pipeline User Guide]

## Table of content
1. [Document Outline](#1)
	* [1.1. Purpose](#1-1)
	* [1.2. Range](#1-2)
2. [Container Platform Pipeline Access](#2)
	* [2.1. Container Platform Pipeline Access Description](#2-1)
3. [Container Platform Pipeline User Manual](#3)
	* [3.1. Container Platform Pipeline User Menu Configuration](#3-1)
	* [3.2. Container Platform Pipeline User Menu Description](#3-2)
	* [3.2.1. User Management](#3-2-1)
	* [3.2.1.1. User Dashboard](#3-2-1-1)
	* [3.2.1.1.1. User List Search Check](#3-2-1-1-1)
	* [3.2.1.1.2. View/ Modify User Details](#3-2-1-1-2)
	* [3.2.2. Pipeline](#3-2-2)
	* [3.2.2.1. My Pipeline](#3-2-2-1)
	* [3.2.2.2. Create New Pipeline ](#3-2-2-2)
	* [3.2.2.2.1. Create New Pipeline(1)](#3-2-2-2-1)
	* [3.2.2.2.2. Create New Pipeline(2)](#3-2-2-2-2)
	* [3.2.2.2.3. Create New Pipeline(3)](#3-2-2-2-3)
	* [3.2.2.3. Pipeline Dashboard](#3-2-2-3)
	* [3.2.2.3.1. Check Pipeline List Search](#3-2-2-3-1)
	* [3.2.2.3.2. Check Pipeline Detailed Information](#3-2-2-3-2)
	* [3.2.2.3.3. Modify Pipeline](#3-2-2-3-3)
	* [3.2.2.3.4. Delete Pipeline](#3-2-2-3-4)
	* [3.2.2.4.   Pipeline Details](#3-2-2-4)
	* [3.2.2.4.1. Manage Participants](#3-2-2-4-1)
	* [3.2.2.4.1.1. Add Participant](#3-2-2-4-1-1)
	* [3.2.2.4.1.2. Check Participant List Search](#3-2-2-4-1-2)
	* [3.2.2.4.1.3. Check/ Modify Participant Detailed Information](#3-2-2-4-1-3)
	* [3.2.2.4.1.4. Delete Participant](#3-2-2-4-1-4)
	* [3.2.2.4.2. Build Job](#3-2-2-4-2)
	* [3.2.2.4.2.1. Create Build Job](#3-2-2-4-2-1)
	* [3.2.2.4.2.2. Check/ Modify Build Job Configuration](#3-2-2-4-2-2)
	* [3.2.2.4.2.3. Execute Build Job](#3-2-2-4-2-3)
	* [3.2.2.4.2.4. Stop Build Job](#3-2-2-4-2-4)
	* [3.2.2.4.2.5. Build Job Log/ History](#3-2-2-4-2-5)
	* [3.2.2.4.2.6. Add Build Job](#3-2-2-4-2-6)
	* [3.2.2.4.2.7. Duplicate Build Job](#3-2-2-4-2-7)
	* [3.2.2.4.2.8. Delete Build Job](#3-2-2-4-2-8)
	* [3.2.2.4.3. Static Analysis Job](#3-2-2-4-3)
	* [3.2.2.4.3.1. Create Static Analysis Job](#3-2-2-4-3-1)
	* [3.2.2.4.3.2. Check/ Modify Static Analysis Job Configuration](#3-2-2-4-3-2)
	* [3.2.2.4.3.3. Execute Static Analysis Job](#3-2-2-4-3-3)
	* [3.2.2.4.3.4. Stop Static Analysis Job](#3-2-2-4-3-4)
	* [3.2.2.4.3.5. Static Analysis Job Log/ History](#3-2-2-4-3-5)
	* [3.2.2.4.3.6. Add Static Analysis Job](#3-2-2-4-3-6)
	* [3.2.2.4.3.7. Static Analysis Job Job Quality Issue Results](#3-2-2-4-3-7)
	* [3.2.2.4.3.8. Duplicate Static Analysis Job](#3-2-2-4-3-8)
	* [3.2.2.4.3.9. Delete Static Analysis Job](#3-2-2-4-3-9)
	* [3.2.2.4.4. Test Job](#3-2-2-4-4)
	* [3.2.2.4.4.1. Create Test Job](#3-2-2-4-4-1)
	* [3.2.2.4.4.2. Check/ Modify Test Job Configuration](#3-2-2-4-4-2)
	* [3.2.2.4.4.3. Execute Test Job](#3-2-2-4-4-3)
	* [3.2.2.4.4.4. Stop Test Job](#3-2-2-4-4-4)
	* [3.2.2.4.4.5. Test Job Log/ History](#3-2-2-4-4-5)
	* [3.2.2.4.4.6. Add Test Job](#3-2-2-4-4-6)
	* [3.2.2.4.4.7. Test Job Quality Issue Results](#3-2-2-4-4-7)
	* [3.2.2.4.4.8. Duplicate Test Job](#3-2-2-4-4-8)
	* [3.2.2.4.4.9. Delete Test Job](#3-2-2-4-4-9)
	* [3.2.2.4.5. Deployment Job](#3-2-2-4-5)
	* [3.2.2.4.5.1. Create Deployment Job](#3-2-2-4-5-1)
	* [3.2.2.4.5.2. Check/ Modify Deployment Job Configuration](#3-2-2-4-5-2)
	* [3.2.2.4.5.3. Execute Deployment Job](#3-2-2-4-5-3)
	* [3.2.2.4.5.4. Stop Deployment Job](#3-2-2-4-5-4)
	* [3.2.2.4.5.5. Deployment Job Log/ History](#3-2-2-4-5-5)
	* [3.2.2.4.5.6. Rollback Deployment Job to Current Job](#3-2-2-4-5-6)
	* [3.2.2.4.5.7. Add Deployment Job](#3-2-2-4-5-7)
	* [3.2.2.4.5.8. Duplicate Deployment Job](#3-2-2-4-5-8)
	* [3.2.2.4.5.9. Delete Deployment Job](#3-2-2-4-5-9)
	* [3.2.2.4.6. Arrange Job's Job](#3-2-2-4-6)
	* [3.2.2.4.7. Add New Job Group](#3-2-2-4-7)
	* [3.2.3. Pipeline Management](#3-2-3)
	* [3.2.3.1. Manage Kubernetes Information](#3-2-3-1)
	* [3.2.3.1.1. Register Kubernetes Information](#3-2-3-1-1)
	* [3.2.3.1.2. Modify Kubernetes Information](#3-2-3-1-2)
	* [3.2.3.2. Track JOB Audit](#3-2-3-2)
	* [3.2.3.2.1. Check Tracked JOB Audit](#3-2-3-2-1)
	* [3.2.3.2.2. Check Searched JOB Audit](#3-2-3-2-2)
	* [3.2.4. Quality Management](#3-2-4)
	* [3.2.4.1. Quality Issue](#3-2-4-1)
	* [3.2.4.2. Coding Rules](#3-2-4-2)
	* [3.2.4.3. Quality Profile](#3-2-4-3)
	* [3.2.4.3.1. Create Quality Profile](#3-2-4-3-1)
	* [3.2.4.3.2. Duplicate Quality Profile](#3-2-4-3-2)
	* [3.2.4.3.3. Modify Quality Profile](#3-2-4-3-3)
	* [3.2.4.3.4. Connect Quality Profile Project](#3-2-4-3-4)
	* [3.2.4.3.5. Delete Quality Profile](#3-2-4-3-5)
	* [3.2.4.4. Quality Gate](#3-2-4-4)
	* [3.2.4.4.1. Create Quality Gate](#3-2-4-4-1)
	* [3.2.4.4.2. Duplicate Quality Gate](#3-2-4-4-2)
	* [3.2.4.4.3. Modify Quality Gate](#3-2-4-4-3)
	* [3.2.4.4.4. Add Quality Gate Condition](#3-2-4-4-4)
	* [3.2.4.4.5. Connect Quality Gate Project](#3-2-4-4-5)
	* [3.2.4.4.6. Delete Quality Gate](#3-2-4-4-6)
	* [3.2.4.5. Manage Staging](#3-2-4-5)
	* [3.2.4.5.1. Environment Information Management](#3-2-4-5-1)
	* [3.2.4.5.1.1. Register Environment Information](#3-2-4-5-1-1)
	* [3.2.4.5.1.2. Search Environment Information List](#3-2-4-5-1-2)
	* [3.2.4.5.1.3. Delete Environment Information](#3-2-4-5-1-3)

<br>

# <div id='1'/> 1. Document Outline

## <div id='1-1'/> 1.1 Purpose
This document describes the use of the container platform pipeline service.

## <div id='1-2'/> 1.2 Range
This document is a usage guide about how users will use the Container Platform Pipeline service based on their Windows environment.

<br>

# <div id='2'/> 2. Container Platform Pipeline Access
## <div id='2-1'/> 2.1 Container Platform Pipeline Access Description
Connect to the container platform pipeline.<br><br>
**Container Platform Pipeline URL** : `http://pipeline.${HOST_DOMAIN}`
+ Enter the `HOST_DOMAIN` value defined in [[Container platform pipeline variable definitions]](../../install-guide/pipeline/cp-pipeline-standalone-guide.md#3.2)

![image](../images/pipeline/IMG_2_1_1.png)
![image](../images/pipeline/IMG_2_1_2.png)

<br>

# <div id='3'/> 3. Container Platform Pipeline User Manual
This chapter describes the container platform pipeline's menu configuration and screen description.

## <div id='3-1'/> 3.1 Container Platform Pipeline User Menu Configuration
The Container Platform Pipeline service consists of parts that retrieve and manage pipelines, parts that manage Kubernetes cluster access information, parts that monitor the quality of source code, parts that manage the settings of deployed applications, and parts that manage users according to their authorities.  
***※ The User management, Pipeline management, and Quality management menu is not visible to the users.***

<table>
  <tr>
    <th>Menu</th>
    <th>Classification</th>
	<th>Description</th>
  </tr>
  <tr>
    <th>User Management</th>
    <td>User Dashboard</td>
	<td>Checks and manages user information of the users who are using container platform pipeline</td>
  </tr>
  <tr>
	<th>Pipeline</th>
	<td>Pipeline Dashboard</td>
	<td>Manages funcions such as register, list and details check, and contributor authority of the pipeline</td>
  </tr>
  <tr>
	<th rowspan="2">Pipeline Management</th>
	<td>Kubernetes Information Management</td>
	<td>Manages Kubernetes Cluster Access Information (kubeconfig)</td>
  </tr>
  <tr>
    <td>Track JOB Audit</td>
    <td>Track the execution status of jobs performed in the container platform deployment pipeline</td>
  </tr>
  <tr>
	<th rowspan="4">Quality Management</th>
	<td>Quality Issue</td>
	<td>Manage whether or not the source code performed is failover and the level of error, and activation status</td>
  </tr>
  <tr>
	<td>Coding Rules</td>
	<td>Manage coding forms for standardized source code</td>
  </tr>
  <tr>
	<td>Quality Profile</td>
	<td>Manages coding rules that serve as criteria for analyzing the quality of source code</td>
  </tr>
  <tr>
	<td>Quality Gate</td>
	<td>Manages criteria for quality analysis of source codes that exceed or fall short of a set quality reference value</td>
  </tr>
  <tr>
	<th>Staging Management</th>
	<td>Environment Information Management</td>
	<td>Manage configuration information for deployed applications</td>
  </tr>
</table>

## <div id='3-2'/> 3.2 Container Platform Pipeline User Menu Description
This chapter describes the four menus of the container platform deployment pipeline.

### <div id='3-2-1'/> 3.2.1. User Management
The user management menu is visible only to the manager, and this chapter describes the rights management and information check, and management of users using the container platform pipeline service.

#### <div id='3-2-1-1'/> 3.2.1.1. User Dashboard
##### <div id='3-2-1-1-1'/> 3.2.1.1.1. User List Search Check
1. Click the "User Management" button on the top right menu.
   ![image](../images/pipeline/IMG_3_2_1_1_1_1.png)
2. Proceed to the user dashboard.
   ![image](../images/pipeline/IMG_3_2_1_1_1_2.png)
3. Two conditions can be searched in the user dashboard list: a search with a user ID as a search term and a search whose search type is an administrator/user. After entering the search term, do "ENTER" or click the "Magnifier" button.
   ![image](../images/pipeline/IMG_3_2_1_1_1_3_1.png)
   ![image](../images/pipeline/IMG_3_2_1_1_1_3_2.png)

##### <div id='3-2-1-1-2'/> 3.2.1.1.2. View/ Modify User Details
1. Select an ID from the user dashboard to go to the user detail view/modify page.
   ![image](../images/pipeline/IMG_3_2_1_1_2_1.png)
2. Check the user's detailed information.
   ![image](../images/pipeline/IMG_3_2_1_1_2_2.png)
3. When you want to modify the information, enter a new input value. Authorities can be selected as administrators/users.
   ![image](../images/pipeline/IMG_3_2_1_1_2_3.png)
4. After modifying the input value, click the "Modify" button.
   ![image](../images/pipeline/IMG_3_2_1_1_2_4.png)
5. Check the modified information.
   ![image](../images/pipeline/IMG_3_2_1_1_2_5.png)

### <div id='3-2-2'/> 3.2.2. Pipeline
This chapter describes the two pipeline managing methods.  
***※ Basically, the user can only view the pipeline, and cannot create, query, modify, or delete it. However, if pipeline participant authorities are given, pipeline creation, inquiry, modification, and deletion are possible depending on the conditions.***

#### <div id='3-2-2-1'/> 3.2.2.1. My Pipeline
1. Regardless of which page you are on, click the My Pipeline button in the Dropdown menu to go to the dashboard.
   ![image](../images/pipeline/IMG_3_2_2_2_1_1.png)
2. Click the pipeline name in the list in My Pipeline to go to the details page of that pipeline.
   ![image](../images/pipeline/IMG_3_2_2_1_2.png)

#### <div id='3-2-2-2'/> 3.2.2.2. Create New Pipeline
##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Create New Pipeline(1)
1. Click "Pipeline List" on the top menu to create a new pipeline immediately by simply entering the pipeline name.
   ![image](../images/pipeline/IMG_3_2_2_2_1_1.png)
2. After pressing the registration button, you can check the pipeline added to the dashboard.
   ![image](../images/pipeline/IMG_3_2_2_2_1_2.png)
3. You can also check the pipeline registered in the Drop down menu.
   ![image](../images/pipeline/IMG_3_2_2_2_1_3.png)

##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Create New Pipeline(2)
1. Click the "Create New" button in the upper right corner.  
   ***※ “Create New” button is not enabled for users.***
   ![image](../images/pipeline/IMG_3_2_2_2_2_1.png)
2. Go to the Create New Pipeline page.
   ![image](../images/pipeline/IMG_3_2_2_2_2_2.png)
3. Pipeline names are required, so make sure to enter the pipeline name and click the "Create" button.
   ![image](../images/pipeline/IMG_3_2_2_2_2_3.png)
4. After it is created, check the pipeline dashboard for the newly added pipeline name.
   ![image](../images/pipeline/IMG_3_2_2_2_2_4.png)

##### <div id='3-2-2-2-3'/> 3.2.2.2.3. Create New Pipeline(3)
***※ Users cannot go to the pipeline detail page unless they are given participant authority to the pipeline.***
1. Click the "Create New" button in the upper right corner.  
   ![image](../images/pipeline/IMG_3_2_2_2_3_1.png)
2. Go to the Create New Pipeline page.  
   ![image](../images/pipeline/IMG_3_2_2_2_3_2.png)
3. Refer to 3.2.2.2.2. Create New Pipeline(2) for next procedures.

#### <div id='3-2-2-3'/> 3.2.2.3. Pipeline Dashboard
##### <div id='3-2-2-3-1'/> 3.2.2.3.1. Check Pipeline List Search
1. The conditions that can be retrieved from the list are pipeline name and two filters in order of name/date/update.
   ![image](../images/pipeline/IMG_3_2_2_3_1_1.png)
2. After entering the search term, do "ENTER" or click the "Magnifier" button.
   ![image](../images/pipeline/IMG_3_2_2_3_1_2.png)
3. When it is a list query/search list query, it is immediately reflected by selecting a search type filter.
   ![image](../images/pipeline/IMG_3_2_2_3_1_3.png)

##### <div id='3-2-2-3-2'/> 3.2.2.3.2. Check Pipeline Detailed Information
1. On the pipeline dashboard, click the pipeline name to go to the pipeline details page.
   ![image](../images/pipeline/IMG_3_2_2_3_2_1.png)
2. On the Pipeline Details page, click the "View/Modify Information" button in the upper right corner.
   ![image](../images/pipeline/IMG_3_2_2_3_2_2.png)
3. Go to the View/Modify Pipeline Information page and check the information of the pipeline in detail.
   ![image](../images/pipeline/IMG_3_2_2_3_2_3.png)

##### <div id='3-2-2-3-3'/> 3.2.2.3.3. Modify Pipeline
1. On the View/Modify Pipeline Information page, modify the input value and click the "Modify" button.
   ![image](../images/pipeline/IMG_3_2_2_3_3_1.png)
2. Once modified, go to the Pipeline Details page. Click the "View/Modify information" button again to check the changed value.
   ![image](../images/pipeline/IMG_3_2_2_3_3_2.png)

##### <div id='3-2-2-3-4'/> 3.2.2.3.4. Delete Pipeline
1. On the View/Modify Pipeline Information page, click the "Delete Pipeline" button.
   ![image](../images/pipeline/IMG_3_2_2_3_4_1.png)
2. After deletion, go to the pipeline dashboard and check if the pipeline has been deleted.
   ![image](../images/pipeline/IMG_3_2_2_3_4_2.png)

#### <div id='3-2-2-4'/> 3.2.2.4. Pipeline Details
This chapter describes overall participant management, such as adding, modifying, deleting, and authorizing participants participating in the pipeline, and how to create, execute, delete, log/history inquiry, and manage participants for one pipeline.
##### <div id='3-2-2-4-1'/> 3.2.2.4.1. Manage Participants
###### <div id='3-2-2-4-1-1'/> 3.2.2.4.1.1. Add Participant
***※	Administrators are pipeline creators, so they are participants with creation authority by default.***
1. On the Pipeline Details page, select the Participants tab.
   ![image](../images/pipeline/IMG_3_2_2_4_1_1_1.png)
2. Click the "Add Participant" button in the upper right corner.
   ![image](../images/pipeline/IMG_3_2_2_4_1_1_2.png)
3. Go to the Add Participants page to search for and select users who are currently invited to the deployment pipeline. Select one of the authorities to view, create, or execute and click the "Add" button.
   ![image](../images/pipeline/IMG_3_2_2_4_1_1_3.png)
4. Verify the added participant from the participants tab.
   ![image](../images/pipeline/IMG_3_2_2_4_1_1_4.png)

###### <div id='3-2-2-4-1-2'/> 3.2.2.4.1.2. Check Participant List Search
1. The condition that can be searched in the participant list is only participant ID search.
   ![image](../images/pipeline/IMG_3_2_2_4_1_2_1.png)

###### <div id='3-2-2-4-1-3'/> 3.2.2.4.1.3.	Check/ Modify Participant Detailed Information
1. Select the participant ID to query for information from the participant list.
   ![image](../images/pipeline/IMG_3_2_2_4_1_3_1.png)
2. Go to the Participant Details Check/Modify page and check the participant's details.
   ![image](../images/pipeline/IMG_3_2_2_4_1_3_2.png)
3. Select the authority you want to modify and click the "Modify" button.
   ![image](../images/pipeline/IMG_3_2_2_4_1_3_3.png)
4. Check the modified participant authority in the Participant List.
   ![image](../images/pipeline/IMG_3_2_2_4_1_3_4.png)

###### <div id='3-2-2-4-1-4'/> 3.2.2.4.1.4.	Delete Participant
1. Select the participant ID to delete from the participants list.
   ![image](../images/pipeline/IMG_3_2_2_4_1_4_1.png)
2. Go to the Check/Modify Participant Details page.
   ![image](../images/pipeline/IMG_3_2_2_4_1_4_2.png)
3. Click the “Delete Participant” button.
   ![image](../images/pipeline/IMG_3_2_2_4_1_4_3.png)
4. Confirm that the participant has been deleted from the participant list.
   ![image](../images/pipeline/IMG_3_2_2_4_1_4_4.png)

##### <div id='3-2-2-4-2'/> 3.2.2.4.2. Build Job
###### <div id='3-2-2-4-2-1'/> 3.2.2.4.2.1. Create Build Job
1. On the Pipeline Details page, click the 'Click here to add a new task' button.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_1.png)
2. Go to the Configuration Details page.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_2.png)
3. Select a job type and select it as a build type. After that, enter the image name to be saved in Harbor. <br>
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_3.png)
   ***※ If Gradle is selected, reflect the source build and test. For static analysis, reflect the Gradle wrapper settings.***

4. Click the Create Docker File button.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_4.png)
5. If you click a name suitable for the builder type in the staging information inquiry, for example, information on how to set the Spring Config Client is checked.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_5.png)
6. After entering the shape management information, click the Branch Lookup button.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_6.png)
   ***※ When you select Github, you do not need to enter your ID and password when inquiring about the branch if you enter the public repository path.***

7. Select the branch you want and click the save button.
   ![image](../images/pipeline/IMG_3_2_2_4_2_1_7.png)
   ***※ Build Job creation can only be created by participants with creation authority among administrators and pipeline participants.***

###### <div id='3-2-2-4-2-2'/> 3.2.2.4.2.2. Check/ Modify Build Job Configuration
1. Click the "Configuration" icon for the created build job.<br>
   ![image](../images/pipeline/IMG_3_2_2_4_2_2_1.png)
2. Go to the configuration detail page and check the configuration information stored at the time of creation.
   ![image](../images/pipeline/IMG_3_2_2_4_2_2_2.png)
3. When modifying, re-enter the information you want to modify in each input form and click the "Save" button (in the example, modify Dockerfile)
   ![image](../images/pipeline/IMG_3_2_2_4_2_2_3.png)
   ***※ When modifying the build job, the shape management information cannot be modified except for the branch.***<br>

4. Go to the configuration detail page and check the modified information.
   ![image](../images/pipeline/IMG_3_2_2_4_2_2_4.png)
   ***※ All pipeline participants can check the build job configuration. However, modifications can only be made by managers and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-3'/> 3.2.2.4.2.3. Execute Build Job
1. Click the "Run" icon on the Pipeline Details page.
   ![image](../images/pipeline/IMG_3_2_2_4_2_3_1.png)
2. You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
   ![image](../images/pipeline/IMG_3_2_2_4_2_3_2.png)
3. When the execution is completed, it turns green and is marked as Build in the task part.
   ![image](../images/pipeline/IMG_3_2_2_4_2_3_3.png)
   ***※	Build Job execution is only possible for administrators and pipeline participants with creation and execution authorities.***

###### <div id='3-2-2-4-2-4'/> 3.2.2.4.2.4. Stop Build Job
1. Click the "Stop" icon when you want to stop and cancel a running build job.
   ![image](../images/pipeline/IMG_3_2_2_4_2_4_1.png)
2.	It can be seen that the stopped build Job turns orange.
	  ![image](../images/pipeline/IMG_3_2_2_4_2_4_2.png)
	  ***※	Stopping the build job is only possible for managers and pipeline participants with creation and execution authorities.***

###### <div id='3-2-2-4-2-5'/> 3.2.2.4.2.5.	Build Job Log/ History
1. You can view logs in real-time by clicking the "log/history" icon when the build job is running.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_1.png)
2. Go to the log lookup page. Check that the log is visible in real-time.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_2.png)
3. Verify that the build job execution is completed, and check the history.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_3.png)
4. You can cancel and run the Job by clicking the "Cancel" or "Run" button at the top of the Log/History page.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_4.png)
5. Click the "Configure" button on the Log/History page to go to the Build Job Configuration Inquiry page.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_5.png)
6. Click the "List" button on the Log/History page to go to the Pipeline Details page.
   ![image](../images/pipeline/IMG_3_2_2_4_2_5_6.png)
   ***※	The Build Job Log/History can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authorities.***

###### <div id='3-2-2-4-2-6'/> 3.2.2.4.2.6.	Add Build Job
1. On the Pipeline Details page, click the "Add" button on the Build Job.
   ![image](../images/pipeline/IMG_3_2_2_4_2_6_1.png)
2. Go to the configuration detail page, enter the appropriate values in the input form, such as creating a build job, and click the "Save" button. (You can select a test and deployment type for the task type in addition to the build.)
   ![image](../images/pipeline/IMG_3_2_2_4_2_6_2.png)
3. Check the added build Job.<br>
   ![image](../images/pipeline/IMG_3_2_2_4_2_6_3.png)
4. When you check the configuration of this task (Job) as a new workgroup in the task trigger, the Job is added as a new group. (We will discuss it later in the topic Adding a new workgroup.)  
   ***※	Adding a build job is only available for administrators and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-7'/> 3.2.2.4.2.7.	Duplicate Build Job
1. On the Pipeline Details page, click the "Duplicate" button on the Build Job.
   ![image](../images/pipeline/IMG_3_2_2_4_2_7_1.png)
2. Check the duplicated Build Job.    
   ![image](../images/pipeline/IMG_3_2_2_4_2_7_2.png)
   ***※	Build Job duplication is only possible for administrators and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-8'/> 3.2.2.4.2.8.	Delete Build Job
1. On the Pipeline Details page, click the "Delete" button on the Build Job.
   ![image](../images/pipeline/IMG_3_2_2_4_2_8_1.png)
2. Verify the deleted Build Job.
   ![image](../images/pipeline/IMG_3_2_2_4_2_8_2.png)
   ***※	Delete Build Job is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-3'/> 3.2.2.4.3. Static Analysis Job
###### <div id='3-2-2-4-3-1'/> 3.2.2.4.3.1. Create Static Analysis Job
1. Click the "Add" button on the Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_1_1.png)
2. Go to the configuration page, select the task type as Static-Analysis, and select the desired quality profile, quality gate, and workgroup from the input type. After that, the action trigger is selected according to each situation. (Initially, no quality gate exists. Create by referring to [Create Quality Gate](#3-2-4-4-1).)
   ![image](../images/pipeline/IMG_3_2_2_4_3_1_2.png)
3. Click GRADLE and MAVEN on the JACO plugins script tab to see how to apply jacoco plugins (for example, apply the plugins according to the situation)
   ![image](../images/pipeline/IMG_3_2_2_4_3_1_3.png)
4. Click the "Save" button and confirm that the test job has been created on the pipeline detail page.
   ![image](../images/pipeline/IMG_3_2_2_4_3_1_4.png)
   ***※	Static analysis Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-3-2'/> 3.2.2.4.3.2. Check/ Modify Static Analysis Job Configuration
1. Click the "Configuration" icon of the generated Static Analysis Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_2_1.png)
2. Go to the configuration detail page and check the configuration information stored at the time of creation.
   ![image](../images/pipeline/IMG_3_2_2_4_3_2_2.png)
3. When modifying, re-enter the information to be modified in each input form and click the "Save" button.
   ![image](../images/pipeline/IMG_3_2_2_4_3_2_3.png)
   ***※	Static analysis Job composition inquiry can be checked by all pipeline participants. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-3'/> 3.2.2.4.3.3.	Execute Static Analysis Job
1. On the Pipeline Details page, click the "Run" icon of the Static Analysis Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_3_1.png)
2. You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
   ![image](../images/pipeline/IMG_3_2_2_4_3_3_2.png)
3. When the execution is completed, it turns green and is displayed as Test(Execution Completed) in the task part.
   ![image](../images/pipeline/IMG_3_2_2_4_3_3_3.png)
   ***※	 Job execution is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-3-4'/> 3.2.2.4.3.4.	Stop Static Analysis Job
1. Click the "Stop" icon when you want to stop and cancel a static analysis Job that is running.
   ![image](../images/pipeline/IMG_3_2_2_4_3_4_1.png)
2. Stopped Static Analysis Job is turned to orange
   ![image](../images/pipeline/IMG_3_2_2_4_3_4_2.png)
   ***※	 Job suspension is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-3-5'/> 3.2.2.4.3.5.	Static Analysis Job Log/History
1. When a static analysis job is in progress, you can view the log in real time by clicking the "log/history" icon.
   ![image](../images/pipeline/IMG_3_2_2_4_3_5_1.png)
2. Go to the log lookup page. Check that the log is visible in real time.
   ![image](../images/pipeline/IMG_3_2_2_4_3_5_2.png)
3. Confirm that the static analysis job execution is completed, and check the history.
   ![image](../images/pipeline/IMG_3_2_2_4_3_5_3.png)
4. For the "Execute", "Cancel", "Configure", and "List" buttons, see 3.2.4.2.5. Build Job Log/History.  
   ***※	Job logs/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons are only available to participants with creation and execution authority.***

###### <div id='3-2-2-4-3-6'/> 3.2.2.4.3.6.	Add Static Analysis Job
1. When you press the Log/History "Quality Issue Results" button in the Test Job, it goes to the Quality Management Dashboard that manages the error resolution, level of error, and activation status of the source code performed.
   ![image](../images/pipeline/IMG_3_2_2_4_3_6_1.png)
2. Refer to 3.2.4 Quality Management Part for Quality Management Dashboard.  
   ***※	 The results of the Job quality issue can be checked by the manager and all pipeline participants.***

###### <div id='3-2-2-4-3-7'/> 3.2.2.4.3.7.	Static Analysis Job Job Quality Issue Results
1. On the Pipeline Details page, click the "Add" button of the Static Analysis Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_7_1.png)
2. Refer to 3.2.2.4.2.7. Add Build Job part for the procedures afterward.  
   ***※	Static analysis jobs can only be added by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-8'/> 3.2.2.4.3.8.	Duplicate Static Analysis Job
1. On the Pipeline Details page, click the "Duplicate" button on the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_8_1.png)
2. Verify that the static analysis Job is duplicated.
   ![image](../images/pipeline/IMG_3_2_2_4_3_8_2.png)
   ***※	Static analysis Job Duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-9'/> 3.2.2.4.3.9.	Delete Static Analysis Job
1. On the Pipeline Details page, click the "Delete" button on the Static Analysis Job.
   ![image](../images/pipeline/IMG_3_2_2_4_3_9_1.png)
2. Confirm that the static analysis Job has been deleted.
   ![image](../images/pipeline/IMG_3_2_2_4_3_9_2.png)
   ***※	 Job deletion is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-4'/> 3.2.2.4.4. Test Job
###### <div id='3-2-2-4-4-1'/> 3.2.2.4.4.1. Create Test Job
1. Click the "Add" button on the Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_1_1.png)
2. Go to the configuration page, select the task type as JUnit-Test, and select the input type to test. After that, the action trigger is selected according to each situation.
   ![image](../images/pipeline/IMG_3_2_2_4_4_1_2.png)
3. Click the "Save" button and confirm that the test job has been created on the pipeline detail page.
   ![image](../images/pipeline/IMG_3_2_2_4_4_1_3.png)
   ***※	Test Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-4-2'/> 3.2.2.4.4.2. Check/ Modify Test Job Configuration
1. Click the “Configure” icon from the created Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_2_1.png)
2. Go to the configuration detail page and check the configuration information stored at the time of creation.
   ![image](../images/pipeline/IMG_3_2_2_4_4_2_2.png)
3. When modifying, re-enter the information to be modified in each input form and click the "Save" button.
   ![image](../images/pipeline/IMG_3_2_2_4_4_2_3.png)
4. Check the modified information by proceeding to the configuration details page.
   ![image](../images/pipeline/IMG_3_2_2_4_4_2_4.png)
   ***※	All pipeline participants can check the configuration of the test job. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-4-3'/> 3.2.2.4.4.3.	Execute Test Job
1. On the Pipeline Details page, click the "Run" icon in the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_3_1.png)
2. You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
   ![image](../images/pipeline/IMG_3_2_2_4_4_3_2.png)
3. When the execution is completed, it turns green and is displayed as Test(Execution Completed) in the task part.
   ![image](../images/pipeline/IMG_3_2_2_4_4_3_3.png)
   ***※	Test Job execution is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-4-4'/> 3.2.2.4.4.4.	Stop Test Job
1. Click the "Stop" icon when you want to stop and cancel a running test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_4_1.png)
2. You can see that the stopped build Job turned orange.
   ![image](../images/pipeline/IMG_3_2_2_4_4_4_2.png)
   ***※	Test Job stop is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-4-5'/> 3.2.2.4.4.5.	Test Job Log/ History
1. You can view logs in real time by clicking the "log/history" icon when the build job is running.
   ![image](../images/pipeline/IMG_3_2_2_4_4_5_1.png)
2. Go to the log lookup page. Check that the log is visible in real time.
   ![image](../images/pipeline/IMG_3_2_2_4_4_5_2.png)
3. Confirm that the test job execution is completed, and check the history.
   ![image](../images/pipeline/IMG_3_2_2_4_4_5_3.png)
4. For the "Execute", "Cancel", "Configure", and "List" buttons, see 3.2.4.2.5. Build Job Log/History.  
   ***※	The test Job log/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authority.***

###### <div id='3-2-2-4-4-6'/> 3.2.2.4.4.6.	Add Test Job
1. Pressing the Log/History "Test Results" button on the Test Job will bring up the JUnit Test Results pop-up window of the source code performed.
   ![image](../images/pipeline/IMG_3_2_2_4_4_6_1.png)
2. See 3.2.2.4.2.6. Add Build Job for subsequent processes.  
   ***※	The results of the test Job quality issue can be checked by the manager and all pipeline participants.***

###### <div id='3-2-2-4-4-7'/> 3.2.2.4.4.7.	Test Job Quality Issue Results
1. On the Pipeline Details page, click the "Add" button on the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_7_1.png)
2. Refer to 3.2.2.4.2.6. Add Build Job for the next procedures.  
   ***※	Only administrators and pipeline participants with creation authority can add test jobs.***

###### <div id='3-2-2-4-4-8'/> 3.2.2.4.4.8.	Duplicate Test Job
1. On the Pipeline Details page, click the "Deplicate" button on the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_8_1.png)
2. Refer to 3.2.2.4.2.7 Duplicate Build Job part for the next procedures.  
   ***※	Test Job duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-4-9'/> 3.2.2.4.4.9.	Delete Test Job
1. On the Pipeline Details page, click the "Delete" button on the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_4_9_1.png)
2. Refer to 3.2.2.4.2.8 Delete Build Job part for the next procedures.  
   ***※	Deleting test jobs is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-5'/> 3.2.2.4.5. Deployment Job
###### <div id='3-2-2-4-5-1'/> 3.2.2.4.5.1.	Create Deployment Job
1. Click the "Add" button on the test job.<br>
   ![image](../images/pipeline/IMG_3_2_2_4_5_1_1.png)
2. Go to the configuration page, select the task type as Deploy, select the desired deployment type and app type from Type, and select the saved Kubernetes information in Pipeline Management. (In order to get Kubernetes information, a prior process is required. Refer to 3.2.3.1. Kubernetes Information Management).  
   ***※	The format of DEPLOYMENT.YML and DEPLOYMENT_SERVICE.YML varies depending on the type selected (type), app type.***
   ![image](../images/pipeline/IMG_3_2_2_4_5_1_2.png)
3. Change DEPLOYMENT.YML and DEPLOYMENT_SERVICE.YML to match app settings.  
   ***※	%{VALUE}% values such as %INTERNAL_SERVICE_PORT% inside the YML file must be changed.***
4. Click the "Save" button and confirm that the deployment job has been created on the pipeline detail page.
   ![image](../images/pipeline/IMG_3_2_2_4_5_1_4.png)
   ***※	Deployment Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-5-2'/> 3.2.2.4.5.2.	Check/ Modify Deployment Job Configuration
1. Click the "Configuration" icon of the created deployment job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_2_1.png)
2. Go to the configuration detail page and check the configuration information stored at the time of creation.
   ![image](../images/pipeline/IMG_3_2_2_4_5_2_2.png)
3. When modifying, re-enter the information to be modified in each input YML and click the "Save" button.
   ![image](../images/pipeline/IMG_3_2_2_4_5_2_3.png)
4. Go to the configuration detail page and check the modified information.
   ![image](../images/pipeline/IMG_3_2_2_4_5_2_4.png)
   ***※	 Deployment Job configuration check can be made by all pipeline participants. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-5-3'/> 3.2.2.4.5.3.	Execute Deployment Job
1. On the Pipeline Details page, click the "Run" icon in the Deployment Job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_3_1.png)
2. You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
   ![image](../images/pipeline/IMG_3_2_2_4_5_3_2.png)
3. When the execution is complete, it turns green and appears as Deploy(Execution Completed) in the task section.
   ![image](../images/pipeline/IMG_3_2_2_4_5_3_3.png)
   ***※	Deployment Job execution is only possible for administrators and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-5-4'/> 3.2.2.4.5.4.	Stop Deployment Job
1. Click the "Stop" icon when you want to stop and cancel a running deployment job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_4_1.png)
2. It can be seen that the stopped deployment Job turns orange.
   ![image](../images/pipeline/IMG_3_2_2_4_5_4_2.png)
   ***※	The stopping of deployment job is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-5-5'/> 3.2.2.4.5.5.	Deployment Job Log/ History
1. When the deployment job is running, you can view the log in real-time by clicking the "log/history" icon.
   ![image](../images/pipeline/IMG_3_2_2_4_5_5_1.png)
2. Go to the log lookup page. Check that the log is visible in real-time.
   ![image](../images/pipeline/IMG_3_2_2_4_5_5_2.png)
3. Confirm that the deployment job execution is completed, and check the history.
   ![image](../images/pipeline/IMG_3_2_2_4_5_5_3.png)
4. As a result of inquiring on the K-PaaS container platform portal, it can be confirmed that an application called 'spring-music-test' was deployed in the deployment section.
   ![image](../images/pipeline/IMG_3_2_2_4_5_5_4.png)
   ***※	The deployment Job log/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authority.***

###### <div id='3-2-2-4-5-6'/> 3.2.2.4.5.6.	Rollback Deployment Job to Current Job
1. The "Rollback to Current Job" button on the Log/History page of the Deployment Job is not currently supported.
   ![image](../images/pipeline/IMG_3_2_2_4_5_6_1.png)

###### <div id='3-2-2-4-5-7'/> 3.2.2.4.5.7.	Add Deployment Job
1. On the Pipeline Details page, click the "Add" button on the Test Job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_7_1.png)
2. Refer to 3.2.2.4.2.6. Add Build Job part for the next procedures.  
   ***※	Only administrators and pipeline participants with creation authority can add deployment jobs.***

###### <div id='3-2-2-4-5-8'/> 3.2.2.4.5.8.	Duplicate Deployment Job
1. On the Pipeline Details page, click the "Duplicate" button on the Deployment Job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_8_1.png)
2. Refer to 3.2.2.4.2.7. Duplicate Build Job part for the next procedures.  
   ***※	Deployment Job duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-5-9'/> 3.2.2.4.5.9.	Delete Deployment Job
1. On the Pipeline Details page, click the "Delete" button on the Deployment Job.
   ![image](../images/pipeline/IMG_3_2_2_4_5_9_1.png)
2. Refer to 3.2.2.4.2.8. Delete Build Job part for the next procedure.  
   ***※	Deleting a deployment job is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-6'/> 3.2.2.4.6. Arrange Job's Job
1. On the Pipeline Details page, click the "Sort Job" icon for each Job.
   ![image](../images/pipeline/IMG_3_2_2_4_6_1.png)
2. A list of the remaining Job numbers that can be sorted within the current workgroup appears in the drop-down menu.
   ![image](../images/pipeline/IMG_3_2_2_4_6_2.png)
3. Among them, when you click the number you want to sort, the Job and location of the number are switched and sorted.
   ![image](../images/pipeline/IMG_3_2_2_4_6_3.png)
   ***※	Job job sorting is only possible for managers and pipeline participants with creation authority.***

##### <div id='3-2-2-4-7'/> 3.2.2.4.7. Add New Job Group
1. Click the "Add New Workgroup" button on the Pipeline Details page.
   ![image](../images/pipeline/IMG_3_2_2_4_7_1.png)
2. Proceed to the Job Configuration Details Page.
   ![image](../images/pipeline/IMG_3_2_2_4_7_2.png)
3. Create a new Job and go to the Pipeline Details page. It is visible that a dotted line is created below the previous workgroup and the newly created Job is divided into the new group.
   ![image](../images/pipeline/IMG_3_2_2_4_7_3.png)
   ***※	Adding a new workgroup is only available for administrators and pipeline participants with creation authority.***

### <div id='3-2-3'/> 3.2.3.Pipeline Management
#### <div id='3-2-3-1'/> 3.2.3.1. Manage Kubernetes Information
###### <div id='3-2-3-1-1'/> 3.2.3.1.1. Register Kubernetes Information
1. Click Pipeline Management > KubeInfo Management Menu.
   ![image](../images/pipeline/IMG_3_2_3_1_1_1.png)
2. Click the "Kube Config Register" button in the upper right corner of the dashboard.
   ![image](../images/pipeline/IMG_3_2_3_1_1_2.png)
3. Proceed to Register kubernetes config page.
   ![image](../images/pipeline/IMG_3_2_3_1_1_3.png)
4. Enter the config name and kube config and click the Register button.
   ![image](../images/pipeline/IMG_3_2_3_1_1_4.png)
5. Check the registered kubernetes config.<br>
   ![image](../images/pipeline/IMG_3_2_3_1_1_5.png)

###### <div id='3-2-3-1-2'/> 3.2.3.1.2. Modify Kubernetes Information
1. In the kubernetes information management dashboard, click the information you want to modify.
   ![image](../images/pipeline/IMG_3_2_3_1_2_1.png)
2. Eneter the value to modify and click “Modify” button.
   ![image](../images/pipeline/IMG_3_2_3_1_2_2.png)
3. Go back to the information detail page through the dashboard and check if it has been modified.
   ![image](../images/pipeline/IMG_3_2_3_1_2_3.png)

#### <div id='3-2-3-2'/> 3.2.3.2. Track JOB Audit
###### <div id='3-2-3-2-1'/> 3.2.3.2.1. Check Tracked JOB Audit
1. Click Pipeline Management > JOB Audit Trail Menu.
   ![image](../images/pipeline/IMG_3_2_3_2_1_1.png)
2. You can view the list of recently registered/executed/modified/deleted JOBs from the JOB Audit Tracking Dashboard.
   ![image](../images/pipeline/IMG_3_2_3_2_1_2.png)

###### <div id='3-2-3-2-2'/> 3.2.3.2.2. Check Searched JOB Audit
1. Enter the message you want to search on the JOB Audit Tracking Dashboard.
   ![image](../images/pipeline/IMG_3_2_3_2_2_1.png)
2. Only trace records that match the message searched in the JOB Audit Tracking page are checked.

### <div id='3-2-4'/> 3.2.4. Quality Management
This chapter describes quality issues and coding rules, quality profiles, and quality gates about source codes inspected through a static analysis Job.
#### <div id='3-2-4-1'/> 3.2.4.1. Quality Issue
1. Proceed to the Quality Issue Dashboard by clicking the quality issue on the Quality Management menu.
   ![image](../images/pipeline/IMG_3_2_4_1_1.png)
2. Click the "Details" button of the coding rule to view.
   ![image](../images/pipeline/IMG_3_2_4_1_2.png)
3.	Click the "List" button in the upper right corner to go to the Quality Issues dashboard with the Job Test List.
	  ![image](../images/pipeline/IMG_3_2_4_1_3.png)

#### <div id='3-2-4-2'/> 3.2.4.2. Coding Rules
1. Proceed to the Coding Rules Dashboard by clicking the coding rules on the quality management menu.
   ![image](../images/pipeline/IMG_3_2_4_2_1.png)
2. Click the "Add to Profile" button, set the issue level in the Add Profile pop-up window, and click the "Add" button.
   ![image](../images/pipeline/IMG_3_2_4_2_2.png)
3. In the lower-left corner, click the java selected in the previous step in the Quality Profile menu to verify that the added coding rule has been added.
   ![image](../images/pipeline/IMG_3_2_4_2_3.png)
4. Click the "Remove from Profile" button.
   ![image](../images/pipeline/IMG_3_2_4_2_4.png)
5. Confirm if the removed coding rule has been deleted from the quality profile.
   ![image](../images/pipeline/IMG_3_2_4_2_5.png)

#### <div id='3-2-4-3'/> 3.2.4.3. Quality Profile
##### <div id='3-2-4-3-1'/> 3.2.4.3.1. Create Quality Profile
1. Proceed to the Quality Profile Dashboard by clicking Quality Profile from the Quality Management Menu.
   ![image](../images/pipeline/IMG_3_2_4_3_1_1.png)
2. Click the "Create" button in the upper right corner.
   ![image](../images/pipeline/IMG_3_2_4_3_1_2.png)
3. Enter the quality profile name, select the development language and click the "Create" button.
   ![image](../images/pipeline/IMG_3_2_4_3_1_3.png)
4. Verify that a quality profile is created.
   ![image](../images/pipeline/IMG_3_2_4_3_1_4.png)

##### <div id='3-2-4-3-2'/> 3.2.4.3.2. Duplicate Quality Profile
1. Click the "Duplicate" button on the Quality Profile dashboard.
   ![image](../images/pipeline/IMG_3_2_4_3_2_1.png)
2. Enter the name of the quality profile you want to duplicate, and click the "Duplicate" button.
   ![image](../images/pipeline/IMG_3_2_4_3_2_2.png)
3. Check the duplicated quality profile.
   ![image](../images/pipeline/IMG_3_2_4_3_2_3.png)

##### <div id='3-2-4-3-3'/> 3.2.4.3.3. Modify Quality Profile
1. Click the "Modify" button on the Quality Profile dashboard.
   ![image](../images/pipeline/IMG_3_2_4_3_3_1.png)
2. When the quality profile pop-up window to modify appears, modify the quality profile name and click the "Modify" button.
   ![image](../images/pipeline/IMG_3_2_4_3_3_2.png)
3. Confirm that the quality profile name has been modified.
   ![image](../images/pipeline/IMG_3_2_4_3_3_3.png)

##### <div id='3-2-4-3-4'/> 3.2.4.3.4. Connect Quality Profile Project
1. Check the quality profile dashboard for connected project items. (The first picture shows the quality profile set to Sonar way when retrieving the test job configuration. Therefore, as an example in the second picture, the newly created quality profile [GuideModified] does not show any connected projects.)
   ![image](../images/pipeline/IMG_3_2_4_3_4_1.png)
2. If no projects are connected, click the "Not Connected" tab.
   ![image](../images/pipeline/IMG_3_2_4_3_4_2.png)
3. When Profile is connected, the project can be seen in the "Connected" tab.
   ![image](../images/pipeline/IMG_3_2_4_3_4_3.png)
   ![image](../images/pipeline/IMG_3_2_4_3_4_4.png)

##### <div id='3-2-4-3-5'/> 3.2.4.3.5. Delete Quality Profile
1. Click the "Delete" button on the Quality Profile dashboard.
   ![image](../images/pipeline/IMG_3_2_4_3_5_1.png)

#### <div id='3-2-4-4'/> 3.2.4.4. Quality Gate
##### <div id='3-2-4-4-1'/> 3.2.4.4.1. Create Quality Gate
1. Select Quality Gate from the Quality Management menu to go to the Quality Gate dashboard.
   ![image](../images/pipeline/IMG_3_2_4_4_1_1.png)
2. Click the "Create" button in the upper right corner.
   ![image](../images/pipeline/IMG_3_2_4_4_1_2.png)
3. Enter the quality gate name and click the "Create" button.
   ![image](../images/pipeline/IMG_3_2_4_4_1_3.png)
4. Check that a quality gate is created.
   ![image](../images/pipeline/IMG_3_2_4_4_1_4.png)

##### <div id='3-2-4-4-2'/> 3.2.4.4.2. Duplicate Quality Gate
1. Click the "Duplicate" button on the Quality Gate dashboard.
   ![image](../images/pipeline/IMG_3_2_4_4_2_1.png)
2. Enter the name of the quality gate you want to duplicate, and click the "Duplicate" button.
   ![image](../images/pipeline/IMG_3_2_4_4_2_2.png)

##### <div id='3-2-4-4-3'/> 3.2.4.4.3. Modify Quality Profile
1. Click the "Modify" button on the Quality Gate dashboard.
   ![image](../images/pipeline/IMG_3_2_4_4_3_1.png)
2. When the quality gate pop-up window to modify appears, modify the quality gate name and click the "Modify" button.
   ![image](../images/pipeline/IMG_3_2_4_4_3_2.png)

##### <div id='3-2-4-4-4'/> 3.2.4.4.4. Add Quality Gate Conditions
1. On the quality gate dashboard, check the additional conditions that can set the conditions for passing the Job test. The user can add the conditions himself and set the criteria. In the Save/Modify column, click Save to save.
   ![image](../images/pipeline/IMG_3_2_4_4_4_1.png)
2. If the test is above/below a certain standard depending on the conditions, pass the test.
   ![image](../images/pipeline/IMG_3_2_4_4_4_2.png)

##### <div id='3-2-4-4-5'/> 3.2.4.4.5. Connect Quality Gate Project
1. Check the connected project items on the quality gate dashboard. (The first picture shows the quality gate set to quality-gate when retrieving the static analysis Job configuration.)
   ![image](../images/pipeline/IMG_3_2_4_4_5_1.png)
2. Quality gates can connect to multiple projects per quality gate like quality profiles.

##### <div id='3-2-4-4-6'/> 3.2.4.4.6. Delete Quality Gate
1.	Click the "Delete" button on the quality gate dashboard.
	  ![image](../images/pipeline/IMG_3_2_4_4_6_1.png)
2.	Confirm that the quality gate has been deleted.
	  ![image](../images/pipeline/IMG_3_2_4_4_6_2.png)

#### <div id='3-2-4-5'/> 3.2.4.5. Manage Staging
##### <div id='3-2-4-5-1'/> 3.2.4.5.1. Environment Information Management
A page that manages the configuration settings of the deployed application. The container platform pipeline provides a spring cloud config server and stores and manages environmental information in DB.

###### <div id='3-2-4-5-1-1'/> 3.2.4.5.1.1. Register Environment Information
1. Access the Environmental Information Management page.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_1.png)
2. Click the Add button on the right side of the Environment Information Management Dashboard.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_2.png)
3. Click the file selection button in the environment information registration page.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_3.png)
   ***※    Select .properties, .yaml', .yml format, and shall not include dividing line such as '----'.***
4. If selected correctly, a pop-up notification window appears saying OK.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_4.png)
5. Enter the application name and profile name according to the situation.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_5.png)
   ***※	In the Container Platform pipeline, the label name is fixed to 'standalone'.***
6. Click the Register button to register the environment information.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_6.png)
7. Ensure that environmental information is properly registered.
   ![image](../images/pipeline/IMG_3_2_4_5_1_1_7.png)

###### <div id='3-2-4-5-1-2'/> 3.2.4.5.1.2. Search Environment Information List
Can be searched through three types: 'Application name', 'profile name', and 'Key search' in the environmental information management dashboard.
1. Expand the application filter to select the desired application.
   ![image](../images/pipeline/IMG_3_2_4_5_1_2_1.png)
2. The selected application is filtered and retrieved.
   ![image](../images/pipeline/IMG_3_2_4_5_1_2_2.png)
3. Check if the selected profile name is filtered and inquired.
   ![image](../images/pipeline/IMG_3_2_4_5_1_2_3.png)
4. Check if it is filtered and inquired with the entered key name.
   ![image](../images/pipeline/IMG_3_2_4_5_1_2_4.png)

###### <div id='3-2-4-5-1-3'/> 3.2.4.5.1.3. Delete Environment Information
The selected application/profile information may be deleted from the environment information management dashboard.
1. Expand the application filter to select the application you want to delete.
   ![image](../images/pipeline/IMG_3_2_4_5_1_3_1.png)
2. Expand the Profile filter and select the profile you want to delete.
   ![image](../images/pipeline/IMG_3_2_4_5_1_3_2.png)
3. Click the 'Delete Application' button.
   ![image](../images/pipeline/IMG_3_2_4_5_1_3_3.png)
4. Check if the selected environment information is deleted.
   ![image](../images/pipeline/IMG_3_2_4_5_1_3_4.png)

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Pipeline Service Use Guide
