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
* In Zeek, BlackHat 2019 talk by David Bernal @d4v3c0d3r "Detecting malicious files with YARA rules as they traverse the network" [slides](https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-The-Network.pdf) [paper](https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-the-Network-wp.pdf) [video](https://www.youtube.com/watch?v=irai0kk942E)
* with MITRE Chopshop: https://www.mitre.org/capabilities/cybersecurity/overview/cybersecurity-blog/scanning-streaming-data-with-yarashop

# Link Dump of Yara Resources

## Yara project and manuals
* https://virustotal.github.io/yara/
* https://github.com/virustotal/yara
* https://yara.readthedocs.io/en/stable/

## Free rules, links to more
* Yara Rules com: https://github.com/Yara-Rules/rules
* Reversing Labs:
  * https://www.reversinglabs.com/products/open-source-yara-rules
  * https://github.com/reversinglabs/reversinglabs-yara-rules
* https://github.com/mikesxrs/Open-Source-YARA-rules
* https://github.com/InQuest/awesome-yara

## free Yara integrations and utilities
* Yaragen / Yara Generator: 
  * https://github.com/Neo23x0/yarGen
  * https://github.com/Xen0ph0n/YaraGenerator
* Loki: https://github.com/Neo23x0/Loki
* Volatility / Rekall: https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#yarascan

### with $$ tools
* VirusTotal (Pro) Hunting: 
    * https://support.virustotal.com/hc/en-us/articles/360000363717-VT-Hunting
* Nextron Systems (Florian Roth)
  * https://www.nextron-systems.com/products/  
* Fidelis Cyber:
  * https://fidelissecurity.com/threatgeek/threat-intelligence/yara-intrusion-prevention/
* Tanium Threat Response:
  * When they say "intelligence" they mean "rules", works pretty well
  * https://docs.tanium.com/threat_response/threat_response/intel.html#Configure_YARA_files
* FireEye Email:
  * https://www.fireeye.com/blog/products-and-services/2018/12/detect-and-block-email-threats-with-custom-yara-rules.html
  
## Yara in free classes
* https://www.enisa.europa.eu/topics/trainings-for-cybersecurity-specialists/online-training-material/technical-operational#developing-countermeasures

### in $$ classes
* Zero2Hero:
  * 
* SANS Institute: 
  * FOR578: https://www.sans.org/cyber-security-courses/cyber-threat-intelligence/
  * ICS515: https://www.sans.org/cyber-security-courses/industrial-control-system-active-defense-and-incident-response/
* Kaspersky XTraining: https://www.kaspersky.com/blog/cybersecurity-expert-training/36887/

## Talks using Yara
* SANS Webcast - YARA - Effectively using and generating rules (Oct 2018)
  * https://www.youtube.com/watch?v=5A_O8X_JljI
* CONFidence 2019: "Utilizing YARA to Find Evolving Malware" - Jay Rosenberg
  * https://www.youtube.com/watch?v=XMZ-c2Zwzjg
* BlackHat 2019 talk by David Bernal @d4v3c0d3r "Detecting malicious files with YARA rules as they traverse the network"
  * https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-The-Network.pdf
  * https://i.blackhat.com/USA-19/Wednesday/us-19-Bernal-Detecting-Malicious-Files-With-YARA-Rules-As-They-Traverse-the-Network-wp.pdf
  * https://www.youtube.com/watch?v=irai0kk942E
  
# misc

* Me? I read quite a bit, take some hard exams, and teach a little. I haven't finished college. I work in information security thanks to DC404  
```
BBSTi, CISSP, GIAC**0x0b, GSE, ITIL, LPI
Information Security Leader & Educator | Twitter, Github: @dfirnotes
```
* Soundrack: HBO Asia Original: Halfworlds seasons
