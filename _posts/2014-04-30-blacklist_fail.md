---
layout: post
title: Blacklist Failures
category: File Analysis
tag: imported
---

As I've [mentioned](http://f.adric.net/index.cgi/wiki?name=Email+EXEs+and+free+tools) [before](http://adric.dreamwidth.org/251488.html) one of the things I'm self-studying these days is file analysis. The chosen text is the most excellent *Practical Malware Analysis* (red with the alien autopsy cover). The authors include lab exercises to demonstrate the analysis techniques from each chapter and they are freely available, so buy a couple copies of the book, such as from the publisher's [site](http://www.nostarch.com/malware).

I'm trying to dig back in again between around the classes I'm teaching and taking this year. I have a new workstation at home that I'm still installing things onto. For my own studies, some work stuff, and a lesson I'm preparing I needed John the Ripper, a password security program. Actually, it's clearer to say [John](http://openwall.com/john/) is a password cracker, and it's one of the best.

I downloaded the malware [samples](http://sourceforge.net/projects/pmalabs/) from th *PMA* book fine with Chrome, but it wouldn't let me download John the Ripper. It not only identified the john for Windows files (standard and community) as malware (fine, I guess) but prevented me from opting to ignore the warning. My option was to disable all of the browser (OS) anti-malware and anti-phishing protections in preferences. I was able to download other security tools Redline and Memoryze fine though. And the malware labs collection ... 

Firefox let me download John just fine, so I did so, immediately. The fruits of that are over at a page full of john the ripper benchmarks I've run over the last few years, [here](http://f.adric.net/index.cgi/wiki?name=john+bench).

As I said Chrome let me download the self-extracting archive of malware without issue. I expected some backtalk from my system's anti-malware so I created a folder and added it to the exclude list. I extracted the malware with 7Zip into c:\Malware and was able to look at the files, though I didn't start in on any of the labs yet.

Meanwhile the anti-malware panic'd over the archive file in my Downloads folder. It generated temporary copies, databases entries, and alerts for each of the dozens of samples in the file and raised the terror alert level to Elmo. I negotiated with it and agreed it could remove all the "dangerous" files it had detected and it spent a good half an hour processing that request and getting happy with itself again.

Lessons we could learn from this:

* Trying to list all of the bad things (blacklisting) never works. Besides the basic philosophical problem, you have to update the list, and synchronize all of the things that need to check for badness or it just easily routes around you.
* Anti-malware that relies on blacklists is, QED, not very useful.
* Users don't respond as desired to blocking an activity outright. In almost every environments they have options. 
* Browser makers have it rough. Between this latest affront and the continued UI horrors of self-signed TLS certificates (Hi Larry!) I'm pretty upset at the Chrome AND Firefox developers in spite of all of their great works.
* Newer computers are faster. Newer phones are faster. Some new phones are faster than old computers.


> Written with [StackEdit](https://stackedit.io/).
