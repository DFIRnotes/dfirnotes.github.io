---
layout: post
title: Poke Science
category: Education
tag: imported
author: adricnet
---
**How to Learn About $SYSTEM Security**
_General techniques for developing better understanding about security functions and asserting confidence in them_

Get one
-------
Get a copy and install it. A few times. Better not to use a real one, a production one, or the one your homework is on.

* Virtual machines and app containers
* Evaluation licenses & dev programs
    * betas and demos

Take notes each time you install. Develop patterns that make sense to you and still take notes about your install choices for things like accounts, network settings, disk and database layouts, project schemes, etc.
    
Cage it
-------
Build a lab around it (or put it in one). Think about snapshots and resets as you design the test environment. Also consider safety.

* Internal monitoring
    * debug hooks, utils like dtrace, ProcessMonitor
    * logs and events
    * drive,memory forensics?
* External monitoring
    * network
    * hardware / vm
    * other emmanations?
* Tools
    * Things to poke with
    * Holes ?

Read
----
* Books: Of course any books about the current version of SYSTEM and
    * Books about the technologies, protocols used
    * Books about the previous system/versions
    * Books by the system's authors or about the SYSTEM history
    * Books about the competition to $SYSTEM
* Manuals : upstream/vendor manuals
* Source: Read the source code or examples
* Online: Read wikis and blogs & search for answers
* Experts? What do any experts say?

Poke
----
Try some things. Note anything odd or interesting.

* Run through the tutorial. 
* Click all the buttons (Exploratory Testing?)
* Load, save, export data.
* Scan and probe the perimeter
* Examine output, results

Record
------
Record data and any findings for experiments and publishing, and also so you don't forget.

* Text for commands, output
* Pictures for screensnips, output
* Video?
* Disks, memory, logs, traffic (forensics data)

Experiment
----------
Do science on it! Don't trust, test. If you can't test it, how much should you trust it?

* Observation
* Hypothesis : Disprovable idea.
* Experiment : How would you disprove it? Try it.
* Result : Disproven? New observations -> Science it.

Publish
-------
Share what you've found (when you can)

* Blog, wiki
* Code sharing
* Papers and Presentations
    * Simon "How to Write Great Research Papers"
    * 15 minutes and done book

