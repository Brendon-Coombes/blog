---
title: Using Microsoft Flow
categories:
 - [Microsoft Flow]
date: 2018-08-20 17:05:54
tags:
 - Microsoft Flow
---

Recently I found myself needing to use an excel file as a source of data for a website, and wanted to try out using Microsoft Flow to get my data rather than manually checking the file for changes and pulling the data myself.

<!-- more --> 

## Backstory

In the office, a few of us have all picked up the Crash Bandicoot N-Sane Trilogy. We all think we are the best at the game and have become quite competitive over the relic times on the levels.

We were attempting to use the leader boards on the [Crash Bandicoot website](https://crashbandicoot.com/leaderboards) but we ran into quite a few problems with this. The first problem was that we can not search by player name on the leaderboard, we would manually have to click through each page of 20 times looking for our time on each level.

The second problem was that the leaderboards seem to be wildly unstable, a lot of the time just attempting to use the web site to browse a page or two deep would cause the API containing the data to crash [no pun intended].

`TODO INSERT PIC

From there we tried to call the public API directly with code to pull the times out, but it seems that querying the API also is not stable as quite a few of the requests fail when going through the pages. Even when it did work, it seems to be rate limited as I became unable to access the API from my home computer after attempting to pull a large amount of data, but was still able to access it via my mobile using my mobile internet rather than my home internet connection.

The final nail in the coffin for attempting to get the data from the API directly was that one person in our team had a Nintendo Switch rather than a Playstation 4, Xbox One, or downloading it on Steam. I initally didn't think this would matter, but leader boards were not included at all as part of the Nintendo Switch version. So the Switch user's times would never be included in the API data anyway.

So another one of the team decided to store our times in an Excel spreadsheet. We all spent half an hour that evening entering all our times for each level manually. When we all had our data entered, we could see who the winner of each level was. The competition could finally commence.

Using this new method, I discovered a new issue. If another member of the team added in a new time after work, I wouldn't see it until I opened it the next day to enter my new times. This wasn't acceptable to me as I wanted to focus on the same levels that the others were playing on, as this makes the competition more fierce.

Enter Microsoft Flow

## Microsoft Flow

## Result
