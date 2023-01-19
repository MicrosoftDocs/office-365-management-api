---
ms.TocTitle: AipProtectionAction
title: AipProtectionAction
description: AipProtectionAction
ms.ContentId: ce562783-e925-4f86-8044-b9dd4e93e784
ms.topic: reference
ms.date: 01/17/2023
ms.localizationpriority: high
---

# AipProtectionAction

Azure Information Protection is a service that allows organizations to classify and label sensitive data, and apply policies to control how that data is accessed and shared.

AipProtectionAction is a type of event that is recorded in the Office 365 Unified Audit Log. It represents an attempt to apply protection to a file or email. The event is useful because it shows how data is protected within an organization. 

## Access the Office 365 Unified Audit Log

The audit logs can be accessed using the following methods:
- The [audit log search tool](#audit-log-search-tool) in the Microsoft Purview compliance portal.
- The [Search-UnifiedAuditLog](#search-unified-audit-log-in-powershell) cmdlet in Exchange Online PowerShell.
- The [Office 365 Management Activity API](/office/office-365-management-api/office-365-management-activity-api-reference).

## Audit log search tool

1. Go to https://compliance.microsoft.com and sign in.
2. In the left pane of the compliance portal, select Audit.
3. On the Search tab, set Record type to **AipProtectionAction** and configure the other parameters.
![AipProtectionAction audit configurations](images/aip-protection-action-search.png) 
4. Select Search to run the search using the critera. Click on an event to view the results. 

For more information on the Audit log search tool, see [audit log search tool](audit-log-search.md).

## Access the Search Unified Audit Log in PowerShell

To access the Unified Audit Log using PowerShell, first connect to an Exchange Online PowerShell session by completing the following steps. 

### Establish remote Powershell session

This will establish a remote PowerShell session with Exchange Online. Once the connection is established, run Exchange Online cmdlets to manage your Exchange Online environment. 

Open a PowerShell window and run the Install-Module -Name ExchangeOnlineManagement command to install the Exchange Online Management module. This module provides cmdlets that can be used to manage Exchange Online.

1. Connect-IPPSSession is a PowerShell cmdlet used to create a remote connection to an Exchange Online PowerShell session.
2. Import-Module ExchangeOnlineManagement is a PowerShell cmdlet used to import the Exchange Online Management module into the current PowerShell session.

```powershell
# Import the PSSSession and Exchange Online cmdlets
Connect-IPPSSession
Import-Module ExchangeOnlineManagement
```

#### Connect with a specific user

Command to prompt for a specific user for  your Exchange Online credentials.

```powershell
$UserCredential = Get-Credential 
```
Command to connect to Exchange Online using the provided credentials. 

```powershell
 Connect-ExchangeOnline -Credential $UserCredential -ShowProgress $true 
```

#### Connect with credentials in the current session

Connect to Exchange Online using the credentials in the current session.

```powershell
Connect-ExchangeOnline
```

### Search-UnifiedAuditLog cmdlet

The Search-UnifiedAuditLog cmdlet is a PowerShell command that can be used to search the Office 365 Unified Audit Log. The Unified Audit Log is a record of user and administrator activity in Office 365 that can be used to track events. For best practices on using this cmdlet, see [Best Practices for using Search-UnifiedAuditLog](BestPractices)

To extract the AipProtectionAction events from the Unified Audit Log using PowerShell, you can use the following command. This will search the Unified Audit Log for the specified date range and return any events with the record type "AipProtectionAction". The results will be exported to a CSV file at the specified path.

```powershell
Search-UnifiedAuditLog -RecordType AipProtectionAction -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date) | Export-Csv -Path <output file>
```

> [!NOTE]
> This is just an example of how the Search-UnifiedAuditLog cmdlet can be used. You may need to adjust the command and specify additional parameters based on your specific requirements. For more information on using PowerShell for unified audit logs, see [search unified audit log](/powershell/module/exchange/search-unifiedauditlog).

## Office 365 Management Activity API

In order to be able to query the Office 365 Management API endpoints, you will need to configure your application with the right permissions. For a step-by-step guide, see [Get started with Office 365 Management APIs](https://learn.microsoft.com/office/office-365-management-api/get-started-with-office-365-management-apis).

### Attributes of the AipProtectionAction event

Event | Type | Description
---|---|---
ApplicationId	| GUID | The ID of the application performing the operation.
ApplicationName | String | Friendly name of the application performing the operation. (Outlook, OWA, Word, Excel, PowerPoint, etc.)
ClientIP | IPv4/IPv6 | The IP address of the device that was used when the activity was logged. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity.
CreationTime | Date/time | The date and time in Coordinated Universal Time (UTC) when the user performed the activity.
DeviceName | String | The device on which the activity happened.
Id | GUID | Unique identifier of an audit record.
Operation | String | The operation type for the audit log. For AipProtectionAction, operations can include: </br>- SensitivityLabelApplied </br>- SensitivityLabelUpdated </br>- SensitivityLabelRemoved </br>- SensitivityLabelPolicyMatched </br>- SensitivityLabeledFileOpened
OrganizationId | GUID | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.     
Platform | Double | The platform where the activity occurred from. </br>0 = Unknown</br>1 = Windows</br>2 = MacOS </br>3 = iOS</br>4 = Android</br>5 = Web Browser
ProcessName | String | The relevant process name (Outlook, MSIP.App, WinWord, etc.) 
ProductVersion | String | Version of the AIP client.
RecordType | Double | The type of operation indicated by the record.
Scope | Double | 0 represents that the event was created by a hosted O365 service. 1 represents that the event was created by an on-premises server.                                         
UserId | String | The User Principal Name (UPN) of the user who performed the action that resulted in the record being logged.
UserKey | GUID | An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.           
UserType | Double | The type of user that performed the operation. </br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy
Version | Double | Version ID of the file in the operation.                      
Workload | String | Stores the Office 365 service where the activity occurred (Exchange, SharePoint, OneDrive, etc).
