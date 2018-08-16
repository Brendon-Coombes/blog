---
title: Xamarin - Set Mac Up as Build Agent
date: 2018-08-16 18:08:10
categories:
 - [Development, Xamarin]
tags:
 - Mac
 - Xamarin
---

When creating an application for IOS, it is required that you use the Apple build tools; which are only availble on OSX. This can become a burden if your main development machine is a Windows machine, and you are only using a Mac to create your IOS application. In this post I go over the steps to set your Mac up as a build agent for your Windows machine and discuss the benefits doing this will open up for your development workflow.
<!-- more -->

## Benefits of a Mac Build Agent

The official Microsoft set-up page gives the following benefits from setting up a Mac build agent:

Visual Studio 2017's Pair to Mac feature discovers, connects to, authenticates with, and remembers Mac build hosts so that Windows-based iOS developers can work productively.

Pair to Mac enables the following development workflow:

- Developers can write Xamarin.iOS code in Visual Studio 2017.
- Visual Studio 2017 opens a network connection to a Mac build host and uses the build tools on that machine to compile and sign the iOS app.
- There is no need to run a separate application on the Mac – Visual Studio 2017 invokes Mac builds securely over SSH.
- Visual Studio 2017 is notified of changes as soon as they happen. For example, when an iOS device is plugged in to the Mac or becomes available on the network, the iOS Toolbar updates instantly.
- Multiple instances of Visual Studio 2017 can connect to the Mac simultaneously.
- It's possible to use the Windows command-line to build iOS applications.

## Mac Set-up

Firstly, you will need a Mac with the latest version of OSX, at the time of this writing that is High Sierra.

On that Mac you will also need to install the following:

1. The latest version of [Xcode](https://developer.apple.com/xcode/)
1. [Visual Studio for Mac](https://docs.microsoft.com/en-us/visualstudio/mac/installation)
1. [Xamarin Tools for Mac](https://docs.microsoft.com/en-us/visualstudio/mac/installation)

The Xamarin tools are an optional add-on when installing Visual Studio for Mac, make sure that you include them when setting up.

When you have these components installed, your Mac is ready to develop IOS applications on. We can now take this a step further and allow the Mac to be connected to from Visual Studio for Windows to build IOS applications. This method will only work for devices that are on the same network.

## Allow remote connections

On your Mac, you need to allow remote connections. In order to do this:

1. Open the System Preferences -> and select the Sharing pane.
1. Check remote login as shown in the screen shot below

{% asset_img "system-preferences-sharing.png" "System Preferences - Sharing"%}

Change the setting to allow "All Users" rather than "Only these users:"

If you are prompted, you may also need to configure the Mac firewall to allow incoming connections.

## Connect from Windows Visual Studio 2017

From Visual Studio 2017, open (or create) your iOS Xamarin application. In the toolbar you should now see the "Pair to Mac" button. If you don't see it, you can find it in Tools > iOS > Pair to Mac.

{% asset_img "pair-to-mac-button.png" "Pair to Mac button in Visual Studio"%}

When you click the button, if you have set your Mac up correctly; you should see it listed as a Mac available to connect to:

{% asset_img "pair-to-mac-window.png" "Pair to Mac window in Visual Studio"%}

Click the "Connect..." button at the bottom left.

At the prompt, enter your username and password and the dialog will connect to your Mac via SSH.

During the connecting phase it will check that the correct components are installed before becoming availbale to use, this can take a couple of minutes; but it should only have to do this once.

When you have connected successfully the "Pair to Mac" button will now have a new icon with a green screen indicating that you are connected to a Mac.

