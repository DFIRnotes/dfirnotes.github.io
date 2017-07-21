---
layout: post
title: GIFAR's Magic Mimes Filed in 8 by 3
category: analysis
tag: brownbag
author: adricnet
---

GIFAR's Magic Mimes Filed in 8 by 3: File types, identification techniques, and their weaknesses to attack

(raw slides text)

File types?
How do computers tell what kind of thing something is?
How do analysts identify artifacts?
What vulnerabilities do these techniques have?
a few examples:
live and raw bytes of common files types:
html/xml/text
pl/py/rb/sh
png, PDF, gif, jpg, bmp 
exe, doc, elf, pe
avi, mov, flv
jar/zip/docx, tar 
html and xml - structured text
pl/py/rb/sh- script text
PDF, png - vector and raster graphics 
exe, ELF, PE – programs and binaries, and a doc
avi, mov, flv - video containers 
jar/zip/docx/tar - archives 

the basic schemes
  file name & extensions (trust it)
  file metadata (tag it)
  file(1) magic (check it)
  What about icons? 
file name & extensions (trust it)
Eight dot three
[short title] . [three letter extension]
Extensions determine type for Win, Mac!
VFAT LFN kludge
Progra~1/goodfile.exe ?
Docume~1/badfile.com ?
file metadata (tag it)
 resource forks and EAs
Classic Macintosh, OS/2
ResEdit etc could change rsrc
NTFS ADS is used for this sometimes
 MIME type tags and headers
BeOS filesystem (and OpenBFS)
the WWW and email

file magic (check it)
“file tests each argument in an attempt to classify it.”
“There are three sets of tests, performed in this order:
filesystem tests, 
magic tests, and 
language tests.”
“The first test that succeeds causes the file type to be printed.”
manual page for file(1)
file magic example checks 
file magic example  
What about icons?
How is all of this used?
Optimizations, shortcuts
Exceptions to security policy
In automated and manual file analysis 
for intelligence, triage, and response
Oh and by Attackers! 
Usage: Optimization
Apache
may try to compress text, html, 
but not PNG,GIF
  

Usage: Exceptions to policy
as configured in HIDS/NIDS :
 MSSE/SAV exclude from scan: "*.jar,*.dll"
 WAF / IPS policy : Disallow requests to “*.cgi, *.pl”
for application security : 
Gmail used to forbid exe files as attachments 
Usage: In file analysis 
Prioritize analysis, triage artifacts by file extension 
forensics tools may organize files by extension 
as well as by determined type
Carvers look for file typed data runs in evidence
Some tools only accept certain file types:
annubis, virustotal, truman, gfi , etc 
May accept exe or sometimes APK, URLs   
Basic Deceptions
Lies

Simple mutations

Deceptions: Lies
Windows hides extensions by default:




You can change extension/name
To easily hide file types in Windows: 
Deceptions: Lies (2)
    Deceptions: Simple mutations
to evade detection:
Compression
Zip it, RAR it, tar it up: changes headers and name
Packing
Various utilities disguise executable or intent
UPX, JavaScript, PHP packers / obfuscators
Encoding
MIME, Base64, ROT13 or uuencode for transmission
Transcoding
Change image or video type by re-encoding
    Deceptions: magic tricks 
Is this a GIF?
GIF98a [other binary data] [and then GIF palette here]<?php readfile('/etc/passwd'); ?>[more binary data]

Deceptions: magic tricks: jar + jpeg 
Polyglots
are multiple file types, maybe?
exhibit properties of multiple file types:
Abuse magic signatures
Multiple headers for multiple parsers
More than one EOF?
Release the GIFAR!
Other published polyglots, chimera
POC||GTFO 2013-
Many (all?) journal PDFs have multiple formats
Zipped contents, bootable disks, a Nintendo game ...
0x14 PDF has its MD5 hash on the cover page
“Jack Of All Formats”: @dan_crowley  SOURCE 2011 
Apache multiple handling of File.en.php.png
Functioning PDF / 7Zip archive, WinRAR / JPEG!
JaCK : Valid PNG with PHP backdoor
 “Funky File Formats” Ange Albertini @corkami 31C3 (2014)
Multi-polyglots, file format abuses + crypto 
Questions?

How do your systems identify file types and how much do you trust it?
Are there vulnerabilities in your systems related to these techniques?
Bonus: How did Sun and Google fix the vulns behind GIFAR?

Next steps
Static artifact analysis is one facet of forensic file analysis and reverse engineering. 
Awesome books, courses include:
Practical Malware Analysis ->
Malware Analysis Cookbook ->->
Life Of Binaries (from OST.info) ->
SANS FOR610 “Reverse Engineering Malware” and GREM  
http://www.giac.org/certification/reverse-engineering-malware-grem
More Polyglots and Chimera

Many examples and attack scenarios @dan_crowley's SOURCE 2011 prez “Jack of all Formats” slideshare

Ange Albertini @corkami has done lots of work here, check out his 31C3 “Funky File Formats” prez slides

And of course POC||GTFO

References
Slide deck and links available online:


[link here]
