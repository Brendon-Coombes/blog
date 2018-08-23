---
title: Application Authentication with Microsoft Graph
categories:
 - [Microsoft Graph]
 - [Development]
date: 2018-08-23 14:31:43
tags:
 - Development
 - Microsoft Graph
---

I have never attempted to use any Office365 or SharePoint APIs in my career to this point. The difficulty involved in figuring out exactly what I needed to do to retrieve and post some data to Office365 was much higher than I anticipated. This was because almost every tutorial I encountered, or quick start option within Office365 was aimed at user centric applications. My application did not have user input, it was an automated task. I also found out half way through this task that there are two distinct ways to create Azure AD applications that require wildly different approaches to use.

The struggle to figure out how it all fits together is what inspired this post, so I can use this for future reference if I need to complete a task like this again.

<!-- more -->

## Development Environment

If you just want to test out how things work, not everyone has access to an Office365 subscription where they have all the necessary permissions to test the stuff out they want to try. A friend at work showed me this great resource: [https://demos.microsoft.com/environments](https://demos.microsoft.com/environments).

This site will spin you up an entire "demo" tenant for you to develop against. They only last 90 days, but in most cases this should be more than enough to test out some stuff and figure out what you need to do. I was super impressed with this service, and even more impressed with how quickly everything was ready for me to use (less than 30 seconds!).

So I set myself up one of these demo environments and began to hack away...

`TODO - finish this paragraoh off`

## Register an Application with Azure AD

This step is where I got tripped up the most, the documentation I was following does not make it obvious, but there are actually two different types of Azure AD applications. Those that are created with the **v1.0 endpoint**, and those that are created with the **v2.0 endpoint**. From here on I will refer to these as v1.0 and v2.0.

If you are wondering which one you should use, check out the documentation [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-limitations).

You most likely want to use v1.0, but I will cover both here.

### v2.0

Microsoft have provided a very simple user interface for creating an Azure AD application registration. You can access it at [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com). This is the starting point for creating a new application that will consume the Microsoft Graph API.

{% asset_img "register-new-office-application.png" "Main interface for registering a new application"%}

Click the **Add an app** button to get started. You will be prompted to give your application a name, I just went with GraphConsoleApplication for this example. I did not check the guided set-up option, all that seemed to do was redirect me to documentation that did not relate to what I wanted to create.

After this step, you will be directed to a page that contains your **Application Id** and allow you to generate **Application Secrets**. You will need these later when you authenticate with the Graph API. Also on this page are Microsoft Graph Permissions.

You will also need to add a **Native Application** platform to the Platforms section. You can create the Native Application, leave the defaults and save. 

### Permissions

There are two types of authentication with Microsoft Graph, each type gives you a different set of access.

#### Delegated Permissions

`TODO - explain delegated permissions`

#### Application Permissions

`TODO - explain application permissions`

You will need an administrator to approve your application permissions before you can use it.

For permissions that administrators need to approve, I found that the process to get them approved was a bit more convoluted than I expected it to be. You need the administrator to approve your application permission request in Azure Active Directory. If an Administrator went to Active Directory now and tried to approve your application permissions, they would not appear.

This is because this web page uses the Azure AD v2.0 endpoint to create the application. The Azure portal will only show application registrations using the v1.0 endpoint, v2.0 applications cannot currently be managed in the Azure Portal.

This leaves us in a strange predicament if we have permissions that require admin approval; **where does the admin go to approve my permissions?**

The answer is that we have to construct a link that will require an admin to sign on and approve our permissions. We need to construct a link like the one below (line breaks are only there to make it more readable):

```cURL
GET https://login.microsoftonline.com/{tenant-ID}/adminconsent?
client_id={YOUR-CLIENT-ID}
&state=12345
&redirect_uri={YOUR-REDIRECT-URI}
```

You can find your tenant ID in the Azure Portal. If you navigate to **Azure Active Directory** -> **Properties** -> **Directory ID** the value for Directory ID is your tenant ID. Your client ID is the same as the application ID for the application we created earlier on the apps.dev.microsoft.com page.

If you constructed this URL correctly, you should be prompted to sign in as an administrator and grant your application permissions

{% asset_img "admin-approve-permissions.png" "Interface for admin to approve permissions"%}

### v1.0

`TODO v1.0`


## Authenticating with Microsoft Graph

We are going to use OAuth 2.0 client credentials grant (sometimes called two-legged OAuth) to access our resources using the identity of an application. This type of authentication flow is commonly used for server-to-server interactions (which is exactly what we are doing).

There are two ways an application can authorize directly with the Graph API.

1. Access Control Lists (ACL)
1. Application Permissions

We are just going to focus on **Application Permissions** as that is the method employed by using registering the application we did above (for both v1.0 and v2.0). If you want to know more about Access Control Lists as a method of authenticating, check out the [Microsoft OAuth2 Client Creds Grant Flow help page](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow).


