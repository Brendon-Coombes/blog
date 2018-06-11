---
title: Publish an Azure Function in the Azure Portal
categories:
 - Development
date: 2018-03-29 08:25:51
tags:
 - Development
 - Azure
 - Azure Functions
---

I was working on an Azure Function today, and the normal "Publish" option in Visual Studio was unavailable to me. So I needed to figure out another way to publish the function. It is quite a simple process to do through the Azure Portal User Interface. Here is a quick guide on how to do it through the portal.
<!-- more --> 

Head to the [Azure Portal](https://portal.azure.com), and then go to your function application.

On the left hand side under your Function App, there will be a node labelled "Functions" if you hover over this a "+" symbol will appear to add a new function to your function app. Click on this to create a new Function App.

{% asset_img "add-new-function-option.png" "Adding a new function to a function app"%}

You will then be taken to a page to select the type of trigger for your function, and the language that your function will be written in. In my case; I am adding a function with a timer trigger that is written in C#

{% asset_img "create-timer-trigger-function.png" "Create time trigger function example"%}

You will be then asked to select your language, name your new function and if you are doing a timer function like I am you will need to add a schedule for your timer function to run on.

The schedule is recorded as a cron expression in the format: {second} {minute} {hour} {day} {month} {day of week}. If you are using online cron builders such as ones like [https://crontab.guru/](https://crontab.guru/); they will often omit the {second} section. So if you build your cron expression in a tool that does not use the {second} value then you will need to account for that when entering your expression, generally you will want 0 in the {second} spot if your cron is not specific to the second.

I want mine to run at 11PM on Thursday nights, so I have entered the expression: 0 0 23 * * 4.

{% asset_img "add-function-information.png" "Add function information"%}

Once you have added your language, name and your cron expression you can create your function.

Azure will automatically generate a run.csx file and function.json file with your cron expression in it.

{% asset_img "edit-runcsx.png" "Edit existing run.csx"%}

I already have a run.csx function created on my machine, so I am going to copy the contents of that into the run.csx file, you are able to edit the function direct in the Azure portal.

My function also needs to pull in some libraries to run, so I need to specify a project.json file to so the additional packages are pulled in so the function can run.

You can add a file 3 different ways from here, 

1. You can add a new file clicking the "+ Add" button, and then modify the contents to your desire.
1. You can click the "Upload" button and upload your file directly.
1. You can drag your file on to the right-hand pane and drop it to upload it.

I dragged my file on to the right hand pane in order to upload it, and all of my function has now been created in Azure without publishing from Visual Studio.