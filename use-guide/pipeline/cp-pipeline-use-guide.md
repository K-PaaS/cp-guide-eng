### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](https://github.com/PaaS-TA/paas-ta-container-platform-guide-eng/tree/master/use-guide/Readme.md) > Pipeline Service Use Guide

<br>

# [PaaS-TA Container Platform Pipeline User Guide]

## Table of content
1. [Document Outline](#1)
	* [1.1. Purpose](#1-1)
	* [1.2. Range](#1-2)
2. [Container](#2)
	* [2.1. Service Container Platform Pipeline Access](#2-1)
	* [2.2. Stand-Alone Deployment Container Platform Pipeline](#2-2)
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
	* [3.2.2.3.   Pipeline Dashboard](#3-2-2-3)
	* [3.2.2.3.1. Check Pipeline List Search](#3-2-2-3-1)
	* [3.2.2.3.2. Check Pipeline Detailed Information](#3-2-2-3-2)
	* [3.2.2.3.3. Modify Pipeline](#3-2-2-3-3)
	* [3.2.2.3.4. Delete Pipeline](#3-2-2-3-4)
	* [3.2.2.4.   Pipeline Details](#3-2-2-4)
	* [3.2.2.4.1. Manage Participants](#3-2-2-4-1)
	* [3.2.2.4.1.1. Add Participant](#3-2-2-4-1-1)
	* [3.2.2.4.1.2.    Check Participant List Search](#3-2-2-4-1-2)
	* [3.2.2.4.1.3.    Check/ Modify Participant Detailed Information](#3-2-2-4-1-3)
	* [3.2.2.4.1.4.    Delete Participant](#3-2-2-4-1-4)
	* [3.2.2.4.2. Build Job](#3-2-2-4-2)
	* [3.2.2.4.2.1.    Create Build Job](#3-2-2-4-2-1)
	* [3.2.2.4.2.2.    Check/ Modify Build Job Configuration](#3-2-2-4-2-2)
	* [3.2.2.4.2.3.    Execute Build Job](#3-2-2-4-2-3)
	* [3.2.2.4.2.4.    Stop Build Job](#3-2-2-4-2-4)
	* [3.2.2.4.2.5.    Build Job Log/ History](#3-2-2-4-2-5)
	* [3.2.2.4.2.6.    Add Build Job](#3-2-2-4-2-6)
	* [3.2.2.4.2.7.    Duplicate Build Job](#3-2-2-4-2-7)
	* [3.2.2.4.2.8.    Delete Build Job](#3-2-2-4-2-8)
	* [3.2.2.4.3. Static Analysis Job](#3-2-2-4-3)
	* [3.2.2.4.3.1.    Create Static Analysis Job](#3-2-2-4-3-1)
	* [3.2.2.4.3.2.    Check/ Modify Static Analysis Job Configuration](#3-2-2-4-3-2)
	* [3.2.2.4.3.3.    Execute Static Analysis Job](#3-2-2-4-3-3)
	* [3.2.2.4.3.4.    Stop Static Analysis Job](#3-2-2-4-3-4)
	* [3.2.2.4.3.5.    Static Analysis Job Log/ History](#3-2-2-4-3-5)
	* [3.2.2.4.3.6.    Add Static Analysis Job](#3-2-2-4-3-6)
	* [3.2.2.4.3.7.    Static Analysis Job Job Quality Issue Results](#3-2-2-4-3-7)
	* [3.2.2.4.3.8.    Duplicate Static Analysis Job](#3-2-2-4-3-8)
	* [3.2.2.4.3.9.    Delete Static Analysis Job](#3-2-2-4-3-9)
	* [3.2.2.4.4. Test Job](#3-2-2-4-4)
	* [3.2.2.4.4.1.    Create Test Job](#3-2-2-4-4-1)
	* [3.2.2.4.4.2.    Check/ Modify Test Job Configuration](#3-2-2-4-4-2)
	* [3.2.2.4.4.3.    Execute Test Job](#3-2-2-4-4-3)
	* [3.2.2.4.4.4.    Stop Test Job](#3-2-2-4-4-4)
	* [3.2.2.4.4.5.    Test Job Log/ History](#3-2-2-4-4-5)
	* [3.2.2.4.4.6.    Add Test Job](#3-2-2-4-4-6)
	* [3.2.2.4.4.7.    Test Job Quality Issue Results](#3-2-2-4-4-7)
	* [3.2.2.4.4.8.    Duplicate Test Job](#3-2-2-4-4-8)
	* [3.2.2.4.4.9.    Delete Test Job](#3-2-2-4-4-9)
	* [3.2.2.4.5. Deployment Job](#3-2-2-4-5)
	* [3.2.2.4.5.1.    Create Deployment Job](#3-2-2-4-5-1)
	* [3.2.2.4.5.2.    Check/ Modify Deployment Job Configuration](#3-2-2-4-5-2)
	* [3.2.2.4.5.3.    Execute Deployment Job](#3-2-2-4-5-3)
	* [3.2.2.4.5.4.    Stop Deployment Job](#3-2-2-4-5-4)
	* [3.2.2.4.5.5.    Deployment Job Log/ History](#3-2-2-4-5-5)
	* [3.2.2.4.5.6.    Rollback Deployment Job to Current Job](#3-2-2-4-5-6)
	* [3.2.2.4.5.7.    Add Deployment Job](#3-2-2-4-5-7)
	* [3.2.2.4.5.8.    Duplicate Deployment Job](#3-2-2-4-5-8)
	* [3.2.2.4.5.9.    Delete Deployment Job](#3-2-2-4-5-9)
	* [3.2.2.4.6. Arrange Job's Job](#3-2-2-4-6)
	* [3.2.2.4.7. Add New Job Group](#3-2-2-4-7)
	* [3.2.3.     Pipeline Management](#3-2-3)
	* [3.2.3.1.   Manage Kubernetes Information](#3-2-3-1)
	* [3.2.3.1.1. Register Kubernetes Information](#3-2-3-1-1)
	* [3.2.3.1.2. Modify Kubernetes Information](#3-2-3-1-2)
	* [3.2.3.2.   Track JOB Audit](#3-2-3-2)
	* [3.2.3.2.1. Check Tracked JOB Audit](#3-2-3-2-1)
	* [3.2.3.2.2. Check Searched JOB Audit](#3-2-3-2-2)
	* [3.2.4.     Quality Management](#3-2-4)
	* [3.2.4.1.   Quality Issue](#3-2-4-1)
	* [3.2.4.2.   Coding Rules](#3-2-4-2)
	* [3.2.4.3.   Quality Profile](#3-2-4-3)
	* [3.2.4.3.1. Create Quality Profile](#3-2-4-3-1)
	* [3.2.4.3.2. Duplicate Quality Profile](#3-2-4-3-2)
	* [3.2.4.3.3. Modify Quality Profile](#3-2-4-3-3)
	* [3.2.4.3.4. Connect Quality Profile Project](#3-2-4-3-4)
	* [3.2.4.3.5. Delete Quality Profile](#3-2-4-3-5)
	* [3.2.4.4.   Quality Gate](#3-2-4-4)
	* [3.2.4.4.1. Create Quality Gate](#3-2-4-4-1)
	* [3.2.4.4.2. Duplicate Quality Gate](#3-2-4-4-2)
	* [3.2.4.4.3. Modify Quality Gate](#3-2-4-4-3)
	* [3.2.4.4.4. Add Quality Gate Condition](#3-2-4-4-4)
	* [3.2.4.4.5. Connect Quality Gate Project](#3-2-4-4-5)
	* [3.2.4.4.6. Delete Quality Gate](#3-2-4-4-6)
	* [3.2.4.5.   Manage Staging](#3-2-4-5)
	* [3.2.4.5.1. Environment Information Management](#3-2-4-5-1)
	* [3.2.4.5.1.1. Register Environment Information](#3-2-4-5-1-1)
	* [3.2.4.5.1.2. Search Environment Information List](#3-2-4-5-1-2)
	* [3.2.4.5.1.3. Delete Environment Information](#3-2-4-5-1-3)
	

# <div id='1'/> 1. Document Outline

## <div id='1-1'/> 1.1 Purpose
This document describes the use of the container platform pipeline service.

## <div id='1-2'/> 1.2 Range
This document is a usage guide about how users will use the Container Platform Pipeline service based on their Windows environment.

# <div id='2'/> 2. Container Platform Pipeline Access

## <div id='2-1'/> 2.1 Service Container Platform Pipeline Access
1. On the space page of the PaaS-TA portal, click the "dashboard" button on the applied distribution pipeline to proceed with access.

2. Check the Container Platform Pipeline Access.
![image](https://user-images.githubusercontent.com/80228983/146734921-c3bb22aa-40b2-4d1a-91ad-4b6361d9e379.png)

3. At this time, the user who ***first created the container platform deployment pipeline service becomes the administrator.*** Click the User Management menu in the upper right corner to go to the user dashboard.

## <div id='2-2'/> 2.2 Stand-Alone Deployment Container Platform Pipeline
1. The Public IP of the deployed cluster is accessed from a web browser at  http://{K8S_MASTER_NODE_IP}:30084.

2. Enter account information at the key cloak login page. (Initial administrator's account: admin / admin )
![image](https://user-images.githubusercontent.com/80228983/146735412-c7b1e66b-c8a1-4f37-8d79-182a666a318d.png)

3. Check the Container Platform Pipeline Access.
![image](https://user-images.githubusercontent.com/80228983/146734921-c3bb22aa-40b2-4d1a-91ad-4b6361d9e379.png)


# <div id='3'/> 3. Container Platform Pipeline User Manual
This chapter describes the container platform pipeline's menu configuration and screen description.

## <div id='3-1'/> 3.1 Container Platform Pipeline User Menu Configuration
The Container Platform Pipeline service consists of parts that retrieve and manage pipelines, parts that manage Kubernetes cluster access information, parts that monitor the quality of source code, parts that manage the settings of deployed applications, and parts that manage users according to their authorities.

***※ The User management, Pipeline management, and Quality management menu is not visible to the users.***
<table>
  <tr>
    <td>Menu</td>
    <td>Classification</td>
		<td>Description</td>
  </tr>
  <tr>
    <td>User Management</td>
    <td>User Dashboard</td>
	  <td>Checks and manages user information of the users who are using container platform pipeline</td>
  </tr>
  <tr>
		<td>Pipeline</td>
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
		<td>Staging Management</td>
		<td>Environment Information Management</td>
		<td>Manage configuration information for deployed applications/td>
  </tr>
</table>


## <div id='3-2'/> 3.2 Container Platform Pipeline User Menu Description
This chapter describes the four menus of the container platform deployment pipeline.

### <div id='3-2-1'/> 3.2.1. User Management
The user management menu is visible only to the manager, and this chapter describes the rights management and information check, and management of users using the container platform pipeline service.

#### <div id='3-2-1-1'/> 3.2.1.1. User Dashboard

##### <div id='3-2-1-1-1'/> 3.2.1.1.1. User List Search Check
1. Click the "User Management" button on the top right menu.
![image](https://user-images.githubusercontent.com/80228983/146738210-5a2c3a2a-fa13-40af-8b5d-dd865671a2d4.png)


2. Proceed to the user dashboard.
![image](https://user-images.githubusercontent.com/80228983/146738266-394f5cb2-aa91-408b-a499-9d2bcfdbfccb.png)

3. Two conditions can be searched in the user dashboard list: a search with a user ID as a search term and a search whose search type is an administrator/user. After entering the search term, do "ENTER" or click the "Magnifier" button.
![image](https://user-images.githubusercontent.com/80228983/146738410-2d7b3fdd-6319-407d-8515-d71bb76caba7.png)

![image](https://user-images.githubusercontent.com/80228983/146738490-72b48ccd-8258-45da-bd6c-064758daa5d1.png)

##### <div id='3-2-1-1-2'/> 3.2.1.1.2. View/ Modify User Details
1. Select an ID from the user dashboard to go to the user detail view/modify page.
![image](https://user-images.githubusercontent.com/80228983/146738595-22e7e2d3-c40e-46f8-816d-3d7fada97908.png)

2. Check the user's detailed information.
![image](https://user-images.githubusercontent.com/80228983/146738690-f1b61c35-06a9-466d-934c-0ed41c9b0f93.png)

3. When you want to modify the information, enter a new input value. Authorities can be selected as administrators/users.
![image](https://user-images.githubusercontent.com/80228983/146738750-8089a489-5ed7-4c0d-a86b-c42286f5d156.png)

4. After modifying the input value, click the "Modify" button.
![image](https://user-images.githubusercontent.com/80228983/146738791-baf6604a-b217-4e1d-9339-9df03dc5f6d5.png)

5. Check the modified information.
![image](https://user-images.githubusercontent.com/80228983/146738856-1c401eb3-2dd2-4257-90b4-5e8ac4bce32a.png)


### <div id='3-2-2'/> 3.2.2. Pipeline
This chapter describes the two pipeline managing methods.

***※ Basically, the user can only view the pipeline, and cannot create, query, modify, or delete it. However, if pipeline participant authorities are given, pipeline creation, inquiry, modification, and deletion are possible depending on the conditions.***

#### <div id='3-2-2-1'/> 3.2.2.1. My Pipeline
1.	Regardless of which page you are on, click the My Pipeline button in the Dropdown menu to go to the dashboard.
![image](https://user-images.githubusercontent.com/80228983/146739051-75da86a8-7e74-4a2c-abe6-ba54a446f20c.png)

2.	Click the pipeline name in the list in My Pipeline to go to the details page of that pipeline.
![image](https://user-images.githubusercontent.com/80228983/146739192-4dedce55-467d-4674-b45a-e6fabe00091c.png)

#### <div id='3-2-2-2'/> 3.2.2.2. Create New Pipeline
##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Create New Pipeline(1)
1. Click "Pipeline List" on the top menu to create a new pipeline immediately by simply entering the pipeline name.
![image](https://user-images.githubusercontent.com/80228983/146739350-09474dfa-085a-4147-88e5-5bcb986742a3.png)
2. After pressing the registration button, you can check the pipeline added to the dashboard.
![image](https://user-images.githubusercontent.com/80228983/146739409-f403fa35-cb11-47d0-b2fa-db2b40870fa8.png)
3. You can also check the pipeline registered in the Dropdown menu.
![image](https://user-images.githubusercontent.com/80228983/146739462-9b291eff-5939-4447-a9e5-aa3a879ed6d1.png)
##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Create New Pipeline(2)
1.	Click the "Create New" button in the upper right corner.
***※ “Create New” button is not enabled for users.***
![image](https://user-images.githubusercontent.com/80228983/146739665-3d0c2dc6-27ca-427e-9362-7f657af33ad0.png)
2.	Go to the Create New Pipeline page.
![image](https://user-images.githubusercontent.com/80228983/146739702-8e709946-6e2d-42e3-bb8e-b4abf8cf4a00.png)
3.	Pipeline names are required, so make sure to enter the pipeline name and click the "Create" button.
![image](https://user-images.githubusercontent.com/80228983/146739772-8fd5e45c-51a9-4b64-8f24-b6096fb4a297.png)
4.	After it is created, check the pipeline dashboard for the newly added pipeline name.
![image](https://user-images.githubusercontent.com/80228983/146739814-8c37e6e5-df08-43ad-9fb6-c4aaa34b26d0.png)

##### <div id='3-2-2-2-3'/> 3.2.2.2.3. Create New Pipeline(3)
***※ Users cannot go to the pipeline detail page unless they are given participant authority to the pipeline.***
1. Click the "Create New" button in the upper right corner.
![image](https://user-images.githubusercontent.com/80228983/146739909-be7e283a-3b56-4016-a686-33fdb0e750b3.png)
2. Go to the Create New Pipeline page.
![image](https://user-images.githubusercontent.com/80228983/146740201-e2ede42c-2562-48b2-9a6b-15b15f70d80f.png)
3. Refer to 3.2.2.2.2. Create New Pipeline(2) for next procedures.

#### <div id='3-2-2-3'/> 3.2.2.3. Pipeline Dashboard
##### <div id='3-2-2-3-1'/> 3.2.2.3.1. Check Pipeline List Search
1. The conditions that can be retrieved from the list are pipeline name and two filters in order of name/date/update.
![image](https://user-images.githubusercontent.com/80228983/146740286-02497cca-7db5-4677-b02a-738cda839d84.png)
2.	After entering the search term, do "ENTER" or click the "Magnifier" button.
![image](https://user-images.githubusercontent.com/80228983/146740392-c9f289d1-b88c-4c3b-a5e9-9c31fa8cf2d6.png)
3.	When it is a list query/search list query, it is immediately reflected by selecting a search type filter.
![image](https://user-images.githubusercontent.com/80228983/146740529-7519a68b-afe5-48fe-af99-651003045b05.png)

##### <div id='3-2-2-3-2'/> 3.2.2.3.2. Check Pipeline Detailed Information
1.	On the pipeline dashboard, click the pipeline name to go to the pipeline details page.
![image](https://user-images.githubusercontent.com/80228983/146740755-66f7f2d0-b599-4f57-a743-38254d25d5a4.png)
2.	On the Pipeline Details page, click the "View/Modify Information" button in the upper right corner.
![image](https://user-images.githubusercontent.com/80228983/146740783-840e90ce-f18a-49e5-8b52-7c2dc1a4c368.png)
3.	Go to the View/Modify Pipeline Information page and check the information of the pipeline in detail.
![image](https://user-images.githubusercontent.com/80228983/146741363-0d4f24f4-3438-4e8c-bf6d-348654939adc.png)


##### <div id='3-2-2-3-3'/> 3.2.2.3.3. Modify Pipeline
1.	On the View/Modify Pipeline Information page, modify the input value and click the "Modify" button.
![image](https://user-images.githubusercontent.com/80228983/146741421-f9545a4a-d5c1-408f-b6d4-1a58cea22e69.png)

2.	Once modified, go to the Pipeline Details page. Click the "View/Modify information" button again to check the changed value.
![image](https://user-images.githubusercontent.com/80228983/146744408-0e19583e-1ddd-4aa3-bd0b-c8d2345b1011.png)

##### <div id='3-2-2-3-4'/> 3.2.2.3.4. Delete Pipeline
1.	On the View/Modify Pipeline Information page, click the "Delete Pipeline" button.
![image](https://user-images.githubusercontent.com/80228983/146744457-34c04434-9812-4519-9931-580d11cc2587.png)
2.	After deletion, go to the pipeline dashboard and check if the pipeline has been deleted.
![image](https://user-images.githubusercontent.com/80228983/146744503-b40ffdeb-f358-456f-a9d3-46feab5de4ea.png)


#### <div id='3-2-2-4'/> 3.2.2.4. Pipeline Details
This chapter describes overall participant management, such as adding, modifying, deleting, and authorizing participants participating in the pipeline, and how to create, execute, delete, log/history inquiry, and manage participants for one pipeline.
##### <div id='3-2-2-4-1'/> 3.2.2.4.1. Manage Participants
###### <div id='3-2-2-4-1-1'/> 3.2.2.4.1.1. Add Participant
***※	Administrators are pipeline creators, so they are participants with creation authority by default.***
1.	On the Pipeline Details page, select the Participants tab.
![image](https://user-images.githubusercontent.com/80228983/146744564-3b3fbc77-dcf7-48fb-b538-a524358ed59f.png)
2.	Click the "Add Participant" button in the upper right corner.
![image](https://user-images.githubusercontent.com/80228983/146744613-c464dc22-5d97-437e-ba74-bbe26206cc84.png)
3.	Go to the Add Participants page to search for and select users who are currently invited to the deployment pipeline. Select one of the authorities to view, create, or execute and click the "Add" button.
![image](https://user-images.githubusercontent.com/80228983/146744679-ea86e8b6-821e-4889-ae05-2ae2cd8a0e42.png)
4.	Verify the added participant from the participants tab.
![image](https://user-images.githubusercontent.com/80228983/146744724-fd9e2e5e-f817-4dfb-a4b3-05ba962f8b81.png)

###### <div id='3-2-2-4-1-2'/> 3.2.2.4.1.2. Check Participant List Search
1.	The condition that can be searched in the participant list is only participant ID search.
![image](https://user-images.githubusercontent.com/80228983/146744773-9afd316f-4516-4c57-9a53-5bfff55f66ec.png)
2.	After entering the search term, do "ENTER" or click the "magnifier" button and search.
![image](https://user-images.githubusercontent.com/80228983/146744836-61c87dc7-3357-452a-b2e1-de1e29d291ae.png)

###### <div id='3-2-2-4-1-3'/> 3.2.2.4.1.3.	Check/ Modify Participant Detailed Information
1.	Select the participant ID to query for information from the participant list.
![image](https://user-images.githubusercontent.com/80228983/146745046-fe208791-1399-4f99-8e19-25da48c07f72.png)
2.	Go to the Participant Details Check/Modify page and check the participant's details.
![image](https://user-images.githubusercontent.com/80228983/146745108-2a629019-8b4d-42c9-948d-4e2209c67500.png)
3.	Select the authority you want to modify and click the "Modify" button.
![image](https://user-images.githubusercontent.com/80228983/146745164-9863d2aa-bea7-4a28-9fa9-b3cd7f08681e.png)
4.	Check the modified participant authority in the Participant List.
![image](https://user-images.githubusercontent.com/80228983/146745207-ad8372c4-9513-4ccd-a729-a21de9215758.png)


###### <div id='3-2-2-4-1-4'/> 3.2.2.4.1.4.	Delete Participant
1.	Select the participant ID to delete from the participants list.
![image](https://user-images.githubusercontent.com/80228983/146745268-723a4048-1fc3-46d4-9e8a-9a447ddfbd0c.png)
2.	Go to the Check/Modify Participant Details page.
![image](https://user-images.githubusercontent.com/80228983/146745316-68d3758b-ae46-4866-9055-e52e324c8886.png)
3.	Click the “Delete Participant” button.
![image](https://user-images.githubusercontent.com/80228983/146745342-0dcb9159-bee3-4a99-b236-7fb8103b4347.png)
4.	Confirm that the participant has been deleted from the participant list.
![image](https://user-images.githubusercontent.com/80228983/146745391-cc7aa6b6-3b8b-4a92-bef8-997a0f270c1c.png)


##### <div id='3-2-2-4-2'/> 3.2.2.4.2. Build Job
###### <div id='3-2-2-4-2-1'/> 3.2.2.4.2.1. Create Build Job
1.	On the Pipeline Details page, click the 'Click here to add a new task' button.
![image](https://user-images.githubusercontent.com/80228983/146745481-b70f5459-54e4-4c6f-863a-5093ef546011.png)
2.	Go to the Configuration Details page.
![image](https://user-images.githubusercontent.com/80228983/146745559-1148971f-f12f-48e6-8a9a-7a67aedfa6f1.png)
3. Select a job type and select it as a build type. After that, enter the image name to be saved in Harbor. <br>
![image](https://user-images.githubusercontent.com/80228983/146745847-993cd35d-5c9c-41f6-ba7f-c43a901b0c2a.png)

***※ If Gradle is selected, reflect the source build and test. For static analysis, reflect the Gradle wrapper settings.***

4. Click the Create Docker File button.
![image](https://user-images.githubusercontent.com/80228983/146745889-fca0a935-c9fe-4745-b10f-132b7af0a775.png)

5. If you click a name suitable for the builder type in the staging information inquiry, for example, information on how to set the Spring Config Client is checked.
![image](https://user-images.githubusercontent.com/80228983/146876710-95db595c-2d71-41f6-bb1b-d62da91eb0f1.png)
![image](https://user-images.githubusercontent.com/80228983/146877968-5fc84abf-f882-46a2-8f28-77f846052031.png)

6. After entering the shape management information, click the Branch Lookup button.
![image](https://user-images.githubusercontent.com/80228983/146746751-8d825ad5-caa4-485f-91f9-72adc15fb9b8.png)

***※ When you select Github, you do not need to enter your ID and password when inquiring about the branch if you enter the public repository path.***

7. Select the branch you want and click the save button.
![image](https://user-images.githubusercontent.com/80228983/146750384-d397253f-225d-4241-bbf1-72351a7f5486.png)


***※ Build Job creation can only be created by participants with creation authority among administrators and pipeline participants.***


###### <div id='3-2-2-4-2-2'/> 3.2.2.4.2.2. Check/ Modify Build Job Configuration
1. Click the "Configuration" icon for the created build job.<br>
![image](https://user-images.githubusercontent.com/80228983/146850081-a3331972-8b8a-4c41-9c8f-fe258d34bf8d.png)
2. Go to the configuration detail page and check the configuration information stored at the time of creation.
![image](https://user-images.githubusercontent.com/80228983/146850120-cee00d5f-c539-4bbf-914d-d14bf559c5f2.png)
3. When modifying, re-enter the information you want to modify in each input form and click the "Save" button (in the example, modify Dockerfile)
![image](https://user-images.githubusercontent.com/80228983/146850268-67058b4b-7c88-4911-a686-8d37aabeec61.png)

***※ When modifying the build job, the shape management information cannot be modified except for the branch.***<br>
4. Go to the configuration detail page and check the modified information.
![image](https://user-images.githubusercontent.com/80228983/146850355-91325e6d-4b7f-4a07-9a7b-0d1c992981f8.png)

***※ All pipeline participants can check the build job configuration. However, modifications can only be made by managers and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-3'/> 3.2.2.4.2.3. Execute Build Job
1. Click the "Run" icon on the Pipeline Details page.<br>
![image](https://user-images.githubusercontent.com/80228983/146850397-8c68b631-d8fe-4449-8d62-24ba86cc6cf2.png)
2. You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
![image](https://user-images.githubusercontent.com/80228983/146850501-bf13c417-cf94-465c-8544-bf35beaece0f.png)
3. When the execution is completed, it turns green and is marked as Build in the task part.
![image](https://user-images.githubusercontent.com/80228983/146850568-30b068e6-b168-4fd8-b9a5-ea3d4178cd81.png)

***※	Build Job execution is only possible for administrators and pipeline participants with creation and execution authorities.***


###### <div id='3-2-2-4-2-4'/> 3.2.2.4.2.4. Stop Build Job
1.	Click the "Stop" icon when you want to stop and cancel a running build job.
![image](https://user-images.githubusercontent.com/80228983/146850630-62dafc69-f867-4b1f-b334-bdac4fad8387.png)
2.	It can be seen that the stopped build Job turns orange.
![image](https://user-images.githubusercontent.com/80228983/146850652-8e95625f-e35b-48ef-bd19-8c1d9f3be335.png)

***※	Stopping the build job is only possible for managers and pipeline participants with creation and execution authorities.***


###### <div id='3-2-2-4-2-5'/> 3.2.2.4.2.5.	Build Job Log/ History
1.	You can view logs in real-time by clicking the "log/history" icon when the build job is running.
![image](https://user-images.githubusercontent.com/80228983/146850705-e4479cf3-e470-4b07-883d-656855cd404a.png)
2.	Go to the log lookup page. Check that the log is visible in real-time.
![image](https://user-images.githubusercontent.com/80228983/146850741-c9f337fc-aedc-443c-a2dd-12373d4ac867.png)
3.	Verify that the build job execution is completed, and check the history.
![image](https://user-images.githubusercontent.com/80228983/146850789-b26d9402-472b-4cc5-8cb1-be28a4b6897a.png)
4.	You can cancel and run the Job by clicking the "Cancel" or "Run" button at the top of the Log/History page.
![image](https://user-images.githubusercontent.com/80228983/146850835-a94b0f16-a0a2-496c-9425-cca7e66cf84c.png)
![image](https://user-images.githubusercontent.com/80228983/146850845-0fecf61e-7799-4209-82e0-df735b1ad213.png)
5.	Click the "Configure" button on the Log/History page to go to the Build Job Configuration Inquiry page.
![image](https://user-images.githubusercontent.com/80228983/146850911-c6360a49-215c-438a-b075-46fb4123dcde.png)
6.	Click the "List" button on the Log/History page to go to the Pipeline Details page.
![image](https://user-images.githubusercontent.com/80228983/146851003-63628715-e4b0-406e-88a8-7e06aec2a8fe.png)

***※	The Build Job Log/History can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authorities.***

###### <div id='3-2-2-4-2-6'/> 3.2.2.4.2.6.	Add Build Job
1.	On the Pipeline Details page, click the "Add" button on the Build Job.
![image](https://user-images.githubusercontent.com/80228983/146851143-3e0902c7-5ade-45f8-bdc6-f1ff35076f62.png)
2.	Go to the configuration detail page, enter the appropriate values in the input form, such as creating a build job, and click the "Save" button. (You can select a test and deployment type for the task type in addition to the build.)
![image](https://user-images.githubusercontent.com/80228983/146851274-16f0fcd6-6282-44e7-a09a-f51d015d35b1.png)
3.	Check the added build Job.<br>
![image](https://user-images.githubusercontent.com/80228983/146851391-d452fb78-32ac-4561-95ea-545e5194ccbf.png)
4.	When you check the configuration of this task (Job) as a new workgroup in the task trigger, the Job is added as a new group. (We will discuss it later in the topic Adding a new workgroup.)

***※	Adding a build job is only available for administrators and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-7'/> 3.2.2.4.2.7.	Duplicate Build Job
1.	On the Pipeline Details page, click the "Duplicate" button on the Build Job.
![image](https://user-images.githubusercontent.com/80228983/146851489-6c7f2590-cf73-4be1-9cbe-45cc694796c7.png)
2. Check the duplicated Build Job.
![image](https://user-images.githubusercontent.com/80228983/146851531-7e56baf5-2a5b-4abb-a0f1-7defa5fccf0b.png)

***※	Build Job duplication is only possible for administrators and pipeline participants with creation authorities.***

###### <div id='3-2-2-4-2-8'/> 3.2.2.4.2.8.	Delete Build Job
1.	On the Pipeline Details page, click the "Delete" button on the Build Job.
![image](https://user-images.githubusercontent.com/80228983/146851560-96146948-a220-4dff-b59d-e122a89ebdd5.png)
2.	Verify the deleted Build Job.
![image](https://user-images.githubusercontent.com/80228983/146851577-ba273df2-d952-42c0-9dad-4d44598371fc.png)

***※	Delete Build Job is only possible for administrators and pipeline participants with creation authority.***


##### <div id='3-2-2-4-3'/> 3.2.2.4.3. Static Analysis Job
###### <div id='3-2-2-4-3-1'/> 3.2.2.4.3.1. Create Static Analysis Job
1.	Click the "Add" button on the Job.
![image](https://user-images.githubusercontent.com/80228983/146851671-71848dd9-7a21-4202-a5d3-d3f6cd37ced0.png)
2.	Go to the configuration page, select the task type as Static-Analysis, and select the desired quality profile, quality gate, and workgroup from the input type. After that, the action trigger is selected according to each situation. (Initially, no quality gate exists. Create by referring to [Create Quality Gate](#3-2-4-4-1).)
![image](https://user-images.githubusercontent.com/80228983/146852468-9f9620b6-280a-4946-9c69-eaa75e647f4d.png) 
3.	Click GRADLE and MAVEN on the JACO plugins script tab to see how to apply jacoco plugins (for example, apply the plugins according to the situation)
![image](https://user-images.githubusercontent.com/80228983/146852604-7539eefd-f3d0-435f-8248-f8ae534d071e.png)
4.	Click the "Save" button and confirm that the test job has been created on the pipeline detail page.
![image](https://user-images.githubusercontent.com/80228983/146852642-91708e7f-6fa9-4ec9-8f18-ba97bbfe6e6e.png)

***※	Static analysis Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-3-2'/> 3.2.2.4.3.2. Check/ Modify Static Analysis Job Configuration
1.	Click the "Configuration" icon of the generated Static Analysis Job.
![image](https://user-images.githubusercontent.com/80228983/146852683-cae485a0-a992-43f8-9355-94aac7736b11.png)
2.	Go to the configuration detail page and check the configuration information stored at the time of creation.
![image](https://user-images.githubusercontent.com/80228983/146852728-f1824c6f-db5c-4265-9979-65bfaf632e05.png)
3.	When modifying, re-enter the information to be modified in each input form and click the "Save" button.
![image](https://user-images.githubusercontent.com/80228983/146853294-af315b80-4ce4-40ae-8d6a-da33f7e8ba45.png)
4.	Go to the configuration detail page and check the modified information.
![image](https://user-images.githubusercontent.com/80228983/146853332-42795e48-c5dc-40f6-bb95-b7bf15db53cb.png)
![image](https://user-images.githubusercontent.com/80228983/146853360-16b00830-b127-4cb7-87a2-aab4cde1602e.png)

***※	Static analysis Job composition inquiry can be checked by all pipeline participants. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-3'/> 3.2.2.4.3.3.	Execute Static Analysis Job
1.	On the Pipeline Details page, click the "Run" icon of the Static Analysis Job.
![image](https://user-images.githubusercontent.com/80228983/146853405-4be9ac3d-a04a-4e77-994f-57826271ee1a.png)
2.	You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
![image](https://user-images.githubusercontent.com/80228983/146853428-0ec56965-6b93-4e7a-b2ed-7eb0391c1e5d.png)
3. When the execution is completed, it turns green and is displayed as Test(Execution Completed) in the task part.

***※	 Job execution is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-3-4'/> 3.2.2.4.3.4.	Stop Static Analysis Job
1.	Click the "Stop" icon when you want to stop and cancel a static analysis Job that is running.
![image](https://user-images.githubusercontent.com/80228983/146853481-0f5823a7-d29e-4e9c-acd8-4e0a89e0e2d7.png)
2.	Stopped Static Analysis Job is turned to orange
![image](https://user-images.githubusercontent.com/80228983/146853489-13d585dc-da3c-4eb4-b6a8-af4d3c72ccd4.png)

***※	 Job suspension is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-3-5'/> 3.2.2.4.3.5.	Static Analysis Job Log/ History
1.	When a static analysis job is in progress, you can view the log in real time by clicking the "log/history" icon.
![image](https://user-images.githubusercontent.com/80228983/146853578-293557f5-9e3a-4b57-b714-aee1d4012e8d.png)
2.	Go to the log lookup page. Check that the log is visible in real time.
![image](https://user-images.githubusercontent.com/80228983/146853606-1ee8a3ef-3ad7-47c8-9a94-b5f14baef299.png)
3.	Confirm that the static analysis job execution is completed, and check the history.
![image](https://user-images.githubusercontent.com/80228983/146853695-5b4d7fc9-ca68-4b70-a5bd-b40f3fe7f709.png)
4.	For the "Execute", "Cancel", "Configure", and "List" buttons, see 3.2.4.2.5. Build Job Log/History.

***※	Job logs/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons are only available to participants with creation and execution authority.***


###### <div id='3-2-2-4-3-6'/> 3.2.2.4.3.6.	Static Analysis Job Job Quality Issue Results
1.	When you press the Log/History "Quality Issue Results" button in the Test Job, it goes to the Quality Management Dashboard that manages the error resolution, level of error, and activation status of the source code performed.
![image](https://user-images.githubusercontent.com/80228983/146853748-363d2706-87ba-4724-ac6f-015075c05740.png)
2.	Refer to 3.2.4 Quality Management Part for Quality Management Dashboard.

***※	 The results of the Job quality issue can be checked by the manager and all pipeline participants.***

###### <div id='3-2-2-4-3-7'/> 3.2.2.4.3.7.	 Add Static Analysis Job
1.	On the Pipeline Details page, click the "Add" button of the Static Analysis Job.
![image](https://user-images.githubusercontent.com/80228983/146854048-a807b104-c7fd-41e1-916d-3ae4bedf67ac.png)
2.	Refer to 3.2.2.4.2.7. Add Build Job part for the procedures afterward.

***※	Static analysis jobs can only be added by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-8'/> 3.2.2.4.3.8.	Duplicate Static Analysis Job
1.	On the Pipeline Details page, click the "Duplicate" button on the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146854081-eb5c227c-9f06-46ab-9c09-18e11be46647.png)
2.	Verify that the static analysis Job is duplicated.
![image](https://user-images.githubusercontent.com/80228983/146854105-c13124bf-c7a3-43cb-a120-8be167d6715e.png)

***※	Static analysis Job Duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-3-9'/> 3.2.2.4.3.9.	Delete Static Analysis Job
1.	On the Pipeline Details page, click the "Delete" button on the Static Analysis Job.
![image](https://user-images.githubusercontent.com/80228983/146854183-3f69d84d-9553-4870-a5dd-7cbb9cf2890f.png)
2.	Confirm that the static analysis Job has been deleted.
![image](https://user-images.githubusercontent.com/80228983/146854202-4204c295-514a-4a4e-bc97-d5e06db1665b.png)

***※	 Job deletion is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-4'/> 3.2.2.4.4. Test Job
###### <div id='3-2-2-4-4-1'/> 3.2.2.4.4.1. Create Test Job
1.	Click the "Add" button on the Job.
![image](https://user-images.githubusercontent.com/80228983/146854258-a09a0a78-765d-4d40-acf8-856da17bdf33.png)
2.	Go to the configuration page, select the task type as JUnit-Test, and select the input type to test. After that, the action trigger is selected according to each situation.
![image](https://user-images.githubusercontent.com/80228983/146854434-072106ab-6050-493f-917c-c93dd30bcf46.png)
3.	Click the "Save" button and confirm that the test job has been created on the pipeline detail page.
![image](https://user-images.githubusercontent.com/80228983/146854467-e84b2ab1-2f5d-4e5a-ad1b-735368c8fe3f.png)

***※	Test Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-4-2'/> 3.2.2.4.4.2. Check/ Modify Test Job Configuration
1.	Click the “Configure” icon from the created Test Job.
![image](https://user-images.githubusercontent.com/80228983/146854497-54981906-68a4-43e3-aa0f-b7fe0d761084.png)
2.	Go to the configuration detail page and check the configuration information stored at the time of creation.
3.	When modifying, re-enter the information to be modified in each input form and click the "Save" button.
![image](https://user-images.githubusercontent.com/80228983/146854560-f4a6c6b3-53be-4fcb-8251-890bd517c47a.png)
4.	Check the modified information by proceeding to the configuration details page.
![image](https://user-images.githubusercontent.com/80228983/146854581-3054e420-18a9-4caa-8f26-1fdce8ba34ca.png)

***※	All pipeline participants can check the configuration of the test job. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-4-3'/> 3.2.2.4.4.3.	Execute Test Job
1.	On the Pipeline Details page, click the "Run" icon in the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146857738-21d46e4c-4679-4373-8be8-31b4aa583439.png)
2.	You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
![image](https://user-images.githubusercontent.com/80228983/146857766-88d020e8-f41c-4f5e-aac2-e8564c2ed237.png)
3. When the execution is completed, it turns green and is displayed as Test(Execution Completed) in the task part.

***※	Test Job execution is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-4-4'/> 3.2.2.4.4.4.	Stop Test Job
1.	Click the "Stop" icon when you want to stop and cancel a running test Job.
![image](https://user-images.githubusercontent.com/80228983/146857820-1b9d8493-aa68-4d5b-9659-590e642c392e.png)
2.	You can see that the stopped build Job turned orange.
![image](https://user-images.githubusercontent.com/80228983/146857870-1e7a427d-50ad-41ce-9a17-d0c456048006.png)

***※	Test Job stop is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-4-5'/> 3.2.2.4.4.5.	Test Job Log/ History
1.	You can view logs in real time by clicking the "log/history" icon when the build job is running.
![image](https://user-images.githubusercontent.com/80228983/146857902-4bba7e92-2da2-48b0-86eb-5fd4314262b4.png)
2.	Go to the log lookup page. Check that the log is visible in real time.
![image](https://user-images.githubusercontent.com/80228983/146857935-14f830cb-f40b-4692-8b5e-fe510d24453f.png)
3.	Confirm that the test job execution is completed, and check the history.
![image](https://user-images.githubusercontent.com/80228983/146857996-3ceb188e-e440-4fb6-94c6-7a271a7fec45.png)
4.	For the "Execute", "Cancel", "Configure", and "List" buttons, see 3.2.4.2.5. Build Job Log/History.

***※	The test Job log/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authority.***


###### <div id='3-2-2-4-4-6'/> 3.2.2.4.4.6.	Test Job Quality Issue Results
1.	Pressing the Log/History "Test Results" button on the Test Job will bring up the JUnit Test Results pop-up window of the source code performed.
![image](https://user-images.githubusercontent.com/80228983/146858037-941cd966-1375-48ca-81a6-8d3d02e4244d.png)
![image](https://user-images.githubusercontent.com/80228983/146858111-9b25dab5-85e0-4e43-af33-3aee82095eb4.png)

***※	The results of the test Job quality issue can be checked by the manager and all pipeline participants.***

###### <div id='3-2-2-4-4-7'/> 3.2.2.4.4.7.	Add Test Job
1.	On the Pipeline Details page, click the "Add" button on the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146858210-30863f47-0e41-43ba-bd3b-4df06e93aa81.png)
2.	Refer to 3.2.2.4.2.6. Add Build Job for the next procedures.

***※	Only administrators and pipeline participants with creation authority can add test jobs.***

###### <div id='3-2-2-4-4-8'/> 3.2.2.4.4.8.	Duplicate Test Job
1.	On the Pipeline Details page, click the "Deplicate" button on the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146858286-38b6cff2-79be-4908-a68d-9c107e18b32e.png)
2.	Refer to 3.2.2.4.2.7 Duplicate Build Job part for the next procedures.


***※	Test Job duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-4-9'/> 3.2.2.4.4.9.	Delete Test Job
1.	On the Pipeline Details page, click the "Delete" button on the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146858302-223245b6-dd0a-4012-a31b-46761c075b70.png)
2.	Refer to 3.2.2.4.2.8 Delete Build Job part for the next procedures.


***※	Deleting test jobs is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-5'/> 3.2.2.4.5.	Deployment Job
###### <div id='3-2-2-4-5-1'/> 3.2.2.4.5.1.	Create Deployment Job
1. Click the "Add" button on the Job.<br>
![image](https://user-images.githubusercontent.com/80228983/146858411-96203d4e-86db-45cb-b4eb-69d8839beaa6.png)
2.	Go to the configuration page, select the task type as Deploy, select the desired deployment type and app type from Type, and select the saved Kubernetes information in Pipeline Management. (In order to get Kubernetes information, a prior process is required. Refer to 3.2.3.1. Kubernetes Information Management).
***※	The format of DEPLOYMENT.YML and DEPLOYMENT_SERVICE.YML varies depending on the type selected (type), app type.***
![image](https://user-images.githubusercontent.com/80228983/146858644-8c03afea-aa2f-4eca-8833-dbfd624990bc.png)
3.	Change DEPLOYMENT.YML and DEPLOYMENT_SERVICE.YML to match app settings.
![image](https://user-images.githubusercontent.com/80228983/146859081-15639ae3-80c8-4ad7-b642-2477d1262c03.png)
***※	%{VALUE}% values such as %INTERNAL_SERVICE_PORT% inside the YML file must be changed.***
4.	Click the "Save" button and confirm that the deployment job has been created on the pipeline detail page.
![image](https://user-images.githubusercontent.com/80228983/146859103-c8cd5ec4-a72b-4250-96e4-3ffb214baf55.png)

***※	Deployment Job creation can only be created by participants with creation authority among managers and pipeline participants.***

###### <div id='3-2-2-4-5-2'/> 3.2.2.4.5.2.	Check/ Modify Deployment Job Configuration
1.	Click the "Configuration" icon of the created deployment job.
![image](https://user-images.githubusercontent.com/80228983/146859159-6a32b287-ccdb-4278-b070-6db2d4900346.png)
2.	Go to the configuration detail page and check the configuration information stored at the time of creation.
![image](https://user-images.githubusercontent.com/80228983/146859182-f382647e-b7d2-4d6c-8c39-5fbcbfc1536a.png)
3.	When modifying, re-enter the information to be modified in each input YML and click the "Save" button.
![image](https://user-images.githubusercontent.com/80228983/146859270-94d0e125-f790-485b-9515-710673d5bf66.png)
4.	Go to the configuration detail page and check the modified information.
![image](https://user-images.githubusercontent.com/80228983/146859308-6871dc46-49b1-423e-b3c5-a3f5956b6e15.png)


***※	 Deployment Job configuration check can be made by all pipeline participants. However, modifications can only be made by managers and pipeline participants with creation authority.***

###### <div id='3-2-2-4-5-3'/> 3.2.2.4.5.3.	Execute Deployment Job
1.	On the Pipeline Details page, click the "Run" icon in the Deployment Job.
![image](https://user-images.githubusercontent.com/80228983/146859343-b0ec964d-ba01-40e7-a73d-ce2703e3b2a1.png)
2.	You can see that it turns blue and blinks when it is executed. (You can view logs in real-time by clicking the "log/history" icon while running.)
![image](https://user-images.githubusercontent.com/80228983/146859370-cacabc1a-1bfd-4fb0-bbfe-dfc355c1057c.png)
3. When the execution is complete, it turns green and appears as Deploy(Execution Completed) in the task section.


***※	Deployment Job execution is only possible for administrators and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-5-4'/> 3.2.2.4.5.4.	Stop Deployment Job
1.	Click the "Stop" icon when you want to stop and cancel a running deployment job.
![image](https://user-images.githubusercontent.com/80228983/146859436-8034f833-8cc9-4b5d-956d-6a96bab44df1.png)
2.	It can be seen that the stopped deployment Job turns orange.
![image](https://user-images.githubusercontent.com/80228983/146859444-3933f6c3-b707-43fa-80c2-84c8d4ad1213.png)

***※	The stopping of deployment job is only possible for managers and pipeline participants with creation and execution authority.***

###### <div id='3-2-2-4-5-5'/> 3.2.2.4.5.5.	Deployment Job Log/ History
1.	When the deployment job is running, you can view the log in real-time by clicking the "log/history" icon.
![image](https://user-images.githubusercontent.com/80228983/146859484-9ef181ed-5612-4206-87ca-6b6df4f2343a.png)
2.	Go to the log lookup page. Check that the log is visible in real-time.
3.	Confirm that the deployment job execution is completed, and check the history.
![image](https://user-images.githubusercontent.com/80228983/146859513-36f03f0d-aeaa-428b-8ff7-5414488f7959.png)
4.	As a result of inquiring on the PaaS-TA container platform portal, it can be confirmed that an application called 'spring-music-test' was deployed in the deployment section.
![image](https://user-images.githubusercontent.com/80228983/146859752-60cc0195-164d-4681-96a7-ee57c948bac8.png)

***※	The deployment Job log/history can be viewed by the administrator and all pipeline participants, but the Run and Stop buttons can only be viewed by participants with creation and execution authority.***

###### <div id='3-2-2-4-5-6'/> 3.2.2.4.5.6.	Rollback Deployment Job to Current Job
1.	The "Rollback to Current Job" button on the Log/History page of the Deployment Job is not currently supported.
![image](https://user-images.githubusercontent.com/80228983/146859913-eaf78822-3a4e-4bf0-9f3b-034e293ac273.png)

###### <div id='3-2-2-4-5-7'/> 3.2.2.4.5.7.	Add Deployment Job
1.	On the Pipeline Details page, click the "Add" button on the Test Job.
![image](https://user-images.githubusercontent.com/80228983/146859973-75e5dc02-d29a-4b2e-8c72-1526c20025ff.png)
2.	Refer to 3.2.2.4.2.6. Add Build Job part for the next procedures.

***※	Only administrators and pipeline participants with creation authority can add deployment jobs.***

###### <div id='3-2-2-4-5-8'/> 3.2.2.4.5.8.	Duplicate Deployment Job
1.	On the Pipeline Details page, click the "Duplicate" button on the Deployment Job.
![image](https://user-images.githubusercontent.com/80228983/146860020-4ead6b2e-4313-4946-989e-2be6c75df8d2.png)
2.	Refer to 3.2.2.4.2.7. Duplicate Build Job part for the next procedures.

***※	Deployment Job duplication is only possible for administrators and pipeline participants with creation authority.***

###### <div id='3-2-2-4-5-9'/> 3.2.2.4.5.9.	Delete Deployment Job
1.	On the Pipeline Details page, click the "Delete" button on the Deployment Job.
![image](https://user-images.githubusercontent.com/80228983/146860050-1d04e411-18b2-4412-a7a9-62970bdef9eb.png)
2.	Refer to 3.2.2.4.2.8. Delete Build Job part for the next procedure.


***※	Deleting a deployment job is only possible for administrators and pipeline participants with creation authority.***

##### <div id='3-2-2-4-6'/> 3.2.2.4.6. Arrange Job's Job
1.	On the Pipeline Details page, click the "Sort Job" icon for each Job.
![image](https://user-images.githubusercontent.com/80228983/146860164-f447fbaa-1f6b-491d-a8b8-ad7a109125d2.png)
2.	A list of the remaining Job numbers that can be sorted within the current workgroup appears in the drop-down menu.
![image](https://user-images.githubusercontent.com/80228983/146860179-bfda5a8b-ca6d-48a6-b77d-dee8d022c543.png)
3.	Among them, when you click the number you want to sort, the Job and location of the number are switched and sorted.
![image](https://user-images.githubusercontent.com/80228983/146867130-e39790ed-765c-4ec4-9668-04e356271c74.png)

***※	Job job sorting is only possible for managers and pipeline participants with creation authority.***

##### <div id='3-2-2-4-7'/> 3.2.2.4.7. Add New Job Group
1.	Click the "Add New Workgroup" button on the Pipeline Details page.
![image](https://user-images.githubusercontent.com/80228983/146867152-0477d45e-b5b5-4b64-8735-ee5ac4596880.png)
2.	Proceed to the Job Configuration Details Page.
![image](https://user-images.githubusercontent.com/80228983/146867201-e8622591-6e47-4f57-b3e9-162f47ee6529.png)
3.	Create a new Job and go to the Pipeline Details page. It is visible that a dotted line is created below the previous workgroup and the newly created Job is divided into the new group.
![image](https://user-images.githubusercontent.com/80228983/146867284-fdac0c1e-962b-49c4-a8ea-759b99b9cc32.png)

***※	Adding a new workgroup is only available for administrators and pipeline participants with creation authority.***

### <div id='3-2-3'/> 3.2.3.Pipeline Management
#### <div id='3-2-3-1'/> 3.2.3.1. Manage Kubernetes Information
###### <div id='3-2-3-1-1'/> 3.2.3.1.1. Register Kubernetes Information
1. Click Pipeline Management > Kubernetes Information Management Menu.
![image](https://user-images.githubusercontent.com/80228983/146867460-8d62380a-8c55-4b41-826c-0237c49aee56.png)
2. Click the "Register Kuber Config" button in the upper right corner of the dashboard.
![image](https://user-images.githubusercontent.com/80228983/146867477-b8c5ff71-29c9-4d91-9e1c-8f55b42222db.png)
3. Proceed to Register kubernetes config page.
![image](https://user-images.githubusercontent.com/80228983/146867522-fec558d7-8caf-4639-aad0-dcabe9ad565e.png)
4. Enter the config name and kube config and click the Register button.
![image](https://user-images.githubusercontent.com/80228983/146867697-4ba6e049-35b1-4192-81a5-13e5d8511b12.png)
5. Check the registered kubernetes config.<br>
![image](https://user-images.githubusercontent.com/80228983/146867770-d5515ac3-8701-4c7d-a1f9-a8d37dbd24f9.png)

###### <div id='3-2-3-1-2'/> 3.2.3.1.2. Modify Kubernetes Information
1.In the kubernetes information management dashboard, click the information you want to modify.
![image](https://user-images.githubusercontent.com/80228983/146868073-7f58c5bf-2cd2-4d77-9de6-ebcd77bfcb34.png)
2.	Eneter the value to modify and click “Modify” button.
![image](https://user-images.githubusercontent.com/80228983/146868101-2e66c9c0-6aef-4425-9c68-79e88be3ab1f.png)
3.	Go back to the information detail page through the dashboard and check if it has been modified.

#### <div id='3-2-3-2'/> 3.2.3.2. Track JOB Audit
###### <div id='3-2-3-2-1'/> 3.2.3.2.1. Check Tracked JOB Audit
1.      Click Pipeline Management > JOB Audit Tracking Menu.
![image](https://user-images.githubusercontent.com/80228983/146868232-07e6f6bc-c4d3-45c6-a16c-4f0297796724.png)
2.      You can view the list of recently registered/executed/modified/deleted JOBs from the JOB Audit Tracking Dashboard.
![image](https://user-images.githubusercontent.com/80228983/146868328-1bdac61c-9e7d-4331-bea8-61ccb5e9657b.png)


###### <div id='3-2-3-2-2'/> 3.2.3.2.2.  Check Searched JOB Audit
1.	Enter the message you want to search on the JOB Audit Tracking Dashboard.
![image](https://user-images.githubusercontent.com/80228983/146868426-029fff3a-3eb2-46ab-9631-2cf8fd890fae.png)
2.	Only trace records that match the message searched in the JOB Audit Tracking page are checked.
![image](https://user-images.githubusercontent.com/80228983/146868447-a51aa764-5e56-4902-83e7-538ba3f60cec.png)


### <div id='3-2-4'/> 3.2.4. Quality Management
This chapter describes quality issues and coding rules, quality profiles, and quality gates about source codes inspected through a static analysis Job.
#### <div id='3-2-4-1'/> 3.2.4.1. Quality Issue
1.	Proceed to the Quality Issue Dashboard by clicking the quality issue on the Quality Management menu.
![image](https://user-images.githubusercontent.com/80228983/146868645-21adac37-a946-4dcb-8fce-b3d4e642b0f9.png)
2.      Click the "Details" button of the coding rule to view.
![image](https://user-images.githubusercontent.com/80228983/146868787-a3182e36-27f6-4925-9c24-4ca302195167.png)
3.	Click the "List" button in the upper right corner to go to the Quality Issues dashboard with the Job Test List.
![image](https://user-images.githubusercontent.com/80228983/146868854-58ede260-4324-4e70-8623-cb5378403a85.png)
<br>
![image](https://user-images.githubusercontent.com/80228983/146868893-612518cf-0f65-4b81-a184-0a1d2b4ac8e7.png)


#### <div id='3-2-4-2'/> 3.2.4.2. Coding Rules
1.	Proceed to the Coding Rules Dashboard by clicking the coding rules on the quality management menu.
![image](https://user-images.githubusercontent.com/80228983/146869100-bf9ced1d-6ac5-43d5-9772-dacb723c71bb.png)
2.	Click the "Add to Profile" button, set the issue level in the Add Profile pop-up window, and click the "Add" button.
![image](https://user-images.githubusercontent.com/80228983/146869126-57b894b2-3bb4-4dfc-a5f8-945e4bcee0dc.png) 
![image](https://user-images.githubusercontent.com/80228983/146869144-225b9a5b-8795-4eee-98c0-d8aea5c5095a.png) 

3. In the lower-left corner, click the default-paasta selected in the previous step in the Quality Profile menu to verify that the added coding rule has been added.
![image](https://user-images.githubusercontent.com/80228983/146869220-88df7154-a0e9-49a2-aed0-399ed89856d6.png)
4.	Click the "Remove from Profile" button.
![image](https://user-images.githubusercontent.com/80228983/146869258-5f0dd80d-4ca1-4edc-ab98-a168a2d04e0b.png)
5.	Confirm if the removed coding rule has been deleted from the quality profile.
![image](https://user-images.githubusercontent.com/80228983/146869296-2399267c-6685-40ee-9e94-e66875b63841.png)

#### <div id='3-2-4-3'/> 3.2.4.3. Quality Profile
##### <div id='3-2-4-3-1'/> 3.2.4.3.1. Create Quality Profile
1.	Proceed to the Quality Profile Dashboard by clicking Quality Profile from the Quality Management Menu.
![image](https://user-images.githubusercontent.com/80228983/146869326-75a4cefe-3359-4868-961d-c40cabff7471.png)
2.	Click the "Create" button in the upper right corner.
![image](https://user-images.githubusercontent.com/80228983/146869368-150fd27b-c848-492a-9bc9-daf82bad9dd4.png)
3.	Enter the quality profile name, select the development language and click the "Create" button.
![image](https://user-images.githubusercontent.com/80228983/146869418-4b14c549-055d-46d9-9645-2ecb39ed267d.png)
4.	Verify that a quality profile is created.
![image](https://user-images.githubusercontent.com/80228983/146869466-70d23957-9668-44e1-90ff-18f743060566.png)

##### <div id='3-2-4-3-2'/> 3.2.4.3.2. Duplicate Quality Profile
1.	Click the "Duplicate" button on the Quality Profile dashboard.
![image](https://user-images.githubusercontent.com/80228983/146869595-579a04e7-92ea-4f2f-b67d-7d4567b417a4.png)
2.	Enter the name of the quality profile you want to duplicate, and click the "Duplicate" button.
![image](https://user-images.githubusercontent.com/80228983/146869642-41573c30-9867-484c-b3db-d36c7c5396de.png)
3.	Check the duplicated quality profile.
![image](https://user-images.githubusercontent.com/80228983/146869693-2c0bced9-3618-4e4b-ba3d-a839706bb567.png)

##### <div id='3-2-4-3-3'/> 3.2.4.3.3. Modify Quality Profile
1.	Click the "Modify" button on the Quality Profile dashboard.
![image](https://user-images.githubusercontent.com/80228983/146869730-5e78b1e5-70ca-4472-b2a4-fdeb45fe7c3a.png)
2.	When the quality profile pop-up window to modify appears, modify the quality profile name and click the "Modify" button.
![image](https://user-images.githubusercontent.com/80228983/146869753-e5a28251-de12-4331-b20e-7abcfd4e2cab.png)
3. Confirm that the quality profile name has been modified.
![image](https://user-images.githubusercontent.com/80228983/146869782-6fb49ca0-1a77-43b7-b497-12ef3a5a4728.png)

##### <div id='3-2-4-3-4'/> 3.2.4.3.4. Connect Quality Profile Project]
1.	Check the quality profile dashboard for connected project items. (The first picture shows the quality profile set to Sonar way when retrieving the test job configuration. Therefore, as an example in the second picture, the newly created quality profile [GuideModified] does not show any connected projects.)
![image](https://user-images.githubusercontent.com/80228983/146869881-a5b19ce0-c43c-430f-bbc0-34e648c7816d.png)
2.	If no projects are connected, click the "Not Connected" tab.
![image](https://user-images.githubusercontent.com/80228983/146870059-5edd86ac-a917-4d68-9057-df6b095d0cef.png) 
3.	When Profile is connected, the project can be seen in the "Connected" tab.
![image](https://user-images.githubusercontent.com/80228983/146870218-7903313a-1acd-4c5d-bb8e-bf2150e3aa3f.png) 
![image](https://user-images.githubusercontent.com/80228983/146870235-e1d10c13-4001-47a9-b936-0684ded07a00.png) 

##### <div id='3-2-4-3-5'/> 3.2.4.3.5. Delete Quality Profile
1.	Click the "Delete" button on the Quality Profile dashboard.
![image](https://user-images.githubusercontent.com/80228983/146870360-07dfcc04-779c-4330-b46c-d7e3582b8ad8.png)

#### <div id='3-2-4-4'/> 3.2.4.4. Quality Gate
##### <div id='3-2-4-4-1'/> 3.2.4.4.1. Create Quality Gate
1.	Select Quality Gate from the Quality Management menu to go to the Quality Gate dashboard.
![image](https://user-images.githubusercontent.com/80228983/146870977-2e815cd0-06a0-45d3-8b74-ae08b13ebb96.png)
2.	Click the "Create" button in the upper right corner.
![image](https://user-images.githubusercontent.com/80228983/146870997-cb6f0416-54f2-4943-add9-f8afef3963ee.png)
3.	Enter the quality gate name and click the "Create" button.
![image](https://user-images.githubusercontent.com/80228983/146871027-05bff6f6-dfc8-4be5-bf9c-12f6c5afcf8b.png)
4.	Check that a quality gate is created.
![image](https://user-images.githubusercontent.com/80228983/146871082-9f7b5524-7a08-4a65-b32e-5451a51d8192.png)

##### <div id='3-2-4-4-2'/> 3.2.4.4.2. Duplicate Quality Gate
1.	Click the "Duplicate" button on the Quality Gate dashboard.
![image](https://user-images.githubusercontent.com/80228983/146871101-e7d27bb3-088e-4e7c-8f2f-94d4c2674d74.png)
2.	Enter the name of the quality gate you want to duplicate, and click the "Duplicate" button.
![image](https://user-images.githubusercontent.com/80228983/146871144-b568c53d-0eb2-44b8-b497-ed41f27d7c63.png)


##### <div id='3-2-4-4-3'/> 3.2.4.4.3. Modify Quality Profile
1.	Click the "Modify" button on the Quality Gate dashboard.
![image](https://user-images.githubusercontent.com/80228983/146871383-81744c10-8694-430d-a5e1-3a8f2ba53fda.png)
2.	When the quality gate pop-up window to modify appears, modify the quality gate name and click the "Modify" button.
![image](https://user-images.githubusercontent.com/80228983/146871410-9462890c-ce73-4824-9200-4e19681bd17d.png)


##### <div id='3-2-4-4-4'/> 3.2.4.4.4. Add Quality Gate Conditions
1.	On the quality gate dashboard, check the additional conditions that can set the conditions for passing the Job test. The user can add the conditions himself and set the criteria. In the Save/Modify column, click Save to save.
![image](https://user-images.githubusercontent.com/80228983/146871550-aadfbdcc-d228-4838-89f7-50e77865b71f.png)
2.	If the test is above/below a certain standard depending on the conditions, pass the test.
![image](https://user-images.githubusercontent.com/80228983/146871698-fd190b68-328f-46f3-813b-1923fe3171e9.png)

##### <div id='3-2-4-4-5'/> 3.2.4.4.5. Connect Quality Gate Project
1. Check the connected project items on the quality gate dashboard. (The first picture shows the quality gate set to Fortest when retrieving the static analysis Job configuration.)
![image](https://user-images.githubusercontent.com/80228983/146872137-88e8c4cd-bae9-4f19-bde7-513be375d303.png)
2. Quality gates can connect to multiple projects per quality gate like quality profiles.


##### <div id='3-2-4-4-6'/> 3.2.4.4.6. Delete Quality Gate
1.	Click the "Delete" button on the quality gate dashboard.
![image](https://user-images.githubusercontent.com/80228983/146872354-95a8dc91-4c18-43d5-9b5d-bbe76bd9226b.png)
2.	Confirm that the quality gate has been deleted.


#### <div id='3-2-4-5'/> 3.2.4.5. Manage Staging
##### <div id='3-2-4-5-1'/> 3.2.4.5.1. Environment Information Management
A page that manages the configuration settings of the deployed application. The container platform pipeline provides a spring cloud config server and stores and manages environmental information in DB.

###### <div id='3-2-4-5-1-1'/> 3.2.4.5.1.1. Register Environment Information
1. Access the Environmental Information Management page.
![image](https://user-images.githubusercontent.com/80228983/146874425-487939dd-2877-45a6-ba78-075c1d096fd0.png) 
2. Click the Add button on the right side of the Environment Information Management Dashboard.
![image](https://user-images.githubusercontent.com/80228983/146874528-b9274fa9-4cfc-43c6-874f-596847eb699f.png)
3. Click the file selection button in the environment information registration page.
![image](https://user-images.githubusercontent.com/80228983/146874591-9421c887-c653-4c24-ae81-9fad12bd44e1.png)
***※    Select .properties, .yaml', .yml format, and shall not include dividing line such as '----'.***
4. If selected correctly, a pop-up notification window appears saying OK.
![image](https://user-images.githubusercontent.com/80228983/146874707-3efa4b94-2af7-437e-8004-efde7ae737a8.png)
5. Enter the application name and profile name according to the situation.
![image](https://user-images.githubusercontent.com/80228983/146874773-59b32463-844f-4d76-bc55-7316bdb777b8.png)
***※	In the Container Platform pipeline, the label name is fixed to 'standalone'.***
6. Click the Register button to register the environment information.
![image](https://user-images.githubusercontent.com/80228983/146874930-d4fd9c03-ef65-4d3d-8d04-a5de6c0f6d71.png)
7. Ensure that environmental information is properly registered.
![image](https://user-images.githubusercontent.com/80228983/146874997-8f397560-05ff-4d01-9ed6-27e23983cc77.png)

###### <div id='3-2-4-5-1-2'/> 3.2.4.5.1.2. Search Environment Information List
Can be searched through three types: 'Application name', 'profile name', and 'Key search' in the environmental information management dashboard.
1. Expand the application filter to select the desired application.
![image](https://user-images.githubusercontent.com/80228983/146875629-2c192489-d556-40ec-8769-53bc31abbf1f.png)
2. The selected application is filtered and retrieved.
![image](https://user-images.githubusercontent.com/80228983/146875710-102c4b0d-d265-498f-a13d-78afbcb333ce.png)
3. Check if the selected profile name is filtered and inquired.
![image](https://user-images.githubusercontent.com/80228983/146875761-2f72dff9-2346-4f84-9a21-952e735f4fe3.png)
3. Check if it is filtered and inquired with the entered key name.
![image](https://user-images.githubusercontent.com/80228983/146875822-e2e85169-4cf7-417f-b80c-3a9ed13bf38b.png)

###### <div id='3-2-4-5-1-3'/> 3.2.4.5.1.3. Delete Environment Information
The selected application/profile information may be deleted from the environment information management dashboard.
1. Expand the application filter to select the application you want to delete.
![image](https://user-images.githubusercontent.com/80228983/146875945-98cf9929-1c37-417d-b1e1-c13d9b077f03.png) 
2. Expand the Profile filter and select the profile you want to delete.
![image](https://user-images.githubusercontent.com/80228983/146876117-de125635-3612-43b1-9de4-1d1781b3c67e.png)
3. Select paasta-delivery-pipeline-inspection-api application 'default' profile and click the 'Application delete' button.
![image](https://user-images.githubusercontent.com/80228983/146876164-ffb906d8-0a8a-4aef-ac1d-82469c87c0ee.png)
4. Check if the selected environment information is deleted.
![image](https://user-images.githubusercontent.com/80228983/146876207-8d3b8d4e-4fe3-4e33-8fbe-5a41fd8bc1f2.png)

<br>

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](https://github.com/PaaS-TA/paas-ta-container-platform-guide-eng/tree/master/use-guide/Readme.md) > Pipeline Service Use Guide