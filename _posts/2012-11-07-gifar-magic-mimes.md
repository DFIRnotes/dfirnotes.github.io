---
layout: post
title: GIFAR's Magical Mimes Filed in 8 by 3 (2012)
category: File Analysis
tags: imported,brownbag
author: adricnet
---

<i>outline and notes for 2012 file types brownbag</i>


<h1>GIFAR's Magical Mimes Filed in 8 by 3</h1><i>File types, identification
technology, and their weaknesses</i>

<h2>File types?</h2>

<h3>a few examples:</h3>
<ul>
<li>live and raw bytes of common files types:</li>
<li>html/xml/text, pl/py/rb/sh, png, swf, PDF, exe, doc, mp3, avi, jar/zip/docx</li>
</ul>

<h3>the basic schemes</h3>
<ul>
<li>file name extensions (trust)</li>
<li>file metadata (tag)</li>
<li>resource forks and EAs</li>
<li>MIME type tags and headers</li>
<li>file(1) magic (check)</li>
<li>icons?</li>
</ul>

<h2>How is all of this used?</h2>

<h3>Optimizations</h3>
<ul>
<li>Apache modules may try to compress GIF, JPG, but not PNG,JAR</li>
</ul>

<h3>Exceptions to policy</h3>
<ul>
<li>configured in HIDS/NIDS : eg MSSE exclude from scan "*.jar"</li>
<li>WAF / IPS policy : Disallow requests to *.cgi, *.pl</li>
<li>Email security : Gmail won't allow exes ...</li>
</ul>

<h3>In response and triage</h3>
<ul>
<li>easy to prioritize/triage by file extension ...</li>
<li>automated analysis may rely on file typing</li>
<li>Fireeye/Damballa, FTK's Cerebus, ?</li>
</ul>

<h2>Basic Deceptions</h2>

<h3>lies</h3>
<ul>
<li>change extension/name</li>
<li>can simply hide files in windows or UNIX, eg ..

<h3>Simple mutation</h3></li>
<li>compression/packing/encoding to evade detection</li>
<li>magic tricks: gif/php stego</li>
</ul>

<h2>Chimera</h2>

<h3>thing</h3>

<h3>Release the GIFAR!</h3>

other examples of multiple valid types
<hr>

<h3>Refs</h3>
<pre>bsk@bebo-bt5:~/anet/gsec$ file -v
file-5.03
magic file from /etc/magic:/usr/share/misc/magic
</pre>

http://linux.die.net/man/1/file

http://www.garykessler.net/library/file_sigs.html

http://www.pkware.com/documents/casestudies/APPNOTE.TXT

http://www.ethicalhacker.net/component/option,com_smf/Itemid,54/topic,7655.msg41049/
http://www.exploit-db.com/exploits/16181/

https://en.wikipedia.org/wiki/Executable_compression

https://en.wikipedia.org/wiki/GIFAR

http://googleonlinesecurity.blogspot.com/2012/08/content-hosting-for-modern-web.html

http://www.gnucitizen.org/blog/gifars-and-other-issues/

http://www.gnucitizen.org/blog/more-on-gifars-and-other-dangerous-attacks/

http://www.zdnet.com/blog/security/black-hat-sneak-preview/1619

/. thread:
http://it.slashdot.org/story/08/08/01/184220/a-photo-that-can-steal-your-online-credentials

copy of original GIFAR presentation?:

http://files.sans.org/summit/pentest09/PDFs/Jeremiah%20Grossman%20-%20WebApp%20Vulnerabilty%20Analysis%20-%20SANS%20PenTest%20Summit09.pdf

R. Brandis. Exploring below the surface of the gifar iceberg. Whitepaper.
2009 http://www.infosecwriters.com/text_resources/pdf/RBrandis_GIFAR.pdf

Image Repurposing for Gifar-Based Attacks by Smitha Sundareswaran, Anna C
Squicciarini : http://academic.research.microsoft.com/Paper/14046706.aspx

DeCore: Detecting Content Repurposing Attacks on Clientsâ Systems by
Smitha Sundareswaran, Anna C Squicciarini
http://www.personal.psu.edu/sus263/DecoRe.pdf

Dan Crowley of Trustwave.com: Jack of All Formats
http://www.slideshare.net/BaronZor/jack-of-all-formats

<h2>Needs</h2>
<ul>
<li>nil</li>
</ul>

<h2>Wants</h2>
<ul>
<li>pic/details for compression / encoding?</li>
<li><b>research</b>&nbsp;on filetype usage in Cerebus, Fireeye, Sourcefire..</li>
<li>? anti virus screen snip of exception config - at work ?</li>
<li>reorg this page and pretty up the links</li>
</ul>

<h3>New</h3>

Background books that don't address file typing in any depth:

PMA

FSFS

MAC<br>

Roel @ Kasperky Lab's blog post about antivirus detection of scripts hiding
as PEs, Nov 2005

<h1 style="font-weight: normal; font-size: 21px; font-family: tahoma, arial; margin: 0px 0px 0.5em; color: rgb(0, 0, 0); background-color: rgb(238, 238, 238);">Magic
byte vulnerability</h1>

https://www.securelist.com/en/blog?weblogid=173180325<br>


<h4>Malware Hidden Inside JPG EXIF Headers</h4>

<a href="http://blog.sucuri.net/2013/07/malware-hidden-inside-jpg-exif-headers.html" rel="noreferrer">http://blog.sucuri.net/2013/07/malware-hidden-inside-jpg-exif-headers.html</a><br>

Newer info from Talos is on their blog [here](http://blogs.cisco.com/security/talos/malicious-pngs)
