---
ms.TocTitle: Troubleshooting the Office 365 Management Activity API
title: Troubleshooting the Office 365 Management Activity API
description: Summarizes the most common questions Microsoft Support receives in supporting this API.
ms.ContentId: 50822603-a1ec-a754-e7dc-67afe36bb1b0
ms.topic: reference (API)
ms.date: 09/05/2018
localization_priority: Priority
---

# Troubleshooting the Office 365 Management Activity API

The Office 365 Management Activity API (also known as the *Unified Auditing API*) is just one part of Office 365 security and compliance offerings, but it gets a lot of attention because it:

- Allows programmatic access to multiple auditing pipeline workloads (such as SharePoint and Exchange)
- Is the main interface used by a variety of third-party vendor products to aggregate and index auditing data

The Management Activity API shouldn’t be confused with the Office 365 Service Communications API. The Management Activity API is for auditing end user activities in the various workloads.  The Service Communications API is for auditing status and messages that are sent by the services that are available in Office 365 (such as Dynamics CRM or Identity Service).

Despite having a relatively few operations and a simple REST interface, there’s a lot of confusion about how to use the Management Activity API and the details of exactly how the data is retrieved.  One thing that should be made clear for anyone who’s getting started with the Management Activity API is that there is no concept of querying by event specifics, such as date that the event occurred, which site collection an event might have been fired from, or the type of event.  Instead, you create subscriptions to specific workloads (for example, SharePoint or Azure AD) and each subscription is per tenant.

This article summarizes the most common questions Microsoft Support receives in supporting this API.  We’ll show a selection of simple PowerShell scripts that can help you answer the most common questions asked by customers or get you started implementing a custom solution by demonstrating the main operations.  Not all the operations are explained in this article, but they are all listed in [Office 365 Management Activity API reference](office-365-management-activity-api-reference.md).

## Enabling unified audit logging in Office 365

If you've just set up an app that's trying to use the Management Activity API and it's not working, be sure that you've enabled unified audit logging for your Office 365 organization. You do this by turning on the Office 365 audit log. For instructions, see [Turn Office 365 audit log search on or off](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).

## Questions about third-party tools and clients

The most common category of questions we’re currently fielding in support come from customers using third-party products to download and aggregate auditing data. Depending on the third-party product, customers may encounter difficulty with the setup or experience an interruption or an inconsistency in the data exposed in those products. Here it should be stated that the first action such customers should take is to contact their vendor’s support organization. In all the service requests that have come to Support, engineers have seen only a single case where a tenant-specific service problem was the cause.

Nevertheless, these customers may still have some unanswered questions. Their vendors may be insisting that it is a service issue, or they may just want to do some initial checking before contacting their vendor. 

## Connecting to the API

Most applications connect to the API using a straightforward Client Credentials OAuth2 flow. Therefore, the first step is to create an Azure AD application that has the permissions needed to access the Management Activity API data. It’s outside the scope of this article to explain the steps to create an Azure AD App registration. For more information, see [Register your application with your Azure Active Directory tenant](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).

### Azure application permissions

The three permissions currently used for the Office 365 Management Activity API are:
1. Read activity data for your organization
2. Read service health information for your organization
3. Read Data Loss Prevention (DLP) policy events, including detected sensitive information 

> [!NOTE] 
> You should enable both the Application Permissions and the Delegated Permissions for at least the first two of the above permission sets. Read DLP policy events will only be necessary if you are interested in the DLP workloads.

### Getting an access token

The following PowerShell script uses the App ID and a Client Secret to obtain the OAuth2 token from the Management Activity API authentication endpoint. It then places the access token into the `$headerParams` array variable, which you’ll attach to your HTTP request: 

```powershell
# Create app of type Web app / API in Azure AD, generate a Client Secret, and update the client id and client secret here
$ClientID = "<YOUR_APPLICATION_ID"
$ClientSecret = "<YOUR_CLIENT_SECRET>"
$loginURL = "https://login.microsoftonline.com/"
$tenantdomain = "<YOUR_DOMAIN>.onmicrosoft.com"
# Get the tenant GUID from Properties | Directory ID under the Azure Active Directory section
$TenantGUID = "<YOUR_TENANT_GUID>"
$resource = "https://manage.office.com"
# auth
$body = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 
```

The `$oauth` variable will contain the response object, which has several properties including the access token. Here’s an example of a response:

```json
token_type     : Bearer
expires_in     : 3599
ext_expires_in : 0
expires_on     : 1508285860
not_before     : 1508281960
resource       : https://manage.office.com
access_token   : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyIsImtpZCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyJ9.eyJhdWQiOiJodHRwczov…
```

## Checking your subscriptions

If you’ve experienced an interruption in data flowing to an existing Management Activity API client or solution, you might wonder if something happened to your subscription. To check your active subscriptions, add the following to the previous script:

```powershell
Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" 
```

#### Sample response 

```json
StatusCode        : 200
Status Description : OK
Content           : [{"contentType":"Audit.Exchange","status":"enabled","webhook":null},{"contentType":"Audit.SharePoint","status":"enabled","webhook":{"authId":"","address":"https://mvcwebapiwebhookreceiver.azurewebsite...
RawContent        : HTTP/1.1 200 OK
                    Pragma: no-cache
                    Content-Length: 266
                    Cache-Control: no-cache
                    Content-Type: application/json; charset=utf-8
                    Expires: -1
                    Server: Microsoft-IIS/8.5,Microsoft-IIS/8.5
                    X-AspNet-Versi...
Forms             : {}
Headers           : {[Pragma, no-cache], [Content-Length, 266], [Cache-Control, no-cache], [Content-Type, application/json; charset=utf-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 266
```

This says that the tenant has both Audit.Exchange and Audit.SharePoint subscriptions enabled. The Exchange subscription has no webhook enabled (null) and the SharePoint subscription has a webhook enabled with the address of the registered endpoint shown.

## Creating a new subscription

To create a new subscription, you use the /start operation:

```powershell
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"
```

> [!NOTE] 
> Remember that `$headerParams` was populated in the first part of the script listed in the [Connecting to the API](#connecting-to-the-api) section in this article.

The previous code will create a new subscription to the Audit.AzureActiveDirectory content type, with a webhook that is null. You can then check your subscriptions using the code in the [Checking your subscriptions](#checking-your-subscriptions) section in this article.

## Checking content availability

To check what content blobs were created during a certain period, you can add the following line to the script in the "Connecting to the API" section:

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint"
```

The previous example will get all the content notifications that became available today, which means from 12:00 AM UTC to the current time. If you want to specify a different time period (keeping in mind that the maximum period for which you can query is 24 hours), add the *starttime* and *endtime* parameters to the URI; for example:

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint&startTime=2017-10-13T00:00&endTime=2017-10-13T11:59"
```

> [!NOTE] 
> You must use either both *starttime* and *endtime* parameters or neither.

The previous request will return a JSON object with a collection of notifications that became available during the time period you specify. The response will look like this:

```json
[{		"contentUri" : "https://manage.office.com/api/v1.0/<<your_tenant_guid>>/activity/feed/audit/20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
		"contentId" : "20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
		"contentType" : "Audit.SharePoint",
		"contentCreated" : "2017-10-13T18:00:51.748Z",
		"contentExpiration" : "2017-10-20T18:00:51.748Z",
}]
```

> [!IMPORTANT] 
> - The *contentUri* property is the URI from which you can retrieve the content blob. The blob itself is what contains the event details; it will contain details about 1 – N events. While there may be 30 JSON objects in the collection, there may be many more events detailed in those 30 content URIs.
> - The *contentCreated* property is not the date that the event being notified was created. This is the date the notification was created. The events detailed in that blob may have been created well before the content blob was created. Therefore, you can never query the API directly for events that occurred within any given period.

### Paging contents for busy tenants

Many larger Office 365 tenants have thousands of events being generated every hour. If this is the case with your organization, and you try to execute a query for a 24-hour period like in the example above, you may need to retrieve more notifications than can be returned in one response. In this case, you’ll need to implement a logical loop of some kind, each time checking the response headers for the **NextPageUrl:** header value. See the [Pagination](office-365-management-activity-api-reference.md#pagination) section in the Office 365 Management Activity API reference for more details. 

The logic for this loop will be something like this:

```powershell
IF the NextPageUrl header is present in the response... 

THEN issue a new request substituting the Uri parameter in the above Invoke-WebRequest example for the value of the NextPageUrl header...
 
ELSE exit the loop
```

It will be difficult to test this looping code unless you have a very active tenant. In our testing, we tried to execute several thousand update operations in a script and was unable to generate a large enough number of notifications to require the **NextPageUrl** header to be sent.

## Using webhooks

There two ways to get a notification that content blobs have been created. The *push* approach is implemented with a webhook endpoint, which is a web application that you create and host yourself or on a cloud platform. You register the webhook at the time you create a subscription to an audited content type. You may also add a webhook registration to an existing subscription using the approach shown below. The *pull* approach requires you to query for a particular timespan (no more than 24 hours) using the [/content operation](office-365-management-activity-api-reference.md#list-available-content). The response will tell you which content blobs were created during the period specified.

Before you add a webhook, be aware of the following two issues:

1. Webhooks are being de-emphasized by Microsoft because of the difficulty in debugging and troubleshooting. Typically, you would host a WebApi endpoint that responds to incoming requests. Many customers either use hosting environments (which they don’t have full control over) or on-premises environments (that have difficulty allowing incoming HTTP requests). Support has seen many problems where the incoming requests from the Office 365 auditing pipeline have been blocked by a firewall or router. In this case, the API will implement a back-off algorithm, which can be confusing and may result in disabling the webhook in the subscription.

2. The webhook must be ready to immediately respond to a validation request after the start operation is executed. If there is a bug in the webhook application, the validation will fail, and the webhook will not be enabled. For information about the validation request schema, see the [Webhook validation](office-365-management-activity-api-reference.md#webhook-validation) section in the Office 365 Management Activity API reference. You should seriously consider the challenges in producing a production-ready webhook to respond to Management Activity API notifications.

If you decide to implement a webhook, it should be tested to verify that the endpoint is prepared to respond with an HTTP 200 response to both the validation request and the normal notification requests before any /start operation that specifies a webhook endpoint is called for the first time. Typically, you would use a mocked request from the API to test this readiness. Carefully read and observe both the [Webhook validation](office-365-management-activity-api-reference.md#webhook-validation) and [Retrieving content](office-365-management-activity-api-reference.md#retrieving-content) sections in the Office 365 Management Activity API reference to understand the schema of these two types of requests.

To add a subscription with a webhook endpoint, add a body parameter to the POST to the /start endpoint; for example:

```json
$body = '{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }}'
$uri = "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed//subscriptions/start?contentType=Audit.SharePoint"
Invoke-RestMethod -Method Post -uri $uri -Headers $headerParams -Body $body
```

Immediately following this call, a validation request will be sent out to `https://webhook.myapp.com/o365/ …` and there should be a listener ready to respond, as per the description in the [Webhook validation](office-365-management-activity-api-reference.md#webhook-validation) section in the Office 365 Management Activity API reference. Your listener must respond with HTTP 200. If you immediately run the /list operation at this point, you will not see the webhook shown as other than null until the validation has returned successfully.

### Checking notifications to webhooks

It’s important to distinguish between the /notifications operation and the /content operation. Checking notifications is only relevant if you have subscribed with a webhook endpoint. This operation will let you know if attempts to send notifications to your webhook have been successful or not. This operation should not be used to list available content. To cross-check the availability of content against what you may have already received in your webhook, you would use the /content operation. But be sure to first check for failed notifications using the [/notifications operation](office-365-management-activity-api-reference.md#list-notifications).

## Requesting content blobs and throttling

After you’ve obtained a list of content URIs, you must request the blobs specified by the URIs. The following is an example of requesting a content blob using PowerShell. This example assumes you have already used the previous example in the [Getting an access token](#getting-an-access-token) section in this article to get an access token and have populated the `$headerParams` variable appropriately.

```powershell
# Get a content blob
$uri = 'https://manage.office.com/api/v1.0/<<your-tenant-guid>>/activity/feed/audit/<<ContentID>$audit_sharepoint$Audit_SharePoint'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

Note the following things about the *$uri* variable in the previous example:

- We used single quotes so that the *$* symbols aren’t interpreted as variables in PowerShell
- The entire URI will be returned in the *contentUri* property of the response to your previous call to the /content endpoint. The placeholder tokens shown are just for illustration.

When attempting to retrieve the available content blobs, many customers (mostly working with busy tenants) have experienced errors that look like this:

```json
Response Code 403: {'error':{'code':'AF429','message':'Too many requests. Method=GetBlob, PublisherId=00000000-0000-0000-0000-000000000000'}}
```

This is likely due to throttling. Note that the value of the PublisherId parameter likely indicates that the client didn’t specify the *PublisherIdentifier* in the request. Also, keep in mind that the correct parameter name is *PublisherIdentifier* even though you see *PublisherId* listed in the 403 error responses.

> [!NOTE] 
> In the API reference, the *PublisherIdentifier* parameter is listed in every operation of the API, but it should also be included in the GET request to the contentUri URL when retrieving the content blob.

If you’re doing simple API calls to troubleshoot problems (for example, checking if a given subscription is active) you can safely omit the *PublisherIdentifier* parameter, but absolutely any code that is eventually meant for production use should include the *PublisherIdentifier* parameter on every call.

If you’re implementing a client for your company’s tenant, the *PublisherIdentifier* is the Tenant GUID. If you are creating an ISV application or add-in for multiple customers, the *PublisherIdentifier* should be the ISV’s Tenant GUID, and not the tenant GUID of end user’s company.

If you include the valid *PublisherIdentifier*, then you will be in a pool that is allotted 60K requests per minute per tenant. This is an exceptionally large number of requests. However, if you don’t include the *PublisherIdentifier* parameter, you will be in the general pool allotted 60K requests per minute for all tenants. In this case, you will more than likely find that your calls are getting throttled. To prevent this, here’s how you would request a content blob using the *PublisherIdentifier*:

```powershell
$contentUri = ($response.Content | ConvertFrom-Json).contentUri[0]
$uri = $contentUri + '?PublisherIdentifier=82b24b6d-0591-4604-827b-705d55d0992f'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

The previous example assumes that the *$response* variable was populated with the response to a request to the /content endpoint and that the *$headerParams* variable includes a valid access token. The script grabs the first item in the array of content URIs from the response and then invokes the GET to download that blob and put it in the *$contents* variable. Your code will likely loop through the contentUri collection, issuing the GET for each *contentUri*.

## Frequently asked questions about the Office 365 Management API

#### What is the maximum time I will have to wait before a notification is sent about a given Office 365 event?

There is no guaranteed maximum latency for notification delivery (in other words, no SLA). Microsoft Support’s experience has been that most notifications are sent within one hour of the event. Often the latency is much shorter, but often it’s longer as well. This varies somewhat from workload to workload, but a general rule is that most notifications will be delivered within 24 hours of the originating event.

#### Aren’t webhook notifications more immediate? After all, aren’t they are event-driven?

No. Webhook notifications are not event-driven in the sense that the event triggers the notification. The content blob still must be created and creating the content blob is what triggers the notification delivery. Recently, there have been longer wait times for notifications when using a webhook compared to querying the API directly with the /content operation. Therefore, the Management Activity API shouldn’t be thought of as a real-time security alert system. Microsoft has other products for that. As far as security is concerned, Management Activity API event notifications can more appropriately be used to determine use patterns over extended periods of time. 

#### Can I query the Management Activity API for a particular event ID or RecordType or other properties in the content blob?

No. Don’t think of the data available through the Management Activity API as being a “log” in the traditional sense. Rather, think of it as a dump of event details. It’s up to you to gather all those event details, store and index them locally, and then implement your own query logic, by using either a custom application or a third-party tool.

#### How do I know the data coming from my existing auditing solution, which collects data from the Management Activity API, is accurate and complete?

The short answer is that Microsoft doesn’t provide any kind of a log that will allow you to cross-check any given application or third-party (ISV) application. There are other Microsoft security products that draw their data from the same pipeline, but those products fall outside the scope of this discussion and  can’t be used to directly cross-check the Management Activity API. If you’re concerned about discrepancies between what your existing solution is providing and what you expect, you should implement the operations illustrated above. But this can be difficult, depending on how your existing tool or solution lists and indexes data. If your existing solution only presents data sorted by the creation time of the actual event, there’s no way to query the API by event creation time so that you could compare result sets. In this scenario, you’d have to collect the notified content blobs for several days, index or sort them manually, and then do a rough comparison.

#### How long will the content blobs remain available?

Content blobs are available 7 days after the notification of the content blob’s availability. This means that if there is a significant delay in the creation of the content blob, you will have more time (the delay plus 7 days) after the actual event creation date before the content blob is no longer available.

#### If there is a 24-hour delay in getting a notification, doesn’t that mean I will have only 6 days to retrieve the content blob?

No. Even if the notification is delayed for an unusually long period (for example, in the case of a service interruption), you would still have 7 days after the first availability of the notification to download the content blob related to the originating event.












