# Oracle Integration with Oracle ERP Cloud.

## Introduction

This lab walks you through  prerequisites for Integration with Oracle ERP Cloud.

Estimated Time: 10 -- minutes

### Objectives

* As an Integration Developer I want to be able to get hands on understanding of Real Time Sync pattern
* As an Integration Developer I want to be able to execute a lab exercise with pre-requisites w.r.t Real Time Sync pattern
* As an Integration Developer I want to be able to implement a use case w.r.t Real time sync pattern with some minimal guidance and not click by click steps (Created a Lab exercise)
* As an Integration Developer I want to be able to understand best practices and troubleshooting steps for Real Time Events
* As an Integration Developer I want to Import Recipes w.r.t Real Time Sync pattern and try out and execute


### Prerequisites

This lab assumes you have:
* An Oracle account



## Task 1: Learn Concepts and Understand Navigation

### Real Time Synchronization

There are couple of ways to Synchronize data from ERP Cloud into downstream applications

* Business Events - Oracle Integration subscribes to events from ERP Cloud
* Business Objects (APIs) – ERP Cloud invokes Oracle Integration Endpoints exposed using Business Object Interface


### *Application Setup and Configuration in ERP Cloud and OIC*

![Workflow for Configuring Prerequisite Steps to Process Business Events](images/Workflow.png =50%x*)
* Create Fusion Applications User (a.k.a Integration User ex. Bala.gupta)
a.	This user is configured in the OIC  ERP Adapter Connection
b.	This account is used to connect to the Fusion Application, and populate endpoint properties in the Adapter Connection Wizard
c.	This user (ex. bala.gupta) must have the below roles/privileges
    • Integration Specialist
    •	AttachmentsUser
    •	FND__MANAGE__CATALOG__SERVICE_PRIV Privilege needs to be added in Custom role ex: OIC__INTEGRATION__ROLE Ref: [Creating Custom Roles](https://docs.oracle.com/en/cloud/saas/applications-common/21a/faser/role-configuration-using-the-security-console.html#FASER2406016)

* Subscribe to Events in ERP Cloud

OIC ERP Cloud adapter supports subscribing to events [Ref: List of Events](https://docs.oracle.com/en/cloud/paas/integration-cloud/erp-adapter/oracle-erp-cloud-adapter-capabilities.html#GUID-9F62ACE1-54EB-4512-94BF-0A5EEB78E8B2) in several modules in ERP Cloud application. Ensure the events are enabled in the  ERP Cloud
ERP Cloud Supports Two categories of events
 1. Standard Business Events
 2. Custom Business Events

Standard Business Events

•	Enable Financial Events
•	Enable Payables Events
•	Enable Supplier Events
•	Enable Sales Order Events
•	Enable Project Portfolio Management Events


* Enabling Financial Events

Financial Events follows an Opt-in feature to enable flag in ERP Business Event metadata excepting to Payables Events. Some business events are disabled by default and you need to enable them before subscribing to them. To enable a business event, use the ERP Business Events REST Endpoints. We can identify the list of business events that are disabled by default using the [Get all business event](https://docs.oracle.com/en/cloud/saas/financials/21a/farfa/api-erp-business-events.html) records REST endpoint.

Note: Before following the steps to enable business events, it is always good idea to verify the [ERP Cloud Adapter documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/erp-adapter/oracle-erp-cloud-adapter-capabilities.html#GUID-010B3735-F0E8-4032-AEBA-69944F2C3A6D) by referring to “Enable By Using”. Follow the below steps if the “Enable By Using” column mentions “REST API”. Payables events needs an explicit Profile option to be set at Site level

* Security: Associate Access FSCM Integration Rest Service privilege to the Integration Account to use the ERP Business Events REST endpoints.
Privilege Name: Access FSCM Integration Rest Service
Privilege Code: FUN_FSCM_REST_SERVICE_ACCESS_INTEGRATION_PRIV

* Steps to Enable a Business Event using REST API
i.	Get the list of all business events to be enabled
ii.	Identify the ErpBusinessEventId of the business event you want to enable
iii.	Enable business event by ErpBusinessEventId
Worked Example
To enable the “ReceivablesInvoicePaid” business event code, follow these steps:

Get the list of all business events to be enabled
Use [Get all business event](https://docs.oracle.com/en/cloud/saas/financials/21a/farfa/api-erp-business-events.html) records REST endpoint to get the list of all business events
Sample URL: https://<server>/fscmRestApi/resources/<version>/erpBusinessEvents

![Sample Response:]
(images/Sample Response.png =50%x*)

Identify the ErpBusinessEventId of the desired business event

Use [Update the enabled indicator for a business event](https://docs.oracle.com/en/cloud/saas/financials/21a/farfa/api-erp-business-events.html) REST endpoint of the ErpBusinessEventId record

Sample URL: https://<server>/fscmRestApi/resources/<version>/erpBusinessEvents/< ErpBusinessEventId from previous step>
* Enabling Payables Events

Navigate to Setup and Maintenance in ERP Cloud. Search for the task “Manage Payables Profile Options”.

Search for “ORA_AP_ENABLE_BUSINESS_EVENTS” Profile Option and set the Site Level value to “Yes”

Sample Request Body:
{
    "EnabledFlag": true
}
![EnablePayableEvents](images/EnablePayableEvents.png =50%x*)
*Enabling Supplier Event
ERP Cloud Adapter supports subscribing to Supplier Create/Update Events. This is a new feature in ERP cloud and needs to be enabled.

These events include SupplierNumber and SupplierID attributes in the output payload. The Oracle Integration needs to use the event attributes to invoke the Suppliers REST API to get the required supplier information from Procurement Cloud.

From ERP Cloud Navigate to Navigator -> My Enterprise -> New Features -> Available Features. Search for “Suppliers” Functional Area

Select Enable and in Edit Suppliers, select the check box for Enable Outbound Supplier Profile Integration Using Oracle Integration Cloud
![EnableSupplierEvents](images/EnableSupplierEvents.png =50%x*)

* Enabling Sales Order Events
From Setup and Maintenance, Search for the task “Manage Business Event Trigger Points”. Enable the Event as per the functional requirement
![SalesOrdersEvents](images/SalesOrdersEvents.png =50%x*)
* Enabling Project Portfolio Management Events
From ERP Cloud Navigate to Navigator -> My Enterprise -> New Features -> Available Features. Below is the list of options and their path to enable the public events. Search for the event name as per the “Option to be Enabled”
Step | Description |
| --- | --- |

|Work Area|	Supported Public Event|	Opt in Work Area|	Option to be Enabled|
| --- | --- |--- |--- |
|Project Management|	Project Deliverable Status Changes|	Project Execution Management|	Generate Public Events on Project Deliverable Status Changes|
|Task Management|	Project Task Progress Status Changes|	Project Execution Management|	Generate Public Events on Project Task Progress Status Changes|
|Task Management|	Project Milestone Completion|	Project Execution Management|	Generate Public Events on Project Milestone Completion|
|Project Foundation|	Project Status Change|	Project Financial Management|	Publish Public Events on Project Status Change|
|Project Financial Management|	Publishing Financial Project Progress|	 Project Financial Management|
Project Control	Generate Public Events When Publishing Financial Project Progress|
|Project Financial Management|	Financial Project Plan Changes|	Project Financial Management|
Project Control	Generate Public Events on Change in Financial Project Plan|
Working Example:  Enabling “Generate Public Events on Change in Financial Project Plan”
![FinancialProjectPlan](images/FinancialProjectPlan.png =50%x*)

* Custom Business Events
Whenever we create a new custom object in Oracle ERP Cloud using Oracle Application Composer,  custom business events can be published explicitly. Once published, these business events can be subscribed from Oracle Integration (OIC). Business Events  are visible for selection when configuring the Oracle ERP Cloud Adapter in OIC as a trigger connection in the Adapter Endpoint Configuration Wizard.
For each Custom parent Object, OIC supports 3 Standard Events (Create, update, and delete). Any operations on child custom objects generate update events for the parent custom objects. When  a record is created, OIC is notified with record detail information through event handler framework on subscription to any related events
Enabling Custom Object Events
•	Create a custom object in ERP Cloud
•	Generate custom object pages (Create/Update Layouts)
•	Set the profile option, ZCX_CUSTOM_OBJECT_EVENTS, which is used to enable and register custom object events. The profile option is disabled by default
Note: Refer Support note 2535444.1  for detailed verbiage of the steps
•	Generate Integration Events from Application Composer. This step publishes custom objects as Integration events
![AppComposer](images/AppComposer.png =50%x*)

* Verify if the events are published in the custom catalog
$ curl --location --request GET 'https://<erp_cloud_host>/soa-infra/PublicEvent/customCatalog' --header 'Authorization: Basic abcedefghiji=' \
The custom object event should be available in the catalog
![CustomEvents](images/CustomEvents.png =50%x*)


* Import certificates into OIC


## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - <Name, Title, Group>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>
