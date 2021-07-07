---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management Activity API schema
title: Office 365 Management Activity API schema
description: The Office 365 Management Activity API schema is provided as a data service in two layers - Common schema and service-specific schema.
ms.ContentId: 1c2bf08c-4f3b-26c0-e1b2-90b190f641f5
ms.topic: reference (API)
ms.date: 
localization_priority: Priority
---

# Office 365 Management Activity API schema

The Office 365 Management Activity API schema is provided as a data service in  two layers:

- **Common schema**. The interface to access core Office 365 auditing concepts such as Record Type, Creation Time, User Type, and Action as well as to provide core dimensions (such as User ID), location specifics (such as Client IP address), and service-specific properties (such as Object ID). It establishes consistent and uniform views for users to extract all Office 365 audit data in a few top level views with the appropriate parameters, and provides a fixed schema for all the data sources, which significantly reduces the cost of learning. Common schema is sourced from product data that is owned by each product team, such as Exchange, SharePoint, Azure Active Directory, Yammer, and OneDrive for Business. The Object ID field can be extended by Microsoft 365 product teams to add service-specific properties.

- **Service-specific schema**. Built on top of the Common schema to provide a set of Microsoft 365 service-specific attributes; for example, SharePoint schema, OneDrive for Business schema, and Exchange admin schema.

## Office 365 Management API schemas

This article provides details on the Common schema as well as service-specific schemas. The following table describes the available schemas.

|Name of schema|Description|
|:-----|:-----|
|[Common schema](#common-schema)|The view to extract Record Type, User ID, Client IP, User type and Action along with core dimensions such as user properties (such as UserID), location properties (such as Client IP), and service-specific properties (such as Object Id).|
|[SharePoint Base schema](#sharepoint-base-schema)|Extends the Common schema with the properties specific to all SharePoint audit data.|
|[SharePoint File Operations](#sharepoint-file-operations)|Extends the SharePoint Base schema with the properties specific to file access and manipulation in SharePoint.|
|[SharePoint Sharing schema](#sharepoint-sharing-schema)|Extends the SharePoint Base schema with the properties specific to file sharing.|
|[SharePoint schema](#sharepoint-schema)|Extends the SharePoint Base schema with the properties specific to SharePoint, but unrelated to file access and manipulation.|
|[Project schema](#project-schema)|Extends the SharePoint Base schema with the properties specific to Project.|
|[Exchange Admin schema](#exchange-admin-schema)|Extends the Common schema with the properties specific to all Exchange admin audit data.|
|[Exchange Mailbox schema](#exchange-mailbox-schema)|Extends the Common schema with the properties specific to all Exchange mailbox audit data.|
|[Azure Active Directory Base schema](#azure-active-directory-base-schema)|Extends the Common schema with the properties specific to all Azure Active Directory audit data.|
|[Azure Active Directory Account Logon schema](#azure-active-directory-account-logon-schema)|Extends the Azure Active Directory Base schema with the properties specific to all Azure Active Directory logon events.|
|[Azure Active Directory Secure STS Logon schema](#azure-active-directory-secure-token-service-sts-logon-schema)|Extends the Azure Active Directory Base schema with the properties specific to all Azure Active Directory Secure Token Service (STS) logon events.|
|[Azure Active Directory schema](#azure-active-directory-schema)|Extends the Common schema with the properties specific to all Azure Active Directory audit data.|
|[DLP schema](#dlp-schema)|Extends the Common schema with the properties specific to Data Loss Prevention events.|
|[Security and Compliance Center schema](#security-and-compliance-center-schema)|Extends the Common schema with the properties specific to all Security and Compliance Center events.|
|[Security and Compliance Alerts schema](#security-and-compliance-alerts-schema)|Extends the Common schema with the properties specific to all Office 365 security and compliance alerts.|
|[Yammer schema](#yammer-schema)|Extends the Common schema with the properties specific to all Yammer events.|
|[Data Center Security Base schema](#data-center-security-base-schema)|Extends the Common schema with the properties specific to all data center security audit data.|
|[Data Center Security Cmdlet schema](#data-center-security-cmdlet-schema)|Extends the Data Center Security Base schema with the properties specific to all data center security cmdlet audit data.|
|[Microsoft Teams schema](#microsoft-teams-schema)|Extends the Common schema with the properties specific to all Microsoft Teams events.|
|[Microsoft Defender for Office 365 and Threat Investigation and Response schema](#microsoft-defender-for-office-365-and-threat-investigation-and-response-schema)|Extends the Common schema with the properties specific to Defender for Office 365 and threat investigation and response data.|
|[Submission schema](#submission-schema)|Extends the Common schema with the properties specific to user and admin submissions in Microsoft Defender for Office 365.|
|[Automated investigation and response events schema](#automated-investigation-and-response-events-in-office-365)|Extends the Common schema with the properties specific to Office 365 automated investigation and response (AIR) events. To see an example, see [Tech Community blog: Improve the Effectiveness of your SOC with Microsoft Defender for Office 365 and the O365 Management API](https://techcommunity.microsoft.com/t5/microsoft-security-and/improve-the-effectiveness-of-your-soc-with-office-365-atp-and/ba-p/1525185).|
|[Hygiene events schema](#hygiene-events-schema)|Extends the Common schema with the properties specific to events in Exchange Online Protection and Microsoft Defender for Office 365.|
|[Power BI schema](#power-bi-schema)|Extends the Common schema with the properties specific to all Power BI events.|
|[Dynamics 365 schema](#dynamics-365-schema)|Extends the Common schema with the properties specific to Dynamics 365 events.|
|[Workplace Analytics schema](#workplace-analytics-schema)|Extends the Common schema with the properties specific to all Microsoft Workplace Analytics events.|
|[Quarantine schema](#quarantine-schema)|Extends the Common schema with the properties specific to all quarantine events.|
|[Microsoft Forms schema](#microsoft-forms-schema)|Extends the Common schema with the properties specific to all Microsoft Forms events.|
|[MIP label schema](#mip-label-schema)|Extends the Common schema with the properties specific to sensitivity labels manually or automatically applied to email messages.|
|[Communication compliance Exchange schema](#communication-compliance-exchange-schema)|Extends the Common schema with the properties specific to the Communication compliance offensive language model.|
|[Office Scripts run events schema](#office-scripts-run-events-schema)|Extends the Common schema with the properties specific to Office Scripts run events.|
|||

## Common schema

**EntityType Name**: AuditRecord

|Parameter|Type|Mandatory?|Description|
|:-----|:-----|:-----|:-----|
|Id|Combination GUIDEdm.Guid|Yes|Unique identifier of an audit record.|
|RecordType|Self.[AuditLogRecordType](#auditlogrecordtype)|Yes|The type of operation indicated by the record. See the [AuditLogRecordType](#auditlogrecordtype) table for details on the types of audit log records.|
|CreationTime|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the user performed the activity.|
|Operation|Edm.String|Yes|The name of the user or admin activity. For a description of the most common operations/activities, see [Search the audit log in the Office 365 Protection Center](https://go.microsoft.com/fwlink/p/?LinkId=708432). For Exchange admin activity, this property identifies the name of the cmdlet that was run. For Dlp events, this can be "DlpRuleMatch", "DlpRuleUndo" or "DlpInfo", which are described under "DLP schema" below.|
|OrganizationId|Edm.Guid|Yes|The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType|Self.[UserType](#user-type)|Yes|The type of user that performed the operation. See the [UserType](#user-type) table for details on the types of users.|
|UserKey|Edm.String|Yes|An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts.|
|Workload|Edm.String|No|The Office 365 service where the activity occurred. 
|ResultStatus|Edm.String|No|Indicates whether the action (specified in the Operation property) was successful or not. Possible values are **Succeeded**, **PartiallySucceeded**, or **Failed**. For Exchange admin activity, the value is either **True** or **False**.<br/><br/>**Important**: Different workloads may overwrite the value of the ResultStatus property. For example, for Azure Active Directory STS logon events, a value of **Succeeded** for ResultStatus indicates only that the HTTP operation was successful; it doesn't mean the logon was successful. To determine if the actual logon was successful or not, see the LogonError property in the [Azure Active Directory STS Logon schema](#azure-active-directory-secure-token-service-sts-logon-schema). If the logon failed, the value of this property will contain the reason for the failed logon attempt. |
|ObjectId|Edm.string|No|For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. For Office Scripts run actions, the full path name of the Office Scripts file that was ran.|
|UserId|Edm.string|Yes|The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, `my_name@my_domain_name`. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see [The app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#the-appsharepoint-user-in-audit-records). |
|ClientIP|Edm.String|Yes|The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format.<br/><br/>For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. <br/><br/>Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is `null`.|
|Scope|Self.[AuditLogScope](#auditlogscope)|No|Was this event created by a hosted O365 service or an on-premises server? Possible values are **online** and **onprem**. Note that SharePoint is the only workload currently sending events from on-premises to O365.|
|||||

### Enum: AuditLogRecordType - Type: Edm.Int32

#### AuditLogRecordType

|Value|Member name|Description|
|:-----|:-----|:-----|
|1|ExchangeAdmin|Events from the Exchange admin audit log.|
|2|ExchangeItem|Events from an Exchange mailbox audit log for actions that are performed on a single item, such as creating or receiving an email message.|
|3|ExchangeItemGroup|Events from an Exchange mailbox audit log for actions that can be performed on multiple items, such as moving or deleted one or more email messages.|
|4|SharePoint|SharePoint events.|
|6|SharePointFileOperation|SharePoint file operation events.|
|7|OneDrive|OneDrive for Business events.|
|8|AzureActiveDirectory|Azure Active Directory events.|
|9|AzureActiveDirectoryAccountLogon|Azure Active Directory OrgId logon events (deprecated).|
|10|DataCenterSecurityCmdlet|Data Center security cmdlet events.|
|11|ComplianceDLPSharePoint|Data loss protection (DLP) events in SharePoint and OneDrive for Business.|
|13|ComplianceDLPExchange|Data loss protection (DLP) events in Exchange, when configured via Unified DLP Policy. DLP events based on Exchange Transport Rules are not supported.|
|14|SharePointSharingOperation|SharePoint sharing events.|
|15|AzureActiveDirectoryStsLogon|Secure Token Service (STS) logon events in Azure Active Directory.|
|16|SkypeForBusinessPSTNUsage|Public Switched Telephone Network (PSTN) events from Skype for Business.|
|17|SkypeForBusinessUsersBlocked|Blocked user events from Skype for Business.|
|18|SecurityComplianceCenterEOPCmdlet|Admin actions from the Security & Compliance Center.|
|19|ExchangeAggregatedOperation|Aggregated Exchange mailbox auditing events.|
|20|PowerBIAudit|Power BI events.|
|21|CRM|Dynamics 365 events.|
|22|Yammer|Yammer events.|
|23|SkypeForBusinessCmdlets|Skype for Business events.|
|24|Discovery|Events for eDiscovery activities performed by running content searches and managing eDiscovery cases in the Security & Compliance Center.|
|25|MicrosoftTeams|Events from Microsoft Teams.|
|28|ThreatIntelligence|Phishing and malware events from Exchange Online Protection and Microsoft Defender for Office 365.|
|29|MailSubmission|Submission events from Exchange Online Protection and Microsoft Defender for Office 365.|
|30|MicrosoftFlow|Microsoft Power Automate (formerly called Microsoft Flow) events.|
|31|AeD|Advanced eDiscovery events.|
|32|MicrosoftStream|Microsoft Stream events.|
|33|ComplianceDLPSharePointClassification|Events related to DLP classification in SharePoint.|
|34|ThreatFinder|Campaign-related events from Microsoft Defender for Office 365.|
|35|Project|Microsoft Project events.|
|36|SharePointListOperation|SharePoint List events.|
|37|SharePointCommentOperation|SharePoint comment events.|
|38|DataGovernance|Events related to retention policies and retention labels in the Security & Compliance Center|
|39|Kaizala|Kaizala events.|
|40|SecurityComplianceAlerts|Security and compliance alert signals.|
|41|ThreatIntelligenceUrl|Safe links time-of-block and block override events from Microsoft Defender for Office 365.|
|42|SecurityComplianceInsights|Events related to insights and reports in the Office 365 security and compliance center.|
|43|MIPLabel|Events related to the detection in the Transport pipeline of email messages that have been tagged (manually or automatically) with sensitivity labels. |
|44|WorkplaceAnalytics|Workplace Analytics events.|
|45|PowerAppsApp|Power Apps events.|
|46|PowerAppsPlan|Subscription plan events for Power Apps. |
|47|ThreatIntelligenceAtpContent|Phishing and malware events for files in SharePoint, OneDrive for Business, and Microsoft Teams from Microsoft Defender for Office 365.|
|48|LabelContentExplorer|Events related to [data classification content explorer](/microsoft-365/compliance/data-classification-content-explorer).|
|49|TeamsHealthcare|Events related to the [Patients application](/MicrosoftTeams/expand-teams-across-your-org/healthcare/patients-audit) in Microsoft Teams for Healthcare.|
|50|ExchangeItemAggregated|Events related to the [MailItemsAccessed mailbox auditing action](/microsoft-365/compliance/mailitemsaccessed-forensics-investigations).|
|51|HygieneEvent|Events related to outbound spam protection. |
|52|DataInsightsRestApiAudit|Data Insights REST API events.|
|53|InformationBarrierPolicyApplication|Events related to the application of information barrier policies.|
|54|SharePointListItemOperation|SharePoint list item events.|
|55|SharePointContentTypeOperation|SharePoint list content type events.|
|56|SharePointFieldOperation|SharePoint list field events.|
|57|MicrosoftTeamsAdmin|Teams admin events.|
|58|HRSignal|Events related to HR data signals that support the Insider risk management solution.|
|59|MicrosoftTeamsDevice|Teams device events.|
|60|MicrosoftTeamsAnalytics|Teams analytics events.|
|61|InformationWorkerProtection|Events related to compromised user alerts.|
|62|Campaign|Email campaign events from Microsoft Defender for Office 365.|
|63|DLPEndpoint|Endpoint DLP events.|
|64|AirInvestigation|Automated incident response (AIR) events.|
|65|Quarantine|Quarantine events.|
|66|MicrosoftForms|Microsoft Forms events.|
|67|ApplicationAudit|Application audit events.|
|68|ComplianceSupervisionExchange|Events tracked by the Communication compliance offensive language model.|
|69|CustomerKeyServiceEncryption|Events related to the customer key encryption service.|
|70|OfficeNative|Events related to sensitivity labels applied to Office documents.|
|71|MipAutoLabelSharePointItem|Auto-labeling events in SharePoint.|
|72|MipAutoLabelSharePointPolicyLocation|Auto-labeling policy events in SharePoint.|
|73|MicrosoftTeamsShifts|Teams Shifts events.|
|75|MipAutoLabelExchangeItem|Auto-labeling events in Exchange.|
|76|CortanaBriefing|Briefing email events.|
|77|Search|Events related to performing search queries in SharePoint and Exchange.|
|78|WDATPAlerts|Events related to alerts generated by Windows Defender for Endpoint.|
|81|MDATPAudit|Microsoft Defender for Endpoint events.|
|82|SensitivityLabelPolicyMatch|Events generated when the file labeled with a sensitivity label is opened or renamed.|
|83|SensitivityLabelAction|Event generated when sensitivity labels are applied, updated, or removed from a file.|
|84|SensitivityLabeledFileAction|Events generated when a file labeled with a sensitivity label is opened or renamed.|
|85|AttackSim|Attack simulator events.|
|86|AirManualInvestigation|Events related to manual investigations in Automated investigation and response (AIR). |
|87|SecurityComplianceRBAC|Security and compliance RBAC events.|
|88|UserTraining|Attack simulator training events in Microsoft Defender for Office 365.|
|89|AirAdminActionInvestigation|Events related to admin actions in  Automated investigation and response (AIR).|
|90|MSTIC|Threat intelligence events in Microsoft Defender for Office 365.|
|91|PhysicalBadgingSignal|Events related to physical badging signals that support the Insider risk management solution.|
|93|AipDiscover|Azure Information Protection (AIP) scanner events.|
|94|AipSensitivityLabelAction|AIP sensitivity label events. |
|95|AipProtectionAction|AIP protection events.|
|96|AipFileDeleted|AIP file deletion events.|
|97|AipHeartBeat|AIP heartbeat events.|
|98|MCASAlerts|Events corresponding to alerts triggered by Microsoft Cloud App Security.|
|99|OnPremisesFileShareScannerDlp|Events related to scanning for sensitive data on file shares.|
|100|OnPremisesSharePointScannerDlp|Events related to scanning for sensitive data in SharePoint.|
|101|ExchangeSearch|Events related to using Outlook on the web (OWA) to search for mailbox items.|
|102|SharePointSearch|Events related to searching an organization's SharePoint home site.|
|103|PrivacyInsights|Privacy insight events.|
|105|MyAnalyticsSettings|MyAnalytics events.|
|106|SecurityComplianceUserChange|Events related to modifying or deleting a user.|
|107|ComplianceDLPExchangeClassification|Exchange DLP classification events.|
|109|MipExactDataMatch|Exact Data Match (EDM) classification events.|
|131|OfficeScriptsRunAction |Office Scripts run events.|
||||

### Enum: User Type - Type: Edm.Int32

#### User Type

|Value|Member name|Description|
|:-----|:-----|:-----|
|0|Regular|A regular user.|
|1|Reserved|A reserved user.|
|2|Admin|An administrator.|
|3|DcAdmin|A Microsoft datacenter operator.|
|4|System|A system account.|
|5|Application|An application.|
|6|ServicePrincipal|A service principal.|
|7|CustomPolicy|A custom policy.|
|8|SystemPolicy|A system policy.|
||||

### Enum: AuditLogScope - Type: Edm.Int32

#### AuditLogScope

|Value|Member name|Description|
|:-----|:-----|:-----|
|0|Online|This event was created by a hosted O365 service.|
|1|Onprem|This event was created by an on-premises server.|
||||

## SharePoint Base schema

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Site|Edm.Guid|No|The GUID of the site where the file or folder accessed by the user is located.|
|ItemType|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[ItemType](#itemtype)"|No|The type of object that was accessed or modified. See the [ItemType](#itemtype) table for details on the types of objects.|
|EventSource|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[EventSource](#eventsource)"|No|Identifies that an event occurred in SharePoint. Possible values are **SharePoint** or **ObjectModel**.|
|SourceName|Edm.String|No|The entity that triggered the audited operation. Possible values are SharePoint or **ObjectModel**.|
|UserAgent|Edm.String|No|Information about the user's client or browser. This information is provided by the client or browser.|
|MachineDomainInfo|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|No|Information about device sync operations. This information is reported only if it's present in the request.|
|MachineId|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|No|Information about device sync operations. This information is reported only if it's present in the request.|
|||||

### Enum: ItemType - Type: Edm.Int32

#### ItemType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Invalid|The item is none of the other item types (that are listed in this table).|
|1|File|The item is a file.|
|5|Folder|The item is a folder.|
|6|Web|The item is a Web.|
|7|Site|The item is a site.|
|8|Tenant|The item is a tenant.|
|9|DocumentLibrary|The item is a document library.|
|11|Page|The item is a Page.|
||||

### Enum: EventSource - Type: Edm.Int32

#### EventSource

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|SharePoint|The event source is SharePoint.|
|1|ObjectModel|The event source is ObjectModel.|
||||

### Enum: SharePointAuditOperation - Type: Edm.Int32

|**Member name**|**Description**|
|:-----|:-----|
|AccessInvitationAccepted|The recipient of an invitation to view or edit a shared file (or folder) has accessed the shared file by clicking on the link in the invitation.|
|AccessInvitationCreated|User sends an invitation to another person (inside or outside their organization) to view or edit a shared file or folder on a SharePoint or OneDrive for Business site. The details of the event entry identifies the name of the file that was shared, the user the invitation was sent to, and the type of the sharing permission selected by the person who sent the invitation.|
|AccessInvitationExpired|An invitation sent to an external user expires. By default, an invitation sent to a user outside of your organization expires after 7 days if the invitation isn't accepted.|
|AccessInvitationRevoked|The site administrator or owner of a site or document in SharePoint or OneDrive for Business withdraws an invitation that was sent to a user outside your organization. An invitation can be withdrawn only before it's accepted.|
|AccessInvitationUpdated|The user who created and sent an invitation to another person to view or edit a shared file (or folder) on a SharePoint or OneDrive for Business site resends the invitation.|
|AccessRequestApproved|The site administrator or owner of a site or document in SharePoint or OneDrive for Business approves a user request to access the site or document.|
|AccessRequestCreated|User requests access to a site or document in SharePoint or OneDrive for Business that they don't have permission to access. |
|AccessRequestRejected|The site administrator or owner of a site or document in SharePoint declines a user request to access the site or document.|
|ActivationEnabled|Users can browser-enable form templates that don't contain form code, require full trust, enable rendering on a mobile device, or use a data connection managed by a server administrator.|
|AdministratorAddedToTermStore|Term store administrator added.|
|AdministratorDeletedFromTermStore|Term store administrator deleted.|
|AllowGroupCreationSet|Site administrator or owner adds a permission level to a SharePoint or OneDrive for Business site that allows a user assigned that permission to create a group for that site.|
|AppCatalogCreated|App catalog created to make custom business apps available for your SharePoint Environment.|
|AuditPolicyRemoved|Document LifeCycle Policy has been removed for a site collection.|
|AuditPolicyUpdate|Document LifeCycle Policy has been updated for a site collection.|
|AzureStreamingEnabledSet|A video portal owner has allowed video streaming from Azure.|
|CollaborationTypeModified|The type of collaboration allowed on sites (for example, intranet, extranet, or public) has been modified.|
|ConnectedSiteSettingModified|User has either created, modified or deleted the link between a project and a project site or the user modifies the synchronization setting on the link in Project Web App.|
|CreateSSOApplication|Target application created in Secure store service.|
|CustomFieldOrLookupTableCreated|User created a custom field or lookup table/item in Project Web App.|
|CustomFieldOrLookupTableDeleted|User deleted a custom field or lookup table/item in Project Web App.|
|CustomFieldOrLookupTableModified|User modified a custom field or lookup table/item in Project Web App.|
|CustomizeExemptUsers|Global administrator customized the list of exempt user agents in SharePoint admin center. You can specify which user agents to exempt from receiving an entire Web page to index. This means when a user agent you've specified as exempt encounters an InfoPath form, the form will be returned as an XML file instead of an entire Web page. This makes indexing InfoPath forms faster.|
|DefaultLanguageChangedInTermStore*|Language setting changed in the terminology store.|
|DelegateModified|User created or modified a security delegate in Project Web App.|
|DelegateRemoved|User deleted a security delegate in Project Web App.|
|DeleteSSOApplication|An SSO application was deleted.|
|eDiscoveryHoldApplied|An In-Place Hold was placed on a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoveryHoldRemoved|An In-Place Hold was removed from a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoverySearchPerformed|An eDiscovery search was performed using an eDiscovery site collection in SharePoint.|
|EngagementAccepted|User accepts a resource engagement in Project Web App.|
|EngagementModified|User modifies a resource engagement in Project Web App.|
|EngagementRejected|User rejects a resource engagement in Project Web App.|
|EnterpriseCalendarModified|User copies, modifies or delete an enterprise calendar in Project Web App.|
|EntityDeleted|User deletes a timesheet in Project Web App.|
|EntityForceCheckedIn|User forces a checkin on a calendar, custom field or lookup table in Project Web App.|
|ExemptUserAgentSet|Global administrator adds a user agent to the list of exempt user agents in the SharePoint admin center.|
|FileAccessed|User or system account accesses a file on a SharePoint or OneDrive for Business site. System accounts can also generate FileAccessed events.|
|FileCheckOutDiscarded|User discards (or undos) a checked out file. That means any changes they made to the file when it was checked out are discarded, and not saved to the version of the document in the document library.|
|FileCheckedIn|User checks in a document that they checked out from a SharePoint or OneDrive for Business document library.|
|FileCheckedOut|User checks out a document located in a SharePoint or OneDrive for Business document library. Users can check out and make changes to documents that have been shared with them.|
|FileCopied|User copies a document from a SharePoint or OneDrive for Business site. The copied file can be saved to another folder on the site.|
|FileDeleted|User deletes a document from a SharePoint or OneDrive for Business site.|
|FileDeletedFirstStageRecycleBin|User deletes a file from the recycle bin on a SharePoint or OneDrive for Business site.|
|FileDeletedSecondStageRecycleBin|User deletes a file from the second-stage recycle bin on a SharePoint or OneDrive for Business site.|
|FileDownloaded|User downloads a document from a SharePoint or OneDrive for Business site.|
|FileFetched|This event has been replaced by the FileAccessed event, and has been deprecated.|
|FileModified|User or system account modifies the content or the properties of a document located on a SharePoint or OneDrive for Business site.|
|FileMoved|User moves a document from its current location on a SharePoint or OneDrive for Business site to a new location.|
|FilePreviewed|User previews a document on a SharePoint or OneDrive for Business site.|
|FileRenamed|User renames a document on a SharePoint or OneDrive for Business site.|
|FileRestored|User restores a document from the recycle bin of a SharePoint or OneDrive for Business site. |
|FileSyncDownloadedFull|User establishes a sync relationship and successfully downloads files for the first time to their computer from a SharePoint or OneDrive for Business document library.|
|FileSyncDownloadedPartial|User successfully downloads any changes to files from SharePoint or OneDrive for Business document library. This event indicates that any changes that were made to files in the document library were downloaded to the user's computer. Only changes were downloaded because the document library was previously downloaded by the user (as indicated by the FileSyncDownloadedFull event).|
|FileSyncUploadedFull|User establishes a sync relationship and successfully uploads files for the first time from their computer to a SharePoint or OneDrive for Business document library.|
|FileSyncUploadedPartial|User successfully uploads changes to files on a SharePoint or OneDrive for Business document library. This event indicates that any changes made to the local version of a file from a document library are successfully uploaded to the document library. Only changes are unloaded because those files were previously uploaded by the user (as indicated by the FileSyncUploadedFull event).|
|FileUploaded|User uploads a document to a folder on a SharePoint or OneDrive for Business site. |
|FileViewed|This event has been replaced by the FileAccessed event, and has been deprecated.|
|FolderCopied|User copies a folder from a SharePoint or OneDrive for Business site to another location in SharePoint or OneDrive for Business.|
|FolderCreated|User creates a folder on a SharePoint or OneDrive for Business site.|
|FolderDeleted|User deletes a folder from a SharePoint or OneDrive for Business site.|
|FolderDeletedFirstStageRecycleBin|User deletes a folder from the recycle bin on a SharePoint or OneDrive for Business site .|
|FolderDeletedSecondStageRecycleBin|User deletes a folder from the second-stage recycle bin on a SharePoint or OneDrive for Business site.|
|FolderModified|User modifies a folder on a SharePoint or OneDrive for Business site. This event includes folder metadata changes, such as tags and properties.|
|FolderMoved|User moves a folder from a SharePoint or OneDrive for Business site.|
|FolderRenamed|User renames a folder on a SharePoint or OneDrive for Business site.|
|FolderRestored|User restores a folder from the Recycle Bin on a SharePoint or OneDrive for Business site.|
|GroupAdded|Site administrator or owner creates a group for a SharePoint or OneDrive for Business site, or performs a task that results in a group being created. For example, the first time a user creates a link to share a file, a system group is added to the user's OneDrive for Business site. This event can also be a result of a user creating a link with edit permissions to a shared file.|
|GroupRemoved|User deletes a group from a SharePoint or OneDrive for Business site. |
|GroupUpdated|Site administrator or owner changes the settings of a group for a SharePoint or OneDrive for Business site. This can include changing the group's name, who can view or edit the group membership, and how membership requests are handled.|
|LanguageAddedToTermStore|Language added to the terminology store.|
|LanguageRemovedFromTermStore|Language removed from the terminology store.|
|LegacyWorkflowEnabledSet|Site administrator or owner adds the SharePoint Workflow Task content type to the site. Global administrators can also enable work flows for the entire organization in the SharePoint admin center.|
|LookAndFeelModified|User modifies a quick launch, gantt chart formats, or group formats.  Or the user creates, modifies, or deletes a view in Project Web App.|
|ManagedSyncClientAllowed|User successfully establishes a sync relationship with a SharePoint or OneDrive for Business site. The sync relationship is successful because the user's computer is a member of a domain that's been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. For more information, see [Use SharePoint Online PowerShell ](https://go.microsoft.com/fwlink/p/?LinkID=534609) to enable OneDrive sync for domains that are on the safe recipients list.|
|MaxQuotaModified|The maximum quota for a site has been modified.|
|MaxResourceUsageModified|The maximum allowable resource usage for a site has been modified.|
|MySitePublicEnabledSet|The flag enabling users to have public MySites has been set by the SharePoint administrator.|
|NewsFeedEnabledSet|Site administrator or owner enables RSS feeds for a SharePoint or OneDrive for Business site. Global administrators can enable RSS feeds for the entire organization in the SharePoint admin center.|
|ODBNextUXSettings|New UI for OneDrive for Business has been enabled.|
|OfficeOnDemandSet|Site administrator enables Office on Demand, which lets users access the latest version of Office desktop applications. Office on Demand is enabled in the SharePoint admin center and requires an Office 365 subscription that includes full, installed Office applications.|
|PageViewed|User views a page on a SharePoint site or OneDrive for Business site. This does not include viewing document library files from a SharePoint site or One Drive for Business site on a browser.|
|PeopleResultsScopeSet|Site administrator creates or changes the result source for People Searches for a SharePoint site.|
|PermissionSyncSettingModified|User modifies the project permission sync settings in Project Web App.|
|PermissionTemplateModified|User creates, modifies or deletes a permissions template in Project Web App.|
|PortfolioDataAccessed|User accesses portfolio content (driver library, driver prioritization, portfolio analyses) in Project Web App.|
|PortfolioDataModified|User creates, modifies, or deletes portfolio data (driver library, driver prioritization, portfolio analyses) in Project Web App.|
|PreviewModeEnabledSet|Site administrator enables document preview for a SharePoint site.|
|ProjectAccessed|User accesses project content in Project Web App.|
|ProjectCheckedIn|User checks in a project that they checked out from a Project Web App.|
|ProjectCheckedOut|User checks out a project located in a Project Web App. Users can check out and make changes to projects that they have permission to open.|
|ProjectCreated|User creates a project in Project Web App.|
|ProjectDeleted|User deletes a project in Project Web App.|
|ProjectForceCheckedIn|User forces a check in on a project in Project Web App.|
|ProjectModified|User modifies a project in Project Web App.|
|ProjectPublished|User publishes a project in Project Web App.|
|ProjectWorkflowRestarted|User restarts a workflow in Project Web App.|
|PWASettingsAccessed|User access the Project Web App settings via CSOM.|
|PWASettingsModified|User modifies the a Project Web App configuration.|
|QueueJobStateModified|User cancels or restarts a queue job in Project Web App.|
|QuotaWarningEnabledModified|Storage quota warning modified.|
|RenderingEnabled|Browser-enabled form templates will be rendered by InfoPath forms services.|
|ReportingAccessed|User accessed the reporting endpoint in Project Web App.|
|ReportingSettingModified|User modifies the reporting configuration in Project Web App.|
|ResourceAccessed|User accesses an enterprise resource content in Project Web App.|
|ResourceCheckedIn|User checks in an enterprise resource that they checked out from Project Web App.|
|ResourceCheckedOut|User checks out an enterprise resource located in Project Web App.|
|ResourceCreated|User creates an enterprise resource in Project Web App.|
|ResourceDeleted|User deletes an enterprise resource in Project Web App.|
|ResourceForceCheckedIn|User forces a checkin of an enterprise resource in Project Web App.|
|ResourceModified|User modifies an enterprise resource in Project Web App.|
|ResourcePlanCheckedInOrOut|User checks in or out a resource plan in Project Web App.|
|ResourcePlanModified|User modifies a resource plan in Project Web App.|
|ResourcePlanPublished|User publishes a resource plan in Project Web App.|
|ResourceRedacted|User redacts an enterprise resource removing all personal information in Project Web App.|
|ResourceWarningEnabledModified|Resource quota warning modified.|
|SSOGroupCredentialsSet|Group credentials set in Secure store service.|
|SSOUserCredentialsSet|User credentials set in Secure store service.|
|SearchCenterUrlSet|Search center URL set.|
|SecondaryMySiteOwnerSet|A user has added a secondary owner to their MySite.|
|SecurityCategoryModified|User creates, modifies or deletes a security category in Project Web App.|
|SecurityGroupModified|User creates, modifies or deletes a security group in Project Web App.|
|SendToConnectionAdded|Global administrator creates a new Send To connection on the Records management page in the SharePoint admin center. A Send To connection specifies settings for a document repository or a records center. When you create a Send To connection, a Content Organizer can submit documents to the specified location.|
|SendToConnectionRemoved|Global administrator deletes a Send To connection on the Records management page in the SharePoint admin center.|
|SharedLinkCreated|User creates a link to a shared file in SharePoint or OneDrive for Business. This link can be sent to other people to give them access to the file. A user can create two types of links: a link that allows a user to view and edit the shared file, or a link that allows the user to just view the file.|
|SharedLinkDisabled|User disables (permanently) a link that was created to share a file.|
|SharingInvitationAccepted*|User accepts an invitation to share a file or folder. This event is logged when a user shares a file with other users.|
|SharingRevoked|User unshares a file or folder that was previously shared with other users. This event is logged when a user stops sharing a file with other users.|
|SharingSet|User shares a file or folder located in SharePoint or OneDrive for Business with another user inside their organization.|
|SiteAdminChangeRequest|User requests to be added as a site collection administrator for a SharePoint site collection. Site collection administrators have full control permissions for the site collection and all subsites.|
|SiteCollectionAdminAdded*|Site collection administrator or owner adds a person as a site collection administrator for a SharePoint or OneDrive for Business site. Site collection administrators have full control permissions for the site collection and all subsites.|
|SiteCollectionCreated| Global administrator creates a new site collection in your SharePoint organization.|
|SiteRenamed|Site administrator or owner renames a SharePoint or OneDrive for Business site|
|StatusReportModified|User creates, modifies or deletes a status report in Project Web App.|
|SyncGetChanges|User clicks **Sync** in the action tray on in SharePoint or OneDrive for Business to synchronize any changes to file in a document library to their computer.|
|TaskStatusAccessed|User accesses the status of one or more tasks in Project Web App.|
|TaskStatusApproved|User approves a status update of one or more tasks in Project Web App.|
|TaskStatusRejected|User rejects a status update of one or more tasks in Project Web App.|
|TaskStatusSaved|User saves a status update of one or more tasks in Project Web App.|
|TaskStatusSubmitted|User submits a status update of one or more tasks in Project Web App.|
|TimesheetAccessed|User accesses a timesheet in Project Web App.|
|TimesheetApproved|User approves timesheet in Project Web App.|
|TimesheetRejected|User rejects a timesheet in Project Web App.|
|TimesheetSaved|User saves a timesheet in Project Web App.|
|TimesheetSubmitted|User submits a status timesheet in Project Web App.|
|UnmanagedSyncClientBlocked|User tries to establish a sync relationship with a SharePoint or OneDrive for Business site from a computer that isn't a member of your organization's domain or is a member of a domain that hasn't been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. The sync relationship is not allowed, and the user's computer is blocked from syncing, downloading, or uploading files on a document library. For information about this feature, see [Use Windows PowerShell cmdlets to enable OneDrive sync for domains that are on the safe recipients list](/powershell/module/sharepoint-online/index).|
|UpdateSSOApplication|Target application updated in Secure store service.|
|UserAddedToGroup|Site administrator or owner adds a person to a group on a SharePoint or OneDrive for Business site. Adding a person to a group grants the user the permissions that were assigned to the group. |
|UserRemovedFromGroup|Site administrator or owner removes a person from a group on a SharePoint or OneDrive for Business site. After the person is removed, they no longer are granted the permissions that were assigned to the group. |
|WorkflowModified|User creates, modifies, or deletes an Enterprise Project Type or Workflow phases or stages in Project Web App.|
|||||

## SharePoint file operations

The file-related SharePoint events listed in the "File and folder activities" section in [Search the audit log in security and compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) use this schema.

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|SiteUrl|Edm.String|Yes|The URL of the site where the file or folder accessed by the user is located.|
|SourceRelativeUrl|Edm.String|No|The URL of the folder that contains the file accessed by the user. The combination of the values for the  _SiteURL_,  _SourceRelativeURL_, and  _SourceFileName_ parameters is the same as the value for the **ObjectID** property, which is the full path name for the file accessed by the user.|
|SourceFileName|Edm.String|Yes|The name of the file or folder accessed by the user.|
|SourceFileExtension|Edm.String|No|The file extension of the file that was accessed by the user. This property is blank if the object that was accessed is a folder.|
|DestinationRelativeUrl|Edm.String|No|The URL of the destination folder where a file is copied or moved. The combination of the values for  _SiteURL_,  _DestinationRelativeURL_, and  _DestinationFileName_ parameters is the same as the value for the **ObjectID** property, which is the full path name for the file that was copied. This property is displayed only for FileCopied and FileMoved events.|
|DestinationFileName|Edm.String|No|The name of the file that is copied or moved. This property is displayed only for FileCopied and FileMoved events.|
|DestinationFileExtension|Edm.String|No|The file extension of a file that is copied or moved. This property is displayed only for FileCopied and FileMoved events.|
|UserSharedWith|Edm.String|No|The user that a resource was shared with.|
|SharingType|Edm.String|No|The type of sharing permissions that were assigned to the user that the resource was shared with. This user is identified by the  _UserSharedWith_ parameter.|
|||||

## SharePoint Sharing schema

 The file share-related SharePoint events. They are different from file- and folder-related events in that a user is taking an action that has some effect on another user. For information about the SharePoint Sharing schema, see [Use sharing auditing in the Office 365 audit log](/microsoft-365/compliance/use-sharing-auditing
).

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|TargetUserOrGroupName |Edm.String|No|Stores the UPN or name of the target user or group that a resource was shared with.|
|TargetUserOrGroupType|Edm.String|No|Identifies whether the target user or group is a Member, Guest, Group, or Partner. |
|EventData|XML code|No|Conveys follow-up information about the sharing action that has occurred, such as adding a user to a group or granting edit permissions.|
|||||

## SharePoint schema

The SharePoint events listed in [Search the audit log in security and compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) (excluding the file and folder events) use this schema.

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|CustomEvent|Edm.String|No|Optional string for custom events.|
|EventData|Edm.String|No|Optional payload for custom events.|
|ModifiedProperties|Collection(ModifiedProperty)|No|The property is included for admin events, such as adding a user as a member of a site or a site collection admin group. The property includes the name of the property that was modified (for example, the Site Admin group), the new value of the modified property (such the user who was added as a site admin), and the previous value of the modified object.|
|||||

## Project schema

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Entity|Edm.String|Yes| [ProjectEntity](#project-entity) the audit was for.|
|Action|Edm.String|Yes|[ProjectAction](#project-action) that was taken.|
|OnBehalfOfResId|Edm.Guid|No|The resource Id the action was taken on behalf of.|
|||||

### Enum: Project Action - Type: Edm.Int32

#### Project action

|**Member name**|**Description**|
|:-----|:-----|
|Accepted|The user accepted an event or workflow.|
|Accessed|The user accessed an entity.|
|Activated|The user activated an entity, event or workflow.|
|Cancelled|The user cancelled an event or workflow.|
|CheckedIn|The user check in an entity.|
|CheckedOut|The user checkout an entity.|
|Copied|The user copied an entity.|
|Created|The user created an entity.|
|Deactivated|The user deactivated an entity.|
|Deleted|The user deleted an entity.|
|Exported|The user exported an entity.|
|ForceCheckedIn|The user caused an entity to be force checked in.|
|Modified|The user modified an entity.|
|Published|The user published an entity.|
|Redacted|The user redacted an entity.|
|Rejected|The user rejected an entity.|
|Restarted|The user restarted an event or workflow.|
|Saved|The user saved an entity.|
|Sent|The user sent an entity.|
|Submitted|The user submitted an entity for review or workflow.|
|||||

### Enum: Project Entity - Type: Edm.Int32

#### Project entity

|**Member name**|**Description**|
|:-----|:-----|
|CustomField|Represents an enterprise custom field.|
|Driver|Represents a portfolio driver.|
|DriverPrioritization|Represents a portfolio prioritization.|
|Engagement|Represents a resource engagement.|
|EnterpriseCalendar|Represents a enterprise  resource calendar.|
|EnterpriseProjectType|Represents an enterprise project type.|
|FiscalPeriod|Represents a fiscal period.|
|GanttChartFormat|Represents a gantt chart format.|
|GroupingFormat|Represents a view grouping format.|
|LineClassification|Represents a timesheet line classification.|
|LookupTable|Represents a enterprise lookup table.|
|PermissionTemplate|Represents a security permission template.|
|PortfolioAnalysis|Represents a portfolio analysis.|
|Project|Represents a project.|
|QueueJob|Represents a queue job.|
|QuickLaunch|Represents a quick launch item.|
|Reporting|Represents the reporting endpoint.|
|Resource|Represents an enterprise resource.|
|ResourcePlan|Represents a resource plan associated with A project.|
|SecurityCategory|Represents a security category.|
|SecurityGroup|Represents a security group.|
|Setting|Represents a Project Web App setting|
|Statusing|Represents a task status update.|
|StatusReport|Represents a status report.|
|TimeReportingPeriod|Represents a period of time for a timesheet|
|Timesheet|Represents a timesheet entity.|
|TimesheetAuditLog|Represents a timesheet audit log.|
|TimesheetManager|Represents the manager of a timesheet.|
|UserDelegate|Represents a user delegation for another user.|
|View|Represents a view definition.|
|WorkflowPhase|Represents a phase in a workflow.|
|WorkflowStage|Represents a stage in a workflow.|
|||||

## Exchange Admin schema

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|ModifiedObjectResolvedName|Edm.String|No|This is the user friendly name of the object that was modified by the cmdlet. This is logged only if the cmdlet modifies the object.|
|Parameters|Collection(Common.NameValuePair)|No|The name and value for all parameters that were used with the cmdlet that is identified in the Operations property.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|No|The property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified object.|
|ExternalAccess|Edm.Boolean|Yes|Specifies whether the cmdlet was run by a user in your organization, by Microsoft datacenter personnel or a datacenter service account, or by a delegated administrator. The value **False** indicates that the cmdlet was run by someone in your organization. The value **True** indicates that the cmdlet was run by datacenter personnel, a datacenter service account, or a delegated administrator.|
|OriginatingServer|Edm.String|No|The name of the server from which the cmdlet was executed.|
|OrganizationName|Edm.String|No|The name of the tenant.|
|||||

## Exchange Mailbox schema

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|LogonType|Self.[LogonType](#logontype)|No| Indicates the type of user who accessed the mailbox and performed the operation that was logged.|
|InternalLogonType|Self.[LogonType](#logontype)|No|Reserved for internal use.|
|MailboxGuid|Edm.String|No|The Exchange GUID of the mailbox that was accessed.|
|MailboxOwnerUPN|Edm.String|No|The email address of the person who owns the mailbox that was accessed.|
|MailboxOwnerSid|Edm.String|No|The SID of the mailbox owner.|
|MailboxOwnerMasterAccountSid|Edm.String|No|Mailbox owner account's master account SID.|
|LogonUserSid|Edm.String|No|The SID of the user who performed the operation.|
|LogonUserDisplayName|Edm.String|No|The user-friendly name of the user who performed the operation.|
|ExternalAccess|Edm.Boolean|Yes|This is true if the logon user's domain is different from the mailbox owner's domain.|
|OriginatingServer |Edm.String|No|This is from where the operation originated.|
|OrganizationName|Edm.String|No|The name of the tenant.|
|ClientInfoString|Edm.String|No|Information about the email client that was used to perform the operation, such as a browser version, Outlook version, and mobile device information.|
|ClientIPAddress|Edm.String|No|The IP address of the device that was used when the operation was logged. The IP address is displayed in either an IPv4 or IPv6 address format.|
|ClientMachineName|Edm.String|No|The machine name that hosts the Outlook client.|
|ClientProcessName|Edm.String|No|The email client that was used to access the mailbox. |
|ClientVersion|Edm.String|No|The version of the email client .|
|||||

### Enum: LogonType - Type: Edm.Int32

#### LogonType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Owner|The mailbox owner.|
|1|Admin|A person with administrative privileges for someone's mailbox.|
|2|Delegated|A person with the delegate privileges for someone's mailbox.|
|3|Transport|A transport service in the Microsoft datacenter.|
|4|SystemService|A service account in the Microsoft datacenter|
|5|BestAccess|Reserved for internal use.|
|6|DelegatedAdmin|A delegated administrator.|
|||||

### ExchangeMailboxAuditGroupRecord schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Folder|Self.[ExchangeFolder](#exchangefolder-complex-type)|No|The folder where a group of items is located.|
|CrossMailboxOperations|Edm.Boolean|No|Indicates if the operation involved more than one mailbox.|
|DestMailboxId|Edm.Guid|No|Set only if the CrossMailboxOperations parameter is **True**. Specifies the target mailbox GUID.|
|DestMailboxOwnerUPN|Edm.String|No|Set only if the CrossMailboxOperations parameter is **True**. Specifies the UPN of the owner of the target mailbox.|
|DestMailboxOwnerSid|Edm.String|No|Set only if the CrossMailboxOperations parameter is **True**. Specifies the SID of the target mailbox.|
|DestMailboxOwnerMasterAccountSid|Edm.String|No|Set only if the CrossMailboxOperations parameter is **True**. Specifies the SID for the master account SID of the target mailbox owner.|
|DestFolder|Self.[ExchangeFolder](#exchangefolder-complex-type)|No|The destination folder, for operations such as Move.|
|Folders|Collection(Self.[ExchangeFolder](#exchangefolder-complex-type))|No|Information about the source folders involved in an operation; for example, if folders are selected and then deleted.|
|AffectedItems|Collection(Self.[ExchangeItem](#exchangeitem-complex-type))|No|Information about each item in the group.|
|||||

### ExchangeMailboxAuditRecord schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Item|Self.[ExchangeItem](#exchangeitem-complex-type)|No|Represents the item upon which the operation was performed|
|ModifiedProperties|Collection(Edm.String)|No|TBD|
|SendAsUserSmtp|Edm.String|No|SMTP address of the user who is being impersonated.|
|SendAsUserMailboxGuid|Edm.Guid|No|The Exchange GUID of the mailbox that was accessed to send email as.|
|SendOnBehalfOfUserSmtp|Edm.String|No|SMTP address of the user on whose behalf the email is sent.|
|SendOnBehalfOfUserMailboxGuid|Edm.Guid|No|The Exchange GUID of the mailbox that was accessed to send mail on behalf of.|
|||||

### ExchangeItem complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Yes|The store ID.|
|Subject|Edm.String|No|The subject line of the message that was accessed.|
|ParentFolder|Edm.ExchangeFolder|No|The name of the folder where the item is located.|
|Attachments|Edm.String|No|A list of the names and file size of all items that are attached to the message.|
|||||

### ExchangeFolder complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Yes|The store ID of the folder object.|
|Path|Edm.String|No|The name of the mailbox folder where the message that was accessed is located.|
|||||

## Azure Active Directory Base schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AzureActiveDirectoryEventType|Self.[AzureActiveDirectoryEventType](#azureactivedirectoryeventtype)|Yes|The type of Azure AD event. |
|ExtendedProperties|Collection(Common.NameValuePair)|No|The extended properties of the Azure AD event.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|No|This property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property.|
|||||

### Enum: AzureActiveDirectoryEventType - Type -Edm.Int32

#### AzureActiveDirectoryEventType

|**Member name**|**Description**|
|:-----|:-----|
|AccountLogon|The account login event.|
|AzureApplicationAuditEvent|The Azure application security event.|
|||||

## Azure Active Directory Account Logon schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Application|Edm.String|No|The application that triggers the account login event, such as Office 15.|
|Client|Edm.String|No|Details about the client device, device OS, and device browser that was used for the of the account login event.|
|LoginStatus|Edm.Int32|Yes|This property is from OrgIdLogon.LoginStatus directly. The mapping of various interesting logon failures could be done by alerting algorithms.|
|UserDomain|Edm.String|Yes|The Tenant Identity Information (TII).|
|||||

### Enum: CredentialType - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|-1|Other|Other authentication.|
|0|Password|User credential is username and password.|
|1|MobilePhone|User credential is mobile phone.|
|2|SecretQuestion|User credential is secret question.|
|3|SecurePin|User credential is secure PIN.|
|4|SecurePinReset|User credential is secure PIN reset.|
|11|EasyID|User credential is EasyID.|
|14|PasswordIndexCredentialType|User credential is PasswordIndexCredentialType.|
|16|Device|User credential is a device.|
|17|ForeignRealmIndex|User credential is ForeignRealmIndex.|
|||||

### Enum: LoginType - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|-1|Other|Other i type.|
|1|InitialAuth|Login with initial authentication|
|2|CookieCopy|Login with cookie.|
|3|SilentReAuth|Login with silent re-authentication.|
|||||

### Enum: AuthenticationMethod - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Min|The authentication method is a Min|
|1|Password|The authentication method is a password.|
|2|Digest|The authentication method is a digest.|
|3|ProxyAuth|The authentication method is a ProxyAuth.|
|4|InfoCard|The authentication method is an InfoCard|
|5|DAToken|The authentication method is a DAToken.|
|6|Sha1RememberMyPassword|The authentication method is a Sha1RememberMyPassword.|
|7|LMPasswordHash|The authentication method is an LMPasswordHash.|
|8|ADFSFederatedToken|The authentication method is an ADFSFederatedToken.|
|9|EID|The authentication method is an EID.|
|10|DeviceID|The authentication method is a DeviceID. |
|11|MD5|The authentication method is MD5.|
|12|EncProxyPasswordHash|The authentication method is a EncProxyPasswordHash.|
|13|LWAFederation|The authentication method is a LWAFederation.|
|14|Sha1HashedPassword|The authentication method is a Sha1HashedPassword.|
|15|SecurePin|The authentication method is a secure Pin.|
|16|SecurePinReset|The authentication method is a secure PIN reset.|
|17|SAML20PostSimpleSign|The authentication method is a SAML20PostSimpleSign.|
|18|SAML20Post|The authentication method is a SAML20Post.|
|19|OneTimeCode|The authentication method is a one-time code.|
|||||

## Azure Active Directory schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Actor|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|No|The user or service principal that performed the action.|
|ActorContextId|Edm.String|No|The GUID of the organization that the actor belongs to.|
|ActorIpAddress|Edm.String|No|The actor's IP address in IPV4 or IPV6 address format.|
|InterSystemsId|Edm.String|No|The GUID that track the actions across components within the Office 365 service.|
|IntraSystemsId|Edm.String|No|The GUID that's generated by Azure Active Directory to track the action.|
|SupportTicketId|Edm.String|No|The customer support ticket ID for the action in "act-on-behalf-of" situations.|
|Target|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|No|The user that the action (identified by the Operation property) was performed on.|
|TargetContextId|Edm.String|No|The GUID of the organization that the targeted user belongs to.|
|||||

### Complex Type IdentityTypeValuePair

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ID|Edm.String|Yes|The value of the identity given the type.|
|Type|Self.IdentityType|Yes|The type of the identity.|
|||||

### Enum: IdentityType - Type: Edm.Int32

#### IdentityType

|**Member name**|**Description**|
|:-----|:-----|
|Claim|The identity is a claim for authorization purpose.|
|Name|The audit action actor or target identity display name.|
|Other|The identity of the actor is other type, such as the ObjectId in GUID generated by the Office 365 service.|
|PUID|The audit action actor or the target passport unique ID (PUID).|
|SPN|The identity of a service principal if the action is performed by the Office 365 service.|
|UPN|The user principal name.|
|||||

## Azure Active Directory Secure Token Service (STS) Logon schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|No|The GUID that represents the application that is requesting the login. The display name can be looked up via the Azure Active Directory Graph API.|
|Client|Edm.String|No|Client device information, provided by the browser performing the login.|
|DeviceProperties|Collection(Common.NameValuePair)|No|This property includes various device details, including Id, Display name, OS, Browser, IsCompliant, IsCompliantAndManaged, SessionId, and DeviceTrustType. The DeviceTrustType property can have the following values:<br/><br/>**0** - Azure AD registered<br/> **1** - Azure AD joined<br/> **2** - Hybrid Azure AD joined|
|ErrorCode|Edm.String|No|For failed logins (where the value for the Operation property is UserLoginFailed), this property contains the Azure Active Directory STS (AADSTS) error code. For descriptions of these error codes, see [Authentication and authorization error codes](/azure/active-directory/develop/reference-aadsts-error-codes#aadsts-error-codes). A value of `0` indicates a successful login.|
|LogonError|Edm.String|No|For failed logins, this property contains a user-readable description of the reason for the failed login.|
|||||

## DLP schema

DLP events are available for Exchange Online, SharePoint Online, and OneDrive For Business. Note that DLP events in Exchange are only available for events based on unified DLP policy (e.g. configured via Security & Compliance Center). DLP events based on Exchange Transport Rules are not supported.

DLP (Data Loss Prevention) events will always have UserKey="DlpAgent" in the common schema. There are three types of DlpEvents that are stored as the value of the Operation property of the common schema:

- DlpRuleMatch - This indicates a rule was matched. These events exist in both Exchange and SharePoint Online and OneDrive for Business. For Exchange it includes false positive and override information. For SharePoint Online and OneDrive for Business, false positive and overrides generate separate events.

- DlpRuleUndo - These only exist in SharePoint Online and OneDrive for Business, and indicate a previously applied policy action has been "undone" – either because of false positive/override designation by user, or because the document is no longer subject to policy (either due to policy change or change to content in doc).

- DlpInfo - These only exist in SharePoint Online and OneDrive for Business and indicate a false positive designation but no action was "undone."

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.[SharePointMetadata](#sharepointmetadata-complex-type)|No|Describes metadata about the document in SharePoint or OneDrive for Business that contained the sensitive information.|
|ExchangeMetaData|Self.[ExchangeMetadata](#exchangemetadata-complex-type)|No|Describes metadata about the email message that contained the sensitive information.|
|ExceptionInfo|Edm.String|No|Identifies reasons why a policy no longer applies and/or any information about false positive and/or override noted by the end user.|
|PolicyDetails|Collection(Self.[PolicyDetails](#policydetails-complex-type))|Yes|Information about 1 or more policies that triggered the DLP event.|
|SensitiveInfoDetectionIsIncluded|Boolean|Yes|Indicates whether the event contains the value of the sensitive data type and surrounding context from the source content. Accessing sensitive data requires the "Read DLP policy events including sensitive details" permission in Azure Active Directory.|
|||||

### SharePointMetadata complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|From|Edm.String|Yes|The user who triggered the event. This will be either the FileOwner, LastModifier, or LastSharer.|
|itemCreationTime|Edm.Date|Yes|Datetimestamp in UTC of when event logged.|
|SiteCollectionGuid|Edm.Guid|Yes|The GUID of the site collection.|
|SiteCollectionUrl|Edm.String|Yes|Name of the SharePoint site.|
|FileName|Edm.String|Yes|Name of the path.|
|FileOwner|Edm.String|Yes|The document owner.|
|FilePathUrl|Edm.String|Yes|The URL of the document|
|DocumentLastModifier|Edm.String|Yes|The user who last modified the document.|
|DocumentSharer|Edm.String|Yes|The user who last modified sharing of the document.|
|UniqueId|Edm.String|Yes|A guid that identifies the file.|
|LastModifiedTime|Edm.DateTime|Yes|Timestamp in UTC for when doc was last modified.|
|IsViewableByExternalUsers|Edm.Boolean|Yes|Determines if the file is accessible to any external user.|
|||||

### ExchangeMetadata complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|MessageID|Edm.String|Yes|The message ID of the email that triggered the event.|
|From|Edm.String|Yes|The user who sent the email.|
|To|Collection(Edm.String)|No|A collection of email addresses that were on the To line of the message.|
|CC|Collection(Edm.String)|No|A collection of email addresses that were on the CC line of the message.|
|BCC|Collection(Edm.String)|No|A collection of email addresses that were on the BCC line of the message.|
|Subject|Edm.String|Yes|Subject of the email message.|
|Sent|Edm.DateTime|Yes|The time in UTC of when the email was sent.|
|RecipientCount|Edm.Int32|Yes|The total number of all recipients on the TO, CC, and BCC lines of the message.|
|||||

### PolicyDetails complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|PolicyId|Edm.Guid|Yes|The guid of the DLP policy for this event.|
|PolicyName|Edm.String|Yes|The friendly name of the DLP policy for this event.|
|Rules|Collection(Self.[Rules](#rules-complex-type))|Yes|Information about the rules within the policy that were matched for this event.|
|||||

### Rules complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RuleId|Edm.Guid|Yes|The guid of the DLP rule for this event.|
|RuleName|Edm.String|Yes|The friendly name of the DLP rule for this event.|
|Actions|Collection(Edm.String)|No|A list of actions taken as a result of a DLP RuleMatch event.|
|OverriddenActions|Collection(Edm.String)|No|A list of actions previously taken that were now undone as a result of a DLPRuleUndo event. |
|Severity|Edm.String|No|The severity (Low, Medium and High) of the rule match.|
|RuleMode|Edm.String|Yes|Indicate whether the DLP Rule was set to Enforce, Audit with Notify, or Audit only.|
|ConditionsMatched|Self.[ConditionsMatched](#conditionsmatched-complex-type)|No|Details about what conditions of the rule were matched for this event.|
|||||

### ConditionsMatched complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.[SensitiveInformation](#sensitiveinformation-complex-type))|No|Information about the type of sensitive information detected.|
|DocumentProperties|Collection(NameValuePair)|No|Information about document properties that triggered a rule match.|
|OtherConditions|Collection(NameValuePair)|No|A list of key value pairs describing any other conditions that were matched.|
|||||

### SensitiveInformation complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.Int|Yes|The aggregated confidence of all pattern matches for the Sensitive Information Type.|
|Count|Edm.Int|Yes|The total number of sensitive instances detected.|
|Location|Edm.String|No||
|SensitiveType|Edm.Guid|Yes|A guid that identifies the type of sensitive data detected.|
|SensitiveInformationDetections|Self.SensitiveInformationDetections|No|An array of objects that contain sensitive information data with the following details – matched value and context of matched value.|
|SensitiveInformationDetailedClassificationAttributes|Collection(SensitiveInformationDetailedConfidenceLevelResult)|Yes|Information about the count of sensitive information type detected for each of the three confidence levels (High, Medium and Low) and wether it matches the DLP rule or not|
|SensitiveInformationTypeName|Edm.String|No|The name of the sensitive information type.|
|UniqueCount|Edm.Int32|Yes|The unique count of sensitive instances detected.|
|||||

### SensitiveInformationDetailedClassificationAttributes complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.int32|Yes|The confidence level of the pattern that was detected.|
|Count|Edm.Int32|Yes|The number of sensitive instances detected for a partcular confidence level.|
|IsMatch|Edm.Boolean|Yes|Indicates if the given count and confidence level of the sensitive type detected results in a DLP rule match.|
|||||

### SensitiveInformationDetections complex type

DLP sensitive data is only available in the activity feed API to users that have been granted "Read DLP sensitive data" permissions.

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DetectedValues|Collection(Common.NameValuePair)|Yes|An array of sensitive information that was detected. Information contains key value pairs with Value = matched value (eg. Value of credit card) and Context = an excerpt from source content that contains the matched value. |
|ResultsTruncated|Edm.Boolean|Yes|Indicates if the logs were truncated due to large number of results. |
|||||

### ExceptionInfo complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Reason|Edm.String|No|For a DLPRuleUndo event, this indicates why the rule no longer applies, which can be one of 3 reasons: Override, Document Change, or Policy Change|
|FalsePositive|Edm.Boolean|No|Indicates whether the user designated this event as a false positive.|
|Justification|Edm.String|No|If the user chose to override policy, any user-specified justification is captured here.|
|Rules|Collection(Edm.Guid)|No|A collection of guids for each rule that was designated as a false positive or override, or for which an action was undone.|
|||||

## Security and Compliance Center schema

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|No|The date and time at which the cmdlet was executed.|
|ClientRequestId|Edm.String|No|A GUID that can be used to correlate this cmdlet with the Security & Compliance Center UX operations. This information is only used by Microsoft support.|
|CmdletVersion|Edm.String|No|The build version of the cmdlet when it was executed.|
|EffectiveOrganization|Edm.String|No|The GUID for the organization impacted by the cmdlet. (Deprecated: This parameter will stop appearing in the future.)|
|UserServicePlan|Edm.String|No|The Exchange Online Protection service plan assigned to the user that executed the cmdlet.|
|ClientApplication|Edm.String|No|If the cmdlet was executed by an application, as opposed to remote PowerShell, this field contains that application's name.|
|Parameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that do not include Personally Identifiable Information.|
|NonPiiParameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that include Personally Identifiable Information. (Deprecated: This field will stop appearing in the future and its content merged with the Parameters field.)|
|||||

## Security and Compliance Alerts schema

Alert signals include:

- All alerts generated based on [Alert policies in Security & Compliance Center](/office365/securitycompliance/alert-policies#default-alert-policies).
- Office 365 related alerts generated in [Office 365 Cloud App Security](/office365/securitycompliance/office-365-cas-overview) and [Microsoft Cloud App Security](/cloud-app-security/what-is-cloud-app-security).

The UserId and UserKey of these events are always SecurityComplianceAlerts. There are three types of alert events that are stored as the value of the Operation property of the common schema:

- AlertTriggered - A new alert is generated due to a policy match.

- AlertEntityGenerated - A new entity is added to an alert. This event is only applicable to alerts generated based on Alert policies in the security and compliance center. Each generated alert can be associated with one or multiple of these events. For example, an alert policy is defined to trigger an alert if any user deletes more than 100 files in 5 minutes. If two users exceed the threshold around the same time, there will be two AlertEntityGenerated events, but only one AlertTriggered event.

- AlertUpdated - An update was made to the metadata of an alert. This event is logged when the status of an alert is changed (for example, from "Active" to "Resolved") and when someone adds a comment to the alert.

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|AlertId|Edm.Guid|Yes|The Guid of the alert.|
|AlertType|Self.String|Yes|Type of the alert. Alert types include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>System</p></li><li><p>Custom</p></li>|
|Name|Edm.String|Yes|Name of the alert.|
|PolicyId|Edm.Guid|No|The Guid of the policy that triggered the alert.|
|Status|Edm.String|No|Status of the alert. Statuses include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Active</p></li><li><p>Investigating</p></li><li><p>Resolved</p></li><li><p>Dismissed</p></li></ul>|
|Severity|Edm.String|No|Severity of the alert. Severity levels include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Low</p></li><li><p>Medium</p></li><li><p>High</p></li></ul>|
|Category|Edm.String|No|Category of the alert. Categories include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>AccessGovernance</p></li><li><p>DataGovernance</p></li><li><p>DataLossPrevention</p></li><li><p>InsiderRiskManagement</p></li><li><p>MailFlow</p></li><li><p>ThreatManagement</p></li><li><p>Other</p></li></ul>|
|Source|Edm.String|No|Source of the alert. Sources include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Office 365 Security & Compliance</p></li><li><p>Cloud App Security</p></li></ul>|
|Comments|Edm.String|No|Comments left by the users who have viewed the alert. By default, it's "New alert".|
|Data|Edm.String|No|The detailed data blob of the alert or alert entity.|
|AlertEntityId|Edm.String|No|The identifier for the alert entity. This parameter is only applicable to AlertEntityGenerated events.|
|EntityType|Edm.String|No|Type of the alert or alert entity. Entity types include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>User</p></li><li><p>Recipients</p></li><li><p>Sender</p></li><li><p>MalwareFamily</p></li></ul>This parameter is only applicable to AlertEntityGenerated events.|
|||||

## Yammer schema

The Yammer events listed in [Search the audit log in the Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#yammer-activities) will use this schema.

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|ActorUserId|Edm.String|No|Email of user that performed the operation.|
|ActorYammerUserId|Edm.Int64|No|ID of user that performed the operation.|
|DataExportType|Edm.String|No|Returns "data" if data export includes messages, notes, files, topics, users and groups; returns "user" if data export includes users only.|
|FileId|Edm.Int64|No|ID of the file in the operation. |
|FileName|Edm.String|No|Name of the file in the operation. Will appear blank if not relevant to the operation.|
|GroupName|Edm.String|No|Name of the group in the operation. Will appear blank if not relevant to the operation.|
|IsSoftDelete|Edm.Boolean|No|Returns "true" if the network's data retention policy is set to Soft Delete; returns "false" if the network's data retention policy is set to Hard Delete.|
|MessageId|Edm.Int64|No|ID of the message in the operation.|
|YammerNetworkId|Edm.Int64|No|Network ID of the user that performed the operation.|
|TargetUserId|Edm.String|No|Email of target user in the operation. Will appear blank if not relevant to the operation.|
|TargetYammerUserId|Edm.Int64|No|ID of target user in the operation.|
|VersionId|Edm.Int64|No|Version ID of the file in the operation.|
|||||

## Data Center Security Base schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.[DataCenterSecurityEventType](#datacentersecurityeventtype)|Yes|The type of cmdlet event in lock box.|
|||||

### Enum: DataCenterSecurityEventType - Type: Edm.Int32

#### DataCenterSecurityEventType

|**Member name**|**Description**|
|:-----|:-----|
|DataCenterSecurityCmdletAuditEvent|This is the enum value for cmdlet audit type event.|
|||

## Data Center Security Cmdlet schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|Yes|The start time of the cmdlet execution.|
|EffectiveOrganization|Edm.String|Yes|The name of the tenant that the elevation/cmdlet was targeted at.|
|ElevationTime|Edm.Date|Yes|The start time of the elevation.|
|ElevationApprover|Edm.String|Yes|The name of a Microsoft manager.|
|ElevationApprovedTime|Edm.Date|No|The timestamp for when the elevation was approved.|
|ElevationRequestId|Edm.Guid|Yes|A unique identifier for the elevation request.|
|ElevationRole|Edm.String|No|The role the elevation was requested for.|
|ElevationDuration|Edm.Int32|Yes|The duration for which the elevation was active.|
|GenericInfo|Edm.String|No|Used for comments and other generic information.|
|||||

## Microsoft Teams schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|MessageId|Edm.String|No|An identifier for a chat or channel message.|
|Members|Collection(Self.[MicrosoftTeamsMember](#microsoftteamsmember-complex-type))|No|A list of users within a Team.|
|TeamName|Edm.String|No|The name of the team being audited.|
|TeamGuid|Edm.Guid|No|A unique identifier for the team being audited.|
|ChannelType|Edm.String|No|The type of channel being audited (Standard/Private).|
|ChannelName|Edm.String|No|The name of the channel being audited.|
|ChannelGuid|Edm.Guid|No|A unique identifier for the channel being audited.|
|ExtraProperties|Collection(Self.[KeyValuePair](#keyvaluepair-complex-type))|No|A list of extra properties.|
|AddOnType|Self.[AddOnType](#addontype)|No|The type of add-on that generated this event.|
|AddonName|Edm.String|No|The name of the add-on that generated the event.|
|AddOnGuid|Edm.Guid|No|A unique identifier for the add-on that generated the event.|
|TabType|Edm.String|No|Only present for tab events. The type of tab that generated the event.|
|Name|Edm.String|No|Only present for settings events. Name of the setting that changed.|
|OldValue|Edm.String|No|Only present for settings events. Old value of the setting.|
|NewValue|Edm.String|No|Only present for settings events. New value of the setting.|
|MessageURLs|Edm.String|No|Present for any URL sent in Teams messages.|
||||

### MicrosoftTeamsMember complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|UPN|Edm.String|No|The user principal name of the user.|
|Role|Self.[MemberRoleType](#memberroletype)|No|The role of the user within the team.|
|DisplayName|Edm.String|No|The display name of the user.|
|||||

### Enum: MemberRoleType - Type: Edm.Int32

#### MemberRoleType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Member|A user who is a member of the team.|
|1|Owner|A user who is the owner of the team.|
|2|Guest|A user who is not a member of the team.|
||||

### KeyValuePair complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Key|Edm.String|No|The key of the key-value pair.|
|Value|Edm.String|No|The value of the key-value pair.|
|||||

### Enum: AddOnType - Type: Edm.Int32

#### AddOnType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Bot|A Microsoft Teams bot.|
|2|Connector|A Microsoft Teams connector.|
|3|Tab|A Microsoft Teams tab.|
||||

## Microsoft Defender for Office 365 and Threat Investigation and Response schema

[Microsoft Defender for Office 365](/office365/securitycompliance/office-365-atp) and Threat Investigation and Response events are available for Office 365 customers who have an Defender for Office 365 Plan 1, Defender for Office 365 Plan 2, or an E5 subscription. Each event in the Defender for Office 365 feed corresponds to the following that were determined to contain a threat:

- An email message sent by or received by a user in the organization with detections that are made on messages at delivery time and from [Zero hour auto purge](https://support.office.com/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15). 

- URLs clicked by a user in the organization that were detected as malicious at time-of-click based on [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links) protection.  

- A file within SharePoint Online, OneDrive for Business, or Microsoft Teams that was detected as malicious by [Microsoft Defender for Office 365](/office365/securitycompliance/atp-for-spo-odb-and-teams) protection.

- An alert that is triggered and that started an [automated investigation](/office365/securitycompliance/automated-investigation-response-office).

> [!NOTE]
> Microsoft Defender for Office 365 and Office 365 Threat Investigation and Response (formerly known as Office 365 Threat Intelligence) capabilites are now part of Defender for Office 365 Plan 2, with additional threat protection capabilities. To learn more, see [Microsoft Defender for Office 365 plans and pricing](https://products.office.com/exchange/advance-threat-protection) and the [Defender for Office 365 Service Description](/office365/servicedescriptions/office-365-advanced-threat-protection-service-description).

### Email message events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AttachmentData|Collection(Self.[AttachmentData](#attachmentdata))|No|Data about attachments in the email message that triggered the event.|
|DetectionType|Edm.String|Yes|The type of detection (for example, **Inline** - detected at delivery time; **Delayed** - detected after delivery; **ZAP** - messages removed by [Zero hour auto purge](https://support.office.com/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15)). Events with ZAP detection type will typically be preceded by a message with a **Delayed** detection type.|
|DetectionMethod|Edm.String|Yes|The method or technology used by Defender for Office 365 for the detection.|
|InternetMessageId|Edm.String|Yes|The Internet Message Id.|
|NetworkMessageId|Edm.String|Yes|The Exchange Online Network Message Id.|
|P1Sender|Edm.String|Yes|The return path of sender of the email message.|
|P2Sender|Edm.String|Yes|The from sender of the email message.|
|Policy|Self.[Policy](#policy-type-and-action-type)|Yes|The type of filtering policy (for example **Anti-spam** or **Anti-phish**) and related action type (such as **High Confidence Spam**, **Spam**, or **Phish**) relevant to the email message.|
|Policy|Self.[PolicyAction](#policy-action)|Yes|The action configured in the filtering policy (for example, **Move to Junk Mail folder** or **Quarantine**) relevant to the email message.|
|P2Sender|Edm.String|Yes|The **From:** sender of the email message.|
|Recipients|Collection(Edm.String)|Yes|An array of recipients of the email message.|
|SenderIp|Edm.String|Yes|The IP address that submitted the email of Office 365. The IP address is displayed in either an IPv4 or IPv6 address format.|
|Subject|Edm.String|Yes|The subject line of the message.|
|Verdict|Edm.String|Yes|The message verdict.|
|MessageTime|Edm.Date|Yes|Date and time in Coordinated Universal Time (UTC) the email message was received or sent.|
|EventDeepLink|Edm.String|Yes|Deep-link to the email event in Explorer or Real-time reports in the Office 365 Security & Compliance Center.|
|Delivery Action |Edm.String|Yes|The original delivery action on the email message.|
|Original Delivery location |Edm.String|Yes|The original delivery location of the email message.|
|Latest Delivery location |Edm.String|Yes|The latest delivery location of the email message at the time of the event.|
|Directionality |Edm.String|Yes|Identifies whether an email message was inbound, outbound, or an intra-org message.|
|ThreatsAndDetectionTech |Edm.String|Yes|The threats and the corresponding detection technologies. This field exposes all the threats on an email message, including the latest addition on spam verdict.  For example, ["Phish: [Spoof DMARC]","Spam: [URL malicious reputation]"]. The different detection threat and detection technologies are described below.|
|AdditionalActionsAndResults |Collection(Edm.String)|No|The additional actions that were taken on the email, such as ZAP or Manual Remediation. Also includes the corresponding results.|
|Connectors |Edm.String|No|The names and GUIDs of the connectors associated with the email.|
|AuthDetails |Collection(Self.[AuthDetails](#authdetails))|No|The authentication checks that are done for the email. Also includes the values for SPF, DKIM, DMARC, and CompAuth.|
|SystemOverrides |Collection(Self.[SystemOverrides](#systemoverrides))|No|Overrides that are applicable to the email. These can be system or user overrides.|
|Phish Confidence Level |Edm.String|No|Indicates the confidence level associated with Phish verdict. It can be Normal or High.|  
|||||

> [!NOTE]
> We recommend that you use the new ThreatsAndDetectionTech field because it shows multiple verdicts and the updated detection technologies. This also aligns with the values you would see within other experiences like Threat Explorer and Advanced Hunting. 

### Detection technologies

|**Name**|**Description**|
|:-----|:-----|
|General filter |Phishing signals based on rules.|
|Impersonation brand | The file type of the attachment.|
|Spoof external domain |Sender is trying to spoof some other domain.|
|Spoof DMARC |DMARC authentication failure for messages.|
|Impersonation domain |	Impersonation of domains that the customer owns or defines.|
|File detonation |File attachments found to be bad during detonated analysis.|
|File reputation |File attachments marked bad due to bad reputation.|
|File detonation reputation |File attachment marked as bad due to previous detonation reputation.|
|Fingerprint matching |The message was marked as bad due to previous messages.|
|Mailbox intelligence impersonation |Impersonation based on mailbox intelligence.|
|Domain reputation |Analysis based on domain reputation.|
|Spoof intra-org |	Sender is trying to spoof the recipient domain. |
|Advanced filter |	Phishing signals based on machine learning.|
|Anti-malware engine	| Detection from anti-malware engines. |
|Mixed analysis detection	| Multiple filters contributed to the verdict for this message. |
|URL malicious reputation	| The message was considered bad due a malicious URL. |
|URL detonation	| The message was considered bad due to a previous malicious URL detonation. |
|URL detonation reputation| The message was considered bad due to malicious URL detonation. |
|Impersonation user|	Impersonation of users defined by admin or learned through mailbox intelligence.|
|Campaign	|Messages identified as part of a campaign.|

### AttachmentData complex type

#### AttachmentData

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|Yes|The file name of the attachment.|
|FileType|Edm.String|Yes|The file type of the attachment.|
|FileVerdict|Self.[FileVerdict](#fileverdict)|Yes|The file malware verdict.|
|MalwareFamily|Edm.String|No|The file malware family.|
|SHA256|Edm.String|Yes|The file SHA256 hash.|
|||||

> [!NOTE]
> Within the Malware family, you will be able to see the exact MalwareFamily name (for example, HTML/Phish.VS!MSR) or Malicious Payload as a static string. A Malicious Payload should still be treated as malicious email when a specific name isn't identified.

### SystemOverrides complex type
 
#### SystemOverrides

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Details|Edm.String|No|The details about the specific override (such as ETR or Safe Sender) that was applied.|
|FinalOverride|Edm.String|No|Indicates the override that impacted the delivery in the case of multiple overrides.|
|Result|Edm.String|No|Indicates whether the email was set to allowed or blocked based on the override.|
|Source|Edm.String|No|Indicates whether the override was user-configured or tenant-configured.|
|||||

### AuthDetails complex type
 
#### AuthDetails
 
|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Name|Edm.String|No|The name of the specific auth check, such as DKIM or DMARC.|
|Value|Edm.String|No|The value associated with the specific auth check, such as True or False.|
|||||
 
### Enum: FileVerdict - Type: Edm.Int32

#### FileVerdict

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Good|No threats detected.|
|1|Bad|Malware found in attachment.|
|-1|Error|Scan / analysis error.|
|-2|Timeout|Scan / analysis timeout.|
|-3|Pending|Scan / analysis not complete.|
|||||

### Enum: Policy - Type: Edm.Int32

#### Policy type and action type

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Anti-spam, HSPM|High Confidence Spam (HSPM) action in the Anti-spam policy.|
|2|Anti-spam, SPM|Spam (SPM) action in the Anti-spam policy.|
|3|Anti-spam, Bulk|Bulk action in the Anti-spam policy.|
|4|Anti-spam, PHSH|Phish (PHSH) action in the Anti-spam policy.|
|5|Anti-phish, DIMP|Domain Impersonation (DIMP) action in the Anti-phish policy.|
|6|Anti-phish, UIMP|User Impersonation (UIMP) action in the Anti-phish policy.|
|7|Anti-phish, SPOOF|Spoof action in the Anti-phish policy.|
|8|Anti-phish, GIMP|Mailbox intelligence action in the Anti-phish policy.|
|9|Anti-malware, AMP| Malware policy action in the Anti-malware policy.|
|10|Safe attachment, SAP| Policy action in the Safe attachments in Defender for Office 365 policy.|
|11|Exchange transport rule, ETR| Policy action in the Exchange Transport Rule.|
|12|Anti-malware, ZAPM| Malware policy action in the Anti-malware policy applied to Zero-hour auto purge (ZAP).|
|13|Anti-phish, ZAPP| Phish policy action in the Anti-phish policy applied to ZAP.|
|14|Anti-phish, ZAPS| Spam policy action in the Anti-spam policy applied to ZAP.|
|15|Anti-spam, High confidence phish email (HPHISH)|High confidence Phish policy action in Anti-spam policy.|
|17|Anti-spam, Outbound spam policy (OSPM)|Policy action in the outbound spam filter policy in Anti-spam.|
||||

### Enum: PolicyAction - Type: Edm.Int32

#### Policy action

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|MoveToJMF|Policy action is to move to Junk Mail folder.|
|1|AddXHeader|Policy action is to add X-header to the email message.|
|2|ModifySubject|Policy action is to modify subject in the email message with information specified by the filtering policy.|
|3|Redirect|Policy action is to redirect email message to email address specificed by the filtering policy.|
|4|Delete|Policy action is to delete (drop) the email message.|
|5|Quarantine|Policy action is to quarantine the email message.|
|6|NoAction| Policy is configured to take no action on the email message.|
|7|BccMessage|Policy action is to Bcc the email message to email address specificed by the filtering policy.|
|8|ReplaceAttachment|Policy action is to replace the attachment in the email message as specified by the filtering policy.|
||||

### URL time-of-click events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|UserId|Edm.String|Yes|Identifier (for example, email address) for the user who clicked on the URL.|
|AppName|Edm.String|Yes|Office 365 service from which the URL was clicked (for example, Mail).|
|URLClickAction|Self.[URLClickAction](#urlclickaction)|Yes|Click action for the URL based on the organization's policies for [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links).|
|SourceId|Edm.String|Yes|Identifier for the Office 365 service from which the URL was clicked (for example, for mail this is the Exchange Online Network Message Id).|
|TimeOfClick|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the user clicked the URL.|
|URL|Edm.String|Yes|URL clicked by the user.|
|UserIp|Edm.String|Yes|The IP address for the user who clicked the URL. The IP address is displayed in either an IPv4 or IPv6 address format.|
|||||

### Enum: URLClickAction - Type: Edm.Int32

#### URLClickAction

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|2|Blockpage|User blocked from navigating to the URL by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links).|
|3|PendingDetonationPage|User presented with the detonation pending page by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links).|
|4|BlockPageOverride|User blocked from navigating to the URL by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links); however user overrode block to navigate to the URL.|
|5|PendingDetonationPageOverride|User presented with the detonation page by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links); however user overrode to navigate to the URL.|
|||||

### File events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileData|Self.[FileData](#filedata)|Yes|Data about the file that triggered the event.|
|SourceWorkload|Self.[SourceWorkload](#sourceworkload)|Yes|Workload or service where the file was found (for example, SharePoint Online, OneDrive for Business, or Microsoft Teams)
|DetectionMethod|Edm.String|Yes|The method or technology used by Microsoft Defender for Office 365 for the detection.|
|LastModifiedDate|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the file was created or last modified.|
|LastModifiedBy|Edm.String|Yes|Identifier (for example, an email address) for the user who created or last modified the file.|
|EventDeepLink|Edm.String|Yes|Deep-link to the file event in Explorer or Real-time reports in the Security & Compliance Center.|
|||||

### FileData complex type

#### FileData

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DocumentId|Edm.String|Yes|Unique identifier for the file in SharePoint, OneDrive, or Microsoft Teams.|
|FileName|Edm.String|Yes|Name of the file that triggered the event.|
|FilePath|Edm.String|Yes|Path (location) for the file in SharePoint, OneDrive, or Microsoft Teams.|
|FileVerdict|Self.[FileVerdict](#fileverdict)|Yes|The file malware verdict.|
|MalwareFamily|Edm.String|No|The file malware family.|
|SHA256|Edm.String|Yes|The file SHA256 hash.|
|FileSize|Edm.String|Yes|Size for the file in bytes.|
|||||

### Enum: SourceWorkload - Type: Edm.Int32

#### SourceWorkload

|**Value**|**Member name**|
|:-----|:-----|
|0|SharePoint Online|
|1|OneDrive for Business|
|2|Microsoft Teams|
|||||

## Submission schema

[Submission](/microsoft-365/security/office-365-security/report-junk-email-messages-to-microsoft) events are available for every [Office 365 customers since it comes with security](/microsoft-365/security/office-365-security/overview). This includes organizations that use Exchange Online Protection and Microsoft Defender for Office 365. Each event in the submission feed corresponds to false positives or false negatives that were submitted as an:

- **Admin submission**. Messages, files, or URLs submitted to Microsoft for analysis.
- **User-reported item**. Messages reported by end users to the admin or Microsoft for review.

### Submission events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AdminSubmissionRegistered|Edm.String|No|Admin submission is registered and is pending for processing.|
|AdminSubmissionDeliveryCheck|Edm.String|No|Admin submission system checked the email's policy.|
|AdminSubmissionSubmitting|Edm.String|No|Admin submission system is submitting the email.|
|AdminSubmissionSubmitted|Edm.String|No|Admin submission system submitted the email.|
|AdminSubmissionTriage|Edm.String|No|Admin submission is triaged by grader.|
|AdminSubmissionTimeout|Edm.String|No|Admin submission is timef out with no result.|
|UserSubmission|Edm.String|No|Submission was first reported by an end user.|
|UserSubmissionTriage|Edm.String|No|User submission is triaged by grader.|
|CustomSubmission|Edm.String|No|Message reported by a user was sent to the organization's custom mailbox as set in the user reported messages settings.|
|AttackSimUserSubmission|Edm.String|No|The user-reported message was actually a phish simulation training message.|
|AdminSubmissionTablAllow|Edm.String|No|An allow was created at time of submission to immediately take action on similar messages while it is being rescanned.|
|SubmissionNotification|Edm.String|No|Admin feedback is sent to end user.|
|||||

## Automated investigation and response events in Office 365

[Office 365 automated investigation and response (AIR)](/office365/securitycompliance/automated-investigation-response-office) events are available for Office 365 customers who have a subscription that includes Microsoft Defender for Office 365 Plan 2 or Office 365 E5. Investigation events are logged based on a change in investigation status. For example, when an administrator takes an action that changes the status of an investigation from Pending Actions to Completed, an event is logged.

Currently, only automated investigation are logged. (Events for manually generated investigations are coming soon.) The following status values are logged:

- Investigation Started
- No threats found
- Terminated by System
- Pending Action
- Threats Found
- Remediated
- Failed
- Terminated by throttling
- Terminated By User
- Running

### Main investigation schema

|Name    |Type    |Description  |
|----|----|----|
|InvestigationId    |Edm.String    |Investigation ID/GUID |
|InvestigationName    |Edm.String    |Name of the investigation |
|InvestigationType    |Edm.String    |Type of the investigation. Can take one of the following values:<br/>- User-Reported Messages<br/>- Zapped Malware<br/>- Zapped Phish<br/>- Url Verdict Change<p>(Manual investigations are currently not available and are coming soon.) |
|LastUpdateTimeUtc    |Edm.Date    |UTC time of the last update for an investigation |
|StartTimeUtc    |Edm.Date    |Start time for an investigation |
|Status     |Edm.String     |State of investigation, Running, Pending Actions, etc. |
|DeeplinkURL    |Edm.String    |Deep link URL to an investigation in Office 365 Security & Compliance Center |
|Actions |Collection (Edm.String)    |Collection of actions recommended by an investigation |
|Data    |Edm.String    |Data string which contains more details about investigation entities, and information about alerts related to the investigation. Entities are available in a separate node within the data blob. |
||||

### Actions

|Field    |Type    |Description |
|----|----|----|
|ID     |Edm.String    |Action ID|
|ActionType    |Edm.String    |The type of the action, such as email remediation |
|ActionStatus    |Edm.String    |Values include: <br/>- Pending<br/>- Running<br/>- Waiting on resource<br/>- Completed<br/>- Failed |
|ApprovedBy    |Edm.String    |Null if auto approved; otherwise, the username/id (this is coming soon) |
|TimestampUtc    |Edm.DateTime    |The timestamp of the action status change |
|ActionId    |Edm.String    |Unique identifier for action |
|InvestigationId    |Edm.String    |Unique identifier for investigation |
|RelatedAlertIds    |Collection(Edm.String)    |Alerts related to an investigation |
|StartTimeUtc    |Edm.DateTime    |Timestamp of action creation |
|EndTimeUtc    |Edm.DateTime    |Action final status update timestamp |
|Resource Identifiers     |Edm.String     |Consists of the Azure Active Directory tenant ID.|
|Entities    |Collection(Edm.String)    |List of one or more affected entities by action |
|Related Alert IDs    |Edm.String    |Alert related to an investigation |
||||

### Entities

#### MailMessage (email)

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"mail-message"  |
|Files    |Collection (Self.File) |Details about the files of this message's attachments |
|Recipient    |Edm.String    |The recipient of this mail message |
|Urls    |Collection(Self.URL) |The Urls contained in this mail message  |
|Sender    |Edm.String    |The sender's email address  |
|SenderIP    |Edm.String    |The sender's IP address  |
|ReceivedDate    |Edm.DateTime    |The received date of this message  |
|NetworkMessageId    |Edm.Guid     |The network message id of this mail message  |
|InternetMessageId    |Edm.String  |The internet message id of this mail message |
|Subject    |Edm.String    |The subject of this mail message  |
||||

#### IP

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"ip" |
|Address    |Edm.String    |The IP address as a string, such as `127.0.0.1`
||||

#### URL

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"url" |
|Url    |Edm.String    |The full URL to which an entity points  |
||||

#### Mailbox (also equivalent to the user) 

|Field    |Type    |Description |
|----|----|----|
|Type    |Edm.String    |"mailbox"  |
|MailboxPrimaryAddress    |Edm.String    |The mailbox's primary address  |
|DisplayName    |Edm.String    |The mailbox's display name |
|Upn    |Edm.String    |The mailbox's UPN  |
||||

#### File

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"file" |
|Name    |Edm.String    |The file name without path |
FileHashes |Collection (Edm.String)    |The file hashes associated with the file |
||||

#### FileHash

|Field    |Type    |Description |
|----|----|----|
|Type    |Edm.String    |"filehash" |
|Algorithm    |Edm.String    |The hash algorithm type, which can be one of these values:<br/>- Unknown<br/>- MD5<br/>- SHA1<br/>- SHA256<br/>- SHA256AC
|Value    |Edm.String    |The hash value  |
||||

#### MailCluster

|Field    |Type    |Description   |
|----|----|----|
|Type    |Edm.String    |"MailCluster" <br/>Determines the type of entity being discussed |
|NetworkMessageIds    |Collection (Edm.String)    |List of the mail message IDs that are part of the mail cluster |
|CountByDeliveryStatus    |Collections (Edm.String)    |Count of mail messages by DeliveryStatus string representation |
|CountByThreatType    |Collections (Edm.String) |Count of mail messages by ThreatType string representation |
|Threats    |Collections (Edm.String)    |The threats of mail messages that are part of the mail cluster. Threats include values like Phish and Malware. |
|Query    |Edm.String    |The query that was used to identify the messages of the mail cluster  |
|QueryTime    |Edm.DateTime    |The query time  |
|MailCount    |Edm.int    |The number of mail messages that are part of the mail cluster  |
|Source    |String    |The source of the mail cluster; the value of the cluster source. |
||||

## Hygiene events schema

Hygiene events are related to outbound spam protection. These events are related to users who are restricted from sending email. For more information, see:

- [Outbound spam protection](/microsoft-365/security/office-365-security/outbound-spam-controls)

- [Remove blocked users from the Restricted Users portal in Office 365](/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam)

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Audit|Edm.String|No|System information related to the hygiene event.|
|Event|Edm.String|No|The type of hygiene event. The values for this parameter are **Listed** or **Delisted**.|
|EventId|Edm.Int64|No|The ID of the hygiene event type.|
|EventValue|Edm.String|No|The user who was impacted.|
|Reason|Edm.String|No|Details about the hygiene event.|
|||||

## Power BI schema

The Power BI events listed in [Search the audit log in the Office 365 Protection Center](/power-bi/service-admin-auditing#activities-audited-by-power-bi) will use this schema.

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
| AppName               | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The name of the app where the event occurred. |
| DashboardName         | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The name of the dashboard where the event occurred. |
| DataClassification    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The [data classification](/power-bi/service-data-classification), if any, for the dashboard where the event occurred. |
| DatasetName           | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The name of the dataset where the event occurred. |
| MembershipInformation | Collection([MembershipInformationType](#membershipinformationtype-complex-type))   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | Membership information about the group. |
| OrgAppPermission      | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | Permissions list for an organizational app (entire organization, specific users, or specific groups). |
| ReportName            | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The name of the report where the event occurred. |
| SharingInformation    | Collection([SharingInformationType](#sharinginformationtype-complex-type))   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"    |  No  | Information about the person to whom a sharing invitation is sent. |
| SwitchState           | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | Information about the state of various tenant level switches. |
| WorkSpaceName         | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  No  | The name of the workspace where the event occurred. |
|||||

### MembershipInformationType complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
| MemberEmail | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The email address of the group. |
| Status      | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | Not currently populated. |
|||||

### SharingInformationType complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
| RecipientEmail    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The email address of the recipient of a sharing invitation. |
| RecipientName    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The name of the recipient of a sharing invitation. |
| ResharePermission | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The permission being granted to the recipient. |
|||||

## Dynamics 365 schema

The audit records for events related to model-driven apps in Dynamics 365 events use both a base and an entity operation schema. For more information, see [Enable and use Activity Logging](/power-platform/admin/enable-use-comprehensive-auditing#model-driven-apps-in-dynamics-365-schema).

### Dynamics 365 base schema

| **Parameters**     | **Type**            | **Mandatory?** | **Description**|
|:------------------ | :------------------ | :--------------|:--------------|
|CrmOrganizationUniqueName|Edm.String|Yes|The unique name of the organization.|
|InstanceUrl|Edm.String|Yes|The URL to the instance.|
|ItemUrl|Edm.String|No|The URL to the record emitting the log.|
|ItemType|Edm.String|No|The name of the entity.|
|UserAgent|Edm.String|No|The unique identifier of the user GUID in the organization.|
|Fields|Collection(Common.NameValuePair)|No|A JSON object that contains the property key-value pairs that were created or updated.|
|||||

### Dynamics 365 entity operation schema

Entity events from model-driven apps in Dynamics 365 use this schema to build on the Dynamics 365 base schema. This schema includes information about the entity operation that triggered the audited event.

| **Parameters**     | **Type**            | **Mandatory?** | **Description**|
|:------------------ | :------------------ | :--------------|:--------------|
|EntityId|Edm.Guid|No|The unique identifier of the entity.|
|EntityName|Edm.String|Yes|The name of the entity in the organization. Example of entities include `contact` or `authentication`.|
|Message|Edm.String|Yes|This parameter contains the operation that was performed in related to the entity. For example, if a new contact was created, the value of the Message property is  `Create` and the corresponding value of the EntityName property is `contact`.|
|Query|Edm.String|No|The parameters of the filter query that was used while executing the FetchXML operation.|
|PrimaryFieldValue|Edm.String|No|Indicates the value for the attribute that is the primary field for the entity.|
|||||

## Workplace Analytics schema

The WorkPlace Analytics events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-workplace-analytics-activities) will use this schema.

| **Parameters**     | **Type**            | **Mandatory?** | **Description**|
|:------------------ | :------------------ | :--------------|:--------------|
| WpaUserRole        | Edm.String | No     | The Workplace Analytics role of the user who performed the action.|
| ModifiedProperties | Collection (Common.ModifiedProperty) | No | This property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property.|
| OperationDetails   | Collection (Common.NameValuePair)    | No | A list of extended properties for the setting that was changed. Each property will have a **Name** and **Value**.|
||||

## Quarantine schema

The quarantine events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#quarantine-activities) will use this schema. For more information about quarantine, see [Quarantine email messages in Office 365](/microsoft-365/security/office-365-security/quarantine-email-messages).

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RequestType|Self.[RequestType](#enum-requesttype---type-edmint32)|No|The type of quarantine request performed by a user.|
|RequestSource|Self.[RequestSource](#enum-requestsource---type-edmint32)|No|The source of a quantine request can come from the Security & Compliance Center (SCC), a cmdlet, or a URLlink.|
|NetworkMessageId|Edm.String|No|The network message id of quarantined email message.|
|ReleaseTo|Edm.String|No|The recipient of the email message.|
|||||

### Enum: RequestType - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Preview|This is a request from a user to preview an email message that is deemed to be harmful.|
|1|Delete|This is a request from a user to delete an email message that is deemed to be harmful.|
|2|Release|This is a request from a user to release an email message that is deemed to be harmful.|
|3|Export|This is a request from a user to export an email message that is deemed to be harmful.|
|4|ViewHeader|This is a request from a user to view the header an email message that is deemed to be harmful.|
||||

### Enum: RequestSource - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|SCC|The Security & Compliance center (SCC) is the source where the request from a user to preview, delete, release, export, or view the header of a potentially harmful email message can originate from. |
|1|Cmdlet|A cmdlet is the source where the request from a user to preview, delete, release, export, or view the header of a potentially harmful email message can originate from.|
|2|URLlink|This is a source where the request from a user to preview, delete, release, export, or view the header of potentially harmful email message can originate from.|
||||

## Microsoft Forms schema

The Microsoft Forms events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-forms-activities) will use this schema.

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FormsUserTypes|Collection(Self.[FormsUserTypes](#formsusertypes))|Yes|The role of the user who performed the action.  The values for this parameter are Admin, Owner, Responder, or Coauthor.|
|SourceApp|Edm.String|Yes|Indicates if the action is from Forms website or from another App.|
|FormName|Edm.String|No|The name of the current form.|
|FormId |Edm.String|No|The Id of the target form.|
|FormTypes|Collection(Self.[FormTypes](#formtypes))|No|Indicates whether this is a Form, Quiz, or Survey.|
|ActivityParameters|Edm.String|No|JSON string containing activity parameters. See [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-forms-activities) for more details.|
||||

### Enum: FormsUserTypes - Type: Edm.Int32

#### FormsUserTypes

|**Value**|**Form User Type**|**Description**|
|:-----|:-----|:-----|
|0|Admin|An administrator who has access to the form.|
|1|Owner|A user who is the owner of the form.|
|2|Responder|A user who has submitted a response to a form.|
|3|Coauthor|A user who has used a collaboration link provided by the form owner to login and edit a form.|
||||

### Enum: FormTypes - Type: Edm.Int32

#### FormTypes

|**Value**|**Form Types**|**Description**|
|:-----|:-----|:-----|
|0|Form|Forms that are created with the New Form option.|
|1|Quiz|Quizzes that are created with the New Quiz option.  A quiz is a special type of form that includes additional features such as point values, auto and manual grading, and commenting.|
|2|Survey|Surveys that are created with the New Survey option.  A survey is a special type of form that includes additional features such as CMS integration and support for Flow rules.|
||||

## MIP label schema

Events in the Microsoft Information Protection (MIP) label schema are triggered when Microsoft 365 detects an email message processed by agents in the Transport pipeline that has a sensitivity label applied to it. The sensitivity label may have been applied manually or automatically, and it may have been applied within or outside of the Transport pipeline. Sensitivity labels can be automatically applied to email messages by auto-apply label policies.

The intent of this audit schema is to represent the sum of all email activity that involves sensitivity labels. In other words, there should be an recorded audit activity for each email message that is sent to or from users in the organization that has a sensitivity label applied to it, regardless of when or how the sensitivity label was applied. For more information about sensitivity labels, see:

- [Learn about sensitivity labels](/microsoft-365/compliance/sensitivity-labels)

- [Apply a sensitivity label to content automatically](/microsoft-365/compliance/apply-sensitivity-label-automatically)

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Sender|Edm.String|No|The email address in the From field of the email message.|
|Receivers|Collection(Edm.String)|No|All email addresses in the To, CC, and Bcc fields of the email message.|
|ItemName|Edm.String|No|The string in the Subject field of the email message.|
|LabelId|Edm.Guid|No|The GUID of the sensitiviy label applied to the email message.|
|LabelName|Edm.String|No|The name of the sensitivity label applied to the email message.|
|LabelAction|Edm.String|No|The actions specified by the sensitivity label that were applied to the email message before the message entered the mail transport pipeline.|
|LabelAppliedDateTime|Edm.Date|No|The date the sensitivity label was applied to the email message.|
|ApplicationMode|Edm.String|No|Specifies how the sensitivity label was applied to the email message. The **Privileged** value indicates the label was manually applied by a user. The **Standard** value indicates the label was auto-applied by a client-side or service-side labeling process.|
|||||

## Communication compliance Exchange schema

The communication compliance events listed in the Office 365 audit log use this schema. This includes audit records for the **SupervisoryReviewOLAudit** operation that's generated when email message content contains offensive language identified by anti-spam models with a match accuracy of \>= 99.5%.

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
| ExchangeDetails |[ExchangeDetails](#exchangedetails)|No|Properties of the email message that triggered the SupervisoryReviewOLAudit event.|
|||||

### Enum: ExchangeDetails - Type: ExchangeDetails

#### ExchangeDetails

| **Member name**   | **Type**| **Description**|
|:----------------- | :-------|:---------------|
| NetworkMessageId  |Edm.Guid|The network message ID of the email message.|
| InternetMessageId |Edm.String|The internet message ID of the email message.|
| AttachmentData|Collection([AttachmentDetails](#attachmentdetails))|Information about files attached to the email message.|
| Recipients|Collection(Edm.String)|The email addresses in the To, Cc, and Bcc fields of the email message. |
| Subject|Edm.String|The text in the Subject field of the email message.|
| MessageTime|Edm.Date|The date and time the email message was sent.|
| From| Edm.String|The email address in the From field of the email message.|
| Directionality|Edm.String|The origination status of the email message.|
||||

### Enum: AttachmentDetails - Type: Edm.Int32

#### AttachmentDetails

| **Member name** | **Type**   | **Description**|
|:--------------- |:---------- | :--------------|
| FileName        | Edm.String | The name of the file attached to the email message.|
| FileType        | Edm.String | The file extension of the file attached to the email message.|
| SHA256          | Edm.String | The SHA-256 hash of the file attached to the email message.|
||||

## Office Scripts run events schema

This schema extends the common schema with the properties specific to Office Scripts run events. For more information, see [Office Scripts in Excel on the web](/office/dev/scripts/overview/excel).

|Paramters  |Type  |Mandatory?  |Description |
|:---------|:---------|:---------|:---------|
|FileObjectId     | Edm.String        |Yes          |The full URL path name of the OneDrive or SharePoint file that the script ran on.         |
|ScriptCreatorId      |Edm.String         |Yes          |The UPN of the user who created the script.         |
|ScriptLastModifiedBy     | Edm.String        |Yes          |The UPN of the user who last modified the script.         |
|ScriptLastModifiedDate     | Edm.Date        |Yes          | The date the script was last modified.         |
|FlowDetailsUrl     | Edm.String         | Yes         | The URL for the Flow Admin center where an admin can perform additional administrative functions, including managing permissions. This parameter is only populated when the script was ran in a Flow using Power Automate.        |
|||||
