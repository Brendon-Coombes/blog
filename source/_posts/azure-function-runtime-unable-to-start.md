---
title: Azure Function Error -  The function runtime is unable to start
categories:
 - [Azure, Azure Functions]
 - Development
date: 2018-06-11 21:05:16
tags:
 - Development
 - Azure
 - Azure Functions
---

Doing some work on an existing Azure Function that was created by someone else; I ran into to an issue in the [Azure portal](https://portal.azure.com) where I would get the error: The function runtime is unable to start. It turns out it was to do with IP restrictions set up before I took this over. Here is the process I took to find the issue and what was required to resolve it.
<!-- more --> 

I discovered this problem when attempting to add a new function to an existing function app. The error would show up on the page just after page load, and I noticed it wasn't just on the function that I created, but on all of the functions within the function app.

{% asset_img "error-on-the-azure-portal.png" "Error on the Azure Portal"%}

Error: The function runtime is unable to start

I had made this change in our staging environment, but had the exact same code in the development environment that worked without problems.

I made sure that I had all of the logging and diagnostic settings enabled for my function app and reproduced the issue again. I then searched through the Kudu logs to see if I could spot the problem in there, but I did not find any logs related to the issue I was experiencing.

Another thing I noticed, was that attempting to run the function even though the error was there would result in a 403: Forbidden. I could not think of any reason the user I was logged in as would not have access to the resources, as the user I was logged in as was the global administrator for the subscription.

{% asset_img "403-forbidden.png" "403 response when trying to run"%}

After more digging, I discovered that this issue can happen due to IP restrictions being set in your function application. This would explain why the very same code worked in the development function app but not the staging one if IP restriction was only set up on the staging environment.

The error we are seeing in the portal is not that the function runtime is unable to start, it is that it the portal cannot access it because it does not have access due to it not being on the whitelist of IP addresses as explained in GitHub issues [2344](https://github.com/Azure/azure-functions-host/issues/2344) and [2643]((https://github.com/Azure/azure-functions-host/issues/2643)) in the Azure Functions host repository.

Reading about this further; I discovered that the options for IP Address Restriction have been disabled in Azure Functions on the Azure Portal due to them being buggy. They will be re-enabled in the portal when the issues around them have been fixed.

Because this option had been disabled, I had no way of knowing if my function app had IP Restrictions through the Azure Portal. Luckily for me, even though the option has been disabled in the portal, you can still view and edit this setting in the resource explorer as explained by David Ebbo in GitHub issue [2471](https://github.com/Azure/azure-functions-ux/issues/2471#issuecomment-387568608) in the Azure Functions UX repository:

> You can remove the restrictions using Resource Explorer. Instructions:
> - Go to [https://resources.azure.com/](https://resources.azure.com/)
> - Find the relevant Function App, e.g. using the search box
> - Under the app, open the config/web node
> - Click Edit (you may need to switch to Read/Write mode first)
> - Near the bottom, look for ipSecurityRestrictions, and set its value to empty array (i.e. []).
> - Click the PUT button to apply the change

Looking at both my development and staging environment resource explorer; I found there was a difference in the **ipSecurityRestrictions** value:

{% asset_img "dev-staging-differences.png" "Dev - Staging differences"%}

Following the instructions David Ebbo suggested above fixed the problem I was experiencing in the portal. After removing the **ipSecurityRestrictions** values in my staging environment, the issue I was experiencing in the Azure portal disappeared.

It is not recommended to use IP restrictions on Azure Functions at the moment as they are buggy, Microsoft are aiming to have this issue resolved mid-June 2018. In the meantime if you experience any strage behaviour in the portal; removing IP Restrictions in the portal will likely resolve your issues.