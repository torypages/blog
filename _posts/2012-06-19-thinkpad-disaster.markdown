---
layout: post
title: Thinkpad Disaster
date: 2012-06-19 09:27:59.000000000 -04:00
categories:
- tech
tags:
- bios
- ibm
- lenovo
- password
- security
- thinkpad
type: post
---
I thought I'd share my recent disaster story with the internet so that perhaps someone else can benefit from my misfortune.

I was playing around with a PXE server a few nights ago so that I could host Linux ISOs on the LAN and boot off them so that I would not have to keep on making various USB Live CDs. Since this is a common occurrence I figured this would be a fruitful endevor.

I had set up the server and it was time to test it out. I shutdown my Thinkpad X60 tablet and attemtted to modify the BIOS for network boot. I then realized that I had a password on there from probably about 5 years ago, a password which I no longer remebered. I thought no problem, I will just pull the BIOS battery and the password would be removed, no problem.

I open up the machine, pull the battery and plug it back in and boot the machine. To my horror, I recieve a message to the effect of, "your device has been tamprered with please enter the BIOS password to continue." That is, the BIOS password I was attempting to remove because I no long remember it. Or in other words, I am locked out of my own machine at the hardware level

Of course I have queried the internet on a possible remedy to my situation but to no avail. I saw one thing about being able to solder some wires to a particular chip and read the password off it via a secondary computer's serial port in combination with a few other minnor electronics but my model was not listed in this tutorial and I could not find the chip upon inspection of the motherboard. I also read that I could have the motherboard replaced to remedy the situation. However, neither of these two solutions were time or money efficient for me and my life right now.

I was quite surprised that this happened but I am glad that my computer savvy friends have not said 'well duh' when I tell them the story. It was an extremely embarassing mistake, though it is hard to blame myself since it would appear that many people too were not familiar with that behaviour, though I am sure this could have been obvious to Thinkpad technicians.

What I do not fully understand is what purpose does this serve? I mean, I assume people place passwords on their computers to protect the data not the hardware and this feature that I have been caught up in does not accomplish much in the way of protecting my data. In fact, I just finished retreiving my data from the hard drive which was in the machine at the time by connecting it to another machine via a SATA-USB adapter. Doing so did require the encryption key I used to encrypt the drive, so, my data was secure, but this security was not provided by the BIOS. I also suppose, and perhaps this already exists, that the hard drive could be cryptographically paired with security chip on the motherboard, and if that were the case I could see the BIOS password making sense. Or maybe this feature was available and I did not realize it. But keep in mind, this machine came pre-installed with Windows XP, a time where I do not recall encrypted hard drives being common place. It seems to me that I have in vain been permanently locked out of an old, both otherwise perfectly good machine.

Bottom line: Be careful when unplugging the BIOS battery of a laptop!
