---
title: Xamarin - Set Mac Up as Build Agent
date: 2018-08-16 18:08:10
categories:
 - [Development, Xamarin]
tags:
 - Mac
 - Xamarin
---

When creating an application for IOS, it is required that it be compiled on the latest version of OSX. This can become a burden if your main development machine is a Windows machine, and you are only using a Mac to create your IOS application. In this post I go over the steps to set your Mac up as a build agent for your Windows machine and discuss the benefits doing this will open up for your development workflow.
<!-- more -->

## Mac Set-up

Firstly, you will need a Mac with the latest version of OSX, at the time of this writing that is High Sierra.

On that Mac you will also need to install the following:

1. The latest version of [Xcode](https://developer.apple.com/xcode/)
1. [Visual Studio for Mac](https://docs.microsoft.com/en-us/visualstudio/mac/installation)
1. [Xamarin Tools for Mac](https://docs.microsoft.com/en-us/visualstudio/mac/installation)

The Xamarin tools are an optional add-on when installing Visual Studio for Mac, make sure that you include them when setting up.

When you have these components installed, your Mac is ready to develop IOS applications on. We can now take this a step further and allow the Mac to be connected to from Visual Studio for Windows to build IOS applications. This method will only work for devices that are on the same network.