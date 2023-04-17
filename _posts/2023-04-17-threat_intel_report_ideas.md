---
layout: post
title: Perfect Threat Intel Report Ideas
category: cybersecurity
author: adricnet
---

What would make up the perfect cybersecurity threat intelligence (CTI) report to receive, ingest, automate around? Honestly, even any three of these things makes for a great report. More context and detail are better, if the report author / information sharing organisation can support it.

## Detailed data, using integrated frameworks and CTI data models
* Intrusion (or attempts) detailed (ala Diamond Model) with time and impact 
   * Actor, Victim (and/or Target), Infrastructure, Capability .. plus time and impact :
     * "who tried to do what to whom, when/how often, and did it work" ?
* Activity and any impact detailed and coded, categorised (ala VERIS or DoD 6510 Categories)
* Notable techniques and tactics observed (eg MITRE ATT&CK ids, versions)
* Possible countermeasures and detections in open formats (Suricata, Sigma, Yara, ClamAV) or public reference (Critical Controls, NIST CSF)
* Recommended Actions / Practices public reference (Critical Controls, NIST, etc)
* References & Citations

## References:
* The Diamond Model of Intrusion Analysis: https://www.threatintel.academy/diamond/
* VERIS Getting Started : http://veriscommunity.net/howto.html
  * VERIS - MITRE Att&ck bridge: https://medium.com/mitre-engenuity/strengthening-the-connection-veris-and-mitre-att-ck-c3aac3fa9cd7
  * DoD 6510 for hard mode: CJCSM 6510.01B https://www.jcs.mil/Portals/36/Documents/Library/Manuals/m651001.pdf?ver=zbA7MXUXDcB9-se9hOxsIA%3d%3d (PDF, check B-A-3 around p54)
* MITRE ATT&CK Framework(s): https://attack.mitre.org/
* LM Intelligence driven CND paper (2010) , arguably still everything you need to know about indicators
  * https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf
* Suricata, Sigma, Yara (Security Onion docs)
  * https://docs.securityonion.net/en/2.3/search.html?q=rules&check_keywords=yes&area=default
