### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Container Platform Source Control Service Use Guide

<br>

# [K-PaaS Container Platform Source Control Service Use Guide]

<br>

## Table of Contents
1. [Document Overview](#1)<br>
   * [1.1. Purpose](#1.1)<br>
   * [1.2. Scope](#1.2)<br>
2. [Accessing Container Platform source control](#2)
3. [Container Platform Source Control User Manual](#3)
   * [3.1. Configure the Container Platform source control user menu](#3.1)
   * [3.2. Container Platform Source Control user menu description](#3.2)
   * [3.2.1 Manage my info](#3.2.1)
   * [3.2.1.1 View/Edit my info Details](#3.2.1.1)
   * [3.2.2 Manage users](#3.2.2)
   * [3.2.2.1 Get a list of users](#3.2.2.1)
   * [3.2.2.2 View/Delete User Details](#3.2.2.2)
   * [3.2.3 Managing repositories](#3.2.3)
   * [3.2.3.1 Creating a repository](#3.2.3.1)
   * [3.2.3.2 Get a list of repositories](#3.2.3.2)
   * [3.2.3.3 View repository details](#3.2.3.3)
   * [3.2.3.4 Modifying and deleting repositories](#3.2.3.4)
   * [3.2.3.5 Get a list of repository files](#3.2.3.5)
   * [3.2.3.6 Get a list of repository commits](#3.2.3.6)
   * [3.2.3.7 Get a list of repository participants](#3.2.3.7)

<br>

# <div id='1'/> 1. Document Overview

### <div id='1.1'/> 1.1. Purpose
This document describes how to use the Container Platform source control service.

<br>

### <div id='1.2'/> 1.2. Scope
This document is written for users who will be using the Container Platform source control service in a Windows environment.

<br>

# <div id='2'/> 2. Accessing Container Platform source control
Access the container platform source control.<br><br>
**Container platform source control URL** : `http://scm.${HOST_DOMAIN}`
+ Enter the `HOST_DOMAIN` value as defined in the [[3.2. Defining Container Platform source control variables]](../../install-guide/source-control/cp-source-control-standalone-guide.md#3.2)

![SCM-IMG-2.1]
![SCM-IMG-2.2]

<br>

# <div id='3'/> 3. Container Platform Source Control User Manual
This chapter describes the menu organization and screen descriptions of the Container Platform Pipeline.

<br>

### <div id='3.1'/> 3.1. Configure the Container Platform source control user menu
The Container Platform source control service consists of internal information management, user management, and repository list.

***※ By default, users don't see the User Management menu.***
<table>
  <tr>
    <td>Menu</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>Manage my info</td>
    <td>View and manage information about current users of Container Platform source control</td>
  </tr>
  <tr>
    <td>Manage users</td>
    <td>View and manage information about users using Container Platform source control</td>
  </tr>
  <tr>
    <td>Repository</td>
    <td>Manage repository registration, list and detailed views, deletion, files, commits, participant permissions, and more.</td>
  </tr>
</table>

<br>

### <div id='3.2'/> 3.2. Container Platform Source Control user menu description
- This chapter describes the three Container Platform source control menus.
### <div id='3.2.1'/> 3.2.1. Manage my info
- Describes how to get and manage information about the current user using the Container Platform source control service.
### <div id='3.2.1.1'/> 3.2.1.1. View/Edit my info Details
1. In the top right menu, click on your current user account name and then click on "Manage my info" in the first list that appears.
   ![SCM-IMG-3.2.1.1.1]

2. Get detailed information about the current user.
   ![SCM-IMG-3.2.1.1.2]

3. Enter the password you want to change and click the Edit button.
   ![SCM-IMG-3.2.1.1.3]

   ***※The changed password is the password used to access the source control repository and is independent of your login information (Keycloak password for a standalone deployment or K-PaaS password for a service deployment)..***

   ***※You must change your password upon initial account login to access repository pulls, pushes, etc..***


### <div id='3.2.2'/> 3.2.2. Manage users
The User Management menu is visible only to administrators, and this chapter describes how to manage permissions and view information about users who use the Container Platform source control service.
### <div id='3.2.2.1'/> 3.2.2.1. Get a list of users
1. Click on the current user's username in the top right menu, then click "Manage Users" in the second list that appears.
   ![SCM-IMG-3.2.2.1.1]
2. In the list of moved screens, you can see all usernames, names, creation date, and modification date by the top admin. On the right side of the list, you can also check the availability of administrators and users.

### <div id='3.2.2.2'/> 3.2.2.2. View/Delete User Details
1. Click User information to go to the user detail view/delete page.
   ![SCM-IMG-3.2.2.2.1]

2. View user details on the View/delete user page to view ID, name, usage, and description information.

3. On the user detail view/delete page, click the "Delete a user" button in the lower left corner to delete the user.
   ![SCM-IMG-3.2.2.2.3]

### <div id='3.2.3'/> 3.2.3. Managing repositories
Describes how to get and manage repository information of the Container Platform source control service.
### <div id='3.2.3.1'/> 3.2.3.1. Create a repository
1. Click "List of repositories" in the top menu.
   ![SCM-IMG-3.2.3.1.1]

2. Enter a name for the repository, select a type, and click the Register button.
   ![SCM-IMG-3.2.3.1.2]

3. Alternatively, you can click the "Create New" button in the top right corner of the repository list screen to go to the New Repository page.
   ![SCM-IMG-3.2.3.1.3]

4. On the Create new repository page, enter a name for the repository (a combination of letters, numbers, and hyphens), select a type (Git, SVN), and click the "Create" button. You will see the notification "A new repository has been created." Click the "Confirm" button to create the repository.
   ![SCM-IMG-3.2.3.1.4.1]
   ![SCM-IMG-3.2.3.1.4.2]

### <div id='3.2.3.2'/> 3.2.3.2. Get a list of repositories
1. On the Repository list page, view the list of repositories by searching for a repository, combo box organizer type, all repositories, and searching by updates.
   ![SCM-IMG-3.2.3.2.1]

### <div id='3.2.3.3'/> 3.2.3.3. View repository details
1. You can view file, commit, and contributor information on the repository details page.
   ![SCM-IMG-3.2.3.3.1]


### <div id='3.2.3.4'/> 3.2.3.4. Modifying and deleting repositories
1. Click the repository name in the repository list to go to the repository details page.
   ![SCM-IMG-3.2.3.4.1]

2. On the repository detail page, click the "View/modify information" button on the right to go to the View/modify information page.
   ![SCM-IMG-3.2.3.4.2]


3. On the View/modify information page, click the "Edit" button. You will see a notification message "Edited." appears, and the repository modification is complete.
   ![SCM-IMG-3.2.3.4.3.1]
   ![SCM-IMG-3.2.3.4.3.2]

4. On the View/modify information page, click the "Delete a repository" button in the lower left corner. You will see a notification message "Deleted." The repository deletion is complete.
   ![SCM-IMG-3.2.3.4.4]

### <div id='3.2.3.5'/> 3.2.3.5. Get a list of repository files
1. You can copy URLs via "Repository clones" on the right side of the repository details view.
   ![SCM-IMG-3.2.3.5.1]

2. Run git remote add and push based on the copied repository clone URL.

   ***※Account information (source control username/password) is required to access the copied repository via push, pull, etc.***

   ***※Username: The source control username you are connected to (e.g.: admin)***

   ***※Password: The password you changed in 3.2.1.1.1*** <br>

<br>

3. On the repository details page, you can see information about branches and tags under "file" on the left side of the first tab.
   ![SCM-IMG-3.2.3.5.3]

4. In the "file" list, you can see the folder/file name, last change, file size, and last update by default.

### <div id='3.2.3.6'/> 3.2.3.6. Get a list of repository commits
1. Click "Commit" in the middle of the tabs on the repository details page to see a list of changes made.
   ![SCM-IMG-3.2.3.6.1]

### <div id='3.2.3.7'/> 3.2.3.7. Get a list of repository participants
1. On the repository details page, click "Contributor" on the right at the end of the tabs. You can see information about the contributors to this repository.
   ![SCM-IMG-3.2.3.7.2.1]

2. Click the Add/Remove Participant button to go to the SourceController user search page.
   ![SCM-IMG-3.2.3.7.2.2]

3. On the Search source controller users page, enter the user you want to add participant permissions to. After confirming the user in the user search list, click the "+" image.
   ![SCM-IMG-3.2.3.7.3.1]
   ![SCM-IMG-3.2.3.7.3.2]

4. At the bottom of the Source Controller user search, you can view and change the participant invitation information, permissions, and description. After checking the ID, name, and email of the user added to the participant invitation, click the "Add" button when you're done setting the required permissions (write/view). The repository participant permissions are added when you see the notification "[ Participant have been added ]".
   ![SCM-IMG-3.2.3.7.4.1]
   ![SCM-IMG-3.2.3.7.4.2]

5. Click the repository participation permissions you want to delete.
   ![SCM-IMG-3.2.3.7.5]

6. Click the Delete participant button in the bottom-left corner of the participant information page. When you see the notification "The participant deletion is complete", the repository participant permissions will be removed.
   ![SCM-IMG-3.2.3.7.6.1]
   ![SCM-IMG-3.2.3.7.6.2]

7. Alternatively, enter the user you want to remove participant permissions from within the source controller user search in the same way as adding repository participant permissions. After confirming the user in the user search list, click the "-" image. The repository participant permissions are deleted when the notification "Participant have been deleted." appears.
   ![SCM-IMG-3.2.3.7.7.1]
   ![SCM-IMG-3.2.3.7.7.2]

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Container Platform Source Control Service Use Guide


[SCM-IMG-2.1]:../images/sourcecontrol/SCM-IMG-2.1.png
[SCM-IMG-2.2]:../images/sourcecontrol/SCM-IMG-2.2.png
[SCM-IMG-3.2.1.1.1]:../images/sourcecontrol/SCM-IMG-3.2.1.1.1.png
[SCM-IMG-3.2.1.1.2]:../images/sourcecontrol/SCM-IMG-3.2.1.1.2.png
[SCM-IMG-3.2.1.1.3]:../images/sourcecontrol/SCM-IMG-3.2.1.1.3.png
[SCM-IMG-3.2.2.1.1]:../images/sourcecontrol/SCM-IMG-3.2.2.1.1.png
[SCM-IMG-3.2.2.2.1]:../images/sourcecontrol/SCM-IMG-3.2.2.2.1.png
[SCM-IMG-3.2.2.2.3]:../images/sourcecontrol/SCM-IMG-3.2.2.2.3.png
[SCM-IMG-3.2.3.1.1]:../images/sourcecontrol/SCM-IMG-3.2.3.1.1.png
[SCM-IMG-3.2.3.1.2]:../images/sourcecontrol/SCM-IMG-3.2.3.1.2.png
[SCM-IMG-3.2.3.1.3]:../images/sourcecontrol/SCM-IMG-3.2.3.1.3.png
[SCM-IMG-3.2.3.1.4.1]:../images/sourcecontrol/SCM-IMG-3.2.3.1.4.1.png
[SCM-IMG-3.2.3.1.4.2]:../images/sourcecontrol/SCM-IMG-3.2.3.1.4.2.png
[SCM-IMG-3.2.3.2.1]:../images/sourcecontrol/SCM-IMG-3.2.3.2.1.png
[SCM-IMG-3.2.3.3.1]:../images/sourcecontrol/SCM-IMG-3.2.3.3.1.png
[SCM-IMG-3.2.3.4.1]:../images/sourcecontrol/SCM-IMG-3.2.3.4.1.png
[SCM-IMG-3.2.3.4.2]:../images/sourcecontrol/SCM-IMG-3.2.3.4.2.png
[SCM-IMG-3.2.3.4.3.1]:../images/sourcecontrol/SCM-IMG-3.2.3.4.3.1.png
[SCM-IMG-3.2.3.4.3.2]:../images/sourcecontrol/SCM-IMG-3.2.3.4.3.2.png
[SCM-IMG-3.2.3.4.4]:../images/sourcecontrol/SCM-IMG-3.2.3.4.4.png
[SCM-IMG-3.2.3.5.1]:../images/sourcecontrol/SCM-IMG-3.2.3.5.1.png
[SCM-IMG-3.2.3.5.3]:../images/sourcecontrol/SCM-IMG-3.2.3.5.3.png
[SCM-IMG-3.2.3.6.1]:../images/sourcecontrol/SCM-IMG-3.2.3.6.1.png
[SCM-IMG-3.2.3.7.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.1.png
[SCM-IMG-3.2.3.7.2.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.2.1.png
[SCM-IMG-3.2.3.7.2.2]:../images/sourcecontrol/SCM-IMG-3.2.3.7.2.2.png
[SCM-IMG-3.2.3.7.3.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.3.1.png
[SCM-IMG-3.2.3.7.3.2]:../images/sourcecontrol/SCM-IMG-3.2.3.7.3.2.png
[SCM-IMG-3.2.3.7.4.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.4.1.png
[SCM-IMG-3.2.3.7.4.2]:../images/sourcecontrol/SCM-IMG-3.2.3.7.4.2.png
[SCM-IMG-3.2.3.7.5]:../images/sourcecontrol/SCM-IMG-3.2.3.7.5.png
[SCM-IMG-3.2.3.7.6.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.6.1.png
[SCM-IMG-3.2.3.7.6.2]:../images/sourcecontrol/SCM-IMG-3.2.3.7.6.2.png
[SCM-IMG-3.2.3.7.7.1]:../images/sourcecontrol/SCM-IMG-3.2.3.7.7.1.png
[SCM-IMG-3.2.3.7.7.2]:../images/sourcecontrol/SCM-IMG-3.2.3.7.7.2.png

