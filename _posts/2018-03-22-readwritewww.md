---
layout: post
title: Reading and Writing the Web
category: attack
tags: brownbag
author: adricnet
---

Reading and Writing the Web: Learning to Attack, a brownbag from the 2018 series

This is lightly formatted raw slides text. PDF of slides: http://dfirfiles.net/myslides/

## chmod +rw www

* Read
  * Source
* Debug
  * Inspector
* Write
  * Proxy

* View Source
* Dev Tools
* debugger;
* Attack Proxies

## View Source: Ctrl-U

* HTML: ```<h2><a href=”#here”>hyper-text</a> markup language</h2>```
  * “markup” and content
* JavaScript : ```onMouseOver(alert(‘Beep!’));```
  * “code”
* CSS: ```font-family:monospace,monospace;```
  * “styles and layout”
* [media]
  * graphics, video, malware, slide decks ...

##Dev Tools : F12 , Ctrl-Shift-C

* FireFox, Chrome, IE and Edge all have ‘em
  * Fn12 on IE and Edge, Cmd-Shift-C for FF/Chrome
  * Similar features, different details
  * Friendly competition has made them all pretty great

### Honourable mentions:

* FireBug : now subsumed by FireFox dev tools
* Browser plugins : Cookie Editor, Tamper Data

## debugger;

*  Add this call to invoke Web Debugger (F12)
  * and Pause execution!
* Then Step, Watch, Pause … Debug!

## Proxies: Burp and ZAP and ...	
* Burp Suite from Portswigger
  * Community (awesome) or Pro (  [ $power -gt 9000] )
  * Java, install available for most platforms (kali, samurai wtf)
* Zed Attack Proxy (ZAP, zaproxy)
  * Open source from OWASP
  * Java, install available for most platforms (kali, samurai wtf)
* And Fiddler and Charlie and w3af and and ...

## Burp Free: quickstart
1. Connection settings in Browser
  * Handy: FoxyProxy extension for FireFox
1. Proxy:Options in Burp
  * Intercept ?
1. Surf => Target:Site Map  
1. From Target:
  * Right click Scope
  * Right-click Repeater

## Live Demo?!?
Metasploitable3
* Joker flag ?
BadStore
*  Login SQLi ?

## $LINKDUMP
```
# for u in $LINKDUMP; do echo -n $u; echo ‘ , ‘; done
https://www.w3schools.com/js/js_debugging.asp , https://developer.mozilla.org/en-US/docs/Tools  , https://developers.google.com/web/tools/chrome-devtools/  , https://securedorg.github.io/RE101/section6/ ,  https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide  ,
https://support.portswigger.net/customer/portal/articles/1816883-getting-started-with-burp-suite  , https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project 
"SANS Webcast: Debugging Python Code for mere mortals" @markbaggett: https://www.youtube.com/watch?v=jP15Odi5q8k , https://www.udacity.com/course/software-debugging--cs259  ,
_Tangled Web: A Guide to Securing Modern Web Applications_, Michal Zalewski: https://nostarch.com/tangledweb 
_The Web Application Hacker's Handbook_: Finding and Exploiting Security Flaws, 
Dafydd Stuttard & Marcus Pinto: http://mdsec.net/wahh/ , _Debugging_, David Agans : http://debuggingrules.com/ for the poster , The Art of Debugging_ ,  Matloff & Salzman,https://nostarch.com/debugging.htm , https://github.com/rapid7/metasploitable3 , https://www.vulnhub.com/series/badstore,21/ 
#  Flag Spoilers
https://gist.github.com/adricnet/00db9b877d3fe5efb856b029df7326b3 
```
