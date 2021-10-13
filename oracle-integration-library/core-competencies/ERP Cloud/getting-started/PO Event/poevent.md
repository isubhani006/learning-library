# Oracle Integration B2B - Inbound

## Introduction

ACME Corp sends an 850 Purchase Order EDI document to Trading Partner Dell Inc. ACME Corp had configured OIC B2B message exchange agreement to send Purchase Order EDI document to External Trading Partner.

This integration takes the input as XML from a Rest Client. In a real world use case you would have the XML originating from a Source System like ERP Cloud or NetSuite. A Backend App Integration transforms XML into EDI X12 format using EDI Translate functionality and sends the EDI document to B2B Integration to send across to External Trading Partner (Dell Inc)


![B2BArchitecture diagram](images/b2b-outbound1.png =50%x*)

High level steps of the Integration:
| Step | Description |
| --- | --- |
| 1 | Your backend application has a requirement to send a business transaction to an external trading partner. It triggers your outbound backend integration by sending it as a notification message.|
| 2 |Your backend integration instance receives the notification that includes the application message in the backend application message format|
| 3 |Using a mapper, the backend application message is transformed into a B2B canonical XML format.|
| 4 |The canonical XML message is provided to a B2B action for outbound translation (the action named EDI-Generate above). A trading partner is specified as an input to the B2B action. The B2B action translates the canonical XML message to a native EDI format (X12 or EDIFACT) and persists it in the Oracle Integration persistence store. A unique ID is assigned to it.|
| 5 |Based on the target trading partner, the current document type, and the outbound agreements defined for the trading partner, an appropriate B2B integration for sending messages is triggered. The message ID is handed to it.|
| 6 |The B2B integration for sending messages instance starts and receives the message ID at its REST Adapter trigger endpoint.|
| 7 |The B2B integration for sending messages instance uses an adapter (AS2 or FTP) to pack the message and then transmit it to the external trading partner through the AS2 or FTP protocol.|


Estimated Lab Time: 60 minutes

### About Product/Technology
Oracle Integration - B2B

### Objectives

In this lab, you will create a basic integration flow:
This labs takes the input as XML from a Rest Client . In a real world use case you would have the XML originating from a Source System like ERP Cloud or NetSuite. A Backend App Integration transforms XML into EDI X12 format using EDI Translate functionality and sends the EDI document to B2B Integration to send across to External Trading Partner (Dell Inc)


*This is the "fold" - below items are collapsed by default*

## **STEP 1**: Create Connection

## **STEP 2**: Create an Integration

Let's create a basic, outbound integration flow that receives an XML document through a REST request, converts it to EDI X12 format, and invokes corresponding trading partner.
Note:
This integration flow uses REST for simplicity. You can substitute the REST Adapter trigger connection with any other adapter, such as the FTP Adapter, NetSuite Adapter, ERP Cloud Adapter, available in Oracle Integration


1. In the navigation pane, click Integrations.

2. On the Integrations page, click Create.

3. Select App Driven Orchestration as the style to use. The Create New Integration dialog is displayed.

    enter the Name of the integration as per the value given below and then click on create
    ```
    <copy>PO Backend</copy>
    ```

4. Change Layout to Horizontal

## **STEP 3**: Configure the REST Adapter Trigger Connection
On the integration canvas, click the start node and select Sample REST Endpoint Interface as the trigger connection.
The Adapter Endpoint Configuration Wizard opens

1. On the Basic Info page, enter the following details
    - In the What do you want to call your endpoint? field, enter the below value.
    ```
    <copy>Receive-App-Msg</copy>
    ```
    - Enter the endpoint's relative resource URI as per the value given below
    ```
    <copy>/outbound</copy>
    ```
    - Select POST as the action to perform on the endpoint.
    - Select to configure both request and response for this endpoint and click on Next
    ![REST Endpont Wizard diagram](images/b2b-outbound2.png =50%x*)

2. On the Request page:
    - Select XML Schema in the Select the request payload format field.
    - Click on Choose File and upload the file [AcmePurchaseOrder.xsd](files/AcmePurchaseOrder.xsd?download=1) and click on Next
    ![Request diagram](images/b2b-outbound3.png =50%x*)
3. In the Response page:
    - Select XML Schema in the response payload format field
    - Click on Choose File and upload the file [PurchaseOrderResult.xsd](files/PurchaseOrderResult.xsd?download=1)
    - Click Next, and on the Summary page, click Done to complete the REST Adapter configuration.
    - The integration flow is now represented as follows in the canvas and click on Save to save your integration flow
    ![Response diagram](images/b2b-outbound4.png =50%x*)
## **STEP 4**: Configure the EDI Translate Action
Add an EDI translate action to the flow to translate XML document to an EDI document
1. On the right side of the canvas, click Actions  , drag B2B, and drop it after the first Receive-App-Msg element.
The Configure B2B Action wizard opens
    - On the Basic Info page, enter the name as per the value given below for the action and select a mode as “B2B Trading Partner mode”, and click Next
    ```
    <copy>EDI-Generate</copy>
    ```
    ![BasicInfor diagram](images/b2b-outbound5.png =50%x*)
    - Select Operation as “Outbound”
   ![Operation diagram](images/b2b-outbound6.png =50%x*)
    - Select Document Definition as Purchase Order (You must have created this as part of B2B activities) and click on Next
   ![DocumentDefinition diagram](images/b2b-outbound7.png =50%x*)
    - Review the Summary page, click on Done to complete the configuration and click on Save to save your integration flow. Click on RESET if required for a better view of your integration flow.
   Note that the corresponding mapping element is automatically added to the integration flow
   ![Summary diagram](images/b2b-outbound8.png =50%x*)
## **STEP 5**: Configure Mapping Actions
Configure data mappings for the EDI-Generate action and Receive-App-Msg action in order to successfully parse the incoming XML message and translate it to EDI message.

1. Click the Map to EDI-Generate action and select Edit.
2. Click on Developer mode
   ![Devmode diagram](images/b2b-outbound9.png =50%x*)
3. From Source, expand the root element, expand AcmePurchaseOrder and From Target, expand the root element, expand TranslateInput, expand edi-xml-document, expand transaction-data and map all the mandatory elements given below.

| Source | Target |
| --- | --- |
| “00” | BEG01 |
| “NE” | BEG02 |
| orderNumber | BEG03 |
| drag and drop format-DateTime function from the Components onto the BEG05 and create a string as given here: xp20:format-dateTime (/nssrcmpr:execute/tns:AcmePurchaseOrder/tns:orderDate, "[Y0001][M01][D01]" )| BEG05 |
|  count (/nssrcmpr:execute/tns:AcmePurchaseOrder/tns:lineItems )  | CTT01 |
| totalAmount | CTT02 |
| “2L” | CUR01 |
| currencyCode | CUR02 |
| currencyConversionRate | CUR03 |
| lineItems | Loop-PO1 |
| lineItems->SKU | PO101 |
| lineItems->Quantity | PO102 |
| lineItems->unitOfMeasure | PO103 |
| lineItems->price | PO104 |
| tradingPartnerId | Application Partner ID(This element is there under TranslateInput Node) |

Once you are done with the validation, test it and results should look like the screenshot given below.
   ![mapperTest diagram](images/mapperTest.png =50%x*)

4. Click Validate and then Close.
5. Save your integration flow.
## **STEP 6**: Switch action after EDI-Generate activity
1. Add a switch action after the EDI-Generate activity
    - For the If branch, Enter the Expression Name as “Success or Warning” and enter the following expression under Expression section. (You may have to select Expression Mode to enter the value given below). If there is an error on namespaces then you can search for “translation-status” and select that element for mapping.
    ```
    <copy>$EDI-Generate/nsmpr6:executeResponse/nsmpr9:TranslateOutput/nsmpr9:translation-status ="Success"</copy>
    ```
    Note:Your namespace prefix may include different values than nsmpr9 and nsmpr6.
This expression indicates that if TranslateOutput > translation-status has a value of Success, then take this route. This is referred to as the success route
    - Click on Validate and Click on Close and save your integration flow
    - In the success route: Add “Integration” Action ->Enter name as "callTradingPartner" and select USGE FTP Send Integration (OR any other outbound B2B integration which you have created) and click on Next->Click on Next ->Click on Done and save your integration flow

    - Edit Map to callTradingPartner -> Select Developer mode and From Source, expand EDI-Generate -> executeResponse->TranslateOutput
| Source | Target |
| --- | --- |
| b2b-message-reference | components.schemas.request-wrapper->messages->b2b-message-reference |
| trading-partner | components.schemas.request-wrapper->trading-partner |
| connectivity-properties-code | Connectivity Properties->Localintegration->code |
| connectivity-properties-version | Connectivity Properties->Localintegration->version |

    - Mappings looks like the below diagram
    ![mappings diagram](images/b2b-outbound12.png =50%x*)

    - Click on Validate and Click on Close and save your integration flow
    - In Otherwise route: Add Throw New Fault Action ->Enter name as “Error” ->click on Create and map the below elements

    $EDI-Generate/nsmpr7:executeResponse/nsmpr10:TranslateOutput/nsmpr10:validation-error to Code
    AND
    $EDI-Generate/nsmpr7:executeResponse/nsmpr10:TranslateOutput/nsmpr10:validation-error-report to Reason
    ![mappings diagram](images/b2b-outbound14.png =50%x*)
    - Validate and Close -> Save your integration flow.
    ![finalflow diagram](images/b2b-outbound15.png =50%x*)

## **STEP 7**: After Switch activity
1. Edit Map to Receive-App-Msg activity.
2. From Source, expand EDI-Generate Response ->executeResponse->TranslateOutput and From Target, expand Purchase Order Result and map the following elements as per the table given below.
| Source | Target |
| --- | --- |
| Translation Status | Translation Status |
| Validation Error Report | Validation Error Report |

3. After completing all the mappings, you can cross check by leveraging Test feature available on Mapper and Click on Validate and Click on Close
4. click Actions Menu  in the top-right corner of canvas, and select Tracking.
5. In the resulting dialog, select orderNumber on the left and move it to the table on the right and click on Save.
6. Check for errors, Save the integration and click Close
   ![finalflow1 diagram](images/finalflow.png =50%x*)

## **STEP 8**: Activate the integration

1. On the Integrations page, click on the activate button against your integration to activate it
2. select “Enable Tracing”, “Include Payload” options and Click Activate in the Activate Integration dialog
3. To execute your sample integration, send a request from a REST client tool, such as Postman OR you can use Oracle Integration console to test. Let us use Oracle Integration Test Console.
4. You have two xml files [USGEPO.xml](files/USGEPO.xml?download=1) and [DellIncPO.xml](files/DellIncPO.xml?download=1), download and open one xml file and copy the data and paste it in the body of the request console and click on Test
  ![TestConsole diagram](images/b2b-outbound17.png =50%x*)
5. Go to Monitoring->Integrations->Tracking-> Cross check your backend integration and trading partner integration ran successfully and now repeat the test with another xml file which would trigger another trading partner
6. If you have FTP Client installed on your machine, you can login using the FTP details provided to you and cross check your edi file created under folders /B2BWorkshop/B2BTPUSGEOut and /B2BWorkshop/B2BTPDELLOut
7. In conclusion, you can use Oracle Integration to accept XML message and convert it into EDI format and send it to the trading partners dynamically

*At the conclusion of the lab add this statement:*
You may now [proceed to the next lab](#next).

## Learn More

* [Oracle B2B Documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/btob.html)
* [What's New](https://docs.oracle.com/en/cloud/paas/integration-cloud/whats-new/index.html)

## Acknowledgements
* **Author** - <Subhani, Director, OIC Product Management>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Subhani, Group, Month Year>
* **Workshop (or Lab) Expiry Date** - <Month Year> -- optional, use this when you are using a Pre-Authorized Request (PAR) URL to an object in Oracle Object Store.
