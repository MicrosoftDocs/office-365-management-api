---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management Activity API reference
title: Office 365 Management Activity API reference
description: Use the Office 365 Management Activity API to retrieve information about user, admin, system, and policy actions and events from Office 365 and Azure AD activity logs. 
ms.ContentId: 52749845-37f8-6076-7ea5-49d9a4055445
ms.topic: reference (API)
ms.date: 
localization_priority: Priority
---

# Office 365 Management Activity API reference

Use the Office 365 Management Activity API to retrieve information about user, admin, system, and policy actions and events from Office 365 and Azure AD activity logs. 

You can use the actions and events from the Office 365 and Microsoft Azure Active Directory audit and activity logs to create solutions that provide monitoring, analysis, and data visualization. These solutions give organizations greater visibility into actions taken on their content. These actions and events are also available in the Office 365 Activity Reports. For more information, see [Search the audit log in the Office 365 Security & Compliance Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c).

The Office 365 Management Activity API is a REST web service that you can use to develop solutions using any language and hosting environment that supports HTTPS and X.509 certificates. The API relies on Azure AD and the OAuth2 protocol for authentication and authorization. To access the API from your application, you'll need to first register it in Azure AD and configure it with appropriate permissions. This will enable your application to request the OAuth2 access tokens it needs to call the API. For more information, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).

For information about the data that the Office 365 Management Activity API returns, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).

> [!IMPORTANT]
> Before you can access data through the Office 365 Management Activity API, you must enable unified audit logging for your Office 365 organization. You do this by turning on the Office 365 audit log. For instructions, see [Turn Office 365 audit log search on or off](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).


## Working with the Office 365 Management Activity API

The Office 365 Management Activity API aggregates actions and events into tenant-specific content blobs, which are classified by the type and source of the content they contain. Currently, these content types are supported:

- Audit.AzureActiveDirectory
    
- Audit.Exchange
    
- Audit.SharePoint
    
- Audit.General (includes all other workloads not included in the previous content types)

- DLP.All (DLP events only for all workloads)
    
For details about the events and properties associated with these content types, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).

To begin retrieving content blobs for a tenant, you first a create subscription to the desired content types. If you are retrieving content blobs for multiple tenants, you create multiple subscriptions to each of the desired content types, one for each tenant.

After you create a subscription, you can poll regularly to discover new content blobs that are available for download, or you can register a webhook endpoint with the subscription and we will send notifications to this endpoint as new content blobs are available.


> [!NOTE] 
> When a subscription is created, it can take up to 12 hours for the first content blobs to become available for that subscription. The content blobs are created by collecting and aggregating actions and events across multiple servers and datacenters. As a result of this distributed process, the actions and events contained in the content blobs will not necessarily appear in the order in which they occurred. One content blob can contain actions and events that occurred prior to the actions and events contained in an earlier content blob. We are working to decrease the latency between the occurrence of actions and events and their availability within a content blob, but we can't guarantee that they appear sequentially.


> [!NOTE] 
> DLP sensitive data is only available in the activity feed API to users that have been granted “Read DLP sensitive data” permissions. For more on Data Loss Prevention (DLP) see [Overview of Data Loss Prevention Policies](https://support.office.com/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)

## Activity API operations

All API operations are scoped to a single tenant and the root URL of the API includes a tenant ID that specifies the tenant context. The tenant ID is a GUID. For information about how to get the GUID, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md). 

Because the notifications we send to your webhook include the tenant ID, you can use the same webhook to receive notifications for all tenants.

The URL for the API endpoint that you use is based on the type of Microsoft 365 or Office 365 subscription plan for your organization.

**Enterprise plan**

```http
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**GCC government plan**

```http
https://manage-gcc.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**GCC High government plan**

```http
https://manage.office365.us/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**DoD government plan**

```http
https://manage.protection.apps.mil/api/v1.0/{tenant_id}/activity/feed/{operation}
```

All API operations require an Authorization HTTP header with an access token obtained from Azure AD. The tenant ID in the access token must match the tenant ID in the root URL of the API and the access token must contain the ActivityFeed.Read claim (this corresponds to the permission [Read activity data for an organization] that you configured for you application in Azure AD).

```json
Authorization: Bearer eyJ0e...Qa6wg
```

The Activity API supports the following operations:

- **Start a subscription** to begin receiving notifications and retrieving activity data for a tenant.
    
- **Stop a subscription** to discontinue retrieving data for a tenant.
    
- **List current subscriptions**
    
- **List available content** and the corresponding content URLs.
    
- **Receiving notifications** sent by a webhook when new content is available.
    
- **Retrieving content** by using the content URL.
    
- **List notifications** sent by a webhook.

- **Retrieve resource friendly names** for objects in the data feed identified by guids.
    

## Start a subscription

This operation starts a subscription to the specified content type. If a subscription to the specified content type already exists, this operation is used to:

- Update the properties of an active webhook.
    
- Enable a webhook that was disabled because of excessive failed notifications.
    
- Re-enable an expired webhook by specifying a later or null expiration date.
    
- Remove a webhook.
    
||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/start?contentType={ContentType}`||
|**Parameters**|contentType|Must be a valid content type.|
||PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
|**Body**|webhook|Optional JSON object with three properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>address</b>: Required HTTPS endpoint that can receive notifications.  A test message will be sent to the webhook to validate the webhook before creating the subscription.</p></li><li><p><b>authId</b>: Optional string that will be included as the WebHook-AuthID header in notifications sent to the webhook as a means of identifying and authorizing the source of the request to the webhook.</p></li><li><p><b>expiration</b>: Optional datetime that indicates a datetime after which notifications should no longer be sent to the webhook.</p></li></ul>|
|**Response**|contentType|The content type specified in the call.|
||status|The status of the subscription. If a subscription is disabled, you will not be able to list or retrieve content.|
||webhook|The webhook properties specified in the call together with the status of the webhook. If the webhook is disabled, you will not receive notification, but you will still be able to list and retrieve content, provided the subscription is enabled.|

#### Sample request

```json
POST {root}/subscriptions/start?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Content-Type: application/json; utf-8
Authorization: Bearer eyJ0e...Qa6wg

{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }
}

```


#### Sample response

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "contentType": "Audit.SharePoint",
    "status": "enabled",
    "webhook": {
        "status": "enabled",
        "address":  "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": null
    }
}

```


## Webhook validation

When the /start operation is called and a webhook is specified, we will send a validation notification to the specified webhook address to validate that an active listener can accept and process notifications. If we do not receive an HTTP 200 OK response, the subscription will not be created. Or, if /start is being called to add a webhook to an existing subscription and a response of HTTP 200 OK is not received, the webhook will not be added and the subscription will remain unchanged.

#### Sample request

```json
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId, if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```


#### Sample response

```json

HTTP/1.1 200 OK

```


## Stop a subscription

This operation stops a subscription to the specified content type. 

When a subscription is stopped, you will no longer receive notifications and you will not be able to retrieve available content. If the subscription is later restarted, you will have access to new content from that point forward. You will not be able to retrieve content that was available between the time the subscription was stopped and restarted.


||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/stop?contentType={ContentType}`||
|**Parameters**|contentType|Must be a valid content type.|
||PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
|**Body**|(empty)||
|**Response**|(empty)|||

#### Sample request

```json
POST {root}/subscriptions/stop?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### Sample response

```json
HTTP/1.1 200 OK
```


## List current subscriptions

This operation returns a collection of the current subscriptions together with the associated webhooks.

||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/list`||
|**Parameters**|PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
|**Body**|(empty)||
|**Response**|JSON array|Each subscription will be represented by a JSON object with three properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>: Indicates the content type.</p></li><li><p><b>status</b>: Indicates the status of the subscription.</p></li><li><p><b>webhook</b>: Indicates the configured webhook, together with the status (enabled, disabled, expired) of the webhook.  If a subscription does not have a webhook, the webhook property will be present but with null value.</p></li></ul>|


#### Sample request

```json
GET {root}/subscriptions/list?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### Sample response

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType" : "Audit.SharePoint",
        "status": "enabled",
        "webhook": {
            "status": "enabled",
            "address": "https://webhook.myapp.com/o365/",
            "authId": "o365activityapinotification",
            "expiration": null
        }
    },

    ...

    {
        "contentType": "Audit.Exchange",
        "webhook": null
    }
]

```


## List available content

This operation lists the content currently available for retrieval for the specified content type. The content is an aggregation of actions and events harvested from multiple servers across multiple datacenters. The content will be listed in the order in which the aggregations become available, but the events and actions within the aggregations are not guaranteed to be sequential. An error is returned if the subscription status is disabled.


||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**Parameters**|contentType|Must be a valid content type.|
||PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
||startTime endTime|Optional datetimes (UTC) indicating the time range of content to return, based on when the content became available. The time range is inclusive with respect to startTime (startTime <= contentCreated) and exclusive with respect to endTime (contentCreated < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDTHH:MM:SS</p></li></ul>Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past. By default, if startTime and endTime are omitted, then the content available in the last 24 hours is returned.<p>**NOTE**: Even though it is possible to specify a startTime and endTime more than 24 hours apart, this is not recommended. Furthermore, if you do get any results in response to a request for more than 24 hours, these could be partial results and should not be taken into account. The request should be issued with an interval of no more than 24 hours between the startTime and endTime.</p>|
|**Response**|JSON array|The available content will be represented by JSON objects with the following properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>: Indicates the content type.</p></li><li><p><b>contentId</b>: An opaque string that uniquely identifies the content.</p></li><li><p><b>contentUri</b>: The URL to use when retrieving the content.</p></li><li><p><b>contentCreated</b>: The datetime when the content was made available.</p></li><li><p><b>contentExpiration</b>: The datetime after which the content will no longer be available for retrieval.</p></li></ul>|


#### Sample request

```json
GET {root}/subscriptions/content?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### Sample response

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```


### Pagination

When listing available content for a time range, the number of results returned is limited to prevent response timeouts. If there are more results in the specified time range than can be returned in single response, the results will be truncated and a header will be added to the response indicating the URL to use to retrieve the next page of results. The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page. If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUri** header is received.


## Receiving notifications

Notifications are sent to the configured webhook for a subscription as new content becomes available. Because the notification includes the tenant identifier, you can use the same webhook to receive notifications for all tenants for which you have subscriptions.

The notification is made as an HTTP POST over TLS (TLS 1.0 and later versions) to the specified webhook address. If the webhook configuration includes an auth ID, we will send it as an HTTP header: Webhook-AuthID. Any response other than HTTP 200 OK will be considered a failure and the notification will be retried. You can also configure your webhook to require client certificate-based authentication and we will authenticate using the manage.office.com certificate.

The body of the request will contain an array of one or more JSON objects that represent the available content blobs. The number of content blobs in each notification is limited to keep the size of the notification relatively small. Because this limit might change, your implementation should query for the length of the array instead of expecting a fixed size. Each object will include the same properties returned by the /content operation, together with the GUID of the tenant to which the data belongs and the GUID of your application that created the subscriptions. This allows the webhook to establish context when it is being used with multiple tenants and applications.

- **tenantId**: The GUID of the tenant to which the content belongs.
    
- **clientId**: The GUID of your application that created the subscription.
    
- **contentType**: Indicates the content type.
    
- **contentId**: An opaque string that uniquely identifies the content.
    
- **contentUri**: The URL to use when retrieving the content.
    
- **contentCreated**: The datetime when the content was made available.
    
- **contentExpiration**: The datetime after which the content will no longer be available for retrieval.
    
The following is an example of a notification.

```json
POST https://webhook.myapp.com/o365/ 
Content-Type: application/json; utf-8
Webhook-AuthID: o365activityapinotification

[
    {
        "tenantId": "{GUID}",
        "clientId": "{GUID}",
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```


## Notification failure and retry

The notification system sends notifications as new content becomes available. If we encounter excessive failures when sending notifications, our retry mechanism will exponentially increase the time between retries. If we continue to encounter failures, we reserve the right to disable the webhook and stop sending notifications to it altogether. The /start operation can be used to re-enable a disabled webhook.


## Retrieving content

To retrieve a content blob, make a GET request against the corresponding content URI that is included in the list of available content and in the notifications sent to a webhook. The returned content will be a collection of one more actions or events in JSON format.

#### Sample request

```json
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

#### Sample response

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "CreationTime": "2015-06-29T20:03:19",
        "Id": "80c76bd2-9d81-4c57-a97a-accfc3443dca",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "failed",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "ExtendedProperties": [
            {
                "Name": "LoginError",
                "Value": "-2147217390;PP_E_BAD_PASSWORD;The entered and stored passwords do not match."
            }
        ],
        "Client": "Exchange",
        "LoginStatus": -2147217390,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:03:34",
        "Id": "4e655d3f-35fa-42e0-b050-264b2d255c7a",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "success",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Client": "Exchange",
        "LoginStatus": 0,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:04:55",
        "Id": "b567caf0-088e-4c1c-a4ea-633a1e3d66c8",
        "Operation": "Add User.",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 8,
        "ResultStatus": "success",
        "UserKey": "1003BFFD8EC47CA6@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ObjectId": "user001@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Actor": [
            {
                "ID": "1cef1fdb-ff52-48c4-8e4e-dfb5ea83d357",
                "Type": 2
            },
            {
                "ID": "admin@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "1003BFFD8EC47CA6",
                "Type": 3
            }
        ],
        "ActorContextId": "41463f53-8812-40f4-890f-865bf6e35190",
        "InterSystemsId": "c2ced078-ad57-4079-a743-5c37f5284790",
        "IntraSystemId": "d1497f7e-15b4-49aa-83ad-11a17ca4a2f4",
        "Target": [
            {
                "ID": "user001@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "10037FFE91510806",
                "Type": 3
            }
        ],
        "TargetContextId": "41463f53-8812-40f4-890f-865bf6e35190"
    }
]

```


## List notifications

This operation lists all notification attempts for the specified content type. If you did not include a webhook when starting the subscription to the content type, there will be no notifications to retrieve. Because we retry notifications in the event of failure, this operation can return multiple notifications for the same content, and the order in which the notifications are sent will not necessarily match the order in which the content became available (especially when there are failures and retries). 

You can use this operation to help investigate issues related to webhooks and notifications, but you should not use it to determine what content is currently available for retrieval. Use the /content operation instead. We return an error if the subscription status is disabled.


||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**Parameters**|contentType|Must be a valid content type.|
||PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
||startTime endTime|Optional datetimes (UTC) that indicate the time range of content to return, based on when the content became available. The time range is inclusive with respect to  _startTime_ ( _startTime_ <= contentCreated) and exclusive with respect to _endTime_ (_contentCreated_ < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDTHH:MM:SS</p></li></ul>Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past. By default, if  _startTime_ and _endTime_ are omitted, the content available in the last 24 hours is returned.|
|**Response**|JSON array|The notifications will be represented by JSON objects with the following properties: <ul><li>**contentType**: indicates the content type.</li><li>**contentId**: an opaque string that uniquely identifies the content.</li><li>**contentUri**: the URL to use when retrieving the content. </li><li>**contentCreated**: the datetime when the content was made available.</li><li>**contentExpiration**: the datetime after which the content will no longer be available for retrieval.</li><li>**notificationSent**: the datetime when the notification was sent.</li><li>**notificationStatus**: indicates the success or failure of the notification attempt.</li></ul>|

#### Sample request

```json
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### Sample response

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z",
        "notificationSent": "2015-05-23T17:36:00.000Z",
        "notificationStatus": "success"

    },
    ...
]

```


### Pagination

When listing notification history for a time range, the number of results returned is limited to prevent response timeouts. If there are more results in the specified time range than can be returned in a single response, the results are truncated and a header is added to the response indicating the URL to use to retrieve the next page of results. The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page. If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUri** header is received.

## Retrieve resource friendly names

This operation retrieves friendly names for objects in the data feed identified by guids. Currently "DlpSensitiveType" is the only supported object. 


||Subscription|Description|
|:-----|:-----|:-----|
|**Path**| `/resources/dlpSensitiveTypes`||
|**Parameters**|PublisherIdentifier|The tenant GUID of the vendor coding against the API. This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code. This parameter is used for throttling the request rate. Make sure this parameter is specified in all issued requests to get a dedicated quota. All requests received without this parameter will share the same quota.|
|**Headers**|Accept-Language|Header to specify the desired language for localized names. For example, use "en-US" for English or "es" for Spanish. The default language (en-US) will be returned if this header is not present.|
|**Body**|(empty)||
|**Response**|JSON array|The available content will be represented by JSON objects with the following properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>id</b>: Indicates the guid of the sensitive information type.</p></li><li><p><b>name</b>: The friendly name of the sensitive information type.</p></li></ul>|

#### Sample request

```json
GET {root}/resources/dlpSensitiveTypes?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

#### Sample response

```json
HTTP/1.1 200 OK

[
    {
        "id": "50842eb7-edc8-4019-85dd-5a5c1f2bb085",
        "name": "CreditCardNumber"
    }, 
    {
        "id": "0e9b3178-9678-47dd-a509-37222ca96b42",
        "name": "EUDebitCardNumber"
    }, 
    ...
    {
    }
]

```

## API throttling

Organizations that access auditing logs through the Office 365 Management Activity API were restricted by throttling limits at the publisher level. This means that for a publisher pulling data on behalf of multiple customers, the limit was shared by all those customers.

We're moving from a publisher-level limit to a tenant-level limit. The result is that each organization will get their own fully allocated bandwidth quota to access their auditing data. All organizations are initially allocated a baseline of 2,000 requests per minute. This is not a static, predefined limit but is modeled on a combination of factors including the number of seats in the organization and that Office 365 and Microsoft 365 E5 organizations will get approximately twice as much bandwidth as non-E5 organizations. There will also be cap on the maximum bandwidth to protect the health of the service.

For more information, see the "High-bandwidth access to the Office 365 Management Activity API" section in [Advanced audit in Microsoft 365](https://docs.microsoft.com/microsoft-365/compliance/advanced-audit#high-bandwidth-access-to-the-office-365-management-activity-api).

> [!NOTE] 
> Even though each tenant can initially submit up to 2,000 requests per minute, Microsoft cannot guarantee a response rate. The response rate depends on various factors, such as client system performance, network capacity, and network speed. 

## Errors

When the service encounters an error, it will report the error response code to the caller, using standard HTTP error-code syntax. . Additional information is included in the body of the failed call as a single JSON object. An example of a full JSON error body is shown below: 

```json

{ 
    "error":{ 
        "code":"AF50000",
        "message": "An internal server error occurred. Retry the request."
    } 
}

```


|||
|:-----|:-----|
|Code|Message|
|AF10001|The permission set ({0}) sent in the request did not include the expected permission **ActivityFeed.Read**.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the permission set in the access token.</p></li></ul>|
|AF20001|Missing parameter: {0}. <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the name of the missing parameter.</p></li></ul>|
|AF20002|Invalid parameter type: {0}. Expected type: {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the name of the invalid parameter.</p></li><li><p>{1} = the expected type (int, datetime, guid).</p></li></ul>|
|AF20003|Expiration {0} provided is set to past date and time.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the expiration passed in the API call.</p></li></ul>|
|AF20010|The tenant ID passed in the URL ({0}) does not match the tenant ID passed in the access token ({1}).<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = tenant ID passed in the URL</p></li><li><p>{1} = tenant ID passed in the access token</p></li></ul>|
|AF20011|Specified tenant ID ({0}) does not exist in the system or has been deleted. <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>	{0} = tenant ID passed in the URL</p></li></ul>|
|AF20012|Specified tenant ID ({0}) is incorrectly configured in the system. <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>	{0} = tenant ID passed in the URL</p></li></ul>|
|AF20013|The tenant ID passed in the URL ({0}) is not a valid GUID.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>	{0} = tenant ID passed in the URL</p></li></ul>|
|AF20020|The specified content type is not valid.|
|AF20021|The webhook endpoint {{0}) could not be validated. {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = webhook address.</p></li><li><p>{1} = "The endpoint did not return HTTP 200." or "The address must begin with HTTPS."</p></li></ul>|
|AF20022|No subscription found for the specified content type.|
|AF20023|The subscription was disabled by {0}.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = "a tenant admin" or "a service admin"</p></li></ul>|
|AF20030|Start time and end time must both be specified (or both omitted) and must be less than or equal to 24 hours apart, with the start time no more than 7 days in the past.|
|AF20031|Invalid nextPage Input: {0}.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the next page indicator passed in the URL</p></li></ul>|
|AF20050|The specified content ({0}) does not exist.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = resource id or resource URL</p></li></ul>|
|AF20051|Content requested with the key {0} has already expired. Content older than 7 days cannot be retrieved.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>•	{0} = resource id or resource URL</p></li></ul>|
|AF20052|Content ID {0} in the URL is invalid.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = resource id or resource URL</p></li></ul>|
|AF20053|Only one language may be present in the Accept-Language header.|
|AF20054|Invalid syntax in Accept-Language header.|
|AF429|Too many requests. Method={0}, PublisherId={1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = HTTP Method</p></li><li><p>{1} = Tenant GUID used as PublisherIdentifier</p></li></ul>|
|AF50000|An internal error occurred. Retry the request.|
