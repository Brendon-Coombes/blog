---
title: Visual Studio Ignoring your .gitignore?
categories:
 - Development
date: 2016-10-06 22:23:25
tags:
 - Development
---
If you are reading this, it is likely that your Visual Studio is not picking up that you don't want some files/folders going to your repository.

There can be a couple of reasons for this, which we will go over to make sure it is working.

The first thing to do is to ensure that your syntax is actually correct. Have a look at the git documentation to make sure you are doing it right.

If you are sill having an issue, then it is likely to be caused by the ms-persist.xml

Try the following:

- Close Visual Studio.
- Navigate to your .git folder
- Delete ms-persist.xml
- Restart Visual Studio

After restarting Visual Studio, you should see that Visual Studio is now respecting your git ignore file.
