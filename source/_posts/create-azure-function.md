---
title: Creating Azure Functions
categories:
 - [Azure, Azure Functions]
 - [Development]
date: 2018-03-15 01:23:45
tags:
 - Azure
 - Azure Functions
---
Azure Functions are a great way of running "serverless" code.

What is serverless code, and what would I use it for? How do I set it up?
<!-- more --> 

## What is a serverless code/architecture?

Serverless code (also called serverless architecture or serverless computing) refer to applications that depend on back end architecture (servers) but the end user does not need to purchase or rent these directly.

The code still runs on a server, but the end-user does not need to handle any of the setting up or configuration. The cost of hosting the application is generally more cost effective as you only pay for the CPU cycles that you use.

## Why would I use it?

Azure Functions provide a great way of hosting a small "function" that you may need to trigger multiple times. Azure Functions provide a way of acheiving this without having to worry about setting up a whole application or infrastructure to use it. You only pay for the CPU cycles you use, and it can scale up as needed.

If that pricing model is scary to you, you can also add an Azure Function to an existing App Service Plan on Azure (or create a new App Service Plan)

Azure Functions [support a variety of languages](https://docs.microsoft.com/en-us/azure/azure-functions/supported-languages) including C#, Python and JavaScript, you are not bound to just Microsoft technology.

You are able to bring in dependencies via Nuget and NPM, so you are able to use 3rd party libraries for your function; you only need to write the code that you actually need to solve the problem at hand.

Azure Functions runtime is open-source and available for anyone to contribute to on [Github](https://github.com/azure/azure-webjobs-sdk-script)

Azure Functions can be triggered to run in a variety of ways including:
- Timer Trigger
- Queue Trigger
- Blob Trigger
- HTTP Endpoint Trigger
- Service Bus Trigger
- Event Hub Trigger
- Cosmos DB Trigger
- IOT Hub Trigger

## How do I set one up?

You can set up an Azure Function in a variety of ways, I am going to go through the process of creating one in Visual Studio; You can follow along in another post I have made about Functions on how to create one entirely in the Azure Portal [here](https://coombes.nz/blog/publish-azure-function/). In order to follow this post; you will need to have a Function App set up already.

To follow along with me, you will need to be using [Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) (or newer) and also have Azure development workload installed. You will also need the latest version of the Azure Functions tools; follow [this guide](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs#check-your-tools-version) to get the Azure Functions tools.

I am following along with the [Microsoft Functions documentation](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio), what I am doing is almost exactly the same as the documentation that they have.

After installing the tools above, you should have a new template in Visual Studio for creating Azure Functions:

{% asset_img "create-azure-function-project.png" "Create new project - Azure Function"%}

When you create you Function App in Visual Studio; you are then presented with some options for your Function App. You now define if you want to use Azure Functions v1(.NET Framework) or v2(.NET Core), You can select a template for a trigger type, and a storage account.

> **As of the time of this writing - Azure Functions v2 are still in preview**

For my test function, I am going to use Azure Functions v2(.NET Core), with the HTTP trigger template and I am just going to use the storage emulator as I am not going to persist anything.

{% asset_img "azure-function-options.png" "New Azure Function - Options"%}

Visual Studio will now generate a project for you that expects you to pass in your name to the function, and in return the function will return "Hello, {name}".

{% asset_img "function-code-example" "Visaul Studio - Azure Function code example"%}

To test your function, you simply run it as you normally would a console application by hitting the F5 key, or pressing the play button in the menu of Visual Studio. If you don't have the Azure Functions CLI tools installed, Visual Studio will prompt you to install them now.

When you Azure function has started, the URL to contact it will be output in the console window.

{% asset_img "azure-function-running.png" "Running Azure Function"%}

If we browse to that URL in a web browser we will be interacting with the function. If I browse to that URL now, I will get an error like this:

{% asset_img "function-error-output.png" "Error output when name parameter is not specified"%}

This is because we are expecting a name to be passed in to the function. We did not specify a name in the URL. If we add a querystring with the name in it like '?name=Brendon', then the function will give us the output we are expecting:

{% asset_img "function-success-output.png" "Successful output"%}

Obviously, this is a very simple example of an Azure Function. In a more realistic scenario we would be formatting the data we return in a more sturdy machine readable format such as JSON, and it would be less likely that people are accessing this endpoint directly; it is probably going to be another service consuming our data, or contacting this endpoint to trigger off a longer running job.

## Publish your function to Azure

To get your function in Azure you need to publish it. To do this from Visual Studio; right-click on your function project and select "Publish".

{% asset_img "visual-studio-publish.png" "Visual studio publish"%}

In this scenario, I do not have an existing function app, so I am going to select "Create New"

{% asset_img "create-new-function-app.png" "Create a new function app in visual studio"%}

You will then be asked to choose an App Name, Subscription, Resource Group, and Hosting Plan for your function in Azure.

{% asset_img "create-function-app-resources.png" "Create new function app resources in visual studio"%}

When you have selected your resources (or created new ones), your application will begin to publish. When it has completed you should now see the function in your Azure portal, you are able to test your new function on the right-hand side of the page of your function in the portal, or get a URL that goes directly to your function

{% asset_img "function-in-azure.png" "Published function in the Azure portal"%}

So there you have it, you have a running Azure Function.