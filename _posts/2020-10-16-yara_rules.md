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
fields of information security and a vibrant online community. Along
with <strike>Snort</strike> Suricata it's a key language for threat research and defense
operations, and has interesting applications for red, purple, and
(rainbow) teams too...

* What's Yara , what's it for?
* What cool tricks can you do with Yara + rules ?
* Where can I get some free rules ?
* Link Dump
* Q&A 

# notes

## Yara is ...

* "YARA, the "pattern matching swiss knife for malware researchers (and everyone else)" is developed by @plusvic and @VirusTotal."
* rules made up of strings and conditions

## Example rules for testing
* always_true (from OSQuery Yara (manual)[https://osquery.readthedocs.io/en/latest/deployment/yara/]
* eicar ( by AirBNB on (GitHub)[https://raw.githubusercontent.com/airbnb/binaryalert/master/rules/public/eicar.yara], found in (blog)[https://holdmybeersecurity.com/2020/03/01/operation-cleanup-eradicating-malware-with-osquery-and-kolide/] "OPERATION CLEANUP: ERADICATING MALWARE WITH OSQUERY AND KOLIDE" by Spartan2194 )
  * WTH is EICAR?: http://www.eicar.org/86-0-Intended-use.html
  * More fun with EICAR: https://biebermalware.wordpress.com/2017/05/10/playing-with-eicar-take-ii/  
* PyInstaller ( me, (str_py2exe.yara)[https://raw.githubusercontent.com/DFIRnotes/rules/master/str_py2exe.yara] )
  * Once upon a pentest...  
* http (Volatility (yarascan)[https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#yarascan] docs)

### Test rules

```
//always_true.yara
rule always_true
{
    meta:
        purpose = "testing"
        source = "https://osquery.readthedocs.io/en/latest/deployment/yara/"
    condition:
            true
}
```

```
// https://raw.githubusercontent.com/airbnb/binaryalert/master/rules/public/eicar.yara
rule eicar_av_test {
    /*
       Per standard, match only if entire file is EICAR string plus optional trailing whitespace.
       The raw EICAR string to be matched is:
       X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
    */

    meta:
        description = "This is a standard AV test, intended to verify that BinaryAlert is working correctly."
        author = "Austin Byers | Airbnb CSIRT"
        reference = "http://www.eicar.org/86-0-Intended-use.html"

    strings:
        $eicar_regex = /^X5O!P%@AP\[4\\PZX54\(P\^\)7CC\)7\}\$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!\$H\+H\*\s*$/

    condition:
        all of them
}
```

```
// Practice Yara rule: check for string artifacts of py2exe builds
// requires Yara 3.4+

/*
  github.com/dfirnotes/rules
  Version 0.0.0
*/

rule has_pythonscript_label
{
  meta:
    author = "@adricnet"
  strings:
    $pyscript_label = "PYTHONSCRIPT"

  condition:
    $pyscript_label
}

rule has_py2exe_err_string
{
  meta:
    author = "@adricnet"
  strings:
    $py2exe_activation_error = "py2exe failed to activate the "

  condition:
    $py2exe_activation_error
}

rule possible_py2exe_created_file
{
  meta:
    author = "@adricnet"
  condition:
    has_pythonscript_label and has_py2exe_err_string
}
```

```http```

## Some Yara Tricks

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
* in osquery !

![CentOS 8](http://dfirfiles.net/samples/cent8_yara_osquery_eicar.png)
![macOS 10.15](http://dfirfiles.net/samples/macos_yara_osquery_eicar.png)

### Yara scanning memory

* behaviour > simple indicator matches
* "malware can hide but it has to run" -Alyssa, FOR526
* _Live "gentilkiwi demo" in Windows 7 VM ???_

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
* Yara in OSQuery:
  * https://osquery.readthedocs.io/en/latest/deployment/yara/ , https://osquery.io/schema/4.5.1/#yara
  * Helpful Blog, EICAR examples: https://holdmybeersecurity.com/2020/03/01/operation-cleanup-eradicating-malware-with-osquery-and-kolide/
* CrowdStrike CrowdResponse: https://www.crowdstrike.com/blog/crowdresponse-release-new-tasks-modules/
* Yaragen / Yara Generator: 
  * https://github.com/Neo23x0/yarGen
  * https://github.com/Xen0ph0n/YaraGenerator
* Loki scanner: https://github.com/Neo23x0/Loki
* Volatility / Rekall yarascan plugin: https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#yarascan
* Yara GUI for windows ? 

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
* Zero2Hero, Automated, BMAC:
  * https://courses.zero2auto.com/
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
* Resource Efficient Malware Scans with YARA + osquery (osquery@scale 2020) https://www.youtube.com/watch?v=kmmPcopxeEM
* https://dfrws.org/conferences/dfrws-usa-2016/sessions/using-grr-and-rekall-scalable-memory-analysis-part-1

# misc

* Me? I read quite a bit, take some hard exams, and teach a little. I haven't finished college. I work in information security thanks to DC404  
```
BBSTi, CISSP, GIAC**0x0b, GSE, ITIL, LPI
Information Security Leader & Educator | Twitter, Github: @dfirnotes
```
* Soundrack: HBO Asia Original: Halfworlds seasons
