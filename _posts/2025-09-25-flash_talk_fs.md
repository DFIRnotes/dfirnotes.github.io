---
layout: post
title: Flash File Systems
category: cybersecurity, evidence
author: adricnet
---

Flash talk idea: Filesystems in Cybesecurity Investigations

_Feedback appreciated! Will update the post as we outline more_

## Why might you care about filesystems 
  in 02025 CE ?

* "in a investigation, details matter" -Dr. Emma Watson, OBE, Bill and Ted's Bogus Journey
* "It's turtles all the way down" _Discworld_ in game text. Retrieved 1990.
* "Cowabunga!" -Vanilla Ice, Teenaged Mutant Ninja Turtles 2: The Secret of the Ooze

* Endpoints: more of these than you may think
  * security appliances and file transfer systems have filesystems
  * phones, tablets, ... watches, badges, and glasses (!) have file systems
* Serverless, chatbots, APIs: maybe filesystems less important
  * although ```../``` won't stop and that's _a filesystem path_ **polite cough**

## Limits and shapes user and miscreant activity:
* how much and what kinds of things they can store
* permissions and acccess controls may restrict access
  * starting with read-only mode!
* Modern operating systems have many self-defense features, including in filessystems 
  * Mac SIP, Linux SELinux & AppArmour, Windows ... so many things

## Limits and shapes the available evidence for analysis
* Filesystem type and configurations vary, with different evidence (or precision) available
  * Does it record Access time (atime) ? Sometimes ...
  * Does it record file/folder creation (B) ? Sometimes ... 

## Less volatile, better chance of getting it sometimes
* versus: other host data: cache, memory, swap
* different tradeoffs and evidence vs network capture

## FS are the primary data source for :
* Timestamps and local activity
* Recovered file artifacts: malware, logs, archives, configuration, extortion demands
  * browser history and downloaded files
  * office app history and temp files
  * malware and misused tools
* Deleted or moved files and folders may be recoverable
  * Usually requires you to get the entire file system with forensics tools
* Live (running) file systems may give you access to volatile data
  * won't be there in a "dead disk" analysis (/proc, /sys, remote mounts )

## Two examples: NTFS and [some flash file system]
* NTFS: almost every Windows system you will see ... vs DOS FS and 'rare birds'
* [flash file systems]: phones, tablets, devices ... initramfs anyone ?
  * how about my watch  ?

### NTFS in one slide

### a FFS in one slide

## Questions / feedback

### Refs (???)

_Feedback appreciated! Will update the post as we outline more_
