---
layout: post
title: Hunting words
category: hunting
author: adricnet
---

_Some words about hunting including some perspectives from different sources_

1. "Random acts of incident response"
  * Orgs that don't have mature/effective IR may quest about on indefensible networks for signs of misbehaviour and compromise
  * On networks without basic hygiene in place (CSC 1-5 / ASD quick wins) this will always succeed and never really help

1. Using data in a structured manner to disprove assumptions about your network and defences (via FireEye team @ BSides Augusta (Harry Potter themed)
  * http://www.irongeek.com/i.php?page=videos/bsidesaugusta2016/living-in-america05-hunting-defense-against-the-dark-arts-jacqueline-stokes-danny-akacki-and-stephen-hinck
  * EG: We don't have unauthorised software installs.
  * EG: SSH outbound to Internet isn't possible from corporate networks
  * Proving these false with data can motivate/empower organisational change / prove points to clients
  * Like most data science getting, cleaning, and normalising data is significant work
  
1. Assume compromise and try to detect post-compromise/next attack phase activities (sniff, spray, pivot, escalate, backdoor, exfil, etc) (SEC511)
  * EG: Pick a network segment and look for outbound file transfers. Are any legitimate activity?
  * EG: Looking for beacons and C2 in web proxy or DNS logs
  * This can be fruitful, but needs some structure to be practical. 
  * And of course without functional IR and the defensible network hygiene it's not of much use.
  
1. Have your more skilled responders manually looking through collected data for potential misbehaviour that can be configured for automated detection
  * This is a step towards @bammv's idea of three detection types: "RealTime, Batch, and Hunting"
  * This also aligns with the two playbook report types in Cisco team's *Crocodile Book*: High Fidelity and Investigation Lead
  
1. Using collected techniques (public or private) for post-compromise detections / incident discovery:
  * EG https://github.com/ThreatHuntingProject/ThreatHunting
  * https://detect-respond.blogspot.com/2015/10/a-simple-hunting-maturity-model.html
