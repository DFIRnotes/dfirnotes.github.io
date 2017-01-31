---
layout: post
title: Web Sec Quiz (2008)
category: Web Security
tag: imported
author: adricnet
date: 2009-08-14
---

##Linux

###Does this look infected?

_Are these anything to be worried about?_

*  PHP file contains ```<?system(getenv("HTTP_ACCEPT_IP"));?>```
*  <i>root</i> running ```find . -type f -name .htaccess -exec grep AddHandler```
*  <i>www-data</i> running ```sh -i```
*  <i>www-data</i> running ```httpd -DSSL```
*  Apache log entry:

```
75.x.y.z - - [30/Aug/2008:22:38:23 -0400] "POST /gallery/2008/aug/28aug.jpg HTTP/1.1" 200 7083 "http://www.rockettube.com/video.php?page=2&amp;viewtype=&amp;category=&amp;catid=&amp;ordertype=DESC&amp;videoold=" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.0.3705; .NET CLR 1.1.4322; Media Center PC 4.0)
```

*  Apache log entry:

```
125.x.y.z - - [30/Aug/2008:22:45:28 -0400] "POST /wsearch.php HTTP/1.1" 302 5138 "http://avg.urlseek.vmn.net/wsearch.php" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1) )"
```

###Bad Perl

_FIXME nulls?, variable include paths, sql in URL..._

Name some of the many security mistakes in this pseudo Perl:

```
#!/usr/bin/perl

use CGI;
use DBI;

print header;
start_html('Update Password');

## get http get arguments
if (param()) {

## include mysql auth credentials from $mysql_keys
include $mysql_config;

$connectionInfo="dbi:mysql:$db;$host";
# make connection to database
$dbh = DBI->connect($connectionInfo,$userid,$passwd);

# prepare and execute query
$query = "UPDATE * SET pass=password('$param(pass)') WHERE user='$param(user)' ";
$dbcon = $dbh->prepare($query);
$dbcon->execute();

}
p('Updated database for user $param(user)');
end_html;
```

###Short Answer

* Octal mode quiz. What do these mean:

	* 022
	* 600
	* 750
	* Group Sticky?

* How do you display the ACEs for a file (assume XAs)?
* Suggest a rule to mitigate this attack signature: A vulnerability is announced in apache for URLs that have 45 capital Bs in a row.
* What's PHP's Safe Mode and how well does it work?
* You suspect a server might have a rootkit. What do you do?
* Where does a RedHat-ish machine keep it's firewall rules?
* Name a few system calls that any one looking for malicious code would check.
* Which type of http request do we check logs for first and why?
* Name two applications that use libpcap and define their uses.
* Diff atime, mtime, and ctime.
* What does SELinux do? How does this apply to web application security?
* Diff sudo and wheel. 
* How do you change the linux firewall policy on ingress to deny?

###Longer answers

* How can chroot be used for application security? What about suexec/suphp?
* Explain a current or historical XSS vulnerability. Suggest some mitigation.
* Suggest some problems with the default handling of sessions in PHP5 or Rails 2.

###Forensics

Explain briefly how to do live forensics of a suspected web bug with the 
basic tools pre-installed on common Linux systems. Assume they deleted
their file as is commonly the case. Rather than telling the narrative explain
the tools and their uses in live forensics for the web.

###Bonus

_These questions cover knowledge of current events and useful background knowledge._

* What did DanK find out was wrong with DNS? Which servers were not vulnerable to this weakness?
* Explain the ruby maintainer bugs recently revealed.
* Name one or more security improvements in MS Windows 2008/Vista that OpenBSD already had.
* Your boss makes a joke about Debian random in a meeting. Explain what this means? How did this more recently affect their competitor RedHat?

##Windows

* Explain how to avoid _Little Bobby Tables_ (xkcd) FIXME link.
* Which Microsoft product can push out patches for non-Microsoft software to networked computers?
* What user needs to be disabled for web sites to load on a fresh install of IIS?
* Which users can by default use RDP?
* _FIXME Event Log infected examples t/f_
* What's the classic default login for MS SQL Server?
