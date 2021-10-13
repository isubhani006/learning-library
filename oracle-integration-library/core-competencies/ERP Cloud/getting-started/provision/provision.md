# Provision an Instance of Oracle Integration

## Introduction

This lab walks you through the process of provisioning an instance of Oracle Integration, assuming you don't already have one available to you.  If you do, you can skip this lab and move on to the next one.

Estimated Lab Time:  5 minutes

### Objectives

In this lab, you will:
* Provision Integration Instance

### Prerequisites

This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

### Background

If you just created a new Cloud account following the instructions in Getting Started, you must wait at least 30 minutes before you attempt to create an instance of Oracle Integration. (It could take anywhere between 10 and 30 minutes for a new user account to be fully provisioned) If you already have a Cloud account, you don't need to wait. Either way, make sure you've signed in to the Oracle Cloud as an Oracle Identity Cloud Service user before proceeding.

## Task 1: Create an Instance of Oracle Integration

1.  On the Oracle Cloud Get Started page, click the menu in the upper left corner to display the services you can provision. In the upper corner, note your selected region. Once created, instances are visible only in the region in which they were created.

	![](./images/hamburger.png)

2.  Open the navigation menu and click Developer Services. Under Application Integration, click Integration

	![](./images/integration-landing-page.png)

3.  From the Compartment list, click through the hierarchy of compartments and select the one in which to create the instance. You may need to expand the + icon to find the compartment to use. Compartments can contain other compartments. It may take several minutes for the new compartment to appear after the policy has been created.

	![](./images/compartment_expand.png)

4.	Click Create Integration Instance.

5.  Enter the following details, and click Create
| Field | Description |
| --- | --- |
| Display Name	  |Enter the display name for the instance. Note that the display name becomes part of the URL for accessing the instance.|
| Consumption Model	 |Lists consumption models available in this tenancy. Typically, one model (Metered) is displayed, but multiple consumption models are listed if your tenancy is enabled for more than one.    Available models include:<p></p><ul><li>Metered (Universal Credit)</li></ul><ul><li>Subscription (OIC4SaaS)</li></ul><ul><li>Oracle Integration Government</li></ul>|
| Edition  | Select Enterprise. Two editions are provided. See [Oracle Integration Editions](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/oracle-integration-editions.html#GUID-ED23D612-B34E-400D-8039-DBCEF5101AF4) to see what's licensed in each edition.|
| License Type	  |Select - Subscribe to a new Oracle Integration License|
| Message Packs	  |1|
| Access Token  | If this field is displayed, you are creating an instance as a non-federated user. Sign in as a federated user and restart creating an instance.|
| Show Advanced Options	| Custom Endpoint: Configure this tab to use a custom endpoint URL for the instance. The custom hostname you want to map to the instance must already be registered on a DNS provider and its SSL certificate stored as a secret in an OCI Vault. We may ignore this for now. For more information refer how to [Configure the Custom Endpoint](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/creating-oracle-integration-instance.html#GUID-930F40E8-5149-4091-9CDA-8E05C8449BA6)|

	![](./images/provision-oic-instance-1.png)

6.	When instance creation completes successfully, the instance shows as Active in the State column and you'll receive an email.

## Task 2: Accessing an Oracle Integration Instance

Navigate to an Oracle Integration instance in the Oracle Cloud Infrastructure Console to open it.

1.	Open the navigation menu and click Developer Services. Under Application Integration, click Integration.
2.	If needed, select a compartment in the Compartment field. The page is refreshed to show any existing instances in that compartment. If needed, select another region. Note that instances are visible only in the region in which they were created.
3.	At the far right, click Task menu, and select Service Console to access the Oracle Integration login page
	![](./images/oic-homepage.png)

## Learn More

* [Provisioning Oracle Integration Instance](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/creating-oracle-integration-instance.html#GUID-930F40E8-5149-4091-9CDA-8E05C8449BA6)


## Acknowledgements
* **Author** - <Name, Title, Group>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>
