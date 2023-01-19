---
ms.TocTitle: Search-UnifiedAuditLog
title: AIP Unified Audit Log Best Practices
description: Understand the AIP Unified Audit Log Search-UnifiedAuditLog cmdlet tool
ms.ContentId: c2cdd604-fa79-424c-b8e1-9474f248279e
ms.topic: reference
ms.date: 01/17/2023
ms.localizationpriority: high
---

# Understand Search-UnifiedAuditLog

The audit log is a tool that records events from a range of workloads. The Search-UnifiedAuditLog cmdlet can be used to search and retrieve data from the audit log. It's important to understand how to use this cmdlet effectively, particularly when it comes to interpreting the information in the AuditData property, as different workloads insert different types of information into this property.

By default, the Search-UnifiedAuditLog cmdlet returns 100 audit records for any search request, unless you specify a different number of records to retrieve using the ResultSize parameter (up to a maximum of **5,000 records**).

A single search can process a maximum of **50,000 audit records** using page retrieval. Because the audit log can contain a large amount of data, it's important to be as specific as possible when using search parameters to avoid returning too many records.

To use the Search-UnifiedAuditLog cmdlet, your account must have the Exchange View-Only Audit Logs or Audit Logs role. These roles are part of the **Compliance Management and Organization Management role groups**, and can be assigned to other role groups as needed.

## RecordTypes

A RecordType is the workload that generated the record. Examples of different types of events found using the Search-UnifiedAuditLog cmdlet include:
1. Adding a member to a group in Azure Active Directory (**AzureActiveDIrectory**)
2. Updating mailbox properties in Exchange (**ExchangeAdmin**)
3. Deleting a file in SharePoint (**SharePointFileOperation**)
4. A user logging onto Microsoft Teams (**MicrosoftTeams**)
5. AIP Heartbeat (**AipHeartbeat**)

These events can be searched for and tracked using the Search-UnifiedAuditLog cmdlet, which allows you to filter and view the details of the events that are recorded in the unified audit log.

## Audit Data structure

```powershell
Search-UnifiedAuditLog -RecordType AipDiscover -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date)
```

Audit records consist of two parts:
General properties
AuditData property

### General properties

General properties that are populated in the same way by all workloads, and the AuditData property that contains workload-specific information. The general properties include the record type, creation date, operation, and user identifier. 

```
RunspaceId   : 136b901e-a6bc-4f24-bb58-5c435090df91
RecordType   : AipDiscover
CreationDate : 2022-09-15 5:49:22 PM
UserIds      : AdeleV@M365x23987777.OnMicrosoft.com
Operations   : Access
ResultIndex  : 8
ResultCount  : 8
Identity     : 20728aaf-1964-1a4a-bd72-784fa3c12132
IsValid      : True
ObjectState  : Unchanged
```

### AuditData property

Find the most important information about an event using AuditData property. Workloads use schemas to describe the properties they insert into audit records, and these schemas are used to help interpret the payload in audit events. It may require some trial and error to fully understand the information in an audit record. A guide to the detailed properties in audit log records can be helpful in this process. The details about the action taken by a user are stored in JavaScript Object Notation (JSON) format, so further processing is necessary to format the data to extract the information.

```json
AuditData:
{
  "SensitivityLabelEventData":{
    "SensitivityLabelId":"8581574a-c314-42e3-bfdc-a63cf96ed86e"
  },
  "SensitiveInfoTypeData":[
    
  ],
  "ProtectionEventData":{
    "IsProtected":false
  },
  "Common":{
    "ApplicationId":"c00e9d32-3c8d-4a7d-832b-029040e7db99",
    "ApplicationName":"Microsoft Azure Information Protection Word Add-In",
    "ProcessName":"WINWORD",
    "Platform":1,
    "DeviceName":"AdeleVanceWindo",
    "Location":"On-premises file shares",
    "ProductVersion":"2.13.49.0"
  },
  "DataState":"Use",
  "ObjectId":"C:\\452Documentcreated.docx",
  "UserId":"AdeleV@M365x23987777.OnMicrosoft.com",
  "ClientIP":"20.237.230.167",
  "Id":"20728aaf-1964-1a4a-bd72-784fa3c12132",
  "RecordType":93,
  "CreationTime":"2022-09-15T17:49:22",
  "Operation":"Access",
  "OrganizationId":"4b080626-0acc-4940-8af8-bfc836ff1a59",
  "UserType":0,
  "UserKey":"AdeleV@M365x23987777.OnMicrosoft.com",
  "Workload":"Aip",
  "Version":1,
  "Scope":1
}
```

The following table lists the AuditData property details.

|Parameter  |Description|
|----------|-----------|
| RecordType| The type of workload that generated the record, such as Azure Active Directory, Microsoft Teams, Exchange Admin, or SharePoint File Operation.|
| CreationTime| The date and time in UTC format when the activity was performed.|
| Operation| The operation logged when someone accesses an item, such as FileViewed in the case of SharePoint Online.|
| OrganizationId| The unique tenant identifier in the form of a GUID.|
| UserKey| The identity used to gain access to the item, typically achieved through membership in a group.|
| Workload| The name of the application that logged the event, such as SharePoint Online, Exchange Online, OneDrive for Business, or Azure AD.|
| ClientIP| The IP address of the client workstation where the action originated.|
| ObjectID| The full path to the object that was accessed.|
| UserID| The Azure AD account identifier for the account that caused the action to occur.|
| UserAgent| The client used to invoke the action.|
| SourceFileName| The name of the file involved in the action.|
| UserType| The type of user that performed the action, with values of "0" (regular user), "1" (administrator), or "2" (Microsoft data center administrator or system account).|
| EventSource| Only used by SharePoint Online, indicating whether the source was SharePoint or ObjectMode.|
| ResultIndex and ResultCount|The ResultIndex and ResultCount properties can be useful when working with large record sets. ResultIndex indicates the record number within the returned set, while ResultCount shows the total number of records returned. For example, if ResultIndex is 1 and ResultCount is 125, it means that the record shown is the first of 125 returned in the set.</br> If the search encounters an internal timeout, ResultIndex will be set to -1. As administrators gain experience with the audit log and the PowerShell cmdlets, they will discover that the audit log is a valuable source of information for understanding who interacted with a document or created new documents during a certain time period. It may take some trial and error to fully utilize the information provided in audit records.|

## Find the correct set events

To find the right events when searching the audit log, it's important to know what you're looking for and use the appropriate filters and parameters. This can be challenging because the audit log can contain a large number of events, and searching for specific actions can be like searching for a small object in a large and disordered list of data.

One way to approach this problem is to take steps to generate an audit event for the action you're interested in, wait for 60 minutes or so to allow the event to be ingested into the audit log, and then search for events within that time period.

This will give you a smaller set of events to work with, which you can then analyze and use to perform further searches. It's also a good idea to use the **Operations** / **RecordTypes** values logged for the events to help you refine your searches and find the specific events you're looking for.

Following is an example of filtering data by looking for very specific operations and in a given date range. View [Record Types](https://learn.microsoft.com/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) to see the record types that can be looked for.

The following script looks for a specific operation.

```powershell
$Operations = ("SensitivityLabelUpdated", "SensitivityLabelApplied", "FileSensitivityLabelApplied")
$StartDate = (Get-Date).AddDays(-90)
$EndDate = (Get-Date).AddDays(1)
[Array]$Records = Search-UnifiedAuditLog -StartDate $StartDate -EndDate $EndDate -Formatted -ResultSize 5000 -Operations $Operations
```

The following script searches for a specific Record Type. This example uses for AipDiscover.

```powershell
Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date) -RecordType AipDiscover -Operations Access -ResultSize 5 -Formatted | Format-Table UserIds, CreationDate, Operations
```

The following script stores the result in an array and searches for a specific value. This examples uses CreationDate.

```powershell
[array]$Records  = Search-UnifiedAuditLog -RecordType AipDiscover -StartDate (Get-Date).AddDays(-100) -EndDate (Get-Date)
$Records | Where-Object {$_.CreationDate -eq "2022-09-15 5:49:22 PM"}
```

## Manage large amounts of audit data

If you need to retrieve a large number of audit records from a large tenant, or if you need to search for multiple operations over an extended period, it's likely that a single search will return more than 5,000 records.

In this case, you can use the ReturnLargeSet and ReturnNextPreviewPage parameters to fetch audit data in pages of up to 5,000 records at a time. This technique allows you to retrieve up to 50,000 audit records. If you need to search for more than 50,000 records, you can split the work across multiple searches, each of which uses different criteria. You can then store the results of these searches in an external repository for later analysis.

To manage large volumes of data from the Search-UnifiedAuditLog cmdlet, you can use the ReturnLargeSet and ReturnNextPreviewPage parameters. These parameters allow you to perform searches that return large sets of results, and then retrieve the next page of results in subsequent searches.

1. Use the SessionId parameter to identify a search session and specify the number of pages you want to retrieve. This will allow the cmdlet to fetch multiple pages of data and return them to you.
2. The SessionId parameter is also used when you want to search for a large amount of audit data using the Search-UnifiedAuditLog cmdlet. The cmdlet will return a maximum of 5000 records per page, so if you want to search for more than that, you'll need to use the SessionId parameter to identify a search session and specify the number of pages you want to retrieve. The cmdlet will then use the session identifier to fetch the additional pages of data and return them to you.
3. If you need to search for more than 50,000 records, split the work across multiple searches and use different criteria for each search.
4. Store the results of the searches in an external repository, such as Azure log analytics, Azure Data Explorer, for easy access and analysis. 
5. Regularly review and update your search criteria and audit data management processes to ensure that you are capturing the right data and efficiently managing it. This will help you stay on top of any potential security issues and improve the overall security of your organization.

### Summary

To fetch a large amount of audit data, follow these steps:

1. Generate a session identifier.
2. Use a loop to retrieve data in multiple pages.
3. Repeatedly run the Search-UnifiedAuditLog command to fetch all the available data.
4. Save the data from each run of Search-UnifiedAuditLog.
5. After all the pages have been fetched, sort the data by date and export it to a CSV file.

### Examples

Search-UnifiedAuditLog has two parameters to support the retrieval of large data sets, SessionID and SessionCommand. Both SessionID and SessionCommand can be combined to process large datasets.
The **SessionId** parameter holds a string value to identify a search session. You can use any value you like from a simple number to a GUID generated with the New-Guid cmdlet. The presence of a session identifier tells Search-UnifiedAuditLog that it might need to fetch several pages of data.

```powershell
eg.
$SessionId = "5b5a5a5a-5b5b-5c5c-5d5d-5e5e5e5e5e5e"
$SessionId = "UnifiedAuditLogSearch 01/02/17"
```

The **SessionCommand** tells Search-UnifiedAuditLog how to handle large amounts of audit data. The returned data might contain duplicate records. This parameter can be set to:

* **ReturnLargeSet**: The audit records returned are unsorted. You can fetch up to 50,000 audit records using this method but must remember to sort the data once it is all fetched.
* **ReturnNextPreviewPage**: Search-UnifiedAuditLog returns audit records sorted by date. However, you can fetch only a maximum of 5,000 records using this method.  The maximum number of records returned through use of either paging or the ResultSize parameter is 5,000 records.

> [!NOTE]
> Always use the same SessionCommand value for a given SessionId value. Don't switch between ReturnLargeSet and ReturnNextPreviewPage for the same session ID. Otherwise, the output is limited to 10,000 results

#### SessionId sample script

In the following example, the script first imports the Exchange Online Management module and creates a remote connection to Exchange Online. It then sets the start and end dates for the search, and uses the Search-UnifiedAuditLog cmdlet to search the Unified Audit Log for entries within the specified date range.

The script then uses a foreach loop to scan through the array of records returned by the cmdlet, and processes each record as needed. In this case, it prints the operation name and user identity for each record.

```powershell
# Import the Exchange Online Management module
Import-Module ExchangeOnlineManagement
# Create a remote connection to Exchange Online
$UserCredential = Get-Credential
Connect-IPPSSession
# Search the Unified Audit Log for entries with the specified SessionID
$SessionId = "5b5a5a5a-5b5b-5c5c-5d5d-5e5e5e5e5e5e"
$StartDate = (Get-Date).AddDays(-90)
$EndDate = (Get-Date).AddDays(1)
[Array]$Records = Search-UnifiedAuditLog -StartDate $StartDate -EndDate $EndDate -SessionId $SessionId

# Scan through the array of records
foreach ($Record in $Records)
{
    # Process each record as needed
    # For example, you could print the operation name and user identity
    Write-Host "Operation: $($Record.RecordType)"
    Write-Host "User Identity: $($Record.Operations)"
    Write-Host "User Identity: $($Record.AuditData)"
}
```

#### ReturnLargeSet sample script

In the following example, the Search-UnifiedAuditLog cmdlet is used to search the audit log for entries between the specified start and end times. The ReturnLargeSet parameter is set by specifying the SessionId and SessionCommand parameters in the $parameters object. The results of the search are then looped through and each entry is output to the console.

**ReturnLargeSet**: Returns unsorted data. By using paging, you can access a maximum of 50,000 results. This is the recommended value if an ordered result is not required and has been optimized for search latency.

```powershell
# Set the start and end time for the audit log search
$startTime = "01/01/2022 00:00:00"
$endTime = "12/31/2022 23:59:59"

# Set the parameters for the search
$parameters = @{
    StartDate = $startTime
    EndDate = $endTime
    SessionId = "UnifiedAuditLogSearch 01/02/17"
    SessionCommand = "ReturnLargeSet"
}

# Perform the search and store the results in a variable
$results = Search-UnifiedAuditLog @parameters

# Loop through the results and output each entry
for ($i = 0; $i -lt $results.Count; $i++) {
    $entry = $results[$i]
    Write-Output $entry
}
```

#### ReturnNextPreviewPage sample script

The following script performs a search using the Search-UnifiedAuditLog cmdlet and the ReturnNextPreviewPage parameter. The search is performed using the specified start and end times. The results of the search are then output to the console.

```powershell
# Set the start and end time for the audit log search
$startTime = "01/01/2022 00:00:00"
$endTime = "12/31/2022 23:59:59"

 
# Set the parameters for search
$parameters = @{
    SessionId = "UnifiedAuditLogSearch 01/02/17"
    SessionCommand = "ReturnNextPreviewPage"
    StartDate = $startTime
    EndDate = $endTime
}

# Retrieve results
$resultpage = Search-UnifiedAuditLog @parameters
```
