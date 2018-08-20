---
title: QuickTip - Visual Studio - Change Default Project Location
categories:
 - [QuickTips, Visual Studio]
date: 2018-08-21 10:09:02
tags: 
- QuickTip
- Visual Studio
---

I have become frustrated with Visual Studio defaulting the file location each time I create a new project. You can change it easily by modifying a single setting within Visual Studio.

<!-- more -->

In Visual Studio when you create a new project or use the File -> Open dialog, it will default a location for you. By default this is `%USERPROFILE%\source\repos`.

PIC HERE

I never want to use this location, and I found myself several times getting frustrated by the fact this happens each time I need to access the disk from Visual Studio. After the most recent frustration I decided to have a hunt through the menus and see if this was a configurable setting.

After a relatively quick search, I found the setting was in **Tools** -> **Options** -> **Projects and Solutions** -> **Locations**.

Here you will find a setting called **Projects location:**, you can change this to your desired location and click OK to apply the change.

PIC HERE

Next time you use the File dialog, the folder it defaults to will be the one you specified in the **Projects location** setting.

No more frustration!