---
layout: post
title: TIL from Linux/Unix Security
category: course
tags: brownbag
author: adricnet
---

A few notes about SEC506 and what I learned from it, plus the the start of my exam prep list for GCUX

# TIL from Linux/Unix Security (SEC506)

## Course overview
* 1-3: Hardening Linux/Unix systems
  * systemd, SSH, firewalls, auditing (single user)
  * rootkits, AIDE, physical access, sudo, hardening (multi-user)
  * More SSH, Logs of all kinds, Syslog-NG, SSH+Logs (-> enterprise)
* 4-5: Application Security
  * chroot, scponly, SELinux
  * DNS and bind, Apache run securely
* 6: DFIR for Linux/UNIX
  * _a one day crash course in IR and DF focused on Linux_

## Learnings

* lots of practice with CentOS
* **SystemD**'s positive contributions and how to use it
* **auditd** and its rules: 
  * turned out to be timely for a project
* **chroot**: both a program and a syscall
* SSH key files: can take options like forced commands
* **SELinux** end to end: configuration, mgmt, and policy
* **DNSSec**: key mgmt and troubleshooting 
* Unix hacks, history, and lore, like:
  * what was the sticky bit originally for?
* ```!!``` , ```!-2$```, ```cd !!:1``` and other useful **Bash** madness
  * This is a thing:
```
[sans@LAB ~]$ one two three four five
bash: one: command not found...
[sans@LAB ~]$ ^five^BANG^
one two three four BANG
bash: one: command not found...
```

# GCUX prep

## Need syntax cheet sheets / tabs to
* aide
* sudoers
* ssh_config
* auditd config rules
* ssh command line n N D d ...
* syslog ng configs
* semanage* & chcon?
* apache configs
* DNS zone syntax
* bind

