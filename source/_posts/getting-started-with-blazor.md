---
title: Getting Started With Blazor
categories:
 - [Development, Web Development]
date: 2018-08-03 15:59:52
tags:
 - Blazor
 - Web Development
---

I have been hearing lots of positive things about Blazor around the office, on podcasts, and on Twitter recently. So I have finally decided to dig into it and see what it is all about.

In this post I get started with Blazor and take you through the steps I took to get a finished application.
<!-- more --> 

## What is Blazor

Blazor is .NET code run on the client side. It does this using the WebAssembly standard. At the time of writing this post; it is still an experimental project and has not been given any indication of commitment from Microsoft in terms of being a long term supported product.

`TODO: More info on BLAZOR / WebAssembly and how it works and what it means for the future.`

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

## Deploying / Hosting a Blazor Application

## What I Created