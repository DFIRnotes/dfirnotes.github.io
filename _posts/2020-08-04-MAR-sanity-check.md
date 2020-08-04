---
layout: post
title: Fun with Chinese Malware
category: education,dfir
author: adricnet
---

_In which yours truly takes REMnux 7 and Ghidra for a spin with some newly famous malware_

There's a newly public Windows malware sample making the rounds and I thought I would use it for practice with the analysis toolset including some newer tools.
The report is at https://us-cert.cisa.gov/ncas/analysis-reports/ar20-216a and I suspect we'll all be studying it for awhile.
They provide hashes and detailed anlaysi for two kinds of samples (two pair if you will): a pretty straight-forward loader and it's encrypted payload. 
They also share Snort and Yara rules which are the native language of the malware analyst :)

## 
If you haven't done code-level analysis of Windows software before, well, this is a great time to start and some good samples. Though, if you are a beginner please start with [Practical Malware Analysis](https://nostarch.com/malware) or another excellent book like Monnappa's [Learning Malware Analysis](https://www.packtpub.com/networking-and-servers/learning-malware-analysis) to get some essential safety procedures, process, and analysis frameworks.

## Get the samples
The MAR includes the SHA256 hashes but not the files. You'll need to find or pay someone to give you copies if you don't already have them.

I checked a few places and ended up getting them from Virus Total Intelligence by punching in the hashes and downloading zip files.

## Ghidra 9
Some intro to Ghidra is helpful before you take on real analysis. Anuj has some excellent material and videos on his [site](https://malwology.com/2020/04/27/sans-for610-reverse-engineering-malware-now-with-ghidra/). If you are used to another toolset Ghidra is a bit different.  For example, I found the integrated decompiler output refreshing.

## Yara rule sanity check
Here, REMnux 7 (newly released) shows that the Yara rule provided in the MAR detects the payload samples. 
The MAR itself draws a map that would help a dedicated analyst recreate the rules with the samples, but anyone can use them to detect these samples and likely the whole family of tools which is a great share.

```
root@remnux-remnux-distro2:/tmp/t# for file in *; do yara cisa_taidoor.yara $file;done
CISA_10292089_01 0d0ccfe7cd476e2e2498b854cef2e6f959df817e52924b3a8bcdae7a8faaa686
CISA_10292089_01 363ea096a3f6d06d56dc97ff1618607d462f366139df70c88310bbf77b9f9f90
root@remnux-remnux-distro2:/tmp/t# for file in *; do yara cisa_taidoor.yara -g -s $file;done
CISA_10292089_01 [rat,loader,TAIDOOR] 0d0ccfe7cd476e2e2498b854cef2e6f959df817e52924b3a8bcdae7a8faaa686
0x0:$s6: 5A 05 B2 CB E7 45 9D C2 1D 60 F0 4C 04 01 43 85 3B F9 8B 7E
CISA_10292089_01 [rat,loader,TAIDOOR] 363ea096a3f6d06d56dc97ff1618607d462f366139df70c88310bbf77b9f9f90
0x0:$s6: 5A 05 B2 CB E7 45 9D C2 1D 60 F0 4C 04 01 43 85 3B F9 8B 7E
root@remnux-remnux-distro2:/tmp/t# file *
0d0ccfe7cd476e2e2498b854cef2e6f959df817e52924b3a8bcdae7a8faaa686: data
363ea096a3f6d06d56dc97ff1618607d462f366139df70c88310bbf77b9f9f90: data
4a0688baf9661d3737ee82f8992a0a665732c91704f28688f643115648c107d4: PE32 executable (DLL) (GUI) Intel 80386, for MS Windows
5450879426134016.zip:                                             Zip archive data, at least v2.0 to extract
6003505287954432.zip:                                             Zip archive data, at least v2.0 to extract
6197632404324352.zip:                                             Zip archive data, at least v2.0 to extract
6725846037987328.zip:                                             Zip archive data, at least v2.0 to extract
6e6d3a831c03b09d9e4a54859329fbfd428083f8f5bc5f27abbfdd9c47ec0e57: PE32+ executable (DLL) (GUI) x86-64, for MS Windows
cisa_taidoor.yara:                                                ASCII text
root@remnux-remnux-distro2:/tmp/t# strings -a -n 12 0d0ccfe7cd476e2e2498b854cef2e6f959df817e52924b3a8bcdae7a8faaa686 | less
root@remnux-remnux-distro2:/tmp/t# xxd 0d0ccfe7cd476e2e2498b854cef2e6f959df817e52924b3a8bcdae7a8faaa686 | head
00000000: 5a05 b2cb e745 9dc2 1d60 f04c 0401 4385  Z....E...`.L..C.
00000010: 3bf9 8b7e d9d8 635a 4969 361c 461b c192  ;..~..cZIi6.F...
00000020: 15e9 4f70 c09d 7046 11e2 c7aa 5be4 6322  ..Op..pF....[.c"
00000030: 7b4a 3e7a 9e63 dc11 6ada 4b73 ee01 406a  {J>z.c..j.Ks..@j
00000040: eb5c d612 b130 45ed d99c c7aa 521c 916e  .\...0E.....R..n
00000050: cdd7 cc13 458f 5b5d ddff c839 b926 4aa6  ....E.[]...9.&J.
00000060: ea9b f44c 95cf 6e6f de18 7d46 8572 113e  ...L..no..}F.r.>
00000070: d67b 9508 d441 dce2 d75e c604 f3eb d3b0  .{...A...^......
00000080: 765e fb60 74e8 431f 400b aa9b d4ed f444  v^.`t.C.@......D
00000090: 7659 9cb0 05a5 fade f96d 7106 f4eb 6b83  vY.......mq...k.
root@remnux-remnux-distro2:/tmp/t# xxd 363ea096a3f6d06d56dc97ff1618607d462f366139df70c88310bbf77b9f9f90 | head
00000000: 5a05 b2cb e745 9dc2 1d60 f04c 0401 4385  Z....E...`.L..C.
00000010: 3bf9 8b7e d9d8 635a 4969 361c 461b c192  ;..~..cZIi6.F...
00000020: 15e9 4f70 c09d 7046 11e2 c7aa 5be4 6322  ..Op..pF....[.c"
00000030: 7b4a 3e7a 9e63 dc11 6ada 4b73 f601 406a  {J>z.c..j.Ks..@j
00000040: eb5c d612 b130 45ed d99c c7aa 521c 916e  .\...0E.....R..n
00000050: cdd7 cc13 458f 5b5d ddff c839 b926 4aa6  ....E.[]...9.&J.
00000060: ea9b f44c 95cf 6e6f de18 7d46 8572 113e  ...L..no..}F.r.>
00000070: d67b 9508 d441 dce2 d75e c604 f3eb d3b0  .{...A...^......
00000080: c98a fd88 cb3c 45f7 ffdf ac73 6b39 f2ac  .....<E....sk9..
00000090: c98d 9a58 ba71 fc36 46b9 43ee e33f 6d6b  ...X.q.6F.C..?mk
```
