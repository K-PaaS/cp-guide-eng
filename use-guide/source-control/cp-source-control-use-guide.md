### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) > Source Control Service Use Guide

<br>

# [K-PaaS Container Platform Source Control Service User Guide]

<br>

## Table of Contents
1. [Document Outline](#1)
     * [1.1. Purpose](#1.1)
     * [1.2. Range](#1.2)
2. [Container Platform Source Control Access](#2)
     * [2.1. Service Container Platform Source Control Access](#2.1)
     * [2.2. Stand-Alone Deployment Container Platform Source Control Access](#2.2)
3. [Container Platform Source Control User Manual](#3)
     * [3.1. Container Platform Source Control User Menu Configuration](#3.1)
     * [3.2. Container Platform Source Control User Menu Description](#3.2)
     * [3.2.1 Manage My Account](#3.2.1)
     * [3.2.1.1 My Account Details Check/Modify](#3.2.1.1)
     * [3.2.2 User Management](#3.2.2)
     * [3.2.2.1 Check User List](#3.2.2.1)
     * [3.2.2.2 Check/ Delete User Details](#3.2.2.2)
     * [3.2.3 Manage Repository ](#3.2.3)
     * [3.2.3.1 Create Repository](#3.2.3.1)
     * [3.2.3.2 Check Repository List](#3.2.3.2)
     * [3.2.3.3 Check Repository Details](#3.2.3.3)
     * [3.2.3.4 Modify/Delete Repository](#3.2.3.4)
     * [3.2.3.5 Check Repository File List](#3.2.3.5)
     * [3.2.3.6 Check Repository Comit List](#3.2.3.6)
     * [3.2.3.7 Check Repository Participants List](#3.2.3.7)

<br>

# <div id='1'/> 1. Document Outline

### <div id='1.1'/> 1.1. Purpose
This document describes the usage of the container platform source control service.

<br>

### <div id='1.2'/> 1.2. Range
The range of this document covers how to use the container platform source control service in the Windows environment.

<br>

# <div id='2'/> 2. Container Platform Source Control Access

### <div id='2.1'/> 2.1. Service Container Platform Source Control Access
1. Connect by clicking the "dashboard" button on the applied source control from the space page of the K-PaaS portal.

<br>

2. Check Container Platform Source Control Access.  
![image](https://user-images.githubusercontent.com/67407365/147020207-16929bd7-4011-4643-8eb9-958a9ccc4611.png)

<br>

3. At this,  ***The user who created the Container Platform Deployment Source Control Service becomes the operator.***

<br>

### <div id='2.2'/> 2.2. Stand-Alone Deployment Container Platform Source Control Access
1. From the deployed cluster, access the web browser with public IP to http://{K8S_MASTER_NODE_IP}:30094 and proceed.

<br>

2. Enter the account information from the keycloak login screen. (Initial Operators Account: admin / admin )
![image](https://user-images.githubusercontent.com/67407365/147020597-632f5a59-5054-49a4-b420-36d8385eadf7.png)

<br>

3. Check the access of container platform source control.
![image](https://user-images.githubusercontent.com/67407365/147020207-16929bd7-4011-4643-8eb9-958a9ccc4611.png)

<br>

# <div id='3'/> 3. Container Platform Source Control User Manual
This chapter describes the container platform pipeline's menu configuration and screen description.

<br>

### <div id='3.1'/> 3.1. Container Platform Source Control User Menu Configuration
The container platform source control service consists of Manage my account, user management, and repository list.

***※ The user managing menu is not visible to the users.***
<table>
  <tr>
    <td>Menu</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>Manage My Account</td>
    <td>Lookup and manages information on the specific user who is using the Container Platform Source Control</td>
  </tr>
  <tr>
    <td>User Management</td>
    <td>Lookup and manages information on the users who are using the Container Platform Source Control</td>
  </tr>
  <tr>
    <td>Repository</td>
    <td>Manage functions such as repository registration, listing and detail lookup, deletion, files, commitments, and participant permissions</td>
  </tr>
</table>

<br>

### <div id='3.2'/> 3.2. Container Platform Source Control User Menu Description
This chapter describes the three menus of the container platform source control.

<br>

### <div id='3.2.1'/> 3.2.1. Manage My Account
This menu describes the information retrieving and managing of a specific user using the container platform source control service.

<br>

### <div id='3.2.1.1'/> 3.2.1.1. My Account Details Check/Modify
1. Click the current user account name in the top right menu, and then click "Manage My Account" at the first part of the list that appears.
![image](https://user-images.githubusercontent.com/67407365/147021223-a17382b4-a754-425f-8f3b-b8601bab1a7f.png)

<br>

2. Check the detailed information of the current user.
![image](https://user-images.githubusercontent.com/67407365/147020801-800f6321-a40c-4a18-8a1e-28ab0dd49bbb.png)

<br>

3. Enter the password to change and click modify button.
![image](https://user-images.githubusercontent.com/67407365/147021304-56f1437f-4a73-4247-bf83-f6095263aba1.png)


***※The changed password is the password used to access the source control repository 
<br>It has nothing to do with login (Keycloak password for Stand-Alone Deployment, K-PaaS password for service deployment)information.*** <br>
***※Password should be changed at the very first login to have access to repository pull, push, etc. *** <br>
<br>

### <div id='3.2.2'/> 3.2.2. User Management
The User Management Menu is only visible to the operator(administrators). This chapter describes how to check and manage the authority of the users who are using the Container Platform Source Control Service.

<br>

### <div id='3.2.2.1'/> 3.2.2.1. Check User List
1. Click the account name of the current user on the top right menu and click "User Management” from the list.
![image](https://user-images.githubusercontent.com/67407365/147021373-8dbc3bb8-32d2-4679-951d-dc7e92a6258d.png)

<br>

2. All the user ID, name, created date, and modification dates based on the operator on top of the proceeded screen's list. In addition, it is possible to check whether administrators and users are using it on the right side of the list.
![image](https://user-images.githubusercontent.com/67407365/147021423-54819e22-10d6-405d-abfd-386227331333.png)

<br>

### <div id='3.2.2.2'/> 3.2.2.2. Check/ Delete User Details
1. Click the current user account name in the top right menu and click "User Management" in the second part of the list that appears.
![image](https://user-images.githubusercontent.com/67407365/147021373-8dbc3bb8-32d2-4679-951d-dc7e92a6258d.png)

<br>

2. Click the user information and go to the Check/Delete User Details page.
![image](https://user-images.githubusercontent.com/67407365/147021518-0de19286-55ab-41c8-b567-40e5f8ec5d07.png)

<br>

3. From the Check/ Delete User Details page, check the ID, name, Usage status, and description information.
![image](https://user-images.githubusercontent.com/67407365/147021556-0fcd21bb-1439-4875-b5db-ec4fa5c43269.png)

<br>

4. From the Check/ Delete User Details Page, click the “Delete User” button at the left bottom part.
![image](https://user-images.githubusercontent.com/67407365/147021579-9ffe96db-3b73-40e7-a8f5-d4dab899fd49.png)

<br>

### <div id='3.2.3'/> 3.2.3. Manage Repository
This chapter describes checking and managing repository information of the container platform source control service.

<br>

### <div id='3.2.3.1'/> 3.2.3.1. Create Repository
1. Click "Repository List" from the menu on top.
![image](https://user-images.githubusercontent.com/67407365/147021750-a73371f4-4298-41e1-ab1a-4d736cfad2a0.png)

<br>

2. Enter the Repository name, select type, and click the Register button.
![image](https://user-images.githubusercontent.com/67407365/147021844-894f4bf7-9d5d-4536-a1f5-64aa9d269c17.png)

<br>

3. As an additional method, click the "Create New" button in the upper right corner of the Repository List screen to go to the Create Repository page.
![image](https://user-images.githubusercontent.com/67407365/147021886-c84aea6b-01ad-41be-867e-ef170d9ccb0a.png)

<br>

4. On the New Repository page, enter the repository name in a combination of English, numeric, and hyphen. Select type (Git, SVN), and click the "Create" button. When the notification message "New Repository has been created," press the "OK" button to complete creating the repository.
![image](https://user-images.githubusercontent.com/67407365/147021952-8231e434-1b5c-49af-8739-83d7c8dcb748.png)
![image](https://user-images.githubusercontent.com/67407365/147021986-3ece3a29-c114-4bbb-8227-cb859702d1f7.png)

<br>

### <div id='3.2.3.2'/> 3.2.3.2. Check Repository List
1. On the Repository List page, check the repository list by searching the repository search, combo box shape management type, all repository, and update sequence.
![image](https://user-images.githubusercontent.com/67407365/147022269-be8fa1f4-4314-4981-b6d7-0d053f3660e9.png)

<br>

### <div id='3.2.3.3'/> 3.2.3.3. Check Repository Details
1. The information of file, Commit, and Contributor can be checked at the repository details page.
![image](https://user-images.githubusercontent.com/67407365/147022467-52235927-8c92-4b70-b924-dd465190537b.png)

<br>

### <div id='3.2.3.4'/> 3.2.3.4. Modify/Delete Repository
1. Click the repository name from the repository list and procceed to the details page.
![image](https://user-images.githubusercontent.com/67407365/147022536-7fc3b6a5-65d0-4aba-a203-1a9f1dfcd171.png)

<br>

2. Click the “View/ Modify Information” button on the right part of the repository details page and proceed.
![image](https://user-images.githubusercontent.com/67407365/147022494-fa331bf1-1d11-43d7-86c6-072dbf244f9a.png)
<br>

3. Click the "Modify" button from the View/ Modify Information page. When the notification message “Successfully Modified.” shows up, the repository modification is successfully done.
![image](https://user-images.githubusercontent.com/67407365/147022601-a22ac31d-79aa-4644-b39f-0d67f10a18cb.png)
![image](https://user-images.githubusercontent.com/67407365/147022639-2db1a1e2-820f-46de-8d82-807fdeb486e3.png)

<br>

4. Click "Delete Repository" button from the View/ Modify Information page. When notification message “Successfully Deleted.” shows up, the repository deletion is successfully done.
![image](https://user-images.githubusercontent.com/67407365/147022660-97dba604-783d-4713-a8ee-6bf3155477eb.png)
![image](https://user-images.githubusercontent.com/67407365/147022714-39b13490-c0f4-4e12-966d-a65334911a21.png)
![image](https://user-images.githubusercontent.com/67407365/147022730-9214894f-e3b9-48e5-8383-312a41e1645b.png)

<br>

### <div id='3.2.3.5'/> 3.2.3.5. Check Repository File List
1. The URL can be copied through the “Clone Repository” at the right part of repository details.
![image](https://user-images.githubusercontent.com/67407365/147022954-8f48fc82-b827-4dcc-a9a2-e4f0462eb68a.png)

<br>

2. Based on the copied clone repository URL, process git remote add and push.

***※Account information (source control ID/password) is required to access the copied repository such as push and pull.*** <br>
***※ID: ID of the accessed source control(e.g: admin)*** <br>
***※Password: The password changed in 3.2.1.1.1*** <br>

<br>

3. Information on branches and tags can be checked in the "file" on the first left side of the tab on the view details page of the repository.
![image](https://user-images.githubusercontent.com/67407365/147023724-71f271cf-0b64-44fa-a12d-77c91766b6da.png)

<br>

4. In the "file" list, the folder/file name, final modification, file size, and last update can be checked as default.
![image](https://user-images.githubusercontent.com/67407365/147023747-456091b1-c1c6-4b2c-881b-565942d8a2b3.png)

<br>

### <div id='3.2.3.6'/> 3.2.3.6. Check Repository Comit List
1. Click "Commit" in the middle of the tab on the Detailed Repository page to view the list of changes.
![image](https://user-images.githubusercontent.com/67407365/147023842-f43bf125-b9b0-4a06-a749-b2998eab63ad.png)

<br>

### <div id='3.2.3.7'/> 3.2.3.7. Check Repository Participants List
1. Click "Contributor" on the right side of the tab on the Repository Details page. Information on participants in the repository can be checked.
![image](https://user-images.githubusercontent.com/67407365/147024402-2af350b6-de7a-42c4-b15a-b1a33be47114.png)

<br>

2. Click the Add/Remove Participants button to go to the Source Controller User Search page.
![image](https://user-images.githubusercontent.com/67407365/147024585-c073f37c-6762-4ea2-b77b-bcdbfcc3c2f3.png)

<br>

3. On the Source Controller User Search page, enter the user to which you want to add participant authority. After checking the user in the user search list, click the "+" image.
![image](https://user-images.githubusercontent.com/67407365/147024844-7b78393a-9073-451a-b5a7-8011c1125028.png)
![image](https://user-images.githubusercontent.com/67407365/147024953-de195989-a08c-4eeb-b50c-cad31170b2c8.png)

<br>

4. Participant invitation information, authorization, and explanations can be checked and modified at the bottom of the source controller user search. After checking the information about the user's ID, name, and email added to the participant invitation, click the "Add" button when you have completed setting the required permissions (write permission/view permission). When the notification message "[Participant has been added.]", the Repository Participant authority has been added.
![image](https://user-images.githubusercontent.com/67407365/147025127-5eaeb80e-55b8-4176-b288-c6259e5d9431.png)
![image](https://user-images.githubusercontent.com/67407365/147025160-28a4081e-9cda-4380-8406-9836c3138db8.png)

<br>

5. Click the repository participant authority to delete.
![image](https://user-images.githubusercontent.com/67407365/147026049-c87df481-814b-426c-84de-1a8199288b0c.png)

<br>

6. Click the delete participant button at the left bottom part of the participant information page. When the notification message “Participant has been deleted.” appears, the repository participant authority deletion is successfully done.
![image](https://user-images.githubusercontent.com/67407365/147025867-8cd6c5a3-2584-4763-bbc2-7c2402f7c896.png)
![image](https://user-images.githubusercontent.com/67407365/147025936-cc53bcb4-b2ad-459f-a3dc-d2c60d1107ce.png)
![image](https://user-images.githubusercontent.com/67407365/147025962-b0eb4865-bc84-4230-8b4f-98ef6f3ec5fb.png)

<br>

7. Or, in the same way as adding participant authority in the repository, enter a user to delete participant authority in the source controller user search. After checking the user in the user search list, click on the "-" image. When the notification message "Participant has been deleted." the repository participant authorities are deleted.
![image](https://user-images.githubusercontent.com/67407365/147026107-61164c48-b938-490f-917b-8863d44eaee7.png)
![image](https://user-images.githubusercontent.com/67407365/147026141-c65dd6de-8c90-48d7-8d52-530140a82513.png)

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../Readme.md) > Source Control Service Use Guide
