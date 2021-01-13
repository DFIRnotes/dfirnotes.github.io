---
layout: post
title: Mailbag VM tools question
category: education,malware
author: adricnet
---

## From the Mailbag: a VM tools question
A question came in via mail this week: 
"I see that there is Windows-based security distribution flare-vm. I am wondering the difference between REMnux and flare-vm.

Is it the case, if I want to analyze Windows malware, I should use flare-vm; if I want to analyze Linux malware, I should use REMnux? Would you please help me understand the difference?"

### REMNux Linux, FLARE Windows ?

The essential difference between FLARE and REMnux is the core operating system and another way of looking at it, as you surmised, is the target of your analysis / which toolset is most useful. Often, especially for analysis of hostile code analysts will  want to use not-the-same-platform for the safety of your analysis environment. For tougher problems and behavioural analysis you want the opposite: an isolated and heavily monitored version of a vulnerable system for the code you are studying. From this perspective you might use REMnux (Linux) for static properties analysis of Windows programs and Windows for static properties analysis of Linux and Mac malware and select your platform for behavioural analysis and debugging based on the target. Complex analysis of network code and multi-stage samples might require two or more of these toolsets simultaneously to safely learn what you want about a sample.

Another factor and key difference between eg REMnux and FLARE  is the ease of redistribution. As Microsoft Windows is copyrighted commercial software generally it cannot be legally or ethically redistributed without a clear agreement from Microsoft. Training providers (including SANS, Offensive Security and others) have secured those agreements and can distribute those materials carefully only to registered students. Many educators and consultants (including me) have a strong bias towards community sharing and freely redistributable tools, particularly so in the classroom. FireEye's take (and Rapid7's, malboxes, Detection Lab and others) has been to share freely the tools to build Windows-based machines from either trial licensed or your own purchased Windows license. They don't redistribute the Windows operating system and so you can't get a fully functioning ready to go toolset that way. Tools and distributions based on free or public software don't have this limitation. This and the use of public cloud resources, virtualisation, and Docker containers has led to a lots of options for managing your reversing or DFIR toolset (also red, purple, appsec, ... ) and some interesting possibilities for automation.

Thanks \$caller for the question, hope this helps!

Refs / Linkdump:

_A bunch of awesome public infosec  toolsets and what they are built with_

* FireEye VM tools: Commando, FLARE, Trivial Pursuit use boxstarter and Chocolatey 
* REMnux ( and SIFT and Security Onion ) : use saltstack and Docker  
  * REMnux with Docker tools for malware analysis https://docs.remnux.org/run-tools-in-containers/remnux-containers
* malboxes, metasploitable3, Detection Lab use packer, vagrant, and a $backend
* My old "dfirnotes" paper on DFIR notebooks and automation ideas using Jupyter Notebooks: https://github.com/adricnet/dfirnotes/

