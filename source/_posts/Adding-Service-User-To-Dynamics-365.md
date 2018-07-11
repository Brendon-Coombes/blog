---
title: Adding a Service User to Dynamics 365
categories:
 - [Dynamics365, Integration]
 - Integration
date: 2018-01-29 15:55:57
tags:
 - Dynamics365
 - Integration
---

A service user is a system user within Dynamics 365 that is used to perform automated operations. In order to carry out any operations in Dynamics 365 you need to be authenticated to the system. Previously this had meant consuming a Dynamics 365 license on a user that is not used in the user interface in any capacity and only interacted with the system via the web services. More recently Microsoft has added two different types of users that can perform these tasks without consuming a Dynamics 365 license.
<!--more-->

There are two different types of service user we can create:
- Non-Interactive User
- Application User

A [non-interactive user](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/admin/create-users-assign-online-security-roles#create-a-non-interactive-user-account) is not a user in the typical sense, but is a system user within Dynamics 365 that allows you to access the Dynamics 365 services without consuming a Dynamics 365 license. You can have up to **5** non-interactive user accounts in each instance of Dynamics 365.

A non-interactive user cannot access the Dynamics 365 web interface and interact with the system, they are only able to access the data via the exposed services.

Non-interactive users can be used in place of application users when more traditional methods of authentication are required (username and password). This can be the case with third party add-ons and integrations to Dynamics 365.

An [application user](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/admin/create-users-assign-online-security-roles#create-an-application-user) is yet another type of user that is not a user in the typical sense. An application user is a user within Dynamics 365 that is tied to an Azure Active Directory Application, and performs tasks in Dynamics 365 on behalf of that application.

Application users are used for server-to-server (S2S) authentication to securely communicate with Dyanmics 365 with custom applications and services. Server-to-server(S2S) is the common way that apps registered on the Microsoft AppSource use to access the Dynamics 365 data of their subscribers.

Application users were introduced in Dynamics 365 (online) in December 2016, they are not available for use in systems that pre-date the December 2016 update.

Any operation that is requested by your custom application or service will be performed by the application user rather than as the user who is accessing your application or service.

Application users cannot use out-of-the-box (OOB) security roles; a custom role must be made for them.

Application users do not authenticate with a username and password, they authenticate with a "Client Id" and "Client Secret".

## Convert an Existing User to a Non-Interactive User

You can convert an existing system user to a non-interactive user account by following these steps:

1. Find the user account in the Office 365 Admin Center.
    - Ensure that a Dynamics 365 (online) license is assigned to the account.
    - Go to Dynamics 365 (online).
1. Go to Settings > Security.
1. Choose Users > Enabled Users, and then click the user’s full name.
1. In the user form, scroll down under Administration to the Client Access License (CAL) Information section and select Non-interactive for Access Mode.
    - You then need to remove the Dynamics 365 (online) license from the account.
1. Go to the Office 365 Admin Center.
1. Click Users > Active Users.
1. Choose the non-interactive user account and under Product licenses, click Edit.
1. Turn off the Dynamics 365 (online) license, and then click Save > Close multiple times.
1. Go back to Dynamics 365 (online) and confirm that the non-interactive user account Access Mode is still set for Non-interactive.

{% asset_img "creating-a-service-user-1.png" "Client Access License"%}


## Create a New Application User

Creating an application user in Dynamics 365 takes a few more steps than setting up a normal system user.

Because application users access Dynamics 365 on behalf of an Azure Active Directory Application, one of these must be set up first. After that an application user can be made within Dynamics 365 to link the Azure Active Directory Application to Dynamics 365 data.

### Create an Azure Active Directory Application

First an application needs to be set up in active directory.
You can find an in-detail guide on [how to create an Azure Active Directory Application on the official Microsoft Documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).

Below is a short summary of the steps that need to be taken:

1. Sign in to the [Azure portal](https://portal.azure.com)
1. In the left-hand navigation, select **Azure Active Directory**, then select **App registrations**, and then click **New application registration**
1. When the 'create' page appears, enter your application's registration information:
    - **Name:** A meaningful name for your application
    - **Application Type:** You will normally use Web app / API for integrating to Dynamics 365
    - **Sign-On URL:** Provide the base URL of your custom application or service.
1. Click create to finish creating your application.

Azure AD assigns a unique Application ID to your application when creating it, take note of this as you will need this later on when linking this application to Dynamics 365.

You need to give your application privileges to access Dyanmics 365 data; to do this from the Azure portal where you created the application:

1. Select **Required Permissions**
1. Click **Add**, then **Select an API**. 
1. In the list, select Dynamics 365 and then click the **Select** button.
1. In **Select Permissions** select **Access Dynamics 365 as organization users** and click the **Select** button.
1. Click **Done** to add these permissions. When you are done you should see the permissions applied:

{% asset_img "creating-a-service-user-5.png" "Azure AD Required Permissions"%}

The final step is to create a key to access the Azure Active Directory Application. When you create a key **it is only shown once**, so ensure that you have it stored securely before leaving the page.

In order to create a key:

1. Click the **Keys** section on the **Settings** page.
1. Add a description for the new key e.g. "D365 Integration Key".
1. Select how long you want the duration to last before expiring.
1. Click **Save**. The right-most column will contain the key value, after you save the configuration changes. **Be sure to copy the key** for use in your client application code, as it is not accessible once you leave this page.


### Create User in Dynamics 365

Now that we have an Azure Application set up, we can start the process of creating the application user in Dynamics 365.

If you try to create a new system user from within Dynamics 365 you are normally told to add and license users via the Microsoft Office 365 Admin Portal

{% asset_img "creating-a-service-user-2.png" "Add and License Users Modal"%}

We do not want to do that, as we don't need to consume a license for our new application user.

In order to create an application user in Dynamics 365, first browse to **Settings** > **Security** > **Users** within Dynamics 365.

When the view displays, **change the view selector to "Application Users"**.

{% asset_img "creating-a-service-user-3.png" "Users View Selector"%}

Now when you click the **New** button in the ribbon when this view is selected, you will not be greeted with the **Add and License Users** modal, but instead be taken to the create users form.

All the fields are locked, and you are unable to edit any of the user fields. 

**Change the form selector to "Application User"** to go to the form where you are able to enter the application user details

{% asset_img "creating-a-service-user-4.png" "Users Form Selector"%}

When you have the application user form open, there are only three fields that need to be filled out to create a user.

- **Application ID** is the application identifier from the Azure AD Application that was created earlier. This should be in the form of a GUID.
- **Full Name** is the name that Dynamics 365 will use as the **Created by** or **Modified by** value when this application user makes changes to Dynamics 365 data.
- **Primary Email** is where any mail sent to this user will go to.

Although other fields are marked as mandatory on this form, they cannot have values entered into them as they are locked. The values for these fields will be automatically populated when the record is saved.

When this user is created, it will have no security roles attached. In order to allow it to access data security roles need to be granted. The only security roles that can be assigned to an application user are custom roles, out of the box roles cannot be applied.

## Example: Authenticating with Dynamics 365 as Application User

Below is an example of connecting to Dynamics 365 as an application user. In order to connect to Dynamics 365 via S2S authentication you need a **client Id** and a **client secret**. The client Id is the application Id is the Azure Active Directory Application Id, and the client secret is the key that was generated for this application.

If you have a multiple tenant authentication, you may also need to specify the tenant Id in the authority URI. This can be found on the properties blade on the Azure Active Directory settings page on the Azure portal.

This example uses the following NuGet packages:
- Microsoft.IdentityModel.Clients.ActiveDirectory *by Microsoft* v3.18

 Example usage:
```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

namespace Dynamics365ServerToServerAuthentication
{
    class Program
    {
        static void Main(string[] args)
        {
            //If you have a single tenant authentication you can use the common endpoint
            //https://login.microsoftonline.com/common/oauth2/authorize
            //If you have a multiple tenant authentication you may need to specify the tenant id (Guid):
            //https://login.microsoftonline.com/TENANT-ID-HERE/oauth2/authorize

            const string authorityUri = "https://login.microsoftonline.com/common/oauth2/authorize";
            const string d365Url = "https://YOUR-DYNAMICS365-INSTANCE.crm6.dynamics.com";
            const string clientId = "YOUR APPLICATION ID";
            const string clientKey = "YOUR APPLICATION KEY";

            ClientCredential clientCredentials = new ClientCredential(clientId, clientKey);
            AuthenticationContext authenticationContext = new AuthenticationContext(authorityUri, false);
            try
            {
                var authenticationResult = authenticationContext.AcquireTokenAsync(d365Url, clientCredentials).GetAwaiter().GetResult();
                Console.WriteLine($"Connection success: {!string.IsNullOrEmpty(authenticationResult.AccessToken)}");
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }

            Console.ReadKey();
        }
    }
}
```