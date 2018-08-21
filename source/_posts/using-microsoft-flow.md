---
title: Using Microsoft Flow
categories:
 - [Microsoft Flow]
date: 2018-08-19 17:05:54
tags:
 - Microsoft Flow
---

Recently the office has been competing for the top relic time in levels on Crash Bandicoot N-Sane trilogy. Due to many reasons, we ended up with an Excel Spreadsheet to store and compare our times with each other. I found myself wanting to know when the data in that spreadsheet had changed. I have wanted to try out using Microsoft Flow for quite a while, and decided that this was the perfect opportunity to get my data rather than manually checking the file for changes.

<!-- more --> 

## Backstory

In the office, a few of us have all picked up the Crash Bandicoot N-Sane Trilogy. We all think we are the best at the game and have become quite competitive over the relic times on the levels.

We were attempting to use the leader boards on the [Crash Bandicoot website](https://crashbandicoot.com/leaderboards) but we ran into quite a few problems with this. The first problem was that we can not search by player name on the leaderboard, we would manually have to click through each page of 20 times looking for our time on each level.

The second problem was that the leaderboards seem to be wildly unstable, a lot of the time just attempting to use the web site to browse a page or two deep would cause the API containing the data to crash [no pun intended].

{% asset_img "crash-bandicoot-api-failures.png" "Crash Bandicoot API failures"%}

From there we tried to call the public API directly with code to pull the times out, but it seems that querying the API also is not stable as quite a few of the requests fail when going through the pages. Even when it did work, it seems to be rate limited as I became unable to access the API from my home computer after attempting to pull a large amount of data, but was still able to access it via my mobile using my mobile internet rather than my home internet connection.

The final nail in the coffin for attempting to get the data from the API directly was that one person in our team had a Nintendo Switch rather than a Playstation 4, Xbox One, or downloading it on Steam. I initally didn't think this would matter, but leader boards were not included at all as part of the Nintendo Switch version. So the Switch user's times would never be included in the API data anyway.

So another one of the team decided to store our times in an Excel spreadsheet. We all spent half an hour that evening entering all our times for each level manually. When we all had our data entered, we could see who the winner of each level was. The competition could finally commence.

Using this new method, I discovered a new issue. If another member of the team added in a new time after work, I wouldn't see it until I opened it the next day to enter my new times. This wasn't acceptable to me as I wanted to focus on the same levels that the others were playing on, as this makes the competition more fierce.

Enter Microsoft Flow

## Microsoft Flow

I decided to dive in to Microsoft Flow to create a Flow that would give me a notification whenever any of my work mates edited the shared excel file. This way I could go and check what levels they are winning at, and focus my energy there.

The process for creating a Flow was surprisingly easy. If you have a Microsoft account, go to [https://flow.microsoft.com](https://flow.microsoft.com) and sign in. You are automatically put on the free plan, which gives you a few 100 free Flow runs per month! This is much more than I will need for this little project. If you need more than that; there are paid plans available too.

I started my flow off with the "When a file is created or modified (properties only)" SharePoint trigger. The file we are each editing is stored on Microsoft Teams; which is backed by SharePoint so this trigger is perfect.

All I need to do is select the correct Site Address, point it at the right library, and finally select a folder within that library.

{% asset_img "crash-bandicoot-flow-trigger.png" "Flow Trigger creation"%}

This is a great start, but if I create a notification as my next step, I will be notified every time that a file is created or modified in that folder! In my case, that wouldn't be too much spam as we don't use the folder I have selected for much at this stage. But to future proof myself; the next thing I did was create a condition to check the filename was the file I was expecting.

You can feed in 'dynamic data' into each of your actions that you create in Flow. What this means is the output for each of your actions can be used as input in the following actions. The output of the trigger I have created has a "Filename with extension' property; which I can use to check if it is the file that I am expecting.

{% asset_img "crash-flow-dynamic-content-editor.png" "Flow Dynamic Content Editor"%}

I have created my condition now to check if the file name is equal to `Crash Bandicoot Times.xlsx`. If it is, then I want to receive a notification; and if it isn't I don't want to do anything. So I am going to leave the 'No' path blank, and continue down the yes path.

On the "Yes" path I have created a new action "Send me a mobile Notification" and entered the text of the notification that I want to receive. In order to receive this notification I need to have the Microsoft Flow app installed (available on [Android](https://play.google.com/store/apps/details?id=com.microsoft.flow), [iOS](https://itunes.apple.com/us/app/microsoft-flow/id1094928825?mt=8) and [Windows](https://aka.ms/flowmobiledownloadwp)), and be logged into the Flow App with the same account that I have created my Flow with.

My Flow is now finished, here is what the finished Flow looks like:

{% asset_img "crash-flow-complete.png" "Completed Flow with Notification"%}

## Result

Now that I have completed my Flow and I have the app downloaded on my phone, I made a change to the document to check if I am notified of that change.

And a couple of minutes after I made the change, I received the notification on my phone:

{% asset_img "crash-flow-run-notification.png" "Mobile Push Notification Received"%}

This was a really simple set-up and has really opened my eyes to how useful Flow can be to automate simple tasks that would have previously taken custom development to do. I have yet to try and do anything complex with Flow, but from my initial look at it; I feel it will be useful for small simple tasks; custom development will still be required for any complex or large tasks.

As microservices and separated components become more and more popular, it is likely that Flow can slot into these chained together components as keeping them small and simple will be much easier. I think it is likely that in more and more projects we see in the future, that Flow is a portion of the solution; but not the entire solution. Flow will be good for managing simple and small business rules in applications. As these rules change, the end-user will be able to re-configure the Flow themselves without needing to invest in a Software Engineer to re-develop anything.

It is in areas like that I think we will see the benefits, and main usage of Flow.