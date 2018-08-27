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

## Register an Application with Azure AD

The first thing you need to do to start retrieving data programatically from Office365 is set up an Azure AD Application.

This step is where I got tripped up the most, the documentation I was following does not make it obvious, but there are actually two different types of Azure AD applications. Those that are created with the **v1.0 endpoint**, and those that are created with the **v2.0 endpoint**. From here on I will refer to these as v1.0 and v2.0.

If you are wondering which one you should use, check out the documentation [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-limitations). I am going to focus on the method for connecting to v2.0.

Before I dig into the details, I want to quickly go over each type of permission.

### Permissions

There are two types of authentication with Microsoft Graph, each type gives you a different set of access.

##### Delegated Permissions

You typically use delegated permissions when you want to call the API as the logged on user. Essentially you are "delegating" your permissions (as the logged on user) to the API. You can select what permissions you want access to on behalf of the user, and when the user logs in through your application they will see what you are requesting access to before approving or denying the request.

##### Application Permissions

Application permissions are normally used when there is not a user context involved. You use application permissions when the application calls the API as itself. This is the type of authentication we will need for server to server communication as the API calls will not be specific to a user.

### v2.0 Azure AD Application

Microsoft have provided a very simple user interface for creating an Azure AD application registration. You can access it at [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com). This is the starting point for creating a new application that will consume the Microsoft Graph API.

{% asset_img "register-new-office-application.png" "Main interface for registering a new application"%}

Click the **Add an app** button to get started. You will be prompted to give your application a name, I just went with GraphConsoleApplication for this example. I did not check the guided set-up option, all that seemed to do was redirect me to documentation that did not relate to what I wanted to create.

After this step, you will be directed to a page that contains your **Application ID** and allow you to generate **Application Secrets**. You will need these later when you authenticate with the Graph API. Also on this page are Microsoft Graph Permissions.

You will also need to add a **Native Application** platform to the Platforms section. You can create the Native Application, leave the defaults and save. 

The final step for you on this page, is to **allow implicit flow**. To do this click the **Edit Application Manifest** button at the bottom of the page, this will take you to a page to edit the config. Update the **oauth2AllowImplicitFlow** and **oauth2AllowIdTokenImplicitFlow** values to true.

To do this you need to click the button at the bottom of the page to edit your application manifest

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

If you constructed this URL correctly, you should be prompted to sign in as an administrator and grant your application permissions.

{% asset_img "admin-approve-permissions.png" "Interface for admin to approve permissions"%}

## Authenticating with Microsoft Graph

We are going to use OAuth 2.0 client credentials grant (sometimes called two-legged OAuth) to access our resources using the identity of an application. This type of authentication flow is commonly used for server-to-server interactions (which is exactly what we are doing).

There are two ways an application can authorize directly with the Graph API.

1. Access Control Lists (ACL)
1. Application Permissions

We are just going to focus on **Application Permissions** as that is the method employed by using registering the application we did above. If you want to know more about Access Control Lists as a method of authenticating, check out the [Microsoft OAuth2 Client Creds Grant Flow help page](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow).


Microsoft has created a library to help facilitate this process: Microsoft Authentication Library. It is often reffered to by its acronym MSAL. This library is open source on [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) and available to consume on [Nuget](https://www.nuget.org/packages/Microsoft.Identity.Client/).

As of this writing the MSAL libary is still "in preview". Microsoft has stated on their page that although it is in preview, it is suitable for production use:

> This library is suitable for use in a production environment. We provide the same production level support for this library as we do our current production libraries. During the preview we may make changes to the API, internal cache format, and other mechanisms of this library, which you will be required to take along with bug fixes or feature improvements. This may impact your application. For instance, a change to the cache format may impact your users, such as requiring them to sign in again. An API change may require you to update your code. When we provide the General Availability release we will require you to update to the General Availability version within six months, as applications written using a preview version of library may no longer work.

After installing that Nuget package, you are able to request a token from Microsoft Graph like this:

```csharp
string clientId = "{YOUR CLIENT ID}";
string clientSecret = "{YOUR CLIENT SECRET}";
string tenantId = "{YOUR TENANT ID}";
string redirectUri = "https://login.microsoftonline.com/common/oauth2/nativeclient";
string authorityFormat = "https://login.microsoftonline.com/{0}/v2.0";
string mSGraphScope = "https://graph.microsoft.com/.default";

MSALCache appTokenCache = new MSALCache(clientId);

ConfidentialClientApplication daemonClient = new ConfidentialClientApplication(clientId, 
                                                                               string.Format(authorityFormat, tenantId), 
                                                                               redirectUri, 
                                                                               new ClientCredential(clientSecret), 
                                                                               null, 
                                                                               appTokenCache.GetMsalCacheInstance());

AuthenticationResult authResult = await daemonClient.AcquireTokenForClientAsync(new[] { mSGraphScope });
```

In the authResult there is a property called AccessToken, it is this access token that will allow us to retrieve information from the Graph. It is also at this point where we will start to notice the permissions that we applied for in the earlier steps. If we try to access anything within the Graph that we do not have permission for we will get a 401 unauthorized response.

We can make basic HTTP requests to the Graph now to see if we can pull some data.

```csharp

string msGraphQuery = "https://graph.microsoft.com/v1.0/sites?search=*";

HttpClient client = new HttpClient();
HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, msGraphQuery);
request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
HttpResponseMessage response = await client.SendAsync(request);

```

If everything was set up correctly, you should see a list of sites that have been pulled from SharePoint.

I am in the process of creating a small sample application to host on GitHub, which I will link here when I have finished it.