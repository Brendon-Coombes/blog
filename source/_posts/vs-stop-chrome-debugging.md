---
title: Disable JavaScript Debugging in Visual Studio
categories:
 - [Development, Web Development]
date: 2018-02-14 07:23:25
tags:
 - Development
 - ASP.NET
---


If you are reading this, it is likely that when you are debugging your ASP.NET application your Visual Studio is inserting itself to handle your JavaScript debugging. While I can see the usefulness of this feature; it can become annoying as each time you start a new instance of your application Visual Studio opens a new instance of Chrome to handle the JavaScript debugging.
<!-- more --> 

The good news is you can turn this feature off if you don't like it.

It is a simple task to disable this feature; first go to **Tools -> Options**

{% asset_img "vs-find-options-menu.png" "Options in Visual Studio"%}

Then scroll down to the **Debugging -> General** option on the menu on the left hand side. Then search for thes setting labelled **"Enable JavaScript debugging for ASP.NET (Chrome and IE)"** and uncheck it.

{% asset_img "enable-javascript-debugging-option.png" "Uncheck enable JavasScript debugging"%}

Now the next time you debug an ASP.NET application your default browser should launch rather than the Visual Studio version of Chrome.

If you need this feature again; simply go back into the options and check the checkbox again.
