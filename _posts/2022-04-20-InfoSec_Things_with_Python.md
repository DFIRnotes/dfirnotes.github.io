---
layout: post
title: InfoSec Things with Python
category: education
author: adricnet
---

# InfoSec Things You Can Do with a little Python
_programming experience helpful, but not required_

An idea for presentation or two, feedback appreciated

* contribute bugs, code, documentation, and tests to projects
* do simple or complex data analysis, and make charts and graphs
  * Jupyter, Pandas : https://infosecjupyterthon.com/introduction.html
* capture, analyse, and manipulate packets
  * scapy: https://scapy.readthedocs.io/en/latest/
* reformat and decode data
  * unhexlify, codecs etc => Didier Steven's tools: https://github.com/DidierStevens/DidierStevensSuite
  * check entropy and detect computer-generated data strings: https://github.com/sans-blue-team/freq.py
* upload and download files over http(s) and and other means
  * http.server, requests
  * hit an API and get stuff
* exploit vulnerabilities
  * ```python -c "print('{}'.format('A' * 1024))"```
* automate & extend security tools:
  * Ghidra eg: https://github.com/ghidraninja/ghidra_scripts
  * Burp Suite https://portswigger.net/burp/documentation/desktop/tools/extender , ZAP https://github.com/zaproxy/zap-api-python
  * Metasploit Framework https://github.com/rapid7/metasploit-framework/blob/master/modules/auxiliary/example.py etc.
* get a (better) terminal session
  * ```python3 -c "import pty;pty.spawn('/bin/whoami')"```
* (advanced) integrate tools and glue systems together
* (advanced) make entirely new tools

## etc
* Cool python3 programming tricks:
  * speciality dictionaries like Counter: https://docs.python.org/3/library/collections.html
* small projects or use cases to work on
* books (NoStarch) and courses recommendations (SEC573)
