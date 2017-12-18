---
title: Adding Application Insights to an Existing Web Application
categories:
 - Application Insights
date: 2017-01-11 15:55:57
tags:
 - Application Insights
 - Web
 - Analytics
---

If you don't know what it is; Application Insights is essentially a Microsoft developed framework for logging and capturing events. It can give you some incredibly useful data on your web application, and as at the writing of this blog post it is **free** for usage under 1gb per month; here is a quick run-through of how to add it to your existing web application.
<!-- more -->
Application Insights gives you a great set of graphs on your dashboard, website traffic, exceptions, page load time, server response time total requests, you can log custom events too.

Do you already have a site built and now want to add application insights?

First create an application insights resource in your azure portal.

To do this, log into you Azure portal and click the plus button in the top left corner, then select "Web + Mobile" then select "Application Insights".

{% asset_img "add_applicationinsights.png" "Add Application Insights in Azure"%}

This will open another blade with some information about Application Insights, read it if you want; then click "Create".

Fill out the required fields on this screen to create your application insights resource.

{% asset_img "new-application-insights.png" "New Application Insights in Azure"%}

Switch to Visual Studio, right click on your existing web application in the Solution Explorer and select "Add Application Insights Telemetry..." 

{% asset_img "add-application-insights-telemetry.png" "Add Application Insights Telemetry..."%}

Visual Studio will ask you to log in; log in with the same Microsoft account you used to create your Application Insights instance in Azure.

When you have logged in you will be shown which application resources are available, select the one that you made earlier and click register.

{% asset_img "add_applnsightstoproject." "Add Application Insights to Project"%}

That is it! You now have Application Insights installed on your web application.

From here you can use Application Insights to:

1. Find and diagnose run-time exceptions
1. Find and diagnose performance issues
1. Monitor and alert on application health
1. Analyze customer usage and information
1. Create custom KPI dashboards

More information on how to do these can be found on the [Application Insights Documentation](https://docs.microsoft.com/en-us/azure/application-insights/) page.

I will go over setting these things up in custom applications in a future blog post.