---
title: Getting Started With Home Automation
categories:
 - [Home Automation, Home Assistant]
date: 2023-06-13 09:59:17
tags:
- Home Assistant
---

Until recently I did not live in a home that was viable to set up any home automation. It was a very small and had a terrible layout. This made it hard to think on what to try and automate as everything was just too awkward. I have now moved into a larger house and am looking at all the awesome things you can do with home automation. A lot of the tutorials online assume that you live in the USA, so trying to figure things out from the New Zealand perspective is a bit more challenging!
<!-- more -->

## What is Home Automation?

Home automation is the automatic control of electronic devices in your home. These devices are connected to your home network which allows them to be controlled remotely. With home automation an event or a device action can trigger another device action so you don't have to control them manually. For example you can turn your lights off at a certain time such as when you go to bed.

Sounds pretty cool right!?

I thought so too, I am behind the times and was not aware of how diverse the technology is and to a complete beginner this can seem quite daunting.

The idea of automating the home for us started with purchasing an Amazon Echo. We originally thought that we would just use this to set timers and ask it to play music occassionally. 
We found that one of the most useful things it was able to do, was to control the television. Our Samsung TV has such a small remote that we often lose it in the couch cushions or it is placed somewhere silly and not found for a couple of days. The TV itself doesn't have any phsyical buttons so without the remote you can't do much!

Rather than stressing about finding it all the time, we could just ask Alexa to change the channel, or turn the volume up. It was peak laziness and we loved it!

I began researching what other things we could do with the Alexa and quickly started to see how big the world of home automation was. There were so many factors you needed to look for when purchasing a device as simple as a light bulb.

Every connected device that you purchase for home automation works on communicating through a protocol (Wi-Fi, Bluetooth, Zigbee, Z-Wave) and reporting a status back to a controller or hub. Where it becomes super confusing is understanding what protocols are compatible with which hubs and platforms.

Because many of these devices are purchased as stand alone they come with their own app to control them. This made things even more confusing to understand as who wants 5 - 6 apps to control different parts of your home! This is solved with a home automation platform.

## How do I know which home automation platform to use?

There are a ton of platforms available to choose from. Some of the main ones you may have heard aroud are:
- Google Home
- Amazon Alexa
- Apple Homekit
- Samsung SmartThings
- Hubitat
- Home Assistant

Selecting which one is right for your depends on a few factors. For me the main deciding factors were; device compatibility, user friendliness, current home devices, price, available internet help, and how "hackable" each platform was. 

When buying devices they often will advertise which platforms they are compatible with, the number of devices available in New Zealand that are compatible with each platform really influenced how I decided on my platform. There are heaps that advertise compatibility with Google Home, and Amazon Alexa for example but not many for the others.

Apple Homekit looked like a good option for the user interface, but none of our devices are Apple devices so we wouldn't be able to utilize many of the features.

I also didn't want to spend a ton of money when deciding on a platform, this was essentially because I am dipping my toe into home automation with out knowing if it is something that I will stick to long term. I don't want to spend money on a ton of hardware and find that home automation is not for me.

How much help was available on the internet was also a big deciding factor, and how configurable/hackable the platform is was also of great interest. I love to dig in deep and understand how things work, and solve problems myself rather than having an unknown mystery box that does magic.

All of these factors led me directly to Home Assistant. Home Assistant is an open-source system for smart home devices with a focus on local control and privacy. A huge selling factor of Home Assistant to me is being able to look and see how things work, add my own connections if I ever build any kind of services myself I want to utilize, and the massive amount of help that can be found onlne.

There are pages and pages of people who are trying to get a device that is not supposed to be compatible with Home Assistant to work with it. Some of this goes as far to flash new firmware on hardware that is purchased!

I have an old computer that I have wiped and have set up Home Assistant OS on a VM to give it a go.

## What should I start with?

Almost every article I read talks about setting up a thermostat to control the heating as a good example of something to do. Unfortunately where I live in New Zealand having central heating is very uncommon, and we mostly have woodburners, heat pumps, and electric heaters to heat our homes. This means setting up a thermostat as a first project is not going to work for me!

I am just starting with something simple, which I think is good advice when on embarking anything new. The objective is just to learn how things fit together. I have recently installed a Unifi Dream Machine Pro into my house to run my home network. My simple idea is to have our Amazon Alexa say Welcome Home "Person" when a phone connects to the home wireless that it knows relates to a person living there.

I know this is not an ideal real world situation, as what if I disconnect my phone for other purposes and rejoin? What if I lose connectivity and regain it in the middle of the night?
Because I don't have any sensors (yet!), this feels like a small enough novelty fun thing to learn to understand the how Home Assistant works, and get a smile from my wife and kids when it happens, so I am happy enough with the premise.

Hopefully I am able to figure it out and start to learn how to make my home and automated home.