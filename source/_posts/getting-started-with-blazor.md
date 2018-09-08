---
title: Getting Started With Blazor
categories:
 - [Development, Web Development]
date: 2018-08-22 15:59:52
tags:
 - Blazor
 - Web Development
---

I have been hearing lots of positive things about Blazor around the office, on podcasts, and on Twitter recently. So I have finally decided to dig into it and see what it is all about.

In this post I get started with Blazor and take you through the steps I took to get a finished application.
<!-- more --> 

## What is Blazor

Blazor is .NET code run on the client side. It does this using the WebAssembly standard. At the time of writing this post; Blazor is still an experimental project and has not been given any indication of commitment from Microsoft in terms of being a long term supported product.

For more information on WebAssembly and how it works and what it does; check out [webassembly.org](https://webassembly.org/).

To read about Blazor and the features that it offers check out [blazor.net](https://blazor.net/).

Because it is so rapidly developed, it is likely that by the time you have read this post, some of what I have written will be out of date. I know that makes this post less than ideal for getting a gauge of how to use it, but I really wanted to share my experience on using Blazor for the first time.

## Creating a Blazor Application

To get started with Blazor you first need to ensure you have the following components installed:

1. The [.NET Core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.300 or later)
1. [Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.7 or later) - with ASP.NET and Web Development workload selected
1. The [Blazor project templates and language extensions](https://go.microsoft.com/fwlink/?linkid=870389)

When you have these components installed; Blazor projects will appear as templates when you create a new ASP.NET Core Web Application within Visual Studio.

{% asset_img "create-blazor-project.png" "Create Blazor Application Template Options in Visual Studio"%}

This will generate a template within Visual Studio that uses Blazor for you to have a look around and see how it works. Blazor does not yet support running with the debugger attached, so if you want to run you need to run with **Ctrl + F5** not **F5**

You can also create a Blazor Application from the command line. Given you want to call your application "BlazorApp" you can run the following commands to create and run the Blazor App:

```cmd
dotnet new blazor -o BlazorApp
cd BlazorApp
dotnet run
```

When you have either created the application through Visual Studio, or through the command line you should have a new project created that is using Blazor!

If you open it up and run it, it is very similar to the other templates that Microsoft has for frameworks such as ReactJS. When you dig into the code, you will start to see where the real magic happens.

In each of the pages "Index", "Counter", and "FetchData" a Blazor compmemt os compiled and executed client-side in the browser.

These components can be re-used across multiple pages as well, if you check out the [help page](https://blazor.net/docs/tutorials/build-your-first-blazor-app.html#build-components), they give a solid example on how to do this.

## Deploying / Hosting a Blazor Application

`TODO - explain how to deploy and host a Blazor application in Azure.

## What I Created

As you may have read in a [previous post](https://coombes.nz/blog/using-microsoft-flow/), our office is quite competitive with Crash Bandicoot, and we currently have a spreadsheet file that we all maintain our own times on and can use to view who the current winner is. In that post I describe how I set up Microsoft Flow to get a notification each time that the spreadsheet file is modified.

I have modifed the Flow to add some additional features. It will now send a link to the spreadsheet file each time it is updated to an Azure Function I have created. That Function pulls the excel file from SharePoint and transforms it to a JSON format, and saves it to a MongoDB document database.

I haven't quite got to the point of connecting the MongoDB to this Blazor application, so at the moment it is just a hardcoded JSON file. I intend to switch that over to the MongoDB version as my next feature.

The Blazor application I have created reads the data from the JSON file and displays it on a web page. I ***borrowed*** the styling from the official [Crash Bandicoot Leaderboards web page](https://www.crashbandicoot.com/leaderboards). All the application does is show who from the office is currently winning at each level on each of the three games. It is a very simple site, but having a data set that you are pasionate about really helps you focus on completing your projects.

I could tell at this point that my work mates were losing interest in the N-Sane trilogy, so I am hoping when I show them what I have created they will be inspired to pick their game back up!

The site itself, only has a single page pulling in some JSON data to display on the page. It has 3 dropdowns built out as components to filter which data is shown on the page at any given time.

The leaderboard component displays each of our times over the background picture that I stole from the offical crash bandicoot site.

Overall this was a well worth it experience to understand how Blazor works.

I have uploaded it to GitHub - check it out here.

`TODO - Add lessons learnt`
`TODO - Point out key features`
`TODO - Add github link`
`TODO - Add MongoDB help
`TODO - Add Azure Function that transforms the excel file