---
title: Application Authentication with Microsoft Graph
date: 2018-08-23 14:31:43
tags:
---

I have never attempted to use any Office365 or SharePoint APIs in my career to this point. The difficulty involved in figuring out exactly what I needed to do to retrieve and post some data to Office365 was much higher than I anticipated. This was because almost every tutorial I encountered, or quick start option within Office365 was aimed at user centric applications. My application did not have user input, it was an automated task.

The struggle to figure out how it all fits together is what inspired this post, so I can use this for future reference if I need to complete a task like this again.

<!-- more -->

## Development Environment

If you just want to test out how things work, not everyone has access to an Office365 subscription where they have all the necessary permissions to test the stuff out they want to try. A friend at work showed me this great resource: [https://demos.microsoft.com/environments](https://demos.microsoft.com/environments).

This site will spin you up an entire "demo" tenant for you to develop against. They only last 90 days, but in most cases this should be more than enough to test out some stuff and figure out what you need to do. I was super impressed with this service, and even more impressed with how quickly everything was ready for me to use (less than 30 seconds!).

So I set myself up one of these demo environments and began to hack away...

`TODO - finish this paragraoh off`

## Register Application in Azure AD

Microsoft have provided a very simple user interface for creating an Azure AD application registration. You can access it at [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com). This is the starting point for creating a new application that will consume the Microsoft Graph API.

{% asset_img "register-new-office-application.png" "Main interface for registering a new application"%}

Click the **Add an app** button to get started. You will be prompted to give your application a name, I just went with GraphConsoleApplication for this example. I did not check the guided set-up option, all that seemed to do was redirect me to documentation that did not relate to what I wanted to create.

After this step, you will be directed to a page that contains your **Application Id** and allow you to generate **Application Secrets**. You will need these later when you authenticate with the Graph API. Also on this page are Microsoft Graph Permissions.

`TODO Delegated vs Application permissions - combine with below`

## Authenticating with Microsoft Graph

`TODO - explain this`

There are two types of authentication with Microsoft Graph
- Delegated User Permissions
- Application Permissions.

ADAL.

