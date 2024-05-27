---
ms.TocTitle: Office 365 Management Activity API schema
title: Office 365 Management Activity API schema
description: The Office 365 Management Activity API schema is provided as a data service in two layers - Common schema and service-specific schema.
ms.ContentId: 1c2bf08c-4f3b-26c0-e1b2-90b190f641f5
ms.topic: reference
ms.date: 03/20/2024
ms.localizationpriority: high
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
|[Copilot schema](copilot-schema.md)|Events include how and when to interact with Copilot, in which Microsoft 365 service the activity took place, and references to the files stored in Microsoft 365 that were accessed during the interaction.|
|[SharePoint Base schema](#sharepoint-base-schema)|Extends the Common schema with the properties specific to all SharePoint audit data.|
|[SharePoint File Operations](#sharepoint-file-operations)|Extends the SharePoint Base schema with the properties specific to file access and manipulation in SharePoint.|
|[SharePoint List Operations](#sharepoint-list-operations)|Extends the SharePoint Base schema with the properties specific to interactions with lists and list items in SharePoint Online.|
|[SharePoint Sharing schema](#sharepoint-sharing-schema)|Extends the SharePoint Base schema with the properties specific to file sharing.|
|[SharePoint schema](#sharepoint-schema)|Extends the SharePoint Base schema with the properties specific to SharePoint, but unrelated to file access and manipulation.|
|[Project schema](#project-schema)|Extends the SharePoint Base schema with the properties specific to Project.|
|[Exchange Admin schema](#exchange-admin-schema)|Extends the Common schema with the properties specific to all Exchange admin audit data.|
|[Exchange Mailbox schema](#exchange-mailbox-schema)|Extends the Common schema with the properties specific to all Exchange mailbox audit data.|
|[Microsoft Entra ID Base schema](#azure-active-directory-base-schema)|Extends the Common schema with the properties specific to all Microsoft Entra audit data.|
|[Microsoft Entra account Logon schema](#azure-active-directory-account-logon-schema)|Extends the Microsoft Entra ID Base schema with the properties specific to all Microsoft Entra logon events.|
|[Microsoft Entra ID Secure STS Logon schema](#azure-active-directory-secure-token-service-sts-logon-schema)|Extends the Microsoft Entra ID Base schema with the properties specific to all Microsoft Entra ID Secure Token Service (STS) logon events.|
|[Microsoft Entra schema](#azure-active-directory-schema)|Extends the Common schema with the properties specific to all Microsoft Entra audit data.|
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
|[Encrypted message portal event schema](#encrypted-message-portal-events-schema)|Extends the Common schema with the properties specific to encrypted message portal accessed by external recipients.|
|[Communication compliance Exchange schema](#communication-compliance-exchange-schema)|Extends the Common schema with the properties specific to the Communication compliance offensive language model.|
|[Reports schema](#reports-schema)|Extends the Common schema with the properties specific to all reports events.|
|[Compliance connector schema](#compliance-connector-schema)|Extends the Common schema with the properties specific to importing non-Microsoft data by using data connectors.|
|[SystemSync schema](#systemsync-schema)|Extends the Common schema with the properties specific to data ingested via SystemSync.|
|[Viva Goals schema](#viva-goals-schema)|Extends the Common schema with the properties specific to all Viva Goals events.|
|[Microsoft Planner schema](#microsoft-planner-schema)|Extends the Common schema with the properties specific to Microsoft Planner events.|
|[Microsoft Project for the web schema](#microsoft-project-for-the-web-schema)|Extends the Common schema with the properties specific to Microsoft Project For The web events.|

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
|UserKey|Edm.String|Yes|An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange. |
|Workload|Edm.String|No|The Office 365 service where the activity occurred. 
|ResultStatus|Edm.String|No|Indicates whether the action (specified in the Operation property) was successful or not. Possible values are **Succeeded**, **PartiallySucceeded**, or **Failed**. For Exchange admin activity, the value is either **True** or **False**.<br/><br/>**Important**: Different workloads may overwrite the value of the ResultStatus property. For example, for Microsoft Entra ID STS logon events, a value of **Succeeded** for ResultStatus indicates only that the HTTP operation was successful; it doesn't mean the logon was successful. To determine if the actual logon was successful or not, see the LogonError property in the [Microsoft Entra ID STS Logon schema](#azure-active-directory-secure-token-service-sts-logon-schema). If the logon failed, the value of this property will contain the reason for the failed logon attempt. |
|ObjectId|Edm.string|No|For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet.|
|UserId|Edm.string|Yes|The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, `my_name@my_domain_name`. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see [The app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#the-appsharepoint-user-in-audit-records). |
|ClientIP|Edm.String|Yes|The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format.<br/><br/>For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. <br/><br/>Also, for Microsoft Entra ID-related events, the IP address isn't logged and the value for the ClientIP property is `null`.|
|Scope|Self.[AuditLogScope](#auditlogscope)|No|Was this event created by a hosted O365 service or an on-premises server? Possible values are **online** and **onprem**. Note that SharePoint is the only workload currently sending events from on-premises to O365.|
|AppAccessContext|CollectionSelf.[AppAccessContext](#complex-type-appaccesscontext)|No|The application context for the user or service principal that performed the action.|

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
|8|AzureActiveDirectory|Microsoft Entra ID events.|
|9|AzureActiveDirectoryAccountLogon|Microsoft Entra ID OrgId logon events (deprecated).|
|10|DataCenterSecurityCmdlet|Data Center security cmdlet events.|
|11|ComplianceDLPSharePoint|Data loss protection (DLP) events in SharePoint and OneDrive for Business.|
|13|ComplianceDLPExchange|Data loss protection (DLP) events in Exchange, when configured via Unified DLP Policy. DLP events based on Exchange Transport Rules are not supported.|
|14|SharePointSharingOperation|SharePoint sharing events.|
|15|AzureActiveDirectoryStsLogon|Secure Token Service (STS) logon events in Microsoft Entra ID.|
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
|78|WDATPAlerts|Events related to alerts generated by Windows Defender for Endpoint.|
|82|SensitivityLabelPolicyMatch|Events generated when the file labeled with a sensitivity label is opened or renamed.|
|83|SensitivityLabelAction|Event generated when sensitivity labels are applied, updated, or removed from a file.|
|84|SensitivityLabeledFileAction|Events generated when a file labeled with a sensitivity label is opened or renamed.|
|85|AttackSim|Events related to user activities in Attack Simulation & Training in Microsoft Defender for Office 365.|
|86|AirManualInvestigation|Events related to manual investigations in Automated investigation and response (AIR). |
|87|SecurityComplianceRBAC|Security and compliance RBAC events.|
|88|UserTraining|Events related to user training in Attack Simulation & Training in Microsoft Defender for Office 365.|
|89|AirAdminActionInvestigation|Events related to admin actions in  Automated investigation and response (AIR).|
|90|MSTIC|Threat intelligence events in Microsoft Defender for Office 365.|
|91|PhysicalBadgingSignal|Events related to physical badging signals that support the Insider risk management solution.|
|[93](AipDiscover.md)|AipDiscover|AIP scanner events|
|[94](AipSensitivityLabelAction.md)|AipSensitivityLabelAction|AIP sensitivity label events|
|[95](AipProtectionAction.md)|AipProtectionAction|AIP protection events|
|[96](AipFileDeleted.md)|AipFileDeleted|AIP file deletion events|
|[97](AipHeartBeat.md)|AipHeartBeat|AIP heartbeat events|
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
|113|MS365DCustomDetection|Events related to custom detection actions in Microsoft 365 Defender.|
|147|CoreReportingSettings|Reports settings events.|
|148|ComplianceConnector|Events related to importing non-Microsoft data using data connectors in the Microsoft Purview compliance portal.|
|[154](OMEPortal.md)|OMEPortal|Encrypted message portal event logs generated by external recipients.|
|174|DataShareOperation|Events related to sharing of data ingested via SystemSync.|
|181|EduDataLakeDownloadOperation|Events related to the export of SystemSync ingested data from the lake.|
|183|MicrosoftGraphDataConnectOperation|Events related to extractions done by Microsoft Graph Data Connect.|
|186|PowerPagesSite|Activities related to Power Pages site.|
|188|PlannerPlan|Microsoft Planner plan events.|
|189|PlannerCopyPlan|Microsoft Planner copy plan events.|
|190|PlannerTask|Microsoft Planner task events.|
|191|PlannerRoster|Microsoft Planner roster and roster membership events.|
|192|PlannerPlanList|Microsoft Planner plan list events.|
|193|PlannerTaskList|Microsoft Planner task list events.|
|194|PlannerTenantSettings|Microsoft Planner tenant settings events.|
|195|ProjectForThewebProject|Microsoft Project for the web project events.|
|196|ProjectForThewebTask|Microsoft Project for the web task events.|
|197|ProjectForThewebRoadmap|Microsoft Project for the web  roadmap events.|
|198|ProjectForThewebRoadmapItem|Microsoft Project for the web  roadmap item events.|
|199|ProjectForThewebProjectSettings|Microsoft Project for the web  project tenant settings events.|
|200|ProjectForThewebRoadmapSettings|Microsoft Project for the web  roadmap tenant settings events.|
|216|Viva Goals|Viva Goals events.|
|217|MicrosoftGraphDataConnectConsent|Events for consent actions performed by tenant admins for Microsoft Graph Data Connect applications.|
|218|AttackSimAdmin|Events related to admin activities in Attack Simulation & Training in Microsoft Defender for Office 365.|
|230|TeamsUpdates|Teams Updates App Events.|
|231|PlannerRosterSensitivityLabel|Microsoft Planner roster sensitivity label events.|
|237|DefenderExpertsforXDRAdmin|Microsoft Defender Experts Administrator action events.|
|251|VfamCreatePolicy|Viva Access Management policy create events.|
|252|VfamUpdatePolicy|Viva Access Management policy update events.|
|253|VfamDeletePolicy|Viva Access Management policy delete events.|
|[261](copilot-schema.md)|CopilotInteraction|Copilot interaction events.|
|287|ProjectForThewebAssignedToMeSettings|Microsoft Project for the web assigned to me tenant settings events.|

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

### Enum: AuditLogScope - Type: Edm.Int32

#### AuditLogScope

|Value|Member name|Description|
|:-----|:-----|:-----|
|0|Online|This event was created by a hosted O365 service.|
|1|Onprem|This event was created by an on-premises server.|

### Complex Type AppAccessContext

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AADSessionId|Edm.String|No|The Microsoft Entra SessionId of the Entra sign-in that was performed by the app on behalf of the user.|
|APIId|Edm.String|No|The Id for the API pathway that is used to access the resource; for example access via the Microsoft Graph API.|
|ClientAppId|Edm.String|No|The Id of the Microsoft Entra app that performed the access on behalf of the user.|
|ClientAppName|Edm.String|No|The name of the Microsoft Entra app that performed the access on behalf of the user.|
|CorrelationId|Edm.String|No|An identifier that can be used to correlate a specific user's actions across Microsoft 365 services.|
|UniqueTokenId|Edm.String|No|UniqueTokenId gets set if the Microsoft Entra token is available for the request. It's a unique, per-token identifier that is case-sensitive.|
|IssuedAtTime|Edm.Date|No|"Issued At" gets set if the Microsoft Entra token is available for the request and it indicates when the authentication for this Microsoft Entra token occurred.|

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
|ListItemUniqueId|Edm.Guid|No|The Guid of uniquely an identifiable item of list. This information is present only if it is applicable.|
|ListId|Edm.Guid|No|The Guid of the list. This information is present only if it is applicable.|
|ApplicationId|Edm.String|No|The ID of the application performing the operation.|
|ApplicationDisplayName|Edm.String|No|The display name of the application performing the operation.|
|IsWorkflow|Edm.Boolean|No|This is set to `True` if SharePoint Workflows triggered the audited event.|

### Enum: ItemType - Type: Edm.Int32

#### ItemType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Invalid|The item is none of the other item types (that are listed in this table).|
|1|File|The item is a file.|
|5|Folder|The item is a folder.|
|6|web|The item is a web.|
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
|ConnectedSiteSettingModified|User has either created, modified or deleted the link between a project and a project site or the user modifies the synchronization setting on the link in Project web app.|
|CreateSSOApplication|Target application created in Secure store service.|
|CustomFieldOrLookupTableCreated|User created a custom field or lookup table/item in Project web app.|
|CustomFieldOrLookupTableDeleted|User deleted a custom field or lookup table/item in Project web app.|
|CustomFieldOrLookupTableModified|User modified a custom field or lookup table/item in Project web app.|
|CustomizeExemptUsers|Global administrator customized the list of exempt user agents in SharePoint admin center. You can specify which user agents to exempt from receiving an entire web page to index. This means when a user agent you've specified as exempt encounters an InfoPath form, the form will be returned as an XML file instead of an entire web page. This makes indexing InfoPath forms faster.|
|DefaultLanguageChangedInTermStore*|Language setting changed in the terminology store.|
|DelegateModified|User created or modified a security delegate in Project web app.|
|DelegateRemoved|User deleted a security delegate in Project web app.|
|DeleteSSOApplication|An SSO application was deleted.|
|eDiscoveryHoldApplied|An In-Place Hold was placed on a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoveryHoldRemoved|An In-Place Hold was removed from a content source. In-Place Holds are managed by using an eDiscovery site collection (such as the eDiscovery Center) in SharePoint.|
|eDiscoverySearchPerformed|An eDiscovery search was performed using an eDiscovery site collection in SharePoint.|
|EngagementAccepted|User accepts a resource engagement in Project web app.|
|EngagementModified|User modifies a resource engagement in Project web app.|
|EngagementRejected|User rejects a resource engagement in Project web app.|
|EnterpriseCalendarModified|User copies, modifies or delete an enterprise calendar in Project web app.|
|EntityDeleted|User deletes a timesheet in Project web app.|
|EntityForceCheckedIn|User forces a check-in on a calendar, custom field or lookup table in Project web app.|
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
|FileRecycled|User moves a document into the SharePoint or OneDrive Recycle Bin.|
|FileRenamed|User renames a document on a SharePoint or OneDrive for Business site.|
|FileRestored|User restores a document from the recycle bin of a SharePoint or OneDrive for Business site. |
|FileSyncDownloadedFull|User downloads a file to their computer from a SharePoint document library or OneDrive for Business using OneDrive sync app (OneDrive.exe).|
|FileSyncDownloadedPartial|This event has been deprecated along with the old OneDrive for Business sync app (Groove.exe).|
|FileSyncUploadedFull|User uploads a new file or changes to a file in SharePoint document library or OneDrive for Business using OneDrive sync app (OneDrive.exe).|
|FileSyncUploadedPartial|This event has been deprecated along with the old OneDrive for Business sync app (Groove.exe).|
|FileUploaded|User uploads a document to a folder on a SharePoint or OneDrive for Business site. |
|FileViewed|This event has been replaced by the FileAccessed event, and has been deprecated.|
|FolderCopied|User copies a folder from a SharePoint or OneDrive for Business site to another location in SharePoint or OneDrive for Business.|
|FolderCreated|User creates a folder on a SharePoint or OneDrive for Business site.|
|FolderDeleted|User deletes a folder from a SharePoint or OneDrive for Business site.|
|FolderDeletedFirstStageRecycleBin|User deletes a folder from the recycle bin on a SharePoint or OneDrive for Business site .|
|FolderDeletedSecondStageRecycleBin|User deletes a folder from the second-stage recycle bin on a SharePoint or OneDrive for Business site.|
|FolderModified|User modifies a folder on a SharePoint or OneDrive for Business site. This event includes folder metadata changes, such as tags and properties.|
|FolderMoved|User moves a folder from a SharePoint or OneDrive for Business site.|
|FolderRecycled|User moves a folder into the SharePoint or OneDrive Recycle Bin.|
|FolderRenamed|User renames a folder on a SharePoint or OneDrive for Business site.|
|FolderRestored|User restores a folder from the Recycle Bin on a SharePoint or OneDrive for Business site.|
|GroupAdded|Site administrator or owner creates a group for a SharePoint or OneDrive for Business site, or performs a task that results in a group being created. For example, the first time a user creates a link to share a file, a system group is added to the user's OneDrive for Business site. This event can also be a result of a user creating a link with edit permissions to a shared file.|
|GroupRemoved|User deletes a group from a SharePoint or OneDrive for Business site. |
|GroupUpdated|Site administrator or owner changes the settings of a group for a SharePoint or OneDrive for Business site. This can include changing the group's name, who can view or edit the group membership, and how membership requests are handled.|
|LanguageAddedToTermStore|Language added to the terminology store.|
|LanguageRemovedFromTermStore|Language removed from the terminology store.|
|LegacyWorkflowEnabledSet|Site administrator or owner adds the SharePoint Workflow Task content type to the site. Global administrators can also enable work flows for the entire organization in the SharePoint admin center.|
|LookAndFeelModified|User modifies a quick launch, gantt chart formats, or group formats.  Or the user creates, modifies, or deletes a view in Project web app.|
|ManagedSyncClientAllowed|User successfully establishes a sync relationship with a SharePoint or OneDrive for Business site. The sync relationship is successful because the user's computer is a member of a domain that's been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. For more information, see [Use SharePoint Online PowerShell](https://go.microsoft.com/fwlink/p/?LinkID=534609) to enable OneDrive sync for domains that are on the safe recipients list.|
|MaxQuotaModified|The maximum quota for a site has been modified.|
|MaxResourceUsageModified|The maximum allowable resource usage for a site has been modified.|
|MySitePublicEnabledSet|The flag enabling users to have public MySites has been set by the SharePoint administrator.|
|NewsFeedEnabledSet|Site administrator or owner enables RSS feeds for a SharePoint or OneDrive for Business site. Global administrators can enable RSS feeds for the entire organization in the SharePoint admin center.|
|ODBNextUXSettings|New UI for OneDrive for Business has been enabled.|
|OfficeOnDemandSet|Site administrator enables Office on Demand, which lets users access the latest version of Office desktop applications. Office on Demand is enabled in the SharePoint admin center and requires an Office 365 subscription that includes full, installed Office applications.|
|PageViewed|User views a page on a SharePoint site or OneDrive for Business site. This does not include viewing document library files from a SharePoint site or One Drive for Business site on a browser.|
|PeopleResultsScopeSet|Site administrator creates or changes the result source for People Searches for a SharePoint site.|
|PermissionSyncSettingModified|User modifies the project permission sync settings in Project web app.|
|PermissionTemplateModified|User creates, modifies or deletes a permissions template in Project web app.|
|PortfolioDataAccessed|User accesses portfolio content (driver library, driver prioritization, portfolio analyses) in Project web app.|
|PortfolioDataModified|User creates, modifies, or deletes portfolio data (driver library, driver prioritization, portfolio analyses) in Project web app.|
|PreviewModeEnabledSet|Site administrator enables document preview for a SharePoint site.|
|ProjectAccessed|User accesses project content in Project web app.|
|ProjectCheckedIn|User checks in a project that they checked out from a Project web app.|
|ProjectCheckedOut|User checks out a project located in a Project web app. Users can check out and make changes to projects that they have permission to open.|
|ProjectCreated|User creates a project in Project web app.|
|ProjectDeleted|User deletes a project in Project web app.|
|ProjectForceCheckedIn|User forces a check in on a project in Project web app.|
|ProjectModified|User modifies a project in Project web app.|
|ProjectPublished|User publishes a project in Project web app.|
|ProjectWorkflowRestarted|User restarts a workflow in Project web app.|
|PWASettingsAccessed|User access the Project web app settings via CSOM.|
|PWASettingsModified|User modifies the a Project web app configuration.|
|QueueJobStateModified|User cancels or restarts a queue job in Project web app.|
|QuotaWarningEnabledModified|Storage quota warning modified.|
|RenderingEnabled|Browser-enabled form templates will be rendered by InfoPath forms services.|
|ReportingAccessed|User accessed the reporting endpoint in Project web app.|
|ReportingSettingModified|User modifies the reporting configuration in Project web app.|
|ResourceAccessed|User accesses an enterprise resource content in Project web app.|
|ResourceCheckedIn|User checks in an enterprise resource that they checked out from Project web app.|
|ResourceCheckedOut|User checks out an enterprise resource located in Project web app.|
|ResourceCreated|User creates an enterprise resource in Project web app.|
|ResourceDeleted|User deletes an enterprise resource in Project web app.|
|ResourceForceCheckedIn|User forces a checkin of an enterprise resource in Project web app.|
|ResourceModified|User modifies an enterprise resource in Project web app.|
|ResourcePlanCheckedInOrOut|User checks in or out a resource plan in Project web app.|
|ResourcePlanModified|User modifies a resource plan in Project web app.|
|ResourcePlanPublished|User publishes a resource plan in Project web app.|
|ResourceRedacted|User redacts an enterprise resource removing all personal information in Project web app.|
|ResourceWarningEnabledModified|Resource quota warning modified.|
|SSOGroupCredentialsSet|Group credentials set in Secure store service.|
|SSOUserCredentialsSet|User credentials set in Secure store service.|
|SearchCenterUrlSet|Search center URL set.|
|SecondaryMySiteOwnerSet|A user has added a secondary owner to their MySite.|
|SecurityCategoryModified|User creates, modifies or deletes a security category in Project web app.|
|SecurityGroupModified|User creates, modifies or deletes a security group in Project web app.|
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
|StatusReportModified|User creates, modifies or deletes a status report in Project web app.|
|SyncGetChanges|User clicks **Sync** in the action tray on in SharePoint or OneDrive for Business to synchronize any changes to file in a document library to their computer.|
|SyntexBillingSubscriptionSettingsChanged|The Syntex Billing subscription settings have changed. This event is triggered when a Syntex trial expires.|
|TaskStatusAccessed|User accesses the status of one or more tasks in Project web app.|
|TaskStatusApproved|User approves a status update of one or more tasks in Project web app.|
|TaskStatusRejected|User rejects a status update of one or more tasks in Project web app.|
|TaskStatusSaved|User saves a status update of one or more tasks in Project web app.|
|TaskStatusSubmitted|User submits a status update of one or more tasks in Project web app.|
|TimesheetAccessed|User accesses a timesheet in Project web app.|
|TimesheetApproved|User approves timesheet in Project web app.|
|TimesheetRejected|User rejects a timesheet in Project web app.|
|TimesheetSaved|User saves a timesheet in Project web app.|
|TimesheetSubmitted|User submits a status timesheet in Project web app.|
|UnmanagedSyncClientBlocked|User tries to establish a sync relationship with a SharePoint or OneDrive for Business site from a computer that isn't a member of your organization's domain or is a member of a domain that hasn't been added to the list of domains (called the safe recipients list) that can access document libraries in your organization. The sync relationship is not allowed, and the user's computer is blocked from syncing, downloading, or uploading files on a document library. For information about this feature, see [Use Windows PowerShell cmdlets to enable OneDrive sync for domains that are on the safe recipients list](/powershell/module/sharepoint-online/index).|
|UpdateSSOApplication|Target application updated in Secure store service.|
|UserAddedToGroup|Site administrator or owner adds a person to a group on a SharePoint or OneDrive for Business site. Adding a person to a group grants the user the permissions that were assigned to the group. |
|UserRemovedFromGroup|Site administrator or owner removes a person from a group on a SharePoint or OneDrive for Business site. After the person is removed, they no longer are granted the permissions that were assigned to the group. |
|WorkflowModified|User creates, modifies, or deletes an Enterprise Project Type or Workflow phases or stages in Project web app.|

## SharePoint file operations

The file-related SharePoint events listed in the "File and folder activities" section in [Search the audit log in the compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#file-and-page-activities) use this schema.

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
|SourceLabel|Edm.String|No|The original label of the file before it's changed by a user action.|
|DestinationLabel|Edm.String|No|The final label of the file after it's changed by a user action.|
|SensitivityLabelOwnerEmail|Edm.String|No|The email address of the owner of the sensitivity label.|
|SensitivityLabelId|Edm.String|No|The current sensitivity label ID of the file.|

## SharePoint list operations

The SharePoint lists and list item related events listed in the "SharePoint list activities" section in [Search the audit log in the compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#sharepoint-list-activities) use this schema.

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ListTitle|Edm.String|No|The title of the SharePoint list.|
|ListName|Edm.String|No|The name of the SharePoint list.|
|ListUrl|Edm.String|No|The URL of the list relative to the containing website.|
|ListBaseType|Edm.String|No|Specifies the base type for a list.|
|ListBaseTemplateType|Edm.String|No|The list definition type on which the list is based.|
|IsHiddenList|Edm.Boolean|No|This value is set to `True` if the SharePoint list is hidden.|
|IsDocLib|Edm.Boolean|No|This value is set to `True` if the SharePoint list is of the type Document Library.|

## SharePoint Sharing schema

The file share-related SharePoint events. They are different from file- and folder-related events in that a user is taking an action that has some effect on another user. For information about the SharePoint Sharing schema, see [Use sharing auditing in the audit log](/microsoft-365/compliance/use-sharing-auditing).

|**Parameter**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|TargetUserOrGroupName |Edm.String|No|Stores the UPN or name of the target user or group that a resource was shared with.|
|TargetUserOrGroupType|Edm.String|No|Identifies whether the target user or group is a Member, Guest, Group, or Partner. |
|EventData|XML code|No|Conveys follow-up information about the sharing action that has occurred, such as adding a user to a group or granting edit permissions.|
|SiteUrl|Edm.String|No|The URL of the site where the file or folder accessed by the user is located.|
|SourceRelativeUrl|Edm.String|No|The URL of the folder that contains the file accessed by the user. The combination of the values for the  _SiteURL_,  _SourceRelativeURL_, and  _SourceFileName_ parameters is the same as the value for the **ObjectID** property, which is the full path name for the file accessed by the user.|
|SourceFileName|Edm.String|No|The name of the file or folder accessed by the user.|
|SourceFileExtension|Edm.String|No|The file extension of the file that was accessed by the user. This property is blank if the object that was accessed is a folder.|
|UniqueSharingId|Edm.String|No|The unique sharing ID associated with the sharing operation.|

## SharePoint schema

The SharePoint events listed in [Search the audit log in the compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) (excluding the file and folder events) use this schema.

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
|Setting|Represents a Project web app setting|
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
|SendOnBehalfOfUserMailboxGuid|Edm.Guid|No|The Exchange GUID of the mailbox that was accessed to send mail on behalf of.|

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
|AzureActiveDirectoryEventType|Self.[AzureActiveDirectoryEventType](#azureactivedirectoryeventtype)|Yes|The type of Microsoft Entra event. |
|ExtendedProperties|Collection(Common.NameValuePair)|No|The extended properties of the Microsoft Entra event.|
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

## Azure Active Directory Secure Token Service (STS) Logon schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|No|The GUID that represents the application that is requesting the login. The display name can be looked up via the Azure Active Directory Graph API.|
|Client|Edm.String|No|Client device information, provided by the browser performing the login.|
|DeviceProperties|Collection(Common.NameValuePair)|No|This property includes various device details, including Id, Display name, OS, Browser, IsCompliant, IsCompliantAndManaged, SessionId, and DeviceTrustType. The DeviceTrustType property can have the following values:<br/><br/>**0** - Microsoft Entra registered<br/> **1** - Microsoft Entra joined<br/> **2** - Hybrid Microsoft Entra joined|
|ErrorCode|Edm.String|No|For failed logins (where the value for the Operation property is UserLoginFailed), this property contains the Azure Active Directory STS (AADSTS) error code. For descriptions of these error codes, see [Authentication and authorization error codes](/azure/active-directory/develop/reference-aadsts-error-codes#aadsts-error-codes). A value of `0` indicates a successful login.|
|LogonError|Edm.String|No|For failed logins, this property contains a user-readable description of the reason for the failed login.|

## DLP schema

DLP events are available for Exchange Online, Endpoint(devices) and SharePoint Online, and OneDrive For Business. Note that DLP events in Exchange are only available for events based on unified DLP policy (e.g. configured via Security & Compliance Center). DLP events based on Exchange Transport Rules are not supported.

DLP (Data Loss Prevention) events will always have UserKey="DlpAgent" in the common schema. There are three types of DlpEvents that are stored as the value of the Operation property of the common schema:

- DlpRuleMatch - This indicates a rule was matched. These events exist in all Exchange, Endpoint(devices) and SharePoint Online and OneDrive for Business. For Exchange it includes false positive and override information. For SharePoint Online and OneDrive for Business, false positive and overrides generate separate events.

- DlpRuleUndo - These only exist in SharePoint Online and OneDrive for Business, and indicate a previously applied policy action has been "undone" – either because of false positive/override designation by user, or because the document is no longer subject to policy (either due to policy change or change to content in doc).

- DlpInfo - These only exist in SharePoint Online and OneDrive for Business and indicate a false positive designation but no action was "undone."

|**Parameters**|**Type**|**Mandatory**|**Description**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.[SharePointMetadata](#sharepointmetadata-complex-type)|No|Describes metadata about the document in SharePoint or OneDrive for Business that contained the sensitive information.|
|ExchangeMetaData|Self.[ExchangeMetadata](#exchangemetadata-complex-type)|No|Describes metadata about the email message that contained the sensitive information.|
|EndpointMetaData|Self.[EndpointMetadata](#endpointmetadata-complex-type)|No|Describes metadata about the document in endpoint that contained the sensitive information| 
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
|DocumentSharer|Edm.String|Yes|The user who last modified sharing of the document.|
|UniqueId|Edm.String|Yes|A guid that identifies the file.|
|LastModifiedTime|Edm.DateTime|Yes|Timestamp in UTC for when doc was last modified.|
|IsViewableByExternalUsers|Edm.Boolean|Yes|Determines if the file is accessible to any external user.|

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

### EndpointMetadata complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.[SensitiveInformation](#sensitiveinformation-complex-type))|No|Information about the type of sensitive information detected.|
|EnforcementMode|Edm.String|Yes|Indicate whether the DLP Rule set to 1/2/3/4 depicting audit/warn(block with override)/block/allow(audit without alerts) respectively.|
|FileExtension|Edm.String|No|The file extension of the document that contained the sensitive information.|
|FileType|Edm.String|No|The file type of the document that conatined the sensitive information.|
|DeviceName|Edm.String|No|The name of the device on which DLP rule match was detected.|

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
|Confidence|Edm.Int|Yes|The aggregated confidence of all pattern matches for the Sensitive Information Type.|
|Count|Edm.Int|Yes|The total number of sensitive instances detected.|
|Location|Edm.String|No||
|SensitiveType|Edm.Guid|Yes|A guid that identifies the type of sensitive data detected.|
|SensitiveInformationDetections|Self.SensitiveInformationDetections|No|An array of objects that contain sensitive information data with the following details – matched value and context of matched value.|
|SensitiveInformationDetailedClassificationAttributes|Collection(SensitiveInformationDetailedConfidenceLevelResult)|Yes|Information about the count of sensitive information type detected for each of the three confidence levels (High, Medium and Low) and wether it matches the DLP rule or not|
|SensitiveInformationTypeName|Edm.String|No|The name of the sensitive information type.|
|UniqueCount|Edm.Int32|Yes|The unique count of sensitive instances detected.|

### SensitiveInformationDetailedClassificationAttributes complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.int32|Yes|The confidence level of the pattern that was detected.|
|Count|Edm.Int32|Yes|The number of sensitive instances detected for a partcular confidence level.|
|IsMatch|Edm.Boolean|Yes|Indicates if the given count and confidence level of the sensitive type detected results in a DLP rule match.|

### SensitiveInformationDetections complex type

DLP sensitive data is only available in the activity feed API to users that have been granted "Read DLP sensitive data" permissions.

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DetectedValues|Collection(Common.NameValuePair)|Yes|An array of sensitive information that was detected. Information contains key value pairs with Value = matched value (eg. Value of credit card) and Context = an excerpt from source content that contains the matched value. |
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
|ClientApplication|Edm.String|No|If the cmdlet was executed by an application, as opposed to remote PowerShell, this field contains that application's name.|
|Parameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that do not include Personally Identifiable Information.|
|NonPiiParameters|Edm.String|No|The name and value for parameters that were used with the cmdlet that include Personally Identifiable Information. (Deprecated: This field will stop appearing in the future and its content merged with the Parameters field.)|

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
|AlertType|Self.String|Yes|Type of the alert. Alert types include: <ul><li>System</li><li>Custom|
|Name|Edm.String|Yes|Name of the alert.|
|PolicyId|Edm.Guid|No|The Guid of the policy that triggered the alert.|
|Status|Edm.String|No|Status of the alert. Statuses include: <ul><li><p>Active</li><li>Investigating</li><li>Resolved</li><li>Dismissed|
|Severity|Edm.String|No|Severity of the alert. Severity levels include: <ul><li>Low</li><li>Medium</li><li>High</li></ul>|
|Category|Edm.String|No|Category of the alert. Categories include: <ul><li>AccessGovernance</li><li>DataGovernance</li><li>DataLossPrevention</li><li>InsiderRiskManagement</li><li>MailFlow</li><li>ThreatManagement</li><li>Other|
|Source|Edm.String|No|Source of the alert. Sources include: <ul><li>Office 365 Security & Compliance</li><li>Cloud App Security|
|Comments|Edm.String|No|Comments left by the users who have viewed the alert. By default, it's "New alert".|
|Data|Edm.String|No|The detailed data blob of the alert or alert entity.|
|AlertEntityId|Edm.String|No|The identifier for the alert entity. This parameter is only applicable to AlertEntityGenerated events.|
|EntityType|Edm.String|No|Type of the alert or alert entity. Entity types include: <ul><li>User</li><li>Recipients</li><li>Sender</li><li>MalwareFamily</li></ul>This parameter is only applicable to AlertEntityGenerated events.|

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

## Data Center Security Base schema

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.[DataCenterSecurityEventType](#datacentersecurityeventtype)|Yes|The type of cmdlet event in lock box.|

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
|Action|Edm.String|No|For shared channel events, the action taken by the invitee or the channel owner for a share with team invite.|
|AddOnGuid|Edm.Guid|No|A unique identifier for the add-on that generated the event.|
|AddOnName|Edm.String|No|The name of the add-on that generated the event.|
|AddOnType|Self.[AddOnType](#addontype)|No|The type of add-on that generated this event.|
|ChannelGuid|Edm.Guid|No|A unique identifier for the channel being audited.|
|ChannelName|Edm.String|No|The name of the channel being audited.|
|ChannelType|Edm.String|No|The type of channel being audited (Standard/Private).|
|ExtraProperties|Collection(Self.[KeyValuePair](#keyvaluepair-complex-type))|No|A list of extra properties.|
|HostedContents|Collection(Self.[HostedContent](#hostedcontent-complex-type))|No|A collection of chat or channel message hosted contents.|
|Invitee|Edm.String|No|For shared channel events, the UPN of the invitee team owner who accepts or declines the invite for a share with team invite.|
|Members|Collection(Self.[MicrosoftTeamsMember](#microsoftteamsmember-complex-type))|No|A list of users within a Team.|
|MessageId|Edm.String|No|An identifier for a chat or channel message.|
|MessageURLs|Edm.String|No|Present for any URL sent in Teams messages.|
|Messages|Collection(Self.[Message](#message-complex-type))|No|A collection of chat or channel messages.|
|MessageSizeInBytes|Edm.Int64|No|The size of a chat or channel message in bytes with UTF-16 encoding.|
|Name|Edm.String|No|Only present for settings events. Name of the setting that changed.|
|NewValue|Edm.String|No|Only present for settings events. New value of the setting.|
|OldValue|Edm.String|No|Only present for settings events. Old value of the setting.|
|SubscriptionId|Edm.String|No|A unique identifier of a Microsoft Graph change notification subscription.|
|TabType|Edm.String|No|Only present for tab events. The type of tab that generated the event.|
|TeamGuid|Edm.Guid|No|A unique identifier for the team being audited.|
|TeamName|Edm.String|No|The name of the team being audited.|

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

### KeyValuePair complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Key|Edm.String|No|The key of the key-value pair.|
|Value|Edm.String|No|The value of the key-value pair.|

### Enum: AddOnType - Type: Edm.Int32

#### AddOnType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Bot|A Microsoft Teams bot.|
|2|Connector|A Microsoft Teams connector.|
|3|Tab|A Microsoft Teams tab.|

### HostedContent complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Yes|A unique identifier of the message hosted content.|
|SizeInBytes|Edm.Int64|No|The message hosted content size in bytes.|

### Message complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AADGroupId|Edm.String|No|A unique identifier of the group in Azure Active Directory that the message belongs to.|
|Id|Edm.String|Yes|A unique identifier of the chat or channel message.|
|ChannelGuid|Edm.String|No|A unique identifier of  the channel the message belongs to.|
|ChannelName|Edm.String|No|The name of the channel the message belongs to.|
|ChannelType|Edm.String|No|The type of the channel the message belongs to.|
|ChatName|Edm.String|No|The name of the chat the message belongs to.|
|ChatThreadId|Edm.String|No|A unique identifier of the chat the message belongs to.|
|ParentMessageId|Edm.String|No|A unique identifier of the parent chat or channel message.|
|SizeInBytes|Edm.Int64|No|The size of the message in bytes with UTF-16 encoding.|
|TeamGuid|Edm.String|No|A unique identifier of the team the message belongs to.|
|TeamName|Edm.String|No|The name of the team the message belongs to.|
|Version|Edm.String|No|The version of the chat or channel message.|
 
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

> [!NOTE]
> We recommend that you use the new ThreatsAndDetectionTech field because it shows multiple verdicts and the updated detection technologies. This field also aligns with the values you would see within other experiences like Threat Explorer and Advanced Hunting. 

### Detection technologies

|Name|Description|
|---|---|
|Advanced filter|Phishing signals based on machine learning.|
|Anti-malware engine|Detection from anti-malware engines.|
|Campaign|Messages identified as part of a campaign.|
|Domain reputation|Analysis based on domain reputation.|
|File detonation|File attachments found to be bad during detonated analysis.|
|File detonation reputation|File attachment marked as bad due to previous detonation reputation.|
|File reputation|File attachments marked bad due to bad reputation.|
|Fingerprint matching|The message was marked as bad due to previous messages.|
|General filter|Phishing signals based on rules.|
|Impersonation brand|The file type of the attachment.|
|Impersonation domain|Impersonation of domains that the customer owns or defines.|
|Impersonation user|Impersonation of users defined by admin or learned through mailbox intelligence.|
|Mailbox intelligence impersonation|Impersonation based on mailbox intelligence.|
|Mixed analysis detection|Multiple filters contributed to the verdict for this message.|
|Spoof DMARC|DMARC authentication failure for messages.|
|Spoof external domain|Sender is trying to spoof some other domain.|
|Spoof intra-org|Sender is trying to spoof the recipient domain.|
|URL detonation|The message was considered bad due to a previous malicious URL detonation.|
|URL detonation reputation|The message was considered bad due to malicious URL detonation.|
|URL malicious reputation|The message was considered bad due a malicious URL.|

### AttachmentData complex type

#### AttachmentData

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|Yes|The file name of the attachment.|
|FileType|Edm.String|Yes|The file type of the attachment.|
|FileVerdict|Self.[FileVerdict](#fileverdict)|Yes|The file malware verdict.|
|MalwareFamily|Edm.String|No|The file malware family.|
|SHA256|Edm.String|Yes|The file SHA256 hash.|

> [!NOTE]
> Within the Malware family, you'll be able to see the exact MalwareFamily name (for example, HTML/Phish.VS!MSR) or Malicious Payload as a static string. A Malicious Payload should still be treated as malicious email when a specific name isn't identified.

### SystemOverrides complex type
 
#### SystemOverrides

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Details|Edm.String|No|The details about the specific override (such as ETR or Safe Sender) that was applied.|
|FinalOverride|Edm.String|No|Indicates the override that impacted the delivery in the case of multiple overrides.|
|Result|Edm.String|No|Indicates whether the email was set to allowed or blocked based on the override.|
|Source|Edm.String|No|Indicates whether the override was user-configured or tenant-configured.|

### AuthDetails complex type
 
#### AuthDetails
 
|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|Name|Edm.String|No|The name of the specific auth check, such as DKIM or DMARC.|
|Value|Edm.String|No|The value associated with the specific auth check, such as True or False.|
 
### Enum: FileVerdict - Type: Edm.Int32

#### FileVerdict

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Good|No threats detected.|
|1|Bad|Malware found in attachment.|
|-1|Error|Scan / analysis error.|
|-2|Timeout|Scan / analysis timeout.|
|-3|Pending|Scan / analysis not complete.|

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

### Enum: URLClickAction - Type: Edm.Int32

#### URLClickAction

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|2|Blockpage|User blocked from navigating to the URL by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links).|
|3|PendingDetonationPage|User presented with the detonation pending page by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links).|
|4|BlockPageOverride|User blocked from navigating to the URL by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links); however user overrode block to navigate to the URL.|
|5|PendingDetonationPageOverride|User presented with the detonation page by [Safe Links in Defender for Office 365](/office365/securitycompliance/atp-safe-links); however user overrode to navigate to the URL.|

### File events

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileData|Self.[FileData](#filedata)|Yes|Data about the file that triggered the event.|
|SourceWorkload|Self.[SourceWorkload](#sourceworkload)|Yes|Workload or service where the file was found (for example, SharePoint Online, OneDrive for Business, or Microsoft Teams)
|DetectionMethod|Edm.String|Yes|The method or technology used by Microsoft Defender for Office 365 for the detection.|
|LastModifiedDate|Edm.Date|Yes|The date and time in Coordinated Universal Time (UTC) when the file was created or last modified.|
|LastModifiedBy|Edm.String|Yes|Identifier (for example, an email address) for the user who created or last modified the file.|
|EventDeepLink|Edm.String|Yes|Deep-link to the file event in Explorer or Real-time reports in the Security & Compliance Center.|

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

### Enum: SourceWorkload - Type: Edm.Int32

#### SourceWorkload

|**Value**|**Member name**|
|:-----|:-----|
|0|SharePoint Online|
|1|OneDrive for Business|
|2|Microsoft Teams|

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

#### IP

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"ip" |
|Address    |Edm.String    |The IP address as a string, such as `127.0.0.1`

#### URL

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"url" |
|Url    |Edm.String    |The full URL to which an entity points  |

#### Mailbox (also equivalent to the user) 

|Field    |Type    |Description |
|----|----|----|
|Type    |Edm.String    |"mailbox"  |
|MailboxPrimaryAddress    |Edm.String    |The mailbox's primary address  |
|DisplayName    |Edm.String    |The mailbox's display name |
|Upn    |Edm.String    |The mailbox's UPN  |

#### File

|Field    |Type    |Description  |
|----|----|----|
|Type    |Edm.String    |"file" |
|Name    |Edm.String    |The file name without path |
FileHashes |Collection (Edm.String)    |The file hashes associated with the file |

#### FileHash

|Field    |Type    |Description |
|----|----|----|
|Type    |Edm.String    |"filehash" |
|Algorithm    |Edm.String    |The hash algorithm type, which can be one of these values:<br/>- Unknown<br/>- MD5<br/>- SHA1<br/>- SHA256<br/>- SHA256AC
|Value    |Edm.String    |The hash value  |

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

### MembershipInformationType complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
| MemberEmail | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The email address of the group. |
| Status      | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | Not currently populated. |

### SharingInformationType complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
| RecipientEmail    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The email address of the recipient of a sharing invitation. |
| RecipientName    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The name of the recipient of a sharing invitation. |
| ResharePermission | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  No  | The permission being granted to the recipient. |


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

### Dynamics 365 entity operation schema

Entity events from model-driven apps in Dynamics 365 use this schema to build on the Dynamics 365 base schema. This schema includes information about the entity operation that triggered the audited event.

| **Parameters**     | **Type**            | **Mandatory?** | **Description**|
|:------------------ | :------------------ | :--------------|:--------------|
|EntityId|Edm.Guid|No|The unique identifier of the entity.|
|EntityName|Edm.String|Yes|The name of the entity in the organization. Example of entities include `contact` or `authentication`.|
|Message|Edm.String|Yes|This parameter contains the operation that was performed in related to the entity. For example, if a new contact was created, the value of the Message property is  `Create` and the corresponding value of the EntityName property is `contact`.|
|Query|Edm.String|No|The parameters of the filter query that was used while executing the FetchXML operation.|
|PrimaryFieldValue|Edm.String|No|Indicates the value for the attribute that is the primary field for the entity.|

## Workplace Analytics schema

The WorkPlace Analytics events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-workplace-analytics-activities) will use this schema.

| **Parameters**     | **Type**            | **Mandatory?** | **Description**|
|:------------------ | :------------------ | :--------------|:--------------|
| WpaUserRole        | Edm.String | No     | The Workplace Analytics role of the user who performed the action.|
| ModifiedProperties | Collection (Common.ModifiedProperty) | No | This property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property.|
| OperationDetails   | Collection (Common.NameValuePair)    | No | A list of extended properties for the setting that was changed. Each property will have a **Name** and **Value**.|

## Quarantine schema

The quarantine events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#quarantine-activities) will use this schema. For more information about quarantine, see [Quarantine email messages in Office 365](/microsoft-365/security/office-365-security/quarantine-email-messages).

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RequestType|Self.[RequestType](#enum-requesttype---type-edmint32)|No|The type of quarantine request performed by a user.|
|RequestSource|Self.[RequestSource](#enum-requestsource---type-edmint32)|No|The source of a quantine request can come from the Security & Compliance Center (SCC), a cmdlet, or a URLlink.|
|NetworkMessageId|Edm.String|No|The network message id of quarantined email message.|
|ReleaseTo|Edm.String|No|The recipient of the email message.|

### Enum: RequestType - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Preview|This is a request from a user to preview an email message that is deemed to be harmful.|
|1|Delete|This is a request from a user to delete an email message that is deemed to be harmful.|
|2|Release|This is a request from a user to release an email message that is deemed to be harmful.|
|3|Export|This is a request from a user to export an email message that is deemed to be harmful.|
|4|ViewHeader|This is a request from a user to view the header an email message that is deemed to be harmful.|
|5|Release request|This is a release request from a user to release an email message that is deemed to be harmful.|

### Enum: RequestSource - Type: Edm.Int32

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|SCC|The Security & Compliance center (SCC) is the source where the request from a user to preview, delete, release, export, or view the header of a potentially harmful email message can originate from. |
|1|Cmdlet|A cmdlet is the source where the request from a user to preview, delete, release, export, or view the header of a potentially harmful email message can originate from.|
|2|URLlink|This is a source where the request from a user to preview, delete, release, export, or view the header of potentially harmful email message can originate from.|

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

### Enum: FormsUserTypes - Type: Edm.Int32

#### FormsUserTypes

|**Value**|**Form User Type**|**Description**|
|:-----|:-----|:-----|
|0|Admin|An administrator who has access to the form.|
|1|Owner|A user who is the owner of the form.|
|2|Responder|A user who has submitted a response to a form.|
|3|Coauthor|A user who has used a collaboration link provided by the form owner to login and edit a form.|

### Enum: FormTypes - Type: Edm.Int32

#### FormTypes

|**Value**|**Form Types**|**Description**|
|:-----|:-----|:-----|
|0|Form|Forms that are created with the New Form option.|
|1|Quiz|Quizzes that are created with the New Quiz option.  A quiz is a special type of form that includes additional features such as point values, auto and manual grading, and commenting.|
|2|Survey|Surveys that are created with the New Survey option.  A survey is a special type of form that includes additional features such as CMS integration and support for Flow rules.|

## MIP label schema

Events in the Microsoft Purview Information Protection label schema are triggered when Microsoft 365 detects an email message processed by agents in the Transport pipeline that has a sensitivity label applied to it. The sensitivity label may have been applied manually or automatically, and it may have been applied within or outside of the Transport pipeline. Sensitivity labels can be automatically applied to email messages by auto-apply label policies.

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

## Encrypted message portal events schema
 
Events for enrypted message portal schema are triggered when when Purview Message Encryption detects an encrypted email message is accessed through the portal by an external recipient. The mail may have been encrypted manually with a sensitivity label or an RMS template, or automatically by a transport rule, a Data Loss Prevention policy, or an auto-labeling policy.
 
The intent of this audit schema is to represent the sum of all portal activity that involves accessing the encrypted mail by external recipients. In other words, there should be a recorded audit activity for a recipient that attempts to sign in to the portal and any activities related to accessing the encrypted mail. This includes mail sent to or from users in the organization when the mail has encryption applied to it, regardless of when or how the encryption was applied. For more information, see, [Learn about encrypted message portal logs](/microsoft-365/compliance/ome-message-access-logs).
 
|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|MessageId|Edm.String|No|The Id of the message.|
|Recipient|Edm.String|No|Recipient email address.|
|Sender|Edm.String|No|Email address of sender.|
|AuthenticationMethod|Self.AuthenticationMethod|No|Authentication method when accessing the message, i.e. OTP, Yahoo, Gmail, Microsoft.|
|AuthenticationStatus|Self.AuthenticationStatus|No|0 – Success, 1- Failure.|
|OperationStatus|Self.OperationStatus|No|0 – Success, 1- Failure.|
|AttachmentName|Edm.String|No|Name of the attachment.|
|OperationProperties|Collection(Common.NameValuePair)|No|Extra properties, i.e. number of OTP passcode sent, email subject, etc.|
 
## Communication compliance Exchange schema

The communication compliance events listed in the Office 365 audit log use this schema. This includes audit records for the **SupervisoryReviewOLAudit** operation that's generated when email message content contains offensive language identified by anti-spam models with a match accuracy of \>= 99.5%.

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
| ExchangeDetails |[ExchangeDetails](#exchangedetails)|No|Properties of the email message that triggered the SupervisoryReviewOLAudit event.|

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

### Enum: AttachmentDetails - Type: Edm.Int32

#### AttachmentDetails

| **Member name** | **Type**   | **Description**|
|:--------------- |:---------- | :--------------|
| FileName        | Edm.String | The name of the file attached to the email message.|
| FileType        | Edm.String | The file extension of the file attached to the email message.|
| SHA256          | Edm.String | The SHA-256 hash of the file attached to the email message.|

## Reports schema

The Reports events listed in [Search the audit log in the Office 365 Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#reports-activities) will use this schema.

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
| ModifiedProperties | Collection (Common.ModifiedProperty) | No | This property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property.|

## Compliance connector schema

Events in the compliance connector schema are triggered when items that are imported by a data connector are skipped or failed to be import to user mailboxes. For more information about data connectors, see [Learn about connectors for third-party data](/purview/archive-third-party-data).

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
|JobId|	Edm.String|	No |This is a unique identifier of the data connector.|
|TaskId| Edm.String| No |Unique identifier of the periodic data connector instance. Data connectors import data in periodic intervals.|
|JobType| Edm.String|	No |The name of the data connector.|
|ItemId| Edm.String| No	|Unique identifier of the item (for example, an email message) being imported.|
|ItemSize| Edm.Int64| No |The size of the item being imported.|
|SourceUserId| Edm.String| No |The unique identifier of the user from the third-party data source. For example, for a Slack data connector, this property specifies the user Id in Slack workspace.|
|FailureType| Self.[FailureType](#enum-failuretype---type-edmint32)| No |Indicates the type of data import failure. For example, the value **incorrectusermapping** indicates the item wasn't imported because no user mapping between the third-party data source and Microsoft 365 could be found.|
|ResultMessage| Edm.String|	No |Indicates the type of failure, such as **Duplicte message**.|
|IsRetry| Edm.Boolean| No |Indicates whether the data connector retried to import the item.|
|Attachments| Collection.[Attachment](#attachment-complex-type)| No	|A list of attachments received from the third-party data source.|

### Enum: FailureType - Type: Edm.Int32

|**Value**|**Member name**|
|:-----|:-----|
|0|Default|
|1|MailboxWrite|

### Attachment complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|No|The name of the attachment.|
|Details|Edm.String|No|Other details about the attachment.
 
 ## SystemSync schema

Events in the SystemSync schema are triggered when the SystemSync ingested data is either exported via Data Lake or shared via other services.

### DataLakeExportOperationAuditRecord

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
|DataStoreType|	DataStoreType|Yes	|Indicates which data store the data was downloaded from. Refer DataStoreType for all possible values.|
|UserAction|	DataLakeUserAction|Yes	|Indicates what action user had performed on the data store. Refer DataLakeUserAction for all possible values.|
|ExportTriggeredAt|	Edm.DateTimeOffset|Yes	|Indicates when the data export was triggered.|
|NameOfDownloadedZipFile|	Edm.String|No	|The name of the compressed file the admin had downloaded from the Data Lake.|

### DataShareOperationAuditRecord

|**Parameters**  |**Type**|**Mandatory?** |**Description**|
|:---------------|:-------|:--------------|:--------------|
|Invitation|	DataShareInvitationType|No	|Details of the invite sent to the recipient of the Data Share.|

#### DataShareInvitationType complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ShareId|Edm.Guid|Yes|System assigned identifier for the Data Share.|
|Invitees|Collection(Edm.Guid)|Yes|List of admin users the invite was sent to.|
|InviteeTenantId|Edm.Guid|Yes|The target tenant whom the invite is intended to.|
|ShareName|Edm.String|Yes|System assigned name for the Data Share.|
|SyncFrequency|Self.SyncFrequency|Yes|Frequency at which the data is synced to the destination storage account once share is established. See SyncFrequency for possible values.|
|SyncStartTime|Edm.DateTimeOffset|Yes|Date and time of first sync.|

**Enum: SyncFrequency - Type: Edm.Int32**

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Hourly|Indicates the data will be synced every hour.|
|1|Daily|Indicates the data will be synced once a day.|


**Enum: DataStoreType - Type: Edm.Int32**

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|CanonicalStore|Indicates data will be downloaded from Canonical store.|
|1|StagingStore|Indicates data will be downloaded from Staging store.|


**Enum: DataLakeUserAction - Type: Edm.Int32**

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|TriggerExport|The admin user triggered export from Data Lake.|
|1|DownloadZipFile|The admin user downloaded the exported data.|

 
 #### MicrosoftGraphDataConnectOperation complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.Guid|Yes|The application identification.|
|ApplicationName|Edm.String|Yes|The application name.|
|PipelineName|Edm.String|Yes|The pipeline name.|
|PipelineRunId|Edm.Guid|No|The identification of this pipeline run.|
|CopyActivityRunId|Edm.Guid|No|The identification of the ADF copy activity.|
|RunStartTime|Edm.Date|Yes|Date and time of the extraction.|
|RunEndTime|Edm.Date|Yes|Date and time of the extraction.|
|DatasetName|Edm.String|Yes|The dataset name being extracted.|
|DatasetColumns|Edm.String|Yes|The set of selected columns being extracted.|
|ScopeList|Edm.String|Yes|The scope of the extraction.|
|ScopeCountRequested|Edm.Int64|No|The requested scope count for this extraction.|
|ScopeCountDelivered|Edm.Int64|No|The delivered scope count for this extraction.|
|UndeliveredScope|Edm.String|No|The undelivered scope of the extraction.|
|RowCount|Edm.Int64|No|The number of rows extracted.|
|Status|Edm.String|Yes|The extraction status.|
|Reason|Edm.String|No|The error message in case of failure.|
 
 ## AipDiscover

The following table contains information related to Azure Information Protection (AIP) scanner events.

| Event         | Description |
|:--            |:--|
|	ApplicationId	|	The ID of the application performing the operation.	|
|	ApplicationName	|	Friendly name of the application performing the operation. Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).	|
|	ClientIP	|	The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. The IP address is displayed in either an IPv4 or IPv6 address format.	|
|	CreationTime	|	The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.	|
|	DataState	|	Describes the state of the data.	|
|	DeviceName	|	The device on which the activity happened.	|
|	Id	|	GUID of the current record.	|
|	IsProtected	|	Whether protected: True/False	|
|	Location	|	The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare and cloud.	|
|	ObjectId	|	File full path (URL). For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user.	|
|	Operation	|	Describes type of access.	|
|	OrganizationId	|	The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.	|
|	Platform	|	Device platform (Win, OSX, Android, iOS) 	|
|	ProcessName	|	The relevant process name, eg. Outlook, msip.app, WinWord.	|
|	ProductVersion	|	Version of the AIP client.	|
|	ProtectionOwner	|	Rights Management owner in UPN format.	|
|	ProtectionType	|	Protection type can be template or ad-hoc.	|
|	RecordType	|	The type of operation indicated by the record. See the AuditLogRecordType table for details on the types of audit log records. For a complete updated list and full description of the Log RecordType, see the Microsoft 365 Compliance audit log activities via O365 Management API blog post. Here we only list the relevant MIP Record types.	|
|	Scope	|	Was this event created by a hosted O365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to O365.	|
|	SensitiveInfoTypeData	|	Stores the datatype of the Sensitive Info type data.	|
|	SensitivityLabelId	|	The current MIP sensitivity label GUID. Use cmdlt Get-Label to get the full values of the GUID.	|
|	TemplateId	|	TemplateID parameter to get a specific template. The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.	|
|	UserId	|	The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records.	|
|	UserKey	|	An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.	|
|UserType        | The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|	Version	|	Version ID of the file in the operation.	|
|	Workload	|	Stores The Office 365 service where the activity occurred.	|
 
## AipSensitivityLabelAction

The following table contains information related to AIP sensitivity label events.

| Event                | Description |
|:---------------------|:------------|
|	ApplicationId	|	Corresponds to the Microsoft Entra Application ID.	|
|	ApplicationName	|	Application friendly name of the application performing the operation.	|
|	CreationDate	|	The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.	|
|	DataState	|	Specifies the state of the data.	|
|	DeviceName	|	The name of the user's device.	|
|	Identity	|	The identity of the user or service to be authenticated.	|
|	IsProtected	|	Whether protected: True/False	|
|	IsProtectedBefore	|	Whether the content was protected before change: True/False	|
|	IsValid	|	Boolean	|
|	Location	|	The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare, and cloud.	|
|	ObjectState	|	Specifies the state of the object.	|
|Operation             | The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities:</br>SensitivityLabelApplied</br>SensitivityLabelUpdated</br>SensitivityLabelRemoved</br>SensitivityLabelPolicyMatched</br>SensitivityLabeledFileOpened.|
|Identity              | The identity of the user or service to be authenticated.|
|	PSComputerName	|	Computer Name	|
|	PSShowComputerName	|	The value is False for documented edited in Office 365.	|
|	Platform	|	Device platform (Win, OSX, Android, iOS). 	|
|	ProcessName	|	Process that hosts MIP SDK.	|
|	ProductVersion	|	Version of the Azure Information Protection client that performed the audit action.	|
|	ProtectionType	|	Protection type can be template or ad-hoc.	|
|	RecordType	|	Shows the value of Label Action. The operation type indicated by the record. For more information, see the full list of record types.	|
|	RunspaceId	|	The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.	|
|	SensitiveInfoTypeData	|	Stores the datatype of the Sensitive Info Type Data	|
|	TemplateId	|	TemplateID parameter to get a specific template. The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.	|
|UserId                | The User Principal Name (UPN) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. <br><br>Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service.|

## AipProtectionAction

| Event | Description |
|:--|:--|
|PSComputerName| Computer name |
|RunspaceId    | The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName| The value is false for a documented edited in Office 365.|
|RecordType        | Shows the value of Label Action. The operation type indicated by the record. Here we are only listing the relevant MIP Record types. For more information, see the [full list of record types](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).|
|CreationTime      | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId            |  The User Principal Name (UPN) of the user who performed the action (specified in the Operation property) that resulted in the record being logged. For example, my_name@my_domain_name. <br><br>Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the [app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide#the-appsharepoint-user-in-audit-records&preserve-view=true). |
|Operation          | The operation type for the audit log. The name of the user or admin activity. For a description of the most common operations/activities.<br> SensitivityLabelApplied<br>SensitivityLabelUpdated<br>SensitivityLabelRemoved<br>SensitivityLabelPolicyMatched<br>SensitivityLabeledFileOpened.|
|Identity           | The identity of the user or service to be authenticated.|
|ObjectState        | State of the Object after the current event. |
|ApplicationId      | The application where the activity happened and displayed in GUID.|
|ApplicationName    |	Application friendly name of the application performing the operation.Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName        | Process name of the Office application. |
|Platform           | The platform on which the activity happened. For example, Windows. |
|DeviceName         | Device the event was recorded on. |
|ProductVersion     | Version of the Azure Information Protection client that performed the audit action.|
|UserId             | The UPN of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. <br><br>Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the [app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide#the-appsharepoint-user-in-audit-records&preserve-view=true). |
|ClientIP           | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. The IP address is displayed in either an IPv4 or IPv6 address format.|
|Id                 | GUID of the current record. |
|RecordType         | Shows the value of Label Action. The operation type indicated by the record. Here we are only listing the relevant MIP Record types.|
|CreationTime       | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|Operation          | The name of the user or admin activity. For a description of the most common operations/activities, see [Search the audit log in the Office 365 Protection Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide&preserve-view=true).|
|OrganizationId     | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType         | The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|UserKey          | An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|Workload         | Stores the Office 365 service where the activity occurred.|
|Version          | Version of the Azure Information Protection client that performed the audit action|
|Scope            | Specifies scope.|

## AipFileDeleted

| Event | Description |
|:--|:--|
|PSComputerName| Computer name |
|RunspaceId    | The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName| The value is false for a documented edited in Office 365.|
|RecordType        | Shows the value of Label Action. The operation type indicated by the record. Here we are only listing the relevant MIP Record types. For more information, see the [full list of record types](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).|
|CreationTime      | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId            |  The User Principal Name (UPN) of the user who performed the action (specified in the Operation property) that resulted in the record being logged. For example, my_name@my_domain_name. <br><br>Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the [app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide#the-appsharepoint-user-in-audit-records&preserve-view=true). |
|Operation          | The operation type for the audit log. The name of the user or admin activity. For a description of the most common operations/activities.<br> SensitivityLabelApplied<br>SensitivityLabelUpdated<br>SensitivityLabelRemoved<br>SensitivityLabelPolicyMatched<br>SensitivityLabeledFileOpened.|
|Identity           | The identity of the user or service to be authenticated.|
|ObjectState        | State of the Object after the current event. |
|ApplicationId      | The application where the activity happened and displayed in GUID.|
|ApplicationName    |	Application friendly name of the application performing the operation.Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName        | Process name of the Office application. |
|Platform           | The platform on which the activity happened. For example, Windows. |
|DeviceName         | Device the event was recorded on. |
|ProductVersion     | Version of the Azure Information Protection client that performed the audit action.|
|UserId             | The UPN of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. <br><br>Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the [app@sharepoint user in audit records](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide#the-appsharepoint-user-in-audit-records&preserve-view=true). |
|ClientIP           | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. The IP address is displayed in either an IPv4 or IPv6 address format.|
|Id                 | GUID of the current record. |
|RecordType         | Shows the value of Label Action. The operation type indicated by the record. Here we are only listing the relevant MIP Record types.|
|CreationTime       | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|Operation          | The name of the user or admin activity. For a description of the most common operations/activities, see [Search the audit log in the Office 365 Protection Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide&preserve-view=true).|
|OrganizationId     | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType         | The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|UserKey          | An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|Workload         | Stores the Office 365 service where the activity occurred.|
|Version          | Version of the Azure Information Protection client that performed the audit action|
|Scope            | Specifies scope.|

## AipHeartBeat

The following table contain information related to AIP heartbeat events.

| Event | Description |
|:--|:--|
|	ApplicationId	|	Corresponds to the Microsoft Entra Application ID.	|
|	ApplicationName	|	Application friendly name of the application performing the operation.	|
|	CreationDate	|	The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.	|
|	DataState	|	Specifies the state of the data.	|
|	DeviceName	|	The name of the user's device.	|
|	Identity	|	The identity of the user or service to be authenticated.	|
|	IsProtected	|	Whether protected: True/False	|
|	IsProtectedBefore	|	Whether the content was protected before change: True/False	|
|	IsValid	|	Boolean	|
|	Location	|	The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare, and cloud.	|
|	ObjectState	|	Specifies the state of the object.	|
|	Operation	|	The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities:	|
|	PSComputerName	|	Computer Name	|
|	PSShowComputerName	|	The value is False for documented edited in Office 365.	|
|	Platform	|	Device platform (Win, OSX, Android, iOS). 	|
|	ProcessName	|	Process that hosts MIP SDK.	|
|	ProductVersion	|	Version of the Azure Information Protection client that performed the audit action.	|
|	ProtectionType	|	Protection type can be template or ad-hoc.	|
|	RecordType	|	Shows the value of Label Action. The operation type indicated by the record. For more information, see the full list of record types.	|
|	RunspaceId	|	The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.	|
|	SensitiveInfoTypeData	|	Stores the datatype of the Sensitive Info Type Data	|
|	TemplateId	|	TemplateID parameter to get a specific template. The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.	|
|	UserId	|	 The UPN of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records.	|
|UserType         | The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|UserKey          | An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 

#### MicrosoftGraphDataConnectConsent complex type

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.Guid|Yes|The application identification.|
|ApplicationVersion|Edm.String|Yes|The application version.|
|AppRegistrationTenantId|Edm.Guid|Yes|The application registration tenant id.|
|Approver|Edm.String|Yes|The approver's user principal name.|
|ApprovalUpdatedDateInUTC|Edm.Date|Yes|The update date time in UTC.|
|ApprovalExpiryDateInUTC|Edm.Date|Yes|The expiry date time in UTC.|
|ApprovalValidDays|Edm.Int32|Yes|The number of days from update for which the approval will be valid.|
|DestinationSinks|Edm.String|Yes|The destination sinks.|
|DestinationTenantId|Edm.Guid|Yes|The destination tenant id.|
|Reason|Edm.String|No|The reason provided by the admin who performed the operation.|
|State|Edm.String|Yes|The consent state.|
|Datasets|CollectionSelf.[MGDCDataset](#complex-type-mgdcdataset)|Yes|Details on the datasets which were consented to as part of this operation.|

#### Complex Type MGDCDataset

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|DatasetName|Edm.String|Yes|The name of the dataset in the consent operation.|
|DatasetColumns|Edm.String|Yes|The list of columns for the dataset in the consent operation.|
|DenyGroups|Edm.String|No|The deny groups list for the dataset in the consent operation.|
|Scope|Edm.String|Yes|The scope types for the dataset in the consent operation. Possible values are All, List and FilterUri.|
|ScopeFiltersUris|Edm.String|No|The scope filter URI for the dataset in the consent operation.|
|ScopeList|Edm.String|No|The scope group list for the dataset in the consent operation.|
|PrivacyPolicyType|Edm.String|Yes|The privacy policy types for the dataset in the consent operation. Possible values are None and DenyList.|

## Viva Goals schema

The audit records for events related to Viva Goals use this schema (in addition to the [Common schema](#common-schema)). For details how you can search for the audit logs from the compliance portal, see [Search the audit log in the Security & Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance). For details about capturing events and activities related to Viva Goals, see [Audit log activities](/microsoft-365/compliance/audit-log-activities).

|**Parameters**  |**Type**  |**Mandatory?**  |**Description**  |
|---------|---------|---------|---------|
|Detail|Edm.String |No |A description of the event or the activity that occurred in Viva Goals.|
|Username |Edm.String </br>Term="Microsoft.Office.Audit.Schema.PIIFlag"</br>Bool="true"</br> |No |The name of the user who trigged the event.|
|UserRole |Edm.String |No |The role of the user who trigged this event in Viva Goals. This will mention if the user is an organization admin or an owner.|
|OrganizationName |Edm.String </br>Term="Microsoft.Office.Audit.Schema.PIIFlag" </br>Bool="true" |No |The name of the organization in Viva Goals where the event was triggered.|
|OrganizationOwner |Edm.String </br>Term="Microsoft.Office.Audit.Schema.PIIFlag" </br>Bool="true" |No |The owner of the organization in Viva Goals where the event occurred.|
|OrganizationAdmins |Collection(Edm.String) </br>Term="Microsoft.Office.Audit.Schema.PIIFlag" </br>Bool="true" |No |The admin(s) of the organization in Viva Goals where the event occurred. There can be one or more admins in the organization.|
|UserAgent |Edm.String </br>Term="Microsoft.Office.Audit.Schema.PIIFlag" </br>Bool="true" |No |The user agent (browser details) of the user who trigged the event. UserAgent might not be present in case of a system generated event.|
|ModifiedFields |Collection(Common.NameValuePair) |No |A list of attributes that were modified along with its new and old values output as a JSON.|
|ItemDetails |Collection(Common.NameValuePair) |No |Additional properties about the object that was modified.|

## Microsoft Planner schema

Microsoft Planner overwrites the definition of ObjectId and ResultStatus in the [Common schema](#common-schema). Microsoft Planner's ObjectId definition is bound to each Microsoft Planner's record type and will be illustrated individually.

Microsoft Planner's ResultStatus is defined as the following.

### Enum: ResultStatus - Type: Edm.Int32

#### ResultStatus

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|Success|The user request succeeded.|
|2|Failure|The user request failed due to reasons other than authorization.|
|3|AuthorizationFailure|The user requested failed due to failed authorization.|

Microsoft Planner extends the [Common schema](#common-schema) with the following record types.

### PlannerPlan record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Id of the plan requested.|
|ContainerType|Self.[ContainerType](#containertype)|Type of the container associated with the plan.|
|ContainerId|Edm.String|Id of the container associated with the plan.|
|SharedWithContainerId|Edm.String|Id of the container with shared access to the plan.|
|SharedWithContainerType|Self.[ContainerType](#containertype)|Type of the container with shared access to the plan.|
|SharedWithContainerAccessLevel|Self.[PlanAccessLevel](#planaccesslevel)|Level of access given to container with shared access to the plan.|

### Enum: ContainerType - Type Edm.Int32

#### ContainerType

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Invalid|Used when the requested plan is not found.|
|2|Group|The plan is associated with a M365 Group.|
|3|TeamsConversation|The plan is associated with a Teams conversation.|
|4|OfficeDocument|The plan is associated with a Office document.|
|5|Roster|The plan is associated with a roster group.|
|6|Project|The plan originates from Microsoft Project.|

### Enum: PlanAccessLevel - Type Edm.Int32

#### PlanAccessLevel

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|1|ReadAccess|Access to read Plan|
|2|ReadWriteAccess|Access to read and write to Plan|
|3|FullAccess|Access to read, write and configure Plan|

### PlannerCopyPlan record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Id of the plan being copied.|
|OriginalPlanId|Edm.String|Id of the plan being copied. Same as ObjectId.|
|OriginalContainerType|Self.[ContainerType](#containertype)|Type of the container associated with the original plan.|
|OriginalContainerId|Edm.String|Id of the container associated with the original plan.|
|NewPlanId|Edm.String|Id of the new plan. Null when the operation failed.|
|NewContainerType|Self.[ContainerType](#containertype)|Type of the container associated with the new plan.|
|NewContainerId|Edm.String|Id of the container associated with the new plan.|

### PlannerTask record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Id of the task requested.|
|PlanId|Edm.String|Id of the plan containing the task.|

### PlannerRoster record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Id of the roster requested.|
|MemberIds|Edm.String|A comma-separated string of member ids changed to the roster.|

### PlannerPlanList record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|A representation of the view query for a list of plans.|
|PlanList|Edm.String|A comma-separated string of plan ids queried.|

### PlannerTaskList record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|A representation of the view query for a list of tasks.|
|PlanList|Edm.String|A comma-separated string of task ids queried.|

### PlannerTenantSettings record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Original tenant settings in JSON.|
|TenantSettings|Edm.String|New tenant settings in JSON.|

### PlannerRosterSensitivityLabel record type

|**Properties**|**Type**|**Description**|
|:-----|:-----|:-----|
|ObjectId|Edm.String|Id of the sensitivity label. Null when the sensitivity label is removed.|
|Roster|Edm.String|Id of the roster to which the sensitivity label is changed.|
|AssignmentMethod|Self.[SensitivityLabelAssignmentMethod](#sensitivitylabelassignmentmethod)|The assignment method of the sensitivity label.|

### Enum: SensitivityLabelAssignmentMethod - Type Edm.Int32

#### SensitivityLabelAssignmentMethod

|**Value**|**Member name**|**Description**|
|:-----|:-----|:-----|
|0|Standard|The sensitivity label is automatically applied but not allowed to override a privileged label assignment.|
|1|Privileged|The sensitivity label is applied manually by a user or by an admin.|
|2|Auto|The sensitivity label is automatically applied and is allowed to override a privileged label assignment.|

## Microsoft Project for the web schema
Microsoft Project For The web extends the [Common schema](#common-schema) with the following record types.

### ProjectForThewebProject record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ProjectId|Edm.Guid|No|Id of the Project being audited.|
|AdditionalInfo|CollectionSelf.[AdditionalInfo](#complex-type-additionalinfo)|No|Additional information.|

### ProjectForThewebTask record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ProjectId|Edm.Guid|Yes|Id of the Project being audited.|
|TaskId|Edm.Guid|Yes|Id of the Task being audited.|
|AdditionalInfo|CollectionSelf.[AdditionalInfo](#complex-type-additionalinfo)|No|Additional information.|

### ProjectForThewebRoadmap record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RoadmapId|Edm.Guid|Yes|Id of the Roadmap being audited.|
|AdditionalInfo|CollectionSelf.[AdditionalInfo](#complex-type-additionalinfo)|No|Additional information.|

### ProjectForThewebRoadmapItem record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RoadmapItemId|Edm.Guid|Yes|Id of the Roadmap Item being audited.|
|AdditionalInfo|CollectionSelf.[AdditionalInfo](#complex-type-additionalinfo)|No|Additional information.|

### Complex Type AdditionalInfo

|**Parameters**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|EnvironmentName|Edm.String|No|Id of the environment where action was performed.|

### ProjectForThewebProjectSetting record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|ProjectEnabled|Edm.Boolean|Yes|The value that was set for Project for the web (1= enabled, 0 disabled).|

### ProjectForThewebRoadampSetting record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|RoadmapEnabled|Edm.Boolean|Yes|The value that was set for Roadmap (1= enabled, 0 disabled).|

### ProjectForThewebAssignedToMeSetting record type

|**Properties**|**Type**|**Mandatory?**|**Description**|
|:-----|:-----|:-----|:-----|
|AssignedToMeEnabled|Edm.Boolean|Yes|The value that was set for AssignedToMe (1= enabled, 0 disabled).|
