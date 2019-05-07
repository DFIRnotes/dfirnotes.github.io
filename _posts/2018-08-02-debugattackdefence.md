---
layout: post
title: Debugging for Attack and Defence
category: attack
tags: brownbag
author: adricnet
---

Debugging for Attack and Defence: Learning to Attack, a brownbag in the 2018 series.

This is lightly formatted raw slides text. PDF of slides: http://dfirfiles.net/myslides/

## set SeDebugPrivilege
* Debuggers
  * Disasm, Decompile?
  * Interactive
  * Applications
* Powers
  * Pause, Break
  * Edit, Patch
* Examples
  * ReverseMe 
  * Javascript
  * Pinning EIP
* Links
  
## No Disassemble?
* Disassembler
  * Machine code (binary) => Assembly language code (text)
* Decompiler
  * Binary => C or Java source code 
* Interactive Debugger
  * Disass, Decompile running code and data structures

## Disassemble
* capstone 
  * radare2 rasm2
* objdump,  otool 
* IDA Pro 		=>
* FileInsight
* CyberChef
…

## Decompile

* Snowman
  * (in x32dbg)      =>
* Hex-Rays 
  * (in IDA Pro)
* JADx & Procyon
  * Java, Dalvik      =>

## Interactive!
* x64dbg 		=> 
  * Olly, Immunity
* IDA Pro
* radare2 		=>
  * Cutter
* gdb, windbg, …

## Applications: attack, defence, CTFs
* Vulnerability research
* Exploit development
  * and testing and testing
* CTFs and puzzles

* Deobfuscation/decoding
* Dynamic malware analysis
  * Restart, try again
* CTFs and puzzles

## Debugging Superpowers
* Stop time                    =>
  * Some can rewind!  
* See through walls            =>
  * Everything is memory is yours to command
* Change data and program flow
* Scriptable       =>
* Extensibile  =>

## Debugging Superpowers
* Stop time                    
  * Some can rewind!  
* See through walls           
  * Everything is memory is yours to command
* Change data and program flow
* Scriptable       
* Extensibile

* **Pause** & **Run**
  * On **Breakpoints**
* **Inspect**
  * **Watch**, Follow
* **Single Stepping**
* Edit memory!
* Commands
* Plugins

## Live Demo?!?
* Exe sample
* Nag screen ?
 
## Dev Tools : F12 , Ctrl-Shift-C
* FireFox, Chrome, IE and Edge all have ‘em
  * Fn12 on IE and Edge, Cmd-Shift-C for FF/Chrome
* Similar features, different details
  * Friendly competition has made them all pretty great
* Honourable mentions:
  * FireBug : now subsumed by FireFox dev tools
  * Browser plugins : Cookie Editor, Tamper Data

## debugger;
* Add this call to invoke Web Debugger (F12)
  * and Pause execution!
* Then Step, Watch, Pause … Debug!

## Towards sploits!
* Learning to crash software … deliberately
* Manipulate program memory and registers
* Take control of execution …
  * ```mov EIP, 0x4141414141414141```
* Run your code!
  * First in a debugger … then on the target!

## $LINKDUMP
```
for u in $LINKDUMP; do echo -n $u; echo ‘ , ‘; done
https://github.com/zerosum0x0/winrepl , 
https://github.com/gchq/CyberChef/ , https://microcorruption.com/about ,
https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide/debugger ,
https://www.offensive-security.com/information-security-training/penetration-testing-training-kali-linux/ , https://www.malwaretech.com/beginner-malware-reversing-challenges , 
https://chocolatey.org/packages/x64dbg.portable/20180719.1655 
https://github.com/ashishb/android-malware/tree/master/towelroot
https://securedorg.github.io/RE101/ , 
https://www.packtpub.com/networking-and-servers/learning-malware-analysis ,
https://github.com/mattgodbolt/compiler-explorer/ , 
https://nostarch.com/bughunter and forthcoming https://nostarch.com/bughunting ,
https://nostarch.com/malware and forthcoming https://nostarch.com/malwaredatascience,
https://www.sans.org/ondemand/course/reverse-engineering-malware-malware-analysis-tools-techniques 
```
