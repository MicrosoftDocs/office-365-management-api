---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management APIs overview
title: Office 365 Management APIs overview
description: Provides a single extensibility platform for all Office 365 customers' and partners' management tasks, including service communications, security, compliance, reporting, and auditing.
ms.ContentId: 4fca85f9-efa6-3b4b-b10d-a07cb31f803f
ms.topic: reference (API)
ms.date: 09/28/2016
ms.localizationpriority: high
---

# Office 365 Management APIs overview

The Office 365 Management APIs provide a single extensibility platform for all Office 365 customers' and partners' management tasks, including service communications, security, compliance, reporting, and auditing. All of the Office 365 Management APIs are consistent in design and implementation with the current suite of Office 365 REST APIs, using common industry-standard approaches, including OAuth v2, OData v4, and JSON. Like the other Office 365 APIs, applications are registered in Azure Active Directory, giving developers a consistent way to authenticate and authorize their apps.

If you have questions about the Office 365 Management APIs, post your question on [Stack Overflow](http://stackoverflow.com/tags/office365), using the [office365] tag.

## Office 365 Service Communications API (preview)

The Office 365 Service Communications API has been released in preview mode. It replaces the Office 365 Service Communications API to provide service health information to tenant administrators and partners. Unlike the previous version, the new Service Communications API delivers a cohesive platform experience, with REST APIs built in a consistent fashion including URL naming, data format, and authentication.

New features are only added to the new version of the API, encouraging early adoption by legacy customers. When the General Announcement of Office 365 Service Communications API was made, the older version of the Service Communications API began a period of deprecation. 

For the operations reference, see [Office 365 Service Communications API reference (preview)](office-365-service-communications-api-reference.md).


## Office 365 Management Activity API

The Office 365 Management Activity API provides information about various user, admin, system, and policy actions and events from Office 365 and Azure Active Directory activity logs. Customers and partners can use this information to create new or enhance existing operations, security, and compliance-monitoring solutions for the enterprise. 

For the operations reference, see [Office 365 Management Activity API reference](office-365-management-activity-api-reference.md).

## See also

- [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md)
- [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md)
- [Troubleshooting the Office 365 Management Activity API](troubleshooting-the-office-365-management-activity-api.md)
- [Office 365 REST APIs](https://docs.microsoft.com/previous-versions/office/office-365-api/how-to/platform-development-overview)

