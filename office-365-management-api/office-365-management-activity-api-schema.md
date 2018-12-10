---
ms.TocTitle: Office 365 Management Activity API schema
title: Office 365 Management Activity API schema
description: The Office 365 Management Activity API schema is provided as a data service in  two layers - Common schema and product-specific schema.
ms.ContentId: 1c2bf08c-4f3b-26c0-e1b2-90b190f641f5
ms.topic: reference (API)
ms.date: 
---

# Office 365 Management Activity API schema
 
The Office 365 Management Activity API schema is provided as a data service in  two layers:

- **Common schema**. The interface to access core Office 365 auditing concepts such as Record Type, Creation Time, User Type, and Action as well as to provide core dimensions (such as User ID), location specifics (such as Client IP address), and product-specific properties (such as Object ID). It establishes consistent and uniform views for users to extract all Office 365 audit data in a few top level views with the appropriate parameters, and provides a fixed schema for all the data sources, which significantly reduces the cost of learning. Common schema is sourced from product data that is owned by each product team, such as Exchange, SharePoint, Azure Active Directory, Yammer, and OneDrive for Business. The Object ID field can be extended by product teams to add product specific properties.
    
- **Product-specific schema**. Built on top of the Common schema to provide a set of product-specific attributes; for example, Sway schema, SharePoint schema, OneDrive for Business schema, and Exchange admin schema.
    
**Which layer should you use for your scenario?**
In general, if the data is available in a higher layer, don't go back to a lower layer. In other words, if the data requirement can be fit in a product-specific schema, you don't need to go back to the Common schema. 

## Office 365 Management API schemas

This article provides details on the Common schema as well as each of the product specific schemas. The following table describes the available schemas. 

|Name of schema|Description|
|:-----|:-----|
|[Common schema](#common-schema)|The view to extract Record Type, User ID, Client IP, User type and Action along with core dimensions such as user properties (such as UserID), location properties (such as Client IP), and product-specific properties (such as Object Id).|
|[SharePoint Base schema](#sharepoint-base-schema)|Extends the Common schema with the properties specific to all SharePoint audit data.|
|[SharePoint File Operations](#sharepoint-file-operations)|Extends the SharePoint Base schema with the properties specific to file access and manipulation in SharePoint.|
|[SharePoint Sharing schema](#sharepoint-sharing-schema)|Extends the SharePoint Base schema with the properties specific to file sharing.|
|[SharePoint schema](#sharepoint-schema)|Extends the SharePoint Base schema with the properties specific to SharePoint, but unrelated to file access and manipulation.|
|[Project schema](#project-schema)|Extends the SharePoint Base schema with the properties specific to Project.|
|[Exchange Admin schema](#exchange-admin-schema)|Extends the Common schema with the properties specific to all Exchange admin audit data.|
|[Exchange Mailbox schema](#exchange-mailbox-schema)|Extends the Common schema with the properties specific to all Exchange mailbox audit data.|
|[Azure Active Directory Base schema](#azure-active-directory-base-schema)|Extends the Common schema with the properties specific to all Azure Active Directory audit data.|
|[Azure Active Directory Account Logon schema](#azure-active-directory-account-logon-schema)|Extends the Azure Active Directory Base schema with the properties specific to all Azure Active Directory logon events.|
|[Azure Active Directory STS Logon schema](#azure-active-directory-sts-logon-schema)|Extends the Azure Active Directory Base schema with the properties specific to all Azure Active Directory STS logon events.|
|[Azure Active Directory schema](#azure-active-directory-schema)|Extends the Common schema with the properties specific to all Azure Active Directory audit data.|
|[DLP schema](#dlp-schema)|Extends the Common schema with the properties specific to Data Loss Prevention events.|
|[Security and Compliance Center schema](#security-and-compliance-center-schema)|Extends the Common schema with the properties specific to all Security and Compliance Center events.|
|[Security and Compliance Alerts schema](#security-and-compliance-alerts-schema)|Extends the Common schema with the properties specific to all Office 365 security and compliance alerts.|
|[Yammer schema](#yammer-schema)|Extends the Common schema with the properties specific to all Yammer events.|
|[Sway schema](#sway-schema)|Extends the Common schema with the properties specific to all Sway events.|
|[Data Center Security Base schema](#data-center-security-base-schema)|Extends the Common schema with the the properties specific to all data center security audit data.|
|[Data Center Security Cmdlet schema](#data-center-security-cmdlet-schema)|Extends the Data Center Security Base schema with the properties specific to all data center security cmdlet audit data.|
|[Microsoft Teams schema](#microsoft-teams-schema)|Extends the Common schema with the properties specific to all Microsoft Teams events.|
|[Microsoft Teams Add-ons schema](#microsoft-teams-add-ons-schema)|Extends the Microsoft Teams schema with the properties specific to Microsoft Teams Add-ons.|
|[Microsoft Teams Settings schema](#microsoft-teams-settings-schema)|Extends the Microsoft Teams schema with the properties specific to Microsoft Teams settings change events.|
|[Office 365 Advanced Threat Protection and Threat Intelligence schema](#office-365-advanced-threat-protection-and-threat-intelligence-schema)|Extends the Common schema with the properties specific to Office 365 Advanced Threat Protection and Threat Intelligence data.|

## Common schema

**EntityType Name**: AuditRecord

|Parameter|Type|Mandatory?|Description|
|:-----|:-----|:-----|:-----|
|Id|Combination GUIDEdm.Guid|Yes|Unique identifier of an audit record.|
|RecordType|Self.[AuditLogRecordType](#auditlogrecordtype)|Yes|The type of operation indicated by the record. See the [AuditLogRecordType](#auditlogrecordtype) table for details on the types of audit log records.|
|CreationTime|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the user performed the activity.|
|Operation|Edm.String|Yes|The name of the user or admin activity. For a description of the most common operations/activities, see [Search the audit log in the Office 365 Protection Center](http://go.microsoft.com/fwlink/p/?LinkId=708432). For Exchange admin activity, this property identifies the name of the cmdlet that was run. For Dlp events, this can be "DlpRuleMatch", "DlpRuleUndo" or "DlpInfo", which are described under "DLP schema" below.|
|OrganizationId|Edm.Guid|Yes|The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType|Self.[UserType](#user-type)|Yes|The type of user that performed the operation. See the [UserType](#user-type) table for details on the types of users.|
|UserKey|Edm.String|Yes|An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts.|
|Workload|Edm.String|No|The Office 365 service where the activity occurred in the Workload string. The possible values for this property are:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Exchange</p></li><li><p>SharePoint</p></li><li><p>OneDrive</p></li><li><p>Azure Active Directory</p></li><li><p>SecurityComplianceCenter</p></li><li><p>Sway</p></li><li><p>ThreatIntelligence</p></li></ul>|
|ResultStatus|Edm.String|No|Indicates whether the action (specified in the Operation property) was successful or not. Possible values are **Succeeded**, **PartiallySucceded**, or **Failed**. For Exchange admin activity, the value is either **True** or **False**.|
|ObjectId|Edm.string|No|For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet.|
|UserId|Edm.string|Yes|The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, `my_name@my_domain_name`. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included.|
|ClientIp|Edm.String|Yes|The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format.|
|Scope|Self.[AuditLogScope](#auditlogscope)|No|Was this event created by a hosted O365 service or an on-premises server? Possible values are **online** and **onprem**. Note that SharePoint is the only workload currently sending events from on-premises to O365.|

### Enum: AuditLogRecordType - Type: Edm.Int32

#### AuditLogRecordType

|Value|Member name|Description|
|:-----|:-----|:-----|
|1|ExchangeAdmin|Events from the Exchange admin audit log.|
|2|ExchangeItem|Events from an Exchange mailbox audit log for actions that are performed on a single item, such as creating or receiving an email message.|
|3|ExchangeItemGroup|Events from an Exchange mailbox audit log for actions that can be performed on multiple items, such as moving or deleted one or more email messages.|
|4|SharePoint|SharePoint events.|
|6|SharePointFileOperation|SharePoint file operation events.|
|8|AzureActiveDirectory|Azure Active Directory events.|
|9|AzureActiveDirectoryAccountLogon|Azure Active Directory OrgId logon events (deprecating).|
|10|DataCenterSecurityCmdlet|Data Center security cmdlet events.|
|11|ComplianceDLPSharePoint|Data loss protection (DLP) events in SharePoint and OneDrive for Business.|
|12|Sway|Events from the Sway service and clients.|
|13|ComplianceDLPExchange|Data loss protection (DLP) events in Exchange, when configured via Unified DLP Policy. DLP events based on Exchange Transport Rules are not supported.|
|14|SharePointSharingOperation|SharePoint sharing events.|
|15|AzureActiveDirectoryStsLogon|Secure Token Service (STS) logon events in Azure Active Directory.|
|18|SecurityComplianceCenterEOPCmdlet|Admin actions from the Security & Compliance Center.|
|20|PowerBIAudit|Power BI events.|
|21|CRM|Microsoft CRM events.|
|22|Yammer|Yammer events.|
|23|Skype for business|Microsoft Skype for business events.|
|24|Discovery|Events for eDiscovery activities performed by running content searches and managing eDiscovery cases in the Security & Compliance Center.|
|25|MicrosoftTeams|Events from Microsoft Teams.|
|26|MicrosoftTeamsAddOns|Events from Microsoft Teams Add-ons.|
|27|MicrosoftTeamsSettingsOperation|Settings changes from Microsoft Teams.|
|28|ThreatIntelligence|Office 365 Advanced Threat Protection and Threat Intelligence events.|
|30|MicrosoftFlow|Microsoft Flow events.|
|32|MicrosoftStream|Microsoft Stream events.|
|35|Project|Microsoft Project events.|
|36|SharepointListOperation|Sharepoint List events.|
|40|SecurityComplianceAlerts|Security and compliance alert signals.|

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

> [!NOTE] 
> Only Exchange operations include a user type. SharePoint operations don't specify a user type. 

### Enum: AuditLogScope - Type: Edm.Int32

#### AuditLogScope

|Value|Member name|Description|
|:-----|:-----|:-----|
|0|Online|This event was created by a hosted O365 service.|
|1|Onprem|This event was created by an on-premises server.|


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

### Enum: EventSource - Type: Edm.Int32

#### EventSource

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|SharePoint|The event source is SharePoint.|
|1|ObjectModel|The event source is ObjectModel.|


### Enum: SharePointAuditOperation - Type: Edm.Int32

|**Member name**|**Description**|
|:-----|:-----|
|AccessInvitationAccepted*|The recipient of an invitation to view or edit a shared file (or folder) has accessed the shared file by clicking on the link in the invitation.|
|AccessInvitationCreated*|User sends an invitation to another person (inside or outside their organization) to view or edit a shared file or folder on a SharePoint or OneDrive for Business site. The details of the event entry identifies the name of the file that was shared, the user the invitation was sent to, and the type of the sharing permission selected by the person who sent the invitation.|
|AccessInvitationExpired*|An invitation sent to an external user expires. By default, an invitation sent to a user outside of your organization expires after 7 days if the invitation isn't accepted.|
|AccessInvitationRevoked*|The site administrator or owner of a site or document in SharePoint or OneDrive for Business withdraws an invitation that was sent to a user outside your organization. An invitation can be withdrawn only before it's accepted.|
|AccessInvitationUpdated*|The user who created and sent an invitation to another person to view or edit a shared file (or folder) on a SharePoint or OneDrive for Business site resends the invitation.|
|AccessRequestApproved|The site administrator or owner of a site or document in SharePoint or OneDrive for Business approves a user request to access the site or document.|
|AccessRequestCreated|User requests access to a site or document in SharePoint or OneDrive for Business that they don't have permission to access. |
|AccessRequestRejected*|The site administrator or owner of a site or document in SharePoint declines a user request to access the site or document.|
|ActivationEnabled*|Users can browser-enable form templates that don't contain form code, require full trust, enable rendering on a mobile device, or use a data connection managed by a server administrator.|
|AdministratorAddedToTermStore*|Term store administrator added.|
|AdministratorDeletedFromTermStore*|Term store administrator deleted.|
|AllowGroupCreationSet*|Site administrator or owner adds a permission level to a SharePoint or OneDrive for Business site that allows a user assigned that permission to create a group for that site.|
|AppCatalogCreated*|App catalog created to make custom business apps available for your SharePoint Environment.|
|AuditPolicyRemoved*|Document LifeCycle Policy has been removed for a site collection.|
|AuditPolicyUpdate*|Document LifeCycle Policy has been updated for a site collection.|
|AzureStreamingEnabledSet*|A video portal owner has allowed video streaming from Azure.|
|CollaborationTypeModified*|The type of collaboration allowed on sites (for example, intranet, extranet, or public) has been modified.|
|ConnectedSiteSettingModified|User has either created, modified or deleted the link between a project and a project site or the user modifies the synchronization setting on the link in Project Web App.|
|CreateSSOApplication*|Target application created in Secure store service.|
|CustomFieldOrLookupTableCreated|User created a custom frield or lookup table/item in Project Web App.|
|CustomFieldOrLookupTableDeleted|User deleted a custom field or lookup table/item in Project Web App.|
|CustomFieldOrLookupTableModified|User modified a custom field or lookup table/item in Project Web App.|
|CustomizeExemptUsers*|Global administrator customized the list of exempt user agents in SharePoint admin center. You can specify which user agents to exempt from receiving an entire Web page to index. This means when a user agent you've specified as exempt encounters an InfoPath form, the form will be returned as an XML file instead of an entire Web page. This makes indexing InfoPath forms faster.|
|DefaultLanguageChangedInTermStore*|Language setting changed in the terminology store.|
|DelegateModified|User created or modified a security delegate in Project Web App.|
|DelegateRemoved|User deleted a security delegate in Project Web App.|
|DeleteSSOApplication*|An SSO application was deleted.|
|eDiscoveryHoldApplied*|An In-Place Hold was placed on a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoveryHoldRemoved*|An In-Place Hold was removed from a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoverySearchPerformed*|An eDiscovery search was performed using an eDiscovery site collection in SharePoint.|
|EngagementAccepted|User accepts a resource engagement in Project Web App.|
|EngagementModified|User modifies a resource engagement in Project Web App.|
|EngagementRejected|User rejects a resource engagement in Project Web App.|
|EnterpriseCalendarModified|User copies, modifies or delete an enterprise calendar in Project Web App.|
|EntityDeleted|User deletes a timesheet in Project Web App.|
|EntityForceCheckedIn|User forces a checkin on a calendar, custom field or lookup table in Project Web App.|
|ExemptUserAgentSet*|Global administrator adds a user agent to the list of exempt user agents in the SharePoint admin center.|
|FileAccessed|User or system account accesses a file on a SharePoint or OneDrive for Business site. System accounts can also generate FileAccessed events.|
|FileCheckOutDiscarded*|User discards (or undos) a checked out file. That means any changes they made to the file when it was checked out are discarded, and not saved to the version of the document in the document library.|
|FileCheckedIn*|User checks in a document that they checked out from a SharePoint or OneDrive for Business document library.|
|FileCheckedOut*|User checks out a document located in a SharePoint or OneDrive for Business document library. Users can check out and make changes to documents that have been shared with them.|
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
|FileSyncUploadedFull*|User establishes a sync relationship and successfully uploads files for the first time from their computer to a SharePoint or OneDrive for Business document library.|
|FileSyncUploadedPartial*|User successfully uploads changes to files on a SharePoint or OneDrive for Business document library. This event indicates that any changes made to the local version of a file from a document library are successfully uploaded to the document library. Only changes are unloaded because those files were previously uploaded by the user (as indicated by the FileSyncUploadedFull event).|
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
|GroupAdded*|Site administrator or owner creates a group for a SharePoint or OneDrive for Business site, or performs a task that results in a group being created. For example, the first time a user creates a link to share a file, a system group is added to the user's OneDrive for Business site. This event can also be a result of a user creating a link with edit permissions to a shared file.|
|GroupRemoved*|User deletes a group from a SharePoint or OneDrive for Business site. |
|GroupUpdated*|Site administrator or owner changes the settings of a group for a SharePoint or OneDrive for Business site. This can include changing the group's name, who can view or edit the group membership, and how membership requests are handled.|
|LanguageAddedToTermStore*|Language added to the terminology store.|
|LanguageRemovedFromTermStore*|Language removed from the terminology store.|
|LegacyWorkflowEnabledSet*|Site administrator or owner adds theSharePoint Workflow Task content type to the site. Global administrators can also enable work flows for the entire organization in theSharePoint admin center.|
|LookAndFeelModified|User modifies a quick launch, gantt chart formats, or group formats.  Or the user creats, modifies, or deletes a view in Project Web App.|
|ManagedSyncClientAllowed|User successfully establishes a sync relationship with a SharePoint or OneDrive for Business site. The sync relationship is successful because the user's computer is a member of a domain that's been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. For more information about this feature, see [Use Windows PowerShell cmdlets to enable OneDrive sync for domains that are on the safe recipients list](http://go.microsoft.com/fwlink/p/?LinkID=534609).|
|MaxQuotaModified*|The maximum quota for a site has been modified.|
|MaxResourceUsageModified*|The maximum allowable resource usage for a site has been modified.|
|MySitePublicEnabledSet*|The flag enabling users to have public MySites has been set by the SharePoint administrator.|
|NewsFeedEnabledSet*|Site administrator or owner enables RSS feeds for a SharePoint or OneDrive for Business site. Global administrators can enable RSS feeds for the entire organization in the SharePoint admin center.|
|ODBNextUXSettings*|New UI for OneDrive for Business has been enabled.|
|OfficeOnDemandSet*|Site administrator enables Office on Demand, which lets users access the latest version of Office desktop applications. Office on Demand is enabled in the SharePoint admin center and requires an Office 365 subscription that includes full, installed Office applications.|
|PageViewed|User views a page on a SharePoint site or OneDrive for Business site. This does not include viewing document library files from a SharePoint site or One Drive for Business site on a browser.|
|PeopleResultsScopeSet*|Site administrator creates or changes the result source for People Searches for a SharePoint site.|
|PermissionSyncSettingModified|User modifies the project permission sync settings in Project Web App.|
|PermissionTemplateModified|User creates, modifies or deletes a permissions template in Project Web App.|
|PortfolioDataAccessed|User accesses portfolio content (driver library, driver priortization, portfolio analyses) in Project Web App.|
|PortfolioDataModified|User creates, modifies, or deletes portfolio data (driver library, driver priortization, portfolio analyses) in Project Web App.|
|PreviewModeEnabledSet*|Site administrator enables document preview for a SharePoint site.|
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
|QuotaWarningEnabledModified*|Storage quota warning modified.|
|RenderingEnabled*|Browser-enabled form templates will be rendered by InfoPath forms services.|
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
|ResourceWarningEnabledModified*|Resource quota warning modified.|
|SSOGroupCredentialsSet*|Group credentials set in Secure store service.|
|SSOUserCredentialsSet*|User credentials set in Secure store service.|
|SearchCenterUrlSet*|Search center URL set.|
|SecondaryMySiteOwnerSet*|A user has added a secondary owner to their MySite.|
|SecurityCategoryModified|User creates, modifies or deletes a security category in Project Web App.|
|SecurityGroupModified|User creates, modifies or deletes a security group in Project Web App.|
|SendToConnectionAdded*|Global administrator creates a new Send To connection on the Records management page in the SharePoint admin center. A Send To connection specifies settings for a document repository or a records center. When you create a Send To connection, a Content Organizer can submit documents to the specified location.|
|SendToConnectionRemoved*|Global administrator deletes a Send To connection on the Records management page in the SharePoint admin center.|
|SharedLinkCreated|User creates a link to a shared file in SharePoint or OneDrive for Business. This link can be sent to other people to give them access to the file. A user can create two types of links: a link that allows a user to view and edit the shared file, or a link that allows the user to just view the file.|
|SharedLinkDisabled*|User disables (permanently) a link that was created to share a file.|
|SharingInvitationAccepted*|User accepts an invitation to share a file or folder. This event is logged when a user shares a file with other users.|
|SharingRevoked*|User unshares a file or folder that was previously shared with other users. This event is logged when a user stops sharing a file with other users.|
|SharingSet|User shares a file or folder located in SharePoint or OneDrive for Business with another user inside their organization.|
|SiteAdminChangeRequest*|User requests to be added as a site collection administrator for a SharePoint site collection. Site collection administrators have full control permissions for the site collection and all subsites.|
|SiteCollectionAdminAdded*|Site collection administrator or owner adds a person as a site collection administrator for a SharePoint or OneDrive for Business site. Site collection administrators have full control permissions for the site collection and all subsites.|
|SiteCollectionCreated*| Global administrator creates a new site collection in your SharePoint organization.|
|SiteRenamed*|Site administrator or owner renames a SharePoint or OneDrive for Business site|
|StatusReportModified|User creates, modifies or deletes a status report in Project Web App.|
|SyncGetChanges*|User clicks **Sync** in the action tray on in SharePoint or OneDrive for Business to synchronize any changes to file in a document library to their computer.|
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
|UnmanagedSyncClientBlocked|User tries to establish a sync relationship with a SharePoint or OneDrive for Business site from a computer that isn't a member of your organization's domain or is a member of a domain that hasn't been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. The sync relationship is not allowed, and the user's computer is blocked from syncing, downloading, or uploading files on a document library. For information about this feature, see [Use Windows PowerShell cmdlets to enable OneDrive sync for domains that are on the safe recipients list](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/index?view=sharepoint-ps).|
|UpdateSSOApplication*|Target application updated in Secure store service.|
|UserAddedToGroup*|Site administrator or owner adds a person to a group on a SharePoint or OneDrive for Business site. Adding a person to a group grants the user the permissions that were assigned to the group. |
|UserRemovedFromGroup*|Site administrator or owner removes a person from a group on a SharePoint or OneDrive for Business site. After the person is removed, they no longer are granted the permissions that were assigned to the group. |
|WorkflowModified|User creates, modifies, or deletes an Enterprise Project Type or Workflow phases or stages in Project Web App.|


> [!NOTE] 
> *This operation is in Preview.



## SharePoint file operations

The file-related SharePoint events listed in the "File and folder activities" section in [Search the audit log in the Office 365 Protection Center](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US) use this schema.



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



## SharePoint Sharing schema

 The file share-related SharePoint events. They are different from file- and folder-related events in that a user is taking an action that has some effect on another user. For information about the SharePoint Sharing schema, see [Use sharing auditing in the Office 365 audit log](https://support.office.com/en-us/article/Use-sharing-auditing-in-the-Office-365-audit-log-50bbf89f-7870-4c2a-ae14-42635e0cfc01?ui=en-US&amp;rs=en-US&amp;ad=US).



|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|TargetUserOrGroupName |Edm.String|No|Stores the UPN or name of the target user or group that a resource was shared with.|
|TargetUserOrGroupType|Edm.String|No|Identifies whether the target user or group is a Member, Guest, Group, or Partner. |
|EventData|XML code|No|Conveys follow-up information about the sharing action that has occurred, such as adding a user to a group or granting edit permissions.|


## SharePoint schema

The SharePoint events listed in [Search the audit log in the Office 365 Protection Center](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US) (excluding the file and folder events) use this schema.



|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|CustomEvent|Edm.String|No|Optional string for custom events.|
|EventData|Edm.String|No|Optional payload for custom events.|
|ModifiedProperties|Collection(ModifiedProperty)|No|The property is included for admin events, such as adding a user as a member of a site or a site collection admin group. The property includes the name of the property that was modified (for example, the Site Admin group), the new value of the modified property (such the user who was added as a site admin), and the previous value of the modified object.|


## Project schema

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Entity|Edm.String|Yes| [ProjectEntity](#project-entity) the audit was for.|
|Action|Edm.String|Yes|[ProjectAction](#project-action) that was taken.|
|OnBehalfOfResId|Edm.Guid|No|The resource Id the action was taken on behalf of.|

<a name="project-action"></a>

### Enum: Project Action - Type: Edm.Int32


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

<a name="project-entity"></a>
### Enum: Project Entity - Type: Edm.Int32

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
|TimesheetAuditLog|Represents a timesheetsheet audit log.|
|TimesheetManager|Represents the manager of a timehseet.|
|UserDelegate|Represents a user delegation for another user.|
|View|Represents a view definition.|
|WorkflowPhase|Represents a phase in a workflow.|
|WorkflowStage|Represents a stage in a workflow.|

## Exchange Admin schema



|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|ModifiedObjectResolvedName|Edm.String|No|This is the user friendly name of the object that was modified by the cmdlet. This is logged only if the cmdlet modifies the object.|
|Parameters|Collection(Common.NameValuePair)|No|The name and value for all parameters that were used with the cmdlet that is identified in the Operations property.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|No|The property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified object.|
|ExternalAccess|Edm.Boolean|Yes|Specifies whether the cmdlet was run by a user in your organization, by Microsoft datacenter personnel or a datacenter service account, or by a delegated administrator. The value **False** indicates that the cmdlet was run by someone in your organization. The value **True** indicates that the cmdlet was run by datacenter personnel, a datacenter service account, or a delegated administrator.|
|OriginatingServer|Edm.String|No|The name of the server from which the cmdlet was executed.|
|OrganizationName|Edm.String|No|The name of the tenant.|


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



### ExchangeMailboxAuditRecord schema



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Item|Self.[ExchangeItem](#exchangeitem-complex-type)|No|Represents the item upon which the operation was performed|
|ModifiedProperties|Collection(Edm.String)|No|TBD|
|SendAsUserSmtp|Edm.String|No|SMTP address of the user who is being impersonated.|
|SendAsUserMailboxGuid|Edm.Guid|No|The Exchange GUID of the mailbox that was accessed to send email as.|
|SendOnBehalfOfUserSmtp|Edm.String|No|SMTP address of the user on whose behalf the email is sent.|
|SendonBehalfOfUserMailboxGuid|Edm.Guid|No|The Exchange GUID of the mailbox that was accessed to send mail on behalf of.|


### ExchangeItem complex type


|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Yes|The store ID.|
|Subject|Edm.String|No|The subject line of the message that was accessed.|
|ParentFolder|Edm.ExchangeFolder|No|The name of the folder where the item is located.|
|Attachments|Edm.String|No|A list of the names and file size of all items that are attached to the message.|

### ExchangeFolder complex type



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Yes|The store ID of the folder object.|
|Path|Edm.String|No|The name of the mailbox folder where the message that was accessed is located.|



## Azure Active Directory Base schema



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AzureActiveDirectoryEventType|Self.[AzureActiveDirectoryEventType](#azureactivedirectoryeventtype)|Yes|The type of Azure AD event. |
|ExtendedProperties|Collection(Common.NameValuePair)|No|The extended properties of the Azure AD event.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|No|This property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property.|

### Enum: AzureActiveDirectoryEventType - Type -Edm.Int32

#### AzureActiveDirectoryEventType

|**Member name**|**Description**|
|:-----|:-----|
|AccountLogon|The account login event.|
|AzureApplicationAuditEvent|The Azure application security event.|

## Azure Active Directory Account Logon schema



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Application|Edm.String|No|The application that triggers the account login event, such as Office 15.|
|Client|Edm.String|No|Details about the client device, device OS, and device browser that was used for the of the account login event.|
|LoginStatus|Edm.Int32|Yes|This property is from OrgIdLogon.LoginStatus directly. The mapping of various interesting logon failures could be done by alerting algorithms.|
|UserDomain|Edm.String|Yes|The Tenant Identity Information (TII).|



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

### Enum: LoginType - Type: Edm.Int32



|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|-1|Other|Other i type.|
|1|InitialAuth|Login with initial authentication|
|2|CookieCopy|Login with cookie.|
|3|SilentReAuth|Login with silent re-authentication.|


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



### Complex Type IdentityTypeValuePair



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ID|Edm.String|Yes|The value of the identity given the type.|
|Type|Self.IdentityType|Yes|The type of the identity.|

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


## Azure Active Directory STS Logon schema



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|No|The GUID that represents the application that is requesting the login. The display name can be looked up via the Azure Active Directory Graph API.|
|Client|Edm.String|No|Client device information, provided by the browser performing the login.|
|LogonError|Edm.String|No|For failed logins, contains the reason why the login failed.|

## DLP schema

DLP events are available for Exchange Online, SharePoint Online, and OneDrive For Business. Note that DLP events in Exchange are only available for events based on unified DLP policy (e.g. configured via Security & Compliance Center). DLP events based on Exchange Transport Rules are not supported.

DLP (Data Loss Prevention) events will always have UserKey="DlpAgent" in the common schema. There are three types of DlpEvents which which are stored as the value of the Operation property of the common schema:

- DlpRuleMatch - This indicates a rule was matched. These events exist in both Exchange and SharePoint Online and OneDrive for Business. For Exchange it includes false positive and override information. For SharePoint Online and OneDrive for Business, false positive and overrides generate separate events.

- DlpRuleUndo - These only exist in SharePoint Online and OneDrive for Business, and indicate a previously applied policy action has been “undone” – either because of false positive/override designation by user, or because the document is no longer subject to policy (either due to policy change or change to content in doc).

- DlpInfo - These only exist in SharePoint Online and OneDrive for Business and indicate a false positive designation but no action was “undone.”



|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.[SharePointMetadata](#sharepointmetadata-complex-type)|No|Describes metadata about the document in SharePoint or OneDrive for Business that contained the sensitive information.|
|ExchangeMetaData|Self.[ExchangeMetadata](#exchangemetadata-complex-type)|No|Describes metadata about the email message that contained the sensitive information.|
|ExceptionInfo|Edm.String|No|Identifies reasons why a policy no longer applies and/or any information about false positive and/or override noted by the end user.|
|PolicyDetails|Collection(Self.[PolicyDetails](#policydetails-complex-type))|Yes|Information about 1 or more policies that triggered the DLP event.|
|SensitiveInfoDetectionIsIncluded|Boolean|Yes|Indicates whether the event contains the value of the sensitive data type and surrounding context from the source content. Accessing sensitive data requires the "Read DLP policy events including sensitive details" permission in Azure Active Directory.|




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
|DocumentSharer|Edm.String|Yes|The user who last modiifed sharing of the document.|
|UniqueId|Edm.String|Yes|A guid that identifies the file.|
|LastModifiedTime|Edm.DateTime|Yes|Timestamp in UTC for when doc was last modified.|


### ExchangeMetadata complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|MessageID|Edm.String|Yes|The message ID of the email that triggered the event.|
|From|Edm.String|Yes|The user who sent the email.|
|To|Collection(Edm.String)|No|A collection of email addresses that were on the To line of the message.|
|CC|Collection(Edm.String)|No|A collection of email addresses that were on the CC line of the message.|
|BCC|Collection(Edm.String)|No|A collection of email addresses that were on the BCC line of the message.|
|Subject|Edm.String|Yes|Suject of the email message.|
|Sent|Edm.DateTime|Yes|The time in UTC of when the email was sent.|
|RecipientCount|Edm.Int32|Yes|The total number of all recipients on the TO, CC, and BCC lines of the message.|


### PolicyDetails complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|PolicyId|Edm.Guid|Yes|The guid of the DLP policy for this event.|
|PolicyName|Edm.String|Yes|The friendly name of the DLP policy for this event.|
|Rules|Collection(Self.[Rules](#rules-complex-type))|Yes|Information about the rules within the policy that were matched for this event.|

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

### ConditionsMatched complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.[SensitiveInformation](#sensitiveinformation-complex-type))|No|Information about the type of sensitive information detected.|
|DocumentProperties|Collection(NameValuePair)|No|Information about document properties that triggered a rule match.|
|OtherConditions|Collection(NameValuePair)|No|A list of key value pairs describing any other conditions that were matched.|

### SensitiveInformation complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.Int|Yes|The confidence of pattern that matched the detection.|
|Count|Edm.Int|Yes|The number of sensitive instances detected.|
|SensitiveType|Edm.Guid|Yes|A guid that indentifies the type of sensitive data detected.|
|SensitiveInformationDetections|Self.SensitiveInformationDetections|No|An array of objects that contain sensitive information data with the following details – matched value and context of matched value.|

### SensitiveInformationDetections complex type 
DLP sensitive data is only available in the activity feed API to users that have been granted “Read DLP sensitive data” permissions. 

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Detections|Collection(Self.Detections)|Yes|An array of sensitive information that was detected. Information contains key value pairs with Value = matched value (eg. Value of credit card of SSN) and Context = an excerpt from source content that contains the matched value. |
|ResultsTruncated|Edm.Boolean|Yes|Indicates if the logs were truncated due to large number of results. |

### ExceptionInfo complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Reason|Edm.String|No|For a DLPRuleUndo event, this indicates why the rule no longer applies, which can be one of 3 reasons: Override, Document Change, or Policy Change|
|FalsePositive|Edm.Boolean|No|Indicates whether the user designated this event as a false positive.|
|Justification|Edm.String|No|If the user chose to override policy, any user-specified justification is captured here.|
|Rules|Collection(Edm.Guid)|No|A collection of guids for each rule that was designated as a false positive or override, or for which an action was undone.|

## Security and Compliance Center schema

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|No|The date and time at which the cmdlet was executed.|
|ClientRequestId|Edm.String|No|A GUID that can be used to correlate this cmdlet with the Security & Compliance Center UX operations. This information is only used by Microsoft support.|
|CmdletVersion|Edm.String|No|The build version of the cmdlet when it was executed.|
|EffectiveOrganization|Edm.String|No|The GUID for the organization impacted by the cmdlet. (Deprecated: This parameter will stop appearing in the future.)|
|UserServicePlan|Edm.String|No|The Exchange Online Protection service plan assigned to the user that executed the cmdlet.|
|ClientApplication|Edm.String|No|If the cmdlet was executed by an application, as opposed to remote powershell, this field contains that application’s name.|
|Parameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that do not include Personally Identifiable Information.|
|NonPiiParameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that include Personally Identifiable Information. (Deprecated: This field will stop appearing in the future and its content merged with the Parameters field.)|

## Security and Compliance Alerts schema

Alert signals include:

- All alerts generated based on [Alert policies in Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/alert-policies#default-alert-policies).
- Office 365 related alerts generated in [Office 365 Cloud App Security](https://docs.microsoft.com/office365/securitycompliance/office-365-cas-overview) and [Microsoft Cloud App Security](https://docs.microsoft.com/en-us/cloud-app-security/what-is-cloud-app-security).

The UserId and UserKey of these events are always SecurityComplianceAlerts. There are two types of alert signals which are stored as the value of the Operation property of the common schema:

- AlertTriggered - A new alert is generated due to a policy match.

- AlertEntityGenerated - A new entity is added to an alert. This event is only applicable to alerts generated based on Alert policies in the Office 365 Security & Compliance Center. Each generated alert can be associated with one or multiple of these events. For example, an alert policy is defined to trigger an alert if any user deletes more than 100 files in 5 minutes. If two users exceed the threshold around the same time, there will be two AlertEntityGenerated events, but only one AlertTriggered event.

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|AlertId|Edm.Guid|Yes|The Guid of the alert.|
|AlertType|Self.String|Yes|Type of the alert. Alert types include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>System</p></li><li><p>Custom</p></li>|
|Name|Edm.String|Yes|Name of the alert.|
|PolicyId|Edm.Guid|No|The Guid of the policy that triggered the alert.|
|Status|Edm.String|No|Status of the alert. Statuses include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Active</p></li><li><p>Investigating</p></li><li><p>Resolved</p></li><li><p>Dismissed</p></li></ul>|
|Severity|Edm.String|No|Severity of the alert. Severity levels include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Low</p></li><li><p>Medium</p></li><li><p>High</p></li></ul>|
|Category|Edm.String|No|Category of the alert. Categories include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>DataLossPrevention</p></li><li><p>ThreatManagement</p></li><li><p>DataGovernance</p></li><li><p>AccessGovernance</p></li><li><p>MailFlow</p></li><li><p>Other</p></li></ul>|
|Source|Edm.String|No|Source of the alert. Sources include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Office 365 Security & Compliance</p></li><li><p>Cloud App Security</p></li></ul>|
|Comments|Edm.String|No|Comments left by the users who have viewed the alert. By default, it's "New alert".|
|Data|Edm.String|No|The detailed data blob of the alert or alert entity.|
|AlertEntityId|Edm.String|No|The identitifier for the alert entity. This parameter is only applicable to AlertEntityGenerated events.|
|EntityType|Edm.String|No|Type of the alert or alert entity. Entity types include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>User</p></li><li><p>Recipients</p></li><li><p>Sender</p></li><li><p>MalwareFamily</p></li></ul>This parameter is only applicable to AlertEntityGenerated events.|

## Yammer schema

The Yammer events listed in [Search the audit log in the Office 365 Protection Center](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US) will use this schema.

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|ActorUserId|Edm.String|No|Email of user that performed the operation.|
|ActorYammerUserId|Edm.Int32|No|ID of user that performed the operation.|
|DataExportType|Edm.String|No|Returns "data" if data export includes messages, notes, files, topics, users and groups; returns "user" if data export includes users only.|
|FileId|Edm.Int32|No|ID of the file in the operation. |
|FileName|Edm.String|No|Name of the file in the operation. Will appear blank if not relevant to the operation.|
|GroupName|Edm.String|No|Name of the group in the operation. Will appear blank if not relevant to the operation.|
|IsSoftDelete|Edm.Boolean|No|Returns "true" if the network's data reteion policy is set to Soft Delete; returns "false" if the network's data retention policy is set to Hard Delete.|
|MessageId|Edm.Int32|No|ID of the message in the operation.|
|YammerNetworkId|Edm.Int32|No|Network ID of the user that performed the operation.|
|TargetUserId|Edm.String|No|Email of target user in the operation. Will appear blank if not relevant to the operation.|
|TargetYammerUserId|Edm.Int32|No|ID of target user in the operation.|
|VersionId|Edm.Int32|No|Version ID of the file in the operation.|

## Sway schema

The Sway events listed in [Search the audit log in the Office 365 Protection Center](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US) (excluding the file and folder events) will use this schema.

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ObjectType|Self.[ObjectType](#objecttype)|No|The access point for the triggered event. Access points include: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>A Sway</p></li><li><p>A Sway that is embedded within a host</p></li><li><p>Sway settings within the Office 365 admin portal</p></li></ul>|
|Endpoint|Self.[Endpoint](#endpoint)|No|The Sway client endpoint for the triggered event. The Sway client endpoint can be web, iOS, Windows, or Android. |
|BrowserName|Edm.String|No|The browser used to access Sway for the triggered event. |
|DeviceType|Self.[DeviceType](#devicetype)|No|The device type used to access Sway for the triggered event. The device type can be desktop, mobile, or tablet.|
|SwayLookupId|Edm.String|No|The Sway ID. |
|SiteUrl|Edm.String|No|The URL for the Sway.|
|OperationResult|Self.[OperationResult](#operationresult)|No|Either success or fail.|


### Enum: ObjectType - Type Edm.Int32

#### ObjectType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Sway|The event was triggered from a Sway.|
|1|SwayEmbedded|The event was triggered from a Sway, which is embedded in a host.|
|2|SwayAdminPortal|The event was triggered from Sway service settings in Office 365 admin portal.|


### Enum: OperationResult - Type Edm.Int32

#### OperationResult

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Succeeded|The event was successful.|
|1|Failed|The event failed.|


### Enum: Endpoint - Type Edm.Int32

#### Endpoint

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|SwayWeb|The event was triggered using the Web client of Sway.|
|1|SwayIOS|The event was triggered using the iOS client of Sway.|
|2|SwayWindows|The event was triggered using the Windows client of Sway.|
|3|SwayAndroid|The event was triggered using the Android client of Sway.|



### Enum: DeviceType - Type Edm.Int32

#### DeviceType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Desktop|The event was triggered using the desktop.|
|1|Mobile|The event was triggered using a mobile device.|
|2|Tablet|The event was triggered using a tablet device.|



### Enum: SwayAuditOperation - Type Edm.Int32

#### SwayAuditOperation

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Create|The user creates a Sway.|
|2|Delete|The user deletes a Sway.|
|3|View|The user views a Sway.|
|4|Edit|The user edits a Sway.|
|5|Duplicate|The user duplicates a Sway.|
|7|Share|The user initiates sharing a Sway. This event captures the user action of clicking on a specific share destination within the Sway share menu. The event does not indicate whether the user actually follows through and completes the share action.|
|8|ChangeShareLevel|The user changes the share level of a Sway. This event captures the user changing the scope of sharing associated with a Sway. For example, public as compared to from within the organization.|
|9|RevokeShare|The user stops sharing a Sway by revoking access. Revoking access changes the links associated with a Sway.|
|10|EnableDuplication|The user enables duplication of a Sway (on by default).|
|11|DisableDuplication|The user disables duplication of a Sway (off by default).|
|12|ServiceOn|The user enables Sway for the entire organization via the Office 365 admin center (on by default).|
|13|ServiceOff|The user disables Sway for the entire organization via the Office 365 admin center (off by default).|
|14|ExternalSharingOn|The user enables external sharing for the entire organization via the Office 365 admin center.|
|15|ExternalSharingOff|The user disables external sharing for the entire organization via the Office 365 admin center.|

## Data Center Security Base schema



|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.[DataCenterSecurityEventType](#datacentersecurityeventtype)|Yes|The type of dmdlet event in lock box.|

### Enum: DataCenterSecurityEventType - Type: Edm.Int32

#### DataCenterSecurityEventType


|**Member name**|**Description**|
|:-----|:-----|
|DataCenterSecurityCmdletAuditEvent|This is the enum value for cmdlet audit type event.|



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


## Microsoft Teams schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|MessageId|Edm.String|No|An identifier for a chat or channel message.|
|MeetupId|Edm.String|No|An identifier for a scheduled or ad-hoc meeting.|
|Members|Collection(Self.[MicrosoftTeamsMember](#MicrosoftTeamsMember-complex-type))|No|A list of users within a Team.|
|TeamName|Edm.String|No|The name of the team being audited.|
|TeamGuid|Edm.Guid|No|A unique identifier for the team being audited.|
|ChannelName|Edm.String|No|The name of the channel being audited.|
|ChannelGuid|Edm.Guid|No|A unique identifier for the channel being audited.|

### MicrosoftTeamsMember complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|UPN|Edm.String|No|The user principal name of the user.|
|Role|Self.[MemberRoleType](#memberroletype)|No|The role of the user within the team.|
|DisplayName|Edm.String|No|The display name of the user.|

### Enum: MemberRoleType - Type: Edm.Int32

#### MemberRoleType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Member|A user who is a member of the team.|
|1|Owner|A user who is the owner of the team.|
|2|Guest|A user who is not a member of the team.|


## Microsoft Teams Add-ons schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AddOnType|Self.[AddOnType](#addontype)|No|The type of add-on that generated this event.|
|AddonName|Edm.String|No|The name of the add-on that generated this event.|
|AddOnGuid|Edm.Guid|No|A unique identifier for the add-on that generated this event.|
|TabType|Edm.String|No|The type of tab that generated this event.|

### Enum: AddOnType - Type: Edm.Int32

#### AddOnType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Bot|A Microsoft Teams bot.|
|2|Connector|A Microsoft Teams connector.|
|3|Tab|A Microsoft Teams tab.|


## Microsoft Teams Settings schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ModifiedProperty|Common.ModifiedProperty|No|The property that was modified. It will contain the **Name**, **OldValue**, and **NewValue** of the property.|
|ExtendedProperties|Collection(Common.NameValuePair)|No|A list of extended properties for the setting being changed. Each property will have a **Name** and **Value**.|

## Office 365 Advanced Threat Protection and Threat Intelligence schema

Office 365 Advanced Threat Protection (ATP) and Threat Intelligence events are available for Office 365 customers who have an ATP,  Threat Intelligence, or E5 subscription. Each event in the ATP and Threat Intelligence feed corresponds to the following that were determined to contain a threat:

- An email message sent by or received by a user in the organization with detections are made on messages at delivery time and from [Zero hour auto purge](https://support.office.com/en-us/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15). 

- URLs clicked by a user in the organization that were detected as malicious at time-of-click based on [Office 365 ATP Safe Links](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links) protection.  

### Email message events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AttachmentData|Collection(Self.[AttachmentData](#AttachmentData))|No|Data about attachments in the email message that triggered the event.|
|DetectionType|Self.[DetectionType](#DetectionType)|Yes|The type of detection.|
|DetectionMethod|Edm.String|Yes|The method or technology used by Office 365 ATP for the detection.|
|InternetMessageId|Edm.String|Yes|The Internet Message Id.|
|NetworkMessageId|Edm.String|Yes|The Exchange Online Network Message Id.|
|P1Sender|Edm.String|Yes|The return path of sender of the email message.|
|P2Sender|Edm.String|Yes|The from sender of the email message.|
|Recipients|Collection(Edm.String)|Yes|An array of recipients of the email message.|
|SenderIp|Edm.String|Yes|The IP address that submitted the email of Office 365. The IP address is displayed in either an IPv4 or IPv6 address format.|
|Subject|Edm.String|Yes|The subject line of the message.|
|Verdict|Edm.String|Yes|The message verdict.|

### Enum: DetectionType - Type: Edm.Int32

#### DetectionType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Inline|Threat detected at delivery time.|
|1|Delayed|Threat detected after delivery.|
|2|ZAP|Messages removed by [Zero hour auto purge](https://support.office.com/en-us/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15). Events with this detection type will typically be preceded by a message with a “Delayed” detection type.|


### AttachmentData complex type

#### AttachmentData

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|Yes|The file name of the attachment.|
|FileType|Edm.String|Yes|The file type of the attachment.|
|FileVerdict|Self.[FileVerdict](#FileVerdict)|Yes|The file malware verdict.|
|MalwareFamily|Edm.String|No|The file malware family.|
|SHA256|Edm.String|Yes|The file SHA256 hash.|

### Enum: FileVerdict - Type: Edm.Int32

#### FileVerdict

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Good|No threats detected.|
|1|Bad|Malware found in attachment.|
|-1|Error|Scan / analysis error.|
|-2|Timeout|Scan / analysis timeout.|
|-3|Pending|Scan / analysis not complete.|

### URL time-of-click events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|UserId|Edm.String|Yes|Identifier (e.g. email address) for the user who clicked on the URL.|
|AppName|Edm.String|Yes|Office 365 service from which the URL was clicked (e.g. Mail).|
|Blocked|Edm.Boolean|Yes|This is true if the URL click is blocked by [Office 365 ATP Safe Links](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links) protection.|
|ClickedThrough|Edm.Boolean|Yes|This is true if the URL block is clicked through (overridden) by the user based on the organization's policies for [Office 365 ATP Safe Links](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links) protection.|
|SourceId|Edm.String|Yes|Identifier for the Office 365 service from which the URL was clicked (e.g. for Mail, this is the Exchange Online Network Message Id).|
|TimeOfClick|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the user clicked the URL.|
|URL|Edm.String|Yes|URL clicked by the user.|
|UserIp|Edm.String|Yes|The IP address for the user who clicked the URL. The IP address is displayed in either an IPv4 or IPv6 address format.|







