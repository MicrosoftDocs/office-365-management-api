---
ms.TocTitle: OMEPortal
title: OMEPortal
description: OMEPortal
ms.ContentId: c6b4789e-d0e8-44d4-806c-0a63d1218463
ms.topic: reference
ms.date: 07/25/2023
ms.localizationpriority: high
---

# OMEPortal

Microsoft Purview Message Encryption is a service that allows organizations to send encrypted mail to any recipients.

OMEPortal is a type of event that is recorded in the Office 365 Unified Audit Log.  It represents any activity to access an encrypted message by an external recipient using the encrypted message portal.  This event is useful to determine when and who accessed the portal.

- authenticate
- open message
- view attachment
- download attachment
- reply/forward message

## Access the Office 365 Unified Audit Log

The audit logs can be accessed using the following methods.

- The [audit log search tool](/office/office-365-management-api/aipsensitivitylabelaction#audit-log-search-tool) in the Microsoft Purview compliance portal.
- The [Search-UnifiedAuditLog](/office/office-365-management-api/aipsensitivitylabelaction#search-unified-audit-log-in-powershell) cmdlet in Exchange Online PowerShell.
- The [Office 365 Management Activity API](/office/office-365-management-api/aipsensitivitylabelaction#office-365-management-activity-api).

### Audit log search tool

1. Go to the [Microsoft Purview compliance portal](https://sip.compliance.microsoft.com/homepage) and sign in.
2. In the left pane of the compliance portal, select **Audit**.

    > [!NOTE]
    > If you don't see **Audit** in the left pane, see [Roles and role groups in Microsoft Defender for Office 365 and Microsoft Purview compliance](/microsoft-365/security/office-365-security/scc-permissions?view=o365-worldwide&preserve-view=true) for information about permissions.

3. On the **New Search** tab, set Record type to **OMEPortal** and configure the other parameters.
:::image type="content" source="images/audit-new-search.png" alt-text="Audit dialog showing new search options":::

For more information on viewing the audit logs in the Microsoft Purview compliance portal, see [Audit log activities](/microsoft-365/compliance/audit-log-activities?view=o365-worldwide).

### Search Unified Audit Log in PowerShell

To access the Unified Audit Log using PowerShell, first open a PowerShell window and connect to Exchange Online PowerShell. For instructions, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps).

#### Search-UnifiedAuditLog cmdlet

The Search-UnifiedAuditLog cmdlet is a PowerShell command that can be used to search the Office 365 Unified Audit Log. The Unified Audit Log is a record of user and administrator activity in Office 365 that can be used to track events. For best practices on using this cmdlet, see [Best Practices for using Search-UnifiedAuditLog](/office/office-365-management-api/aip-unified-audit-logs-best-practices).

To extract the OMEPortal events from the Unified Audit Log using PowerShell, you can use the following command. This will search the Unified Audit Log for the specified date range and return any events with the record type "OMEPortal". The results will be exported to a CSV file at the specified path.

```powershell
Search-UnifiedAuditLog -RecordType OMEPortal -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date) | Export-Csv -Path <output file> 
```

The following is an example of OMEPortal event from PowerShell, with opening message in the encrypted message portal activity.

```json

RecordType   : OMEPortal 

CreationDate : 3/22/2023 7:00:59 PM 

UserIds      : admin@M365x965352.onmicrosoft.com 

Operations   : MessageAccess 

AuditData    : {"CreationTime":"2023-03-22T19:00:59","Id":"a3500d45-6ab3-4971-a762-a01a846015d0","Operation":"MessageAccess","OrganizationId":"84b06764-3e5e-4f51-9a78-046a7f38b59b","RecordType":154,"ResultStatus":"Success","UserKey":"admin@M365x965352.onmicrosoft.com","UserType":0,"Version":1,"Workload":"Exchange","UserId":"admin@M365x965352.onmicrosoft.com","AuthenticationMethod":"Google","AuthenticationStatus":0,"MessageId":"<MW5PR18MB5094602B59DFEC335A3C5E58AA869@MW5PR18MB5094.namprd18.prod.outlook.com>","OperationProperties":[{"Name":"MailSubject","Value":"Support case 1234567"}],"OperationStatus":0,"Recipient":"samschan.msft@gmail.com","Sender":"admin@M365x965352.onmicrosoft.com"} 

ResultIndex  : 6 

ResultCount  : 7 

Identity     : a3500d45-6ab3-4971-a762-a01a846015d0 

IsValid      : True 

ObjectState  : Unchanged 

```

Use the following command to specifically search for the scenario in which user views an attachment. An example of the result of the PowerShell cmdlet is also shown below.

```PowerShell
Search-UnifiedAuditLog -Operations AttachmentReview -RecordType OMEPortal -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date)
```

The following is an example of AipSensitivityLabelAction event from PowerShell, with sensitivity label updated.

```json

RecordType   : OMEPortal 

CreationDate : 3/22/2023 7:04:09 PM 

UserIds      : admin@M365x965352.onmicrosoft.com 

Operations   : AttachmentReview 

AuditData    : {"CreationTime":"2023-03-22T19:04:09","Id":"ef29c387-c81b-4812-9020-90b378d92cde","Operation":"AttachmentReview","OrganizationId":"84b06764-3e5e-4f51-9a78-046a7f38b59b","RecordType":154,"ResultStatus":"Failure","UserKey":"admin@M365x965352.onmicrosoft.com","UserType":0,"Version":1,"Workload":"Exchange","UserId":"admin@M365x965352.onmicrosoft.com","AttachmentName":"Form.docx","AuthenticationMethod":"OTP","AuthenticationStatus":0,"MessageId":"<MW5PR18MB5094CD4A4C6D8A5636154FEBAA869@MW5PR18MB5094.namprd18.prod.outlook.com>","OperationProperties":[{"Name":"MailSubject","Value":"Support case 987654"}],"OperationStatus":1,"Recipient":"samschan.msft@gmail.com","Sender":"admin@M365x965352.onmicrosoft.com"} 

ResultIndex  : 4 

ResultCount  : 7 

Identity     : ef29c387-c81b-4812-9020-90b378d92cde 

IsValid      : True 

ObjectState  : Unchanged 
```

The other operations that can be specifically searched for within OMEPortal include AttachmentDownload, AuthenticationRequest, MessageAccess, and MessageForward.

## Attributes of the OMEPortal event

The following table contains information related to OMEPortal events.

|Event      |Type         |Description |
|-----------|------------|-------------|
|Activity  | String      | The activity type for the audit log. For OMEPortal, operations can include: <br/>- AttachmentDownload<br/>- AttachmentReview<br/>- AuthenticateRequest<br/>- MessageAccess<br/>- MessageForward |
|AttachmentName |String  |Part of AuditData. <br/>The name of the attachment in the message.|
|AuditData | String      |The log details on the OMEPortal activity.<br/>|
|AuthenticationMethod  |String |Part of AuditData. <br/>The type of authentication used to login to portal. Values are: <br/>- OTP <br/>- Google <br/>- Yahoo <br/>- Microsoft |
|AuthenticationStatus  |Double |The status of the authentication. <br/>0 = success<br/>1= failure  |
|CreationTime |Date/time |The date and time in Coordinated Universal Time (UTC) when the user performed the activity. |
|Id        |GUID         |Unique identifier of an audit record. |
|MessageId |String       |The message ID. |
|Operation |String       |Same value as activity. |
|OperationProperties     |String |Part of AuditData. <br/>Contains the email subject. |
|Recipient  |String      |Part of AuditData. <br/>The email address of user accessing the message in the encrypted message portal. |
|RecordType |Double      |The type of operation indicated by the record. 154 represents an OMEPortal record. |
|ResultsStatus |String   |Operation was either successful or a failure. |
|Sender   |String        |Part of AuditData. <br/>The email address of user that sent the encrypted message. |
|Workload |String        |Exchange to indicate email in the encrypted message portal. |
|UserId   |String        |The User Principal Name (UPN) of the user in your organization who sent the encrypted mail the action that resulted in the record being logged. |
