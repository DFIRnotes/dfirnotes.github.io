---
layout: post
title: Email EXEs and Free Tools
category: File Analysis
tag: imported
author: adricnet
---

Since I don't really want someone else's pictures and didn't order anything from FedEx this week so I could safely ignore the odd emails coming in with subjects like "Re:" and "Your package is available for pickup" and zip file attachments. But I'm a curious sort ...

These mail campaigns have been running for awhile and I've actually looked at them a little bit earlier this year. As you can quickly see the zip files in these contain only one file, an executable named the same as the zip: <em>FedEx_Invoice.zip</em> contains <em>FedEx_Invoice.exe</em> and <em>My_summer_photos_in_Egypt_2011.zip</em> contains <em>My_summer_photos_in_Egypt_2011.exe</em>, but before we go any further...

<h3>Warning!</h3>
<em>These files are dangerous and should be handled with due care by trained professionals or the local equivalent in a safe lab environment. Don't run suspicious files or documents normally! Don't get infected!</em>

There, now that that's out of the way I can explain how I poked at these things on my Mac and Linux machines with some regard for my safety and free tools, and what I found out about them.

Did you a get suspicious link rather than a file attachment? No need to feel left out of the fun! Right this way to [Free URL analysis tools].

<h3>is it bad? clam AV, virustotal</h3>

Since we have a suspicious file the first thing to check is "is it a virus?" Unfortunately that's a notoriously difficult question to answer, so we'll start with an easier one: "What do anti-virus programs think of this file?". We can start with [ClamAV](http://www.clamav.net/lang/en/) a freely available, cross platform virus scanner:

```
$ sudo freshclam
$ clamscan My_summer_photos_in_Egypt_2011.zip 
LibClamAV Warning: ***********************************************************
LibClamAV Warning: ***  This version of the ClamAV engine is outdated.     ***
LibClamAV Warning: *** DON'T PANIC! Read http://www.clamav.net/support/faq ***
LibClamAV Warning: ***********************************************************
My_summer_photos_in_Egypt_2011.zip: OK

----------- SCAN SUMMARY -----------
Known viruses: 1168645
Engine version: 0.96.5
Scanned directories: 0
Scanned files: 1
Infected files: 0
Data scanned: 0.67 MB
Data read: 0.32 MB (ratio 2.06:1)
Time: 4.956 sec (0 m 4 s)
```

Clam AV doesn't find anything bad about this file... By the way, clamscan can look inside zip files without you needing to tell it to these days and you can configure that and many other options for file types and scan settings on the command line. Run <em>clamscan --help</em> to see.

Clam AV is one of dozens of scanners available, some free and some expensive. Maybe we should try a couple more? How many do you already have installed? Thankfully, there's [VirusTotal](https://www.virustotal.com/) a very cool site that runs samples against a large set of anti-virus engines. Let's give that a try instead of downloading and installing another AV:

<h3>File already analysed</h3>
  
       
          This file was already analysed by VirusTotal on 
          <strong><span id="last-analysis-date">2012-03-15 18:19:23</span></strong>.
      
      
          Detection ratio:
          <strong><span id="detection-ratio">21/43</span></strong>
      
      You can take a look at the last analysis or analyse it again now. 

21/43 may sound like a lot but it's only half. ClamAV (still) doesn't detect this one (or the other). More interesting is that when I first submitted that file to VT yesterday it only registered in 6 or so. Virus scanner makers have agreements with VT (and others) to feed them new samples so they can keep improving their engines and signatures. Just remember that scan results like this are time specific and will change. So it's definitely a malicious file, very interesting. What else can we find out with the tools we have?

<h3>static basics: file, unzip, strings</h3>

On Unixy systems the built in command for "what is this thing?" is <em>file</em>. <em>file(1)</em> compares the first few bytes of the file to a list of file types (mime magic) it keeps. It's easily fooled but still very handy to get some idea what you might be dealing with:

```
$ file My_summer_photos_in_Egypt_2011.zip
My_summer_photos_in_Egypt_2011.zip: Zip archive data, at least v2.0 to extract
```

Since it's a zip archive we can take a peek with another standard utility, unzip from the [InfoZip](http://www.info-zip.org/) project:

```
$ unzip -l My_summer_photos_in_Egypt_2011.zip
Archive:  My_summer_photos_in_Egypt_2011.zip
Length      Date    Time    Name
---------  ---------- -----   ----
360448  2012-03-14 11:18   My_summer_photos_in_Egypt_2011.exe
---------                     -------
360448                     1 file
```

As noted above each zip has an exe in it. Exe is the file format for Windows programs. We can look at the files with a couple more tools before getting help with the analysis. Open 'er up with unzip and file what we get out:

```
$ unzip My_summer_photos_in_Egypt_2011.zip
$ file My_summer_photos_in_Egypt_2011.exe 
My_summer_photos_in_Egypt_2011.exe: PE32 executable for MS Windows (GUI) Intel 80386 32-bit
```

Yep, file says it is a Windows program. That matches up with the exe name and evidence is mounting that this is a Windows program they mailed me. Let's look at the <em>strings</em> in the executable for hints: 

```
$ strings -n8 My_summer_photos_in_Egypt_2011.exe | grep http
$ strings -n8 My_summer_photos_in_Egypt_2011.exe | head -5 
xhP*yhQ&gt;
o_a	rKY=2O
GetStdHandle
CreateEventA
LoadLibraryExA
$ strings -n8 My_summer_photos_in_Egypt_2011.exe | tail -5 
MSASN1.dll
MessageBoxA
USER32.dll
jhertauiklservnmiasads.dll
lxndertlopuytxca
```

Strings looks for character sequences in the file of the specified length. Although we didn't get any URLs (drat!) we definitely see some Windows system stuff which supports file's finding that this is a Windows program.

... which is about all <strong>I</strong> can get out of this file without a lot more software and skills (debuggers, dynamic malware analysis) than I have handy. Enter Anubis..

<h3>Anubis: Analyzing Unknown Binaries</h3>

[Anubis](http://anubis.iseclab.org/?action=home) is a "is a service for analyzing malware. Submit your Windows executable and receive an analysis report telling you what it does. Alternatively, submit a suspicious URL and receive a report that shows you all the activities of the Internet Explorer process when visiting this URL. "<br /><br />I loaded one of the suspicious Windows executables we found into Anubis and went back to my homework for a bit. Anubis gives me the following status and a nice animated progress bar that moves along (better if you let them run JavaScript):
<h1 class="bodystart">Task Overview</h1>
  
    <table><tbody><tr>
      <td><strong>Task ID: </strong></td>
      <td>195dcac886fea4bd4393f7775147c7179</td>
    </tr>
    <tr>
      <td>
        <b>
                  File Name:
                </b>
      </td>
      <td>My_summer_photos_in_Egypt_2011.exe</td>
    </tr>
    <tr>
      <td><strong>MD5: </strong></td>
      <td>e196d5ad2f241998dfb1dafad6a4ebc6</td>
    </tr>
    <tr>
      <td><strong>Analysis Submitted: </strong></td>
      <td>2012-03-15 19:34:13</td>
    </tr>
        <tr>
      <td><strong>Analysis Started: </strong></td>
      <td>2012-03-15 19:34:13</td>
    </tr>
            <tr>
      <td><strong>Time Remaining: </strong></td>
      <td>7 minutes and 39 seconds (0 jobs in queue)</td></tr></tbody></table>

They autorefresh you to the status page after your submission is processed in and the URL includes a cookie so that you can get [back](http://anubis.iseclab.org/?action=result&amp;task_id=195dcac886fea4bd4393f7775147c7179) if you need to. After a few minutes pass the report is ready and we autorefresh to the report summary which offers us the report in several friendly formats and a packet capture. Very cool! The reports are an interesting read to be sure, detailing all of the activity the program attempted when executed including its network communication (the <strong>p</strong>acket <strong>cap</strong>ture).

<h3>Pcap in Wireshark</h3>

Load up the traffic.pcap file in Wireshark to take a peek. We can see just by scrolling through that there is some HTTP traffic from the suspect host to an outside server and two URLs are requested. Looks like they are tracking the requests a little bit as a number (affid) is sent in, maybe an affiliate id? Anyway, I want to see that new file!

![http gets](http://f.adric.net/index.cgi/doc/trunk/analysis/http%20gets.png) FIXME image link

We can use one of Wireshark's nifty features by going to Statistics/Conversation List/TCP to have it generate a list of all TCP conversations in the open capture. For this one there are only two:

![tcp conversations](http://f.adric.net/index.cgi/doc/trunk/analysis/2%20TCP%20Conversations.png) FIXME image link

of which the second one is far more interesting. It looks like it downloaded another exe file And if we tell Wireshark to follow that stream we can see it:

![Follow TCP Stream](http://f.adric.net/index.cgi/doc/trunk/analysis/Follow%20TCP%20Stream.png) FIXME image link

Use the "Select stream" choicebox to pick only the response stream and Save As to get another file to analyze. I named mine ober.exe as it only seemed fitting. A quick ':10dd' in vi knocks out the HTTP response headers (quite obvious at the top of the file) and we have a second potential Win32 executable in 2-ober.exe.

<h3>Another exe file?</h3>

```
$ file 2-ober.exe 
2-ober.exe: PE32 executable (GUI) Intel 80386, for MS Windows
$ sudo freshclam
$ clamscan 2-ober.exe 
2-ober.exe: OK
$ strings -n 8 2-ober.exe | grep 'M:'
M:\eahxkgmek\teirsdtq\suQgjEIHE\XsmeNbYntrct\nqbrhad.pdb
```

This definitely another Windows program and although the standard library (DLL) names and calls are present as before we see an obvious, if weird, file path (who has an M: drive?) and a big chunk of uninterrupted data at the end, possibly encrypted? Hmm.. 

VirusTotal for this [file](https://www.virustotal.com/file/577d2d2d104d9a7caf7c9046cf75f918c12bff5088d38fc7ac0ae1210a6dc31c/analysis/1332090272/) today gives hit on 20 / 42 engines. Half of the engines say this file is something pretty bad, possibly some kind of Trojan or backdoor. Part of the current fun of the anti-malware industry is that there is little agreement on naming things, so almost every one one of those 20 engine hits call it something different, though they all say it's bad!

And here's the Anubis [link](http://anubis.iseclab.org/?action=result&amp;task_id=16a0f8528dcd54404a9ea8d9ab5645644&amp;call=first) for 2-ober.exe. Oddly enough Anubis didn't detect any risky activity during it's analysis, but as they say "This does NOT imply that execution of this executable is safe."

<h3>Wrap-up</h3>

Just look at what we can find out about some suspicious EXE files with free tools and some Unix command line utilities! Also worth taking to heart is that you should always use as many tools as you can to analyse any artifact as the output and findings will differ. 
In a live incident you can use the Anubis and virtustotal reports to inform your organization's response to a threat and plan eradication and recovery strategies. In an organization that doesn't have dedicated malware analysts or fancy tools that can make a huge difference.

<h3>References</h3>

Many of the tools and techniques (especially the Wireshark tricks) used here are discussed in Richard Bejtlich's ([http://taosecurity.com/]) excellent, canonical NSM book <em>Beyond Intrusion Detection: The Tao of Network Security Monitoring</em> as well as in various SANS analysis courses:

* [SEC 503](https://www.sans.org/security-training/hacker-techniques-exploits-incident-handling-40-mid)
* [SEC 504](https://www.sans.org/security-training/hacker-techniques-exploits-incident-handling-40-mid)
* [FOR 572](http://www.sans.org/security-west-2012/description.php?tid=5121)
