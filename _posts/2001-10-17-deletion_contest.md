---
layout: post
title: Deletion Contest (2001)
category: Humour
tag: imported
author: adricnet
---

_Kiddies, don't try this at $home_, a fake contest I wrote up in the fall of 2001

I generally have a policy that no disk I've written data onto ever leaves my possession. (Most geeks I know have similar policies or elaborate routines for media destruction.) So, when asked by the store owner (the one who is quitting the business, and therefore greatly reducing my meager income) to get Linux off those machines and put Windows back on them, I was eager to comply (well, the first half, anyway).

The first machine was a notebook with a busted screen that I had set up with a 17" monitor so I could actually dial out (ugh) and check my mail. So, after I checked my mail that morning, I switched to the first terminal, logged in as root, and:

```
  #cfdisk ##remove file system partition(s), write to disk, quit

  #rm -rf /*
```

Which is approximately the Unix equivalent of telling the computer to eat on it's own tail (or root) until it chokes. It tried valiantly, and in the end had to give up because it couldn't unlink imaginary files in /proc/ and /dev. The shell commands were all gone, as were reboot, shutdown, and init.

```
  #ls
  bash: ls: command not found ## so I used echo *

  #reboot 
  bash: reboot: command not found

  #shutdown -r now 
  bash: shutdown: command not found

  #init q 
  bash: init: command not found
```

At this point I disconnected all the cables on the back of the thing, and slid the battery out. It's grinding fan finally started to slow down, and finally it whimpered softly and fell silent.

The next machine was a crappy Cyrix workstation with a full 8 GB of disk. Since, when all else fails, I try to learn something from an experience, I tried to be a little more creative. Having read a little on computer forensics, I have a much better idea than the average idiot about just how easy it is to resurrect data. I intended, more for experimental purposes than practical ones, to try and whip up something quite a bit more troublesome, but didn't take any time to ponder. Think of it as a drill. So, I switched to the root console, removed the partition as before and executed the following command:

```
  #dd if=/dev/urandom of=/dev/hda1
```

and after it ran for a few minutes, that machine was whirling in tighter and tighter circles and became unreponsive even to scrolling commands. (For all those in the audience who didn't gasp upon reading it, immediately turn off their monitor/computer/NOC to avoid it making it into a copy buffer by accident, that command tells Unix to pick a random number, and write it to disk, starting with the first block of the disk. There is no terminating condition, and no numbers of times to perform the action, so it will go until it runs out of disk space or until something really important gets overwritten.)

I think that's not too bad for a few minutes quick work (fire drill) of an amateur sysadmin, but I expect y'all to do better. Get cracking! One liners only please for this event, and let's keep it to shell (no Perl,Python, C). Feel free to time yourself for 5 minutes of pondering, or not. I will craft a CafePress shirt out of the most amusing or educational entry, and buy one for the author. Please get your submissions in by 31 Dec 2001.

First filed by: adric 17 Oct 2001 ~1100 ET BBEdit Lite/ MacOS 

wikied elsewhere by adric around 13:30, 10 January 2007 (PST)
