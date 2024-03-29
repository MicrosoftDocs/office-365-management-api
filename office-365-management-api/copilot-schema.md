---
ms.TocTitle: CopilotInteraction
title: CopilotInteraction
description: Copilot interaction events
ms.ContentId: 5fc9eee3-9f95-4c8f-915a-672ae5e1c9a8
ms.topic: reference
ms.date: 11/09/2023
ms.localizationpriority: high
---


# Copilot interaction events overview

For [auditing](/purview/audit-solutions-overview), details are [captured](/purview/audit-log-activities#copilot-activities) when users interact with Copilot. Events include how and when users interact with Copilot, in which Microsoft 365 service the activity took place, and references to the files stored in Microsoft 365 that were accessed during the interaction. If these files have a sensitivity label applied, that's also captured.  

Copilot events can be accessed in the **Audit** solution from the Microsoft Purview compliance portal. To search for copilot events, select **Copilot activities** and **Interacted with Copilot**. You can also select **Copilot** as a workload. More information on searching the audit log can be found in [Audit New Search](/purview/audit-new-search).  

![audit search copilot interaction dialog box](images/audit-search-copilot-interaction.png)

## Schema example

```xml

<edmx:DataServices>
    <Schema Namespace="Microsoft.Office.Audit.Schema.CopilotInteraction" xmlns="http://docs.oasis-open.org/odata/ns/edm">
        <ComplexType Name="SensitivityLabelIdData">
            <Property Name="Id" Type="Edm.String">
                <Annotation Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"/>
            </Property>
            <Property Name="Type" Type="Edm.String"/>
            <Property Name="Name" Type="Edm.String">
                <Annotation Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"/>
            </Property>
            <Property Name="SensitivityLabelId" Type="Edm.String"/>
        </ComplexType>

        <ComplexType Name="CopilotId">
            <Property Name="Id" Type="Edm.String">
                <Annotation Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"/>
            </Property>
            <Property Name="Type" Type="Edm.String"/>
        </ComplexType>

        <ComplexType Name="EventData">
            <Property Name="AppHost" Type="Edm.String" />
            <Property Name="Contexts" Type="Collection(Self.CopilotId)" />
            <Property Name="ThreadId" Type="Edm.String" />
            <Property Name="MessageIds" Type="Collection(Edm.String)" />
            <Property Name="AccessedResources" Type="Collection(Self.SensitivityLabelIdData)" />
        </ComplexType>

        <EntityType Name="CopilotInteractionAuditRecord" BaseType="AuditRecord" >
            <Annotation Term="Microsoft.Office.Audit.Schema.WorkloadType" EnumMember="Microsoft.Office.Audit.Schema.WorkloadType/Copilot"/>
            <Property Name="CopilotEventData" Type="Self.EventData" />
        </EntityType>
    </Schema>
</edmx:DataServices>
```

## Audit log examples of CopilotInteraction 

Copilot Interaction logs provide information regarding the RecordID, CreationDate, RecordType, Operation, UserID, and the Audit Data field. The Audit Data field will include any additional information based on the context of user interaction with Copilot, which can include the AppHost, Contexts, ThreadID, MessageIDs, and AccessedResources. More information regarding each of these copilot schema properties can be found in the Audit copilot schema definition section. 

Example 1: Copilot Interaction Experience within M365 Apps
- RecordID: 99b0a960-13a0-461f-8c5c-cb2316ea273d
- CreationDate: 12/13/2023 17:12
- RecordType: 261
- Operation: CopilotInteraction
- UserID: admin@MODERNCOMMS975184.onmicrosoft.com
- AuditData: {"CreationTime":"2023-12-13T17:12:36","Id":"99b0a960-13a0-461f-8c5c-cb2316ea273d","Operation":"CopilotInteraction","OrganizationId":"408e31c8-8f05-410a-9221-84335f0ce512","RecordType":261,"UserKey":"12d24f71-64c7-49b3-821f-f8884f3f373e","UserType":0,"Version":1,"Workload":"Copilot","ClientIP":"::ffff:10.11.0.229","UserId":"admin@MODERNCOMMS975184.onmicrosoft.com","CopilotEventData":{"AccessedResources":[],"AppHost":"appchat","Contexts":[{"Id":"https:\/\/moderncomms975184-my.sharepoint.com\/personal\/admin_moderncomms975184_onmicrosoft_com\/_layouts\/15\/Doc.aspx?sourcedoc=%7B9FDE1491-B079-4180-9B1E-B87F4ACA19F7%7D&file=AboutElephants.docx&action=default&mobileredirect=true","Type":"docx"}],"MessageIds":[],"ThreadId":"19:lgojcxwbvhJnfU3IhUJW5M-nSX2U7tjccgSrtYAoG341@thread.v2"}}

Example 2: Copilot Interaction within M365 Chat Experience 
- RecordID: 537312b6-dce7-4d9b-8b12-58283204b720
- CreationDate: 12/14/2023 2:11	
- RecordType: 261
- Operation: CopilotInteraction
- UserID: admin@MODERNCOMMS975184.onmicrosoft.com
- AuditData: {"CreationTime":"2023-12-14T02:11:55","Id":"537312b6-dce7-4d9b-8b12-58283204b720","Operation":"CopilotInteraction","OrganizationId":"408e31c8-8f05-410a-9221-84335f0ce512","RecordType":261,"UserKey":"12d24f71-64c7-49b3-821f-f8884f3f373e","UserType":0,"Version":1,"Workload":"Copilot","ClientIP"2401:4900:60d2:179b:c004:b6f:b678:b148","UserId":"admin@MODERNCOMMS975184.onmicrosoft.com","CopilotEventData":{"AccessedResources":[],"AppHost":"bizchat","Contexts":[],"MessageIds":[],"ThreadId":"19:Xn3uQZYgZ7f2ue0vp5w9MglEVjFyp5pza1efaC6g2U41@thread.v2"}}
 
## Audit copilot schema definitions

|Attribute |Definition  |
|----------|------------|
|*AppHost*       |The type of Copilot used during the interaction. <br>For example, bizchat refers to the M365 Chat experience on Bing and Teams. App chat refers to the copilot experiences on apps like Excel, PowerPoint, Teams, and more.<br> Example: "AppHost": "appchat", “AppHost”: “bizchat”   |
|*Contexts*     |Context contains a collection of attributes within AppChat around the user interaction to help describe where the user was during the copilot interaction. ID is  identifier of the resource that was being used during the copilot interaction. Type is the name of the app or service within context.  <br>Example: Some examples of supported apps and services include M365 Office (docx, pptx, xlsx), TeamsMeeting, TeamsChannel, and TeamsChat. If Copilot is used in Excel, then context will be the identifier of the Excel Spreadsheet and the file type. |
|*ThreadId*               |The ID of the copilot and user interaction thread.  |
|*MessageIds*            |This is currently reserved with Microsoft Internal. |
|*AccessedResources*     |References to all the files and documents Copilot used in M365 services like OneDrive and SharePoint Online to respond to the user’s request.   |

