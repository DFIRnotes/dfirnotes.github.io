---
layout: post
title: Emily's Photos
category: File Analysis
tag: imported
author: adricnet
---

*Emily* sent me so many copies of this executable in the last couple days that I decided to take a look:

<img alt="emily's emails in thunderbird" src="http://atlbbs.com/filetypes/emily-photos-tbird.png"/>

Saving all of the attachments and unzipping them manually, hashing them:

```
    $ ls -l photo*exe photo*/*exe
    -rwxr-xr-x@ 1 adric  staff   49664 Sep  8 08:42 photo 2.exe
    -rwxr-xr-x@ 1 adric  staff  114688 Sep 26 10:59 photo 2/photo.exe
    -rwxr-xr-x@ 1 adric  staff   54272 Sep 29 02:36 photo 3/photo.exe
    -rwxr-xr-x@ 1 adric  staff   52224 Sep 26 05:48 photo 4/photo.exe
    -rw-r--r--@ 1 adric  staff   49664 Sep 29 16:58 photo.exe
    -rwxr-xr-x@ 1 adric  staff   56320 Sep 24 12:07 photo/photo.exe

    $ openssl sha256 photo*exe photo*/*exe
    SHA256(photo 2.exe)=  7e65209964002774c052dd55e05ee4636ec9a3bcad7dc1f7f6688703b4943469
    SHA256(photo.exe)= 7e65209964002774c052dd55e05ee4636ec9a3bcad7dc1f7f6688703b4943469
    SHA256(photo 2/photo.exe)= 133c3a1eb172896a5c5f29f4292420839dd9c080740dba4fd419222264cd5aa2
    SHA256(photo 3/photo.exe)= c45e90976e5d98ecd982dc7d3414cc3f992f1ebe6f176288493e66b7f376d168
    SHA256(photo 4/photo.exe)= 4aac94d293955a483514059434a7d1202c83e95178998733fd9311f620370783
    SHA256(photo/photo.exe)= 937687e116372fc3c1ee825fe6abf6e7e86e7b4f626235bff0c8bc261a38fb1a
```

I may not be *Emily*'s only correspondent, sadly. Public results for these files' hashes are pretty damning. For example if we query the earliest in the set, *photo/photo.exe* with SHA256 *937687...* on [VirusTotal](https://www.virustotal.com/en/file/937687e116372fc3c1ee825fe6abf6e7e86e7b4f626235bff0c8bc261a38fb1a/analysis/) we see detection ratio of 40/55 scanners, plus 4 votes against it (not many VT users vote). checking the hash of the most recent sample, good ol' *7e652099...* [shows](https://www.virustotal.com/en/file/7e65209964002774c052dd55e05ee4636ec9a3bcad7dc1f7f6688703b4943469/analysis/) even higher detection rates 42/55 and 5 no votes. The larger sample *133c3...* has lower detection on [VT](https://www.virustotal.com/en/file/133c3a1eb172896a5c5f29f4292420839dd9c080740dba4fd419222264cd5aa2/analysis/) but still looks like a bad thing with 33/55 detecting malware and 3 votes against.

There's behavioural data for some of these samples aggregated from multiple analysis runs. They don't tell me much other than that the analysis system is using VirtualBox.

Monkeys?
--------
 
Another sample submitted this week is interesting because it's a CIL ("Dot Net framework") class assembly, apparently from Visual Basic Dot Net according to *pedump* and *file*:

```
    === Packer / Compiler ===

    MS Visual C# / Basic .NET
    
    $ file transact_store/transact_store.exe 
    transact_store/transact_store.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows
```

This opens up the opportunity to try out some other tools and *monodis* is able to quickly render the executable (back) into verbose pseudocode and disassembly:

```
    $ monodis -h
    monodis -- Mono Common Intermediate Language Disassembler
    monodis transact_store/transact_store.exe > transact_store/monodis.txt
    
    $ wc -l transact_store/monodis.txt 
      4575 transact_store/monodis.txt
```

Even without knowing VB.net there is a lot of information about the functions of the binary in that output. For instance, there is little question that it has a GUI.

```
    $ grep module transact_store/monodis.txt 
    .module QjvZVBkVM.exe // GUID = {5AA55973-4E89-454C-9679-12B7C1FA3224}
    .module extern 'kernel32.dll'
    
    $ grep extern transact_store/monodis.txt 
    .assembly extern System.Windows.Forms
    .assembly extern mscorlib
    .assembly extern System
    .assembly extern System.Drawing
    .module extern 'kernel32.dll'
```

A [VT](https://www.virustotal.com/en/file/c82d2ce33e9e089eac427fd1101755b89112c7ca512489eddac8faaebfa97522/analysis/) lookup of this sample by hash shows that most engines consider it a nasty trojan with 40/55 detecting and 21:0 votes against from the community. It's listed using the internal name of 'QjvZVBkVM' from the PE headers, but our sample's transact_store file name is listed in the VT Additional Information tab as one of its aliases.

Since I don't have a full on Cuckoo sandbox setup yet at home, I'll check with the public one at [Malwr](http://malwr.com) and in fact they have two analysis of these file (hash) to peruse. You need a free account to be able to search their database, no doubt out of self-defense of the network and computing resources.

The Cuckoo & community rules in that Malwr.com analysis [run](https://malwr.com/analysis/Y2I3YTk1NmVjZGJmNDViNDk1ZmMzZDM1YmNhN2M5YmM/) tell us some interesting things about our sample, namely:

* File has been identified by at least one AntiVirus on VirusTotal as malicious
* Installs itself for autorun at Windows startup

For further detail their report has extensive static and behavioural analysis, tracking the creation of a sub-process and the persistence attempts. Malwr's sandbox didn't log any network activity.

More photos and a nice behavioural analysis from Malwar
------------

Yesterday I got another missive from *Emily* with different characteristics:

```
    $ pedump photo23/photo/photo.exe 
    ...
    # StringTable 000004b0:
    FileDescription     :  " "
    FileVersion         :  "20.105.55.41"
    InternalName        :  "GJGUVVLSj.exe"
    LegalCopyright      :  "XumOCGfQ (C) mLZGRFmQNb"
    OriginalFilename    :  "GJGUVVLSj.exe"
    ProductVersion      :  "20.105.55.41"
    Assembly Version    :  "72.72.11.13"

    === Packer / Compiler ===

     MS Visual C# / Basic .NET
```

Pulling the hash and checking for public intel ...

```
    $ openssl sha256 photo23/photo/photo.exe 
    SHA256(photo23/photo/photo.exe)= 8734073b26307ebcb9ca04f43c82f847a5f935eaebc5497f3f3675702b18d9df
```

nets us a Malwr (Cuckoo) [analysis](https://malwr.com/analysis/OTc0OTFkNjM1N2JkNDUyZWIyNWYyNzc4ZmI4MDNlMzY/) with some interesting details. In the Behavioural Analysis on page 1 of 5 we can visually scan for file activity by colour (beige?) to find interesting tidbits like *photo.exe* reading XML configs from windows and then writing a config file

```C:\DOCUME~1\User\LOCALS~1\Temp\photo.exe.config``` .

On page 2, still just looking at Files (beige?) we see that it wants but fails updates/creates new version of several system DotNetFramework and CLR security configuration files:```C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\config\security.config```

Later on there is some complicated and scary looking stuff where device ids are scraped from the registry and a bunch of encoded content (complete with blocks of nulls) are read from the device. Then it checks the reg keys and the filesystems again for itself, as if to satisfy itself on success.

Which might not sound too bad, but after that it writes some of the same encoded stuff to the *photo.exe* file, calls a bunch of cryptography libraries (to decrypt / check signatures?) and then proceeds to make the changes to the DotNet security configs it tried earlier and succeed?!

After a lot more registry reading and some specifics with DotNet form and security libraries that kinda look like it was looking for a specific version of one (probably not good) it then creates a new process and loads the contents of *photos.exe* from disk with the encoded blocks in play here and the PE executable into memory for execution.

If we now review the complete list of touched/sought files on the Overview page we have a much better understanding of what it wanted with those files, and some big hints about some of the bug's capabilities, techniques, and intent. So you not only have some raw indicators to plugin to a tool but some context to make them useful.

A little understanding and sometime a lot of searching can deliver some really valuable insights into samples with these public tools without you needing to do any reversing or hex maths yourself, so give it a try!
