---
title: Posh Git On Mac
date: 2018-08-13 12:34:34
categories:
 - [Git]
 - [Development]
 - [bash, Extensions]
tags:
 - Git
 - Posh Git
 - Bash
 - OSX
---

I have recently purchased a Mac in order to create some IOS apps. This is the first time I have used Apple products outside of basic use in school many years ago. I have started installing various development tools that are available to get me going. One thing that I found was missing was my beloved Posh-git. Posh-git is a great powershell extension on Windows that makes seeing what is happening in git much easier. I want that same functionality on the Mac.
<!-- More -->

## What is Posh Git

Posh Git is an amazing PowerShell module that integrates Git status information into the prompt within powershell. Posh git also provides tab completion for git commands within Powershell too.

Here is a sample view of what it adds to the prompt:

{% asset_img "posh-git-prompt.png" "Posh Git Prompt example"%}

You can read all about Posh git and how to use it on the [Github repository here](https://github.com/dahlbyk/posh-git). Have a read through of the README to understand what each of the 8 numbers mean and colours and symbols represent.

Having this information available so quickly and easily will speed up your Git commands by quite a bit.


## Getting it working on Mac

To get this working on Mac, we have a couple of options:

1. We can install PowerShell on Mac
1. We can try and replicate the same functionality in bash.

While I think it is great that you can now install PowerShell on more operating systems than just Windows, I really want to get the authentic Mac experience when trying to use a Mac for development. So if I can avoid it, I would rather not install PowerShell on the Mac, as it feels like it is cheating and just making the Mac more like Windows.

So I decided to try to get it to work with bash, and I found that it was easier to get working than I expected.

[Posh-git for Bash](https://github.com/lyze/posh-git-sh)
