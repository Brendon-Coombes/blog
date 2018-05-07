---
title: Creating Azure Functions
categories:
 - Azure Functions
date: 2017-11-27 22:23:25
tags:
 - Azure
 - Azure Functions
---
Azure Functions are a great way of running "serverless" code.

What is serverless code, and what would I use it for? How do I set it up?
<!-- more --> 

## What is a serverless code/architecture?

Serverless code (also called serverless architecture or serverless computing) refer to applications that depend on back end architecture (servers) but the end user does not need to purchase or rent these directly.

The code still runs on a server, but the end-user does not need to handle any of the setting up or configuration. The cost of hosting the application is generally 

It is estimated that **over 50%** of web traffic is made up of bots!

A "bot"; historically referred to as a "Web Robot" is a software application that runs automated tasks over the internet. Typically a bot will perform tasks that can be easily automated and are structually reptitive. A quite frequent use of bots in the past was to create spam users on web site, or to write comments on websites containing only advertisements. These bots are known as spambots; this isn't what I will be writing about today. 

The type of bots that are taking off now are known as chat bots.
These bots are interacted with in a conversational manner.

Are you using a bot without realising it? Do you use Siri, Cortana, Google Now, or Amazon's Alexa?
All of these assistants are bots!

I will show you how you can set up your own personal assisstant bot using the Microsoft Bot Framework needing very little knowledge of bots, and how you can then access and use that bot.

Before we create a bot, we need to talk about channels, and what the bot framework is actually providing.

## What is a channel?

A channel is how your bot is consumed. How will users interact with it?
For example will it be Facebook Messenger, or Slack, or Skype?

There are many different channels your bot can be consumed on and the Bot Framework provides many options out of the box to wire your bot up to different channels.

Amazon Alexa, Google Home and the Microsoft Invoke with Cortana are all examples of headless bots, although users are interacting with a bot, they do not (normally) have a visible interface; meaning that all of the interaction is done vocally.

You can use the same skills in the bot framework on a headless device like the Invoke with Cortana, if you program your bot to support headless interfaces.

## How does the bot framework help me?

The bot framework enables you to focus on the logic of the bot without worrying about how the content will be delivered to the end user. You do not have to create an interface for every channel, this is all provided to you out of the box.

This enabled you to provide a seamless experience across channels and have a singular back-end that each of the channels you enable communicate with.

## Create a bot

To get started creating a simple bot; head to [https://dev.botframework.com/](https://dev.botframework.com/). This is the main portal for the Microsoft Bot Framework.

Click the "Create a bot or skill" button

{% asset_img "CreateBotOrSkill.png" "Create a bot or skill"%}

From there; click the "Create a Bot" button, and then the "Create" button on the page it leads you to.
This will redirect you to creating a "Bot Service" within the Azure portal.

You now have three choices for items within the Bot Service to create:

{% asset_img "CreateBotInAzure.png" "Create a bot in Azure"%}

1. Web App Bot
*This is your actual bot, where the logic for the functionality of the bot is located, built on Azure Web App functionality*
1. Bot Channels Registration
*This is used to deliver your bot to multiple channels*
1. Functions Bot
*This could also be your actual bot, where the logic for the functionality of the bot is located, built on Azure Functions. For the purposes of this demo, we are going to use a Web App Bot*

Select "Web App Bot".
Fill out the required fields and ensure that you select the free pricing tier if you want to avoid any charges to your Azure account.

One of the options "Bot template" allows you to start with a template, that will help you structure your bot and use existing bot framework functionality based on the type of bot you are trying to create. As I am just making a basic bot for this example; I am going to stick with the default "Basic C#".

You can also select whether or not to use Application Insights with your bot out of the box. Application Insights is a great tool for analytics, so I am going to include it with my bot.

{% asset_img "CreateBotInAzureDetails.png" "Create a bot in Azure (details)"%}

It will take 5 or 10 minutes for Azure to provision all of the things it needs to create your bot if you do not already have a storage account, app service, and resource group to use.

After your bot has been created, you will see it in your resources.

You can test that your bot works by selecting the "Test in Web chat" option from within Azure.
This will open a chat window to talk to your bot, although we have not actually configured anything yet, the bot will still respond.

In an upcoming blog post, we will look at how to configure and customise the bot from source code.

{% asset_img "BotBuiltInAzure.png" "Bot Created in Azure"%}

