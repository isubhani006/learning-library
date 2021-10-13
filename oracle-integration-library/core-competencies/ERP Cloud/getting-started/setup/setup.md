# Setup the Lab prerequisites

## Introduction

This lab walks you through the steps to setup the Lab Prerequisites

Estimated Lab Time: 5 minutes

### Objectives

In this lab, you will:
* Enable File Server
* Create directory and configure permissions
* Connect FTP to File Server

### Prerequisites

This lab assumes you have:

* Successfully provisioned Oracle Integration Instance and able to access the homepage

## Task 1: Enable File Server

To begin using File Server in Oracle Integration, it must first be enabled for the Oracle Integration instance in the Oracle Cloud Infrastructure Console. Enabling File Server is a one time action.

If you select File Server from the navigation pane and it's not yet enabled for Oracle Integration, the following message appears:
	![](images/file_server_enablement0.png)

To enable File Server:

1.	Select your instance in the Oracle Cloud Infrastructure Console.
		The Integration Instance Details page is displayed.
2.	Click the Enable link for File Server on the Integration Instance Information tab.

	![](images/file_server_enablement1a.png)

3.	When prompted to confirm enabling File Server, click Enable. The OIC icon turns orange and its status changes to Updating. Enablement can take several minutes.

	![](images/file_server_enablement2.png)

4.	Once complete, the OIC icon changes back to green with an Active status, and File Server shows as Enabled.

	![](images/file_server_enablement3.png)

## Task 2: Configure File Server
Configure File Server settings.

1.	Click Service Console from the buttons along the top of the Integration Instance Details page.
Oracle Integration is displayed.

2.	From the left navigation pane, select Settings, then File Server, then Settings which provides the overall health and to change its main settings.

	![](images/file_server_settings.png)
Note: Leave the default Authentication Type to be **Password or Key**

3.	Select **Files** from left Navigation pane

	![](images/file_server_files1.png)

4. From the Top right corner Click **Create** and create a folder **B2BWorkshop**

	![](images/file_server_files2.png)
5.	Select B2BWorkshop and create a folder structure as below

	![](images/file_server_files3.png)

Note: You will be using the above folder structure in next labs

6.	You will have to enable User and configure home folder. Select **Users** from left Navigation pane. Click on Edit against the User to enable the user and configure the following.

	*	Select Home Folder Type as **Custom**
	* Select Home Folder as **B2BWorkshop**

	![](images/user-permissions2.png)

	Click on the X icon to close the Window
	![](images/user-permissions3.png)

7. Notice that Folder permissions are not provided. Navigate to **Files** menu and Select Permissions
	![](images/user-permissions4.png)

8. Click on Add Permissions and Select the user to assign the permissions and select **Add**
	![](images/user-permissions5.png)

9.	Select All permissions and Save
	![](images/user-permissions6.png)

## Task 3: Connect to File Server with FTP Client

1.	Information required to connect to File Server. Navigate to File Server -> Settings and gather the information from the Settings page
	* IP Address & Port No
	* Username & Password (You would be using Password based Authentication Type)

2. Using your favorite FTP Client connect to the File Server using SFTP- SSH File Transfer Protocols

A Sample Configuration using FileZilla FTP Client
	![](images/user-permissions7.png)

Directory Listing created in File Server. If all the permissions are provided you should be able to read/write/list etc
	![](images/user-permissions8.png)

## Learn More

* [File Server](https://docs.oracle.com/en/cloud/paas/integration-cloud/file-server/file-server-overview.html)

## Acknowledgements
* **Author** - <Name, Title, Group>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>
