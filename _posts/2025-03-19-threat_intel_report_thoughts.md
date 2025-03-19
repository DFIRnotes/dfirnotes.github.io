---
layout: post
title: CTI Report Data Thoughts
category: cybersecurity, threat intelligence
author: adricnet
---

Some unfinished ideas and references about what's useful to cybersecurity defense to and from CTI ... leads into the infamous 127.0.0.1 story.

Full reports are read and ingested to learn from, to add to understanding, to refer back to
* include investigation details, response actions, background
* Includes detections or technical details
* includes context: victimology, time data
* may include recommended actions and remediations

Alligator book HF vs INV (rather than "alerts"):
* High fidelity event (HF): specific actionable, low FP and FA tuning, has a response plan 
* Investigation lead (INV): list of entities or IDs that meet some criteria of concern , has response plan

* An HF indicator that "you are compromised" (the mythical IoC) .. is basically impossible with atomic indicators
  * really only works with
    * full domain names / application IDs 
    * JA3(S) TLS fingerprints and the like
  * needs tight time boundaries for any IP addresses, and still probably not useful
  * combined with a vulnerable app a URL or request string might also qualify
  * file hashes were never very useful for direct detection but can be useful vs LOLBAS and data
* Detection rule IoC is _possible_ but much harder to share/operationalise 
  * behavioural pattern to look for OR more usefully
  * a combination of factors encoded as a good Sigma, Yara, Suricata rule
  * (these are compund or complex indicators in some models)

* high fidelity indications of attack (IoA) are possible
  * simple/atomic or complex/compound could describe activity of concern
  * might connect usefully with Sightings
    * yes we saw the attempted activity
    * yes/no we can confirm impact, follow-on investigation

* List of machines with unexpected "127.0.0.1" in "/etc/hosts" is a useful lead (INV) (and a good threat hunt for some programs)
* 127.0.0.1/32 in a SIEM rule (or block list) is much less desirable ( that customer was more confused than upset, thankfully )
* "large file uploads to Amazon AWS East S3 endpoints on the Ides of March" could be part of a good detection
* these 14 /16 (Azure) CIDR blocks are bad and you should feel bad for talking to them -> not helpful most places

TODO: Post and link the 127 0 0 1 story

