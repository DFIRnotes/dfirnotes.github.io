---
layout: post
title: Yara: Make Your Own Rules
category: education,dfir
author: adricnet
---

_dc404 Oct 2020 presentation notes and link dump_

# pitch
Yara is a really useful tool for matching patterns in files and data
developed by the Virustotal team. It has applications across many
fields of information security and a vibrant online community.Along
with Snort it's a key language for threat research and defense
operations, and has interesting application for red, purple, and
rainbow teams too...

* What's Yara , what's it for?
* Where can I get some free rules ?
* What cool tricks can you do with Yara + rules ?
* Link Dump

# notes

## Yara is ...

* "YARA, the "pattern matching swiss knife for malware researchers (and everyone else)" is developed by @plusvic and @VirusTotal."

## tricks

### Yara scanning files

* better than hash checks
  * though hash research advances:
    * https://blogs.jpcert.or.jp/ja/2016/05/impfuzzy.html  
* better than mere ```strings -a```
* as seen in CISA MARs and other fine products,helping you hunt for 
  * Hidden Cobras:
    * https://us-cert.cisa.gov/ncas/analysis-reports/ar20-045g#yara
  * Flying Kittens:
    * https://www.crowdstrike.com/blog/cat-scratch-fever-crowdstrike-tracks-newly-reported-iranian-actor-flying-kitten/
  * Evil Pythons
    * https://github.com/DFIRnotes/rules/blob/master/str_py2exe.yara
  * and other fantastic beasts

### Yara scanning memory

* behaviour > simple indicator matches
* "malware can hide but it has to run" -Alyssa, FOR526
* "gentilkiwi demo" in Windows 7 VM

### Yara scanning network traffic ?
* In Suricata, vis LuaJIT engine: https://github.com/B0fH/yara-suricata
* In Zeek, BlackHat 2019 talk by David Bernal @d4v3c0d3r "Detecting malicious files with YARA rules as they traverse the network"
  * https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-The-Network.pdf
  * https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-the-Network-wp.pdf
  * https://www.youtube.com/watch?v=irai0kk942E

# Link Dump

## Yara project and manuals
* https://virustotal.github.io/yara/
* https://github.com/virustotal/yara
* https://yara.readthedocs.io/en/stable/

## Free rules, links to more
* 
* Yara Rules com: https://github.com/Yara-Rules/rules
* Reversing Labs:
  * https://www.reversinglabs.com/products/open-source-yara-rules
  * https://github.com/reversinglabs/reversinglabs-yara-rules
* https://github.com/InQuest/awesome-yara

## free Yara integrations and utilities
* Yaragen: 
* Loki: https://github.com/Neo23x0/Loki
* Suricata / Zeek ?
* Volatility / Rekall

### with $$ tools
* VirusTotal (Pro) Hunting: 
    * https://support.virustotal.com/hc/en-us/articles/360000363717-VT-Hunting
* https://www.nextron-systems.com/products/  
* https://fidelissecurity.com/threatgeek/threat-intelligence/yara-intrusion-prevention/
* https://docs.tanium.com/threat_response/threat_response/intel.html#Configure_YARA_files
* 

## Yara in free classes
* https://www.enisa.europa.eu/topics/trainings-for-cybersecurity-specialists/online-training-material/technical-operational#developing-countermeasures

### in $$ classes
* SANS Institute: FOR578, ICS515

