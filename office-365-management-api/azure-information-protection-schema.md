---
ms.subservice: office-365-service-communications-api
ms.service: office-365
ms.TocTitle: Office 365 Management Activity API schema
title: Azure Information Protection (AIP) scanner events
description: Audit events from the Azure Information Protection (AIP) Unified Labeling client
ms.ContentId: 4c8793f3-d0b4-4d62-87b4-931cc31d99f5
ms.topic: reference (API)
ms.date: 09/26/2022
ms.localizationpriority: high
---

# Azure Information Protection (AIP) scanner events

The AIP Unified Labeling client includes the Add-in for Office, the Scanner, the Viewer for Windows, the client PowerShell, and the Classify-and-Protect shell extension for Windows. All these components generate audit events that appear in the Office 365 activity logs and can be queried using the Office 365 Management Activity API. For more information, see the [Office 365 Management Activity API reference](/office/office-365-management-api/office-365-management-activity-api-reference).

The five events (also called “AuditLogRecordType”) specific to AIP listed below, and more details about each can be found within the [API reference](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).

|Value| Member name | Description|
|:--|:--|:--|
|[93](#aipdiscover)|AipDiscover|AIP scanner events|
|[94](#aipsensitivitylabelaction)|AipSensitivityLabelAction|AIP sensitivity label events|
|[95](#aipprotectionaction)|AipProtectionAction|AIP protection events|
|[96](#aipfiledeleted)|AipFileDeleted|AIP file deletion events|
|[97](#aipheartbeat)|AipHeartBeat|AIP heartbeat events|
 
## AipDiscover

The following tables contain information related to Azure Information Protection (AIP) scanner events.

| Event | Description |
|:--|:--|
|ClientIP | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. The IP address is displayed in either an IPv4 or IPv6 address format.|
|CreationTime | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|DataState | Describes the state of the data.
|Id| GUID of the current record. |
|ObjectId | File full path (URL). For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user.|
|Operation | Describes type of access.
|OrganizationId | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|RecordType | The type of operation indicated by the record. See the AuditLogRecordType table for details on the types of audit log records. For a complete updated list and full description of the Log RecordType, see the [Microsoft 365 Compliance audit log activities via O365 Management API](https://techcommunity.microsoft.com/t5/security-compliance-and-identity/microsoft-365-compliance-audit-log-activities-via-o365/ba-p/2957297) blog post. Here we only list the relevant MIP Record types.|
|Scope | Was this event created by a hosted O365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to O365.|
|SensitiveInfoTypeData| Stores the datatype of the Sensitive Info type data.|
|SensitivityLabelId| The current MIP sensitivity label GUID. Use cmdlt Get-Label to get the full values of the GUID.|
|UserId|The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|UserKey| An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|UserType| The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|Version| Version ID of the file in the operation. |
|Workload|Stores The Office 365 service where the activity occurred.|
|ApplicationId  |  The ID of the application performing the operation.|
|ApplicationName |	Friendly name of the application performing the operation. Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName     |  The relevant process name, eg. Outlook, msip.app, WinWord.|
|Platform   |  Device platform (Win, OSX, Android, iOS) |
|DeviceName |  The device on which the activity happened.|
|Location   |   The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare and cloud.|
|ProductVersion  |   Version of the AIP client. |
|ProtectionType| Protection type can be template or ad-hoc.|
|TemplateId |  TemplateID parameter to get a specific template. The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.|
|IsProtected|  Whether protected: True/False|
|ProtectionOwner| Rights Management owner in UPN format.|

## AipSensitivityLabelAction

The following tables contain information related to AIP sensitivity label events.

| Event | Description |
|:--|:--|
|PSComputerName |   Computer Name |
|RunspaceId |   The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName |   The value is False for documented edited in Office 365.  |
|RecordType|  | Shows the value of Lable Action. The operation type indicated by the record. For more information, see the [full list of record types](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).|
|CreationDate|  The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId|  The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|Operation| The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities:</br>SensitivityLabelApplied, SensitivityLabelUpdated, SensitivityLabelRemoved, SensitivityLabelPolicyMatched, SensitivityLabeledFileOpened.|
|Identity|   The identity of the user or service to be authenticated.|
|IsValid|  Boolean | 
|ObjectState|  Specifies the state of the object.|
|SensitiveInfoTypeData|Stores the datatype of the Sensitive Info Type Data |
|ProtectionType|Protection type can be template or ad-hoc.|
|TemplateId |TemplateID parameter to get a specific template.The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.|
|IsProtected|Whether protected: True/False|
|IsProtectedBefore|	Whether the content was protected before change: True/False|
|ApplicationId| Corresponds to the Azure AD Application ID. |
|ApplicationName| Application friendly name of the application performing the operation.</br>Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName| Process that hosts MIP SDK.|
|Platform|	Device platform (Win, OSX, Android, iOS). |
|DeviceName|The name of the user's device.|
|Location| The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare and cloud.|
|ProductVersion| Version of the Azure Information Protection client that performed the audit action.|
|DataState| Specifies the state of the data.|

## AipProtectionAction

This contains information related to AIP protection events.

## AipFileDeleted

This event contains information related to AIP file deletion events.

## AipHeartBeat

The following tables contain information related to AIP heartbeat events.

| Event | Description |
|:--|:--|
|PSComputerName| Computer Name |
|RunspaceId| The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName| The value is false for a documented edited in Office 365.|
|RecordType| Shows the value of Label Action. The operation type indicated by the record. For a complete updated list and full description of the Log RecordType, please refer to this article. Here we are only listing the relevant MIP Record types.|
|CreationTime| The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId|  The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|Operation| The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities. SensitivityLabelApplied ,SensitivityLabelUpdated,SensitivityLabelRemoved ,SensitivityLabelPolicyMatched ,SensitivityLabeledFileOpened.|
|Identity|The identity of the user or service to be authenticated.|
|ObjectState|State of the Object after the current event|
|ApplicationId | The application that where the activity happened and displayed in GUID.|
|ApplicationName |	Application friendly name of the application performing the operation.Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName| Process name of the Office application. |
|Platform | The platform on which the activity happened. For example, Windows. |
|DeviceName | Device the event was recorded on. |
|ProductVersion |Version of the Azure Information Protection client that performed the audit action.|
|UserId | The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|ClientIP | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null.The IP address is displayed in either an IPv4 or IPv6 address format.|
|Id| GUID of the current record |
|RecordType|Shows the value of Label Action. The operation type indicated by the record. For a complete updated list and full description of the Log RecordType, please refer to this article. Here we are only listing the relevant MIP Record types.|
|CreationTime| The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|Operation|The name of the user or admin activity. For a description of the most common operations/activities, see Search the audit log in the Office 365 Protection Center ([title](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide)|
|OrganizationId| The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType| The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|UserKey|  An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|Workload|Stores the The Office 365 service where the activity occurred.|
|Version|Version of the Azure Information Protection client that performed the audit action|
|Scope| Specifies scope.|
