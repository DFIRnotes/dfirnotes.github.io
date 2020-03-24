---
layout: post
title: HNFC Again (Again)
category: education,dfir
author: adricnet
---

HNFC Again, Again

To prepare for the _Linux Forensics_ class Hal is giving this week, I had a go at an old favourite, the original Honey Net Forensic Challenge (HNFC) image set...

To prepare for the _Linux Forensics_ class Hal is giving this week, I had a go at an old favourite, the original Honey Net Forensic Challenge (HNFC) image set.My [mirror](http://dfirfiles.net/chals/) is at dfirfiles, see it [hosted](https://web.archive.org/web/20190716094835/http://old.honeynet.org/challenge/) on the Internet Archive, and check out https://forensicswiki.xyz/wiki/index.php?title=Forensic_corpora for more.

## Plaso, split images

* Got Plaso installed and working, but only via [git](https://github.com/log2timeline/plaso) clone and pip3 manual install (all debs, PPAs failed in my tests due to distance from upstream)
* Stuffed one Plaso datafile full of five drives worth of image processing, but sis not discover how to specify mount points, hmm...

```
# for f in *.dd ; do log2timeline.py hnfc.plaso $f ; done
[...]
# psort.py -w plaso-out.tsv hnfc.plaso

plaso - psort version 20200227

Storage file            : hnfc.plaso
Processing time         : 00:00:26

Events:         Filtered        In time slice   Duplicates      MACB grouped    Total
                0               0               5               92818           92838

Identifier              PID     Status          Memory          Events          Tags            Reports
Main                    779682  exporting       90.8 MiB        92838 (526)     0 (0)           0 (0)

Processing completed.
```

```
# head -2 plaso-out.tsv 
datetime,timestamp_desc,source,source_long,message,parser,display_name,tag
1899-12-30T00:00:00+00:00,Content Modification Time,PLSRecall,PL/SQL Developer Recall file,Sequence number: 541611054 
[...]

# wc -l plaso-out.tsv 
92834 plaso-out.tsv

# pinfo.py hnfc.plaso
************************** Plaso Storage Information ***************************
            Filename : hnfc.plaso
      Format version : 20190309
Serialization format : json
--------------------------------------------------------------------------------

*********************************** Sessions ***********************************

[...]

************************* Events generated per parser **************************
Parser (plugin) name : Number of events
--------------------------------------------------------------------------------
            filestat : 92689
          pls_recall : 1
              syslog : 133
                utmp : 15
               Total : 92838
--------------------------------------------------------------------------------

```
## Sleuthkit FTW

I RTFM'd to refresh my memory on the core toolset and found the online docs excellent. They essentially told me everything I wanted and needed to know to get an <strike>old style</strike> *classic* MACTime timeline, look for deleted entries in the ext2, and dig in.

* http://wiki.sleuthkit.org/index.php?title=FS_Analysis
* http://wiki.sleuthkit.org/index.php?title=Timeline

And I hacked my own old [script](https://github.com/DFIRnotes/rules/blob/master/mount_hnfc.sh) to get them mounted so I could browse the filesystem, patches forthcoming :)

```
wget http://dfirfiles.net/chals/challenge-images.tar
tar xf challenge-images.tar
for i in *.gz; do gunzip $i; done

mkdir -p /mnt/hnfc

mount -o loop,ro ./hnfc/honeypot.hda8.dd /mnt/hnfc/
mount -o loop,ro ./hnfc/honeypot.hda1.dd /mnt/hnfc/boot
mount -o loop,ro ./hnfc/honeypot.hda6.dd /mnt/hnfc/home
mount -o loop,ro ./hnfc/honeypot.hda5.dd /mnt/hnfc/usr
mount -o loop,ro ./hnfc/honeypot.hda7.dd /mnt/hnfc/var

echo 'To chroot: chroot /mnt/hnfc /bin/bash'

```

### FLS timeline with mactime, quick script
```
#!/bin/bash                                                                                       
                                                                                                  
fls="fls"                                                                                         
out=">> hnfs-fls.body"                                                                            
                                                                                                  
fls -r  honeypot.hda1.dd -m /boot >> hnfc-fls.body                                                
fls -r  honeypot.hda5.dd -m /usr >> hnfc-fls.body                                                 
fls -r  honeypot.hda6.dd -m /home >> hnfc-fls.body
fls -r  honeypot.hda7.dd -m /var >> hnfc-fls.body
fls -r  honeypot.hda8.dd -m / >> hnfc-fls.body

echo "FLS'd"
```
```
# mactime -b hnfc-fls.body -d -h > hnfc-fls.mactime.csv
# wc -l hnf*.csv
103817 hnfc-fls.mactime.csv
```
I edited out the nice header lines added by -h for easy Gnumeric spreadsheet usage.

## Analysis

Some avenues that bore fruit today and are all pretty great analytical tools.

### Bash History

_What were the last actions recorded by the shell?_

There's only one non-root user in /home, so I took a peek at the tail end of the of the shell history file to see what was last recorded. It's mounted so we can go look at it with normal shell tools and could chroot in if we wanted to.

```
# mount | grep home
/root/lf/hnfc/honeypot.hda6.dd on /root/lf/hfc_root/home type ext2 (ro,relatime)
# cd hfc_root/home/drosen/
# tail .bash_history 
gunzip *
tar -xvf *
rm tpack*
cd " "
./install
exit
```

Someone installed (specifically opened archives and ran a script named install) something and removed (unlinked) something. Is that interesting ? Oh, and that's the whole file. That's pretty odd for a user account on a system that's been used. We might leap to the hypothesis that the history file was cleared.

I add "*tpack*" to a list of keywords for this case as I've not seen that before.

### Try the tail ?

_What's the last collection of events in the timeline after it's all assembled? _

Sometimes you come into a case knowing the "cause of death", and sometimes you don't, but it's always good to make sure. If there were response efforts before the images were taken you should see and discount those activities before you dig into the evidence.

```
# tail hnfc-fls.mactime.csv
Mon Mar 23 2020 15:27:47,21,.a..,l/lrwxrwxrwx,0,0,18,"/boot/System.map -> System.map-2.2.14-5.0"
Mon Mar 23 2020 15:27:47,22,.a..,l/lrwxrwxrwx,0,0,19,"/boot/module-info -> module-info-2.2.14-5.0"
Mon Mar 23 2020 15:28:07,10,.a..,l/lrwxrwxrwx,0,0,12,"/usr/tmp -> ../var/tmp"
Mon Mar 23 2020 15:29:13,1024,.a..,d/drwxr-xr-x,0,0,12097,"/var/log"
Mon Mar 23 2020 15:29:17,1460292,.a..,r/rrw-r--r--,0,0,12098,"/var/log/lastlog"
Mon Mar 23 2020 15:29:32,0,.a..,r/rrw-r--r--,0,0,12100,"/var/log/htmlaccess.log"
Mon Mar 23 2020 15:29:38,7974,.a..,r/rrw-r--r--,0,0,12104,"/var/log/messages"
Mon Mar 23 2020 15:29:48,268,.a..,r/rrw-r--r--,0,0,12111,"/var/log/secure"
Tue Mar 24 2020 09:33:03,4096,.a..,d/drwx------,500,500,15395,"/home/drosen"
Tue Mar 24 2020 09:33:11,52,.a..,r/rrw-------,500,500,15401,"/home/drosen/.bash_history"
```
Well there's the bash_history file .. Wait 2020? Ooops ... Let's scan up a bit. Hmm, even at the 2000 section where we should be we don't see anything too crazy looking as far as Linux filesystem activity on a running server goes. We see access to lots of common files and some crons execute ... nothing jumps out.

_Is that tpack thing in here?_

With the keyword we got from command history, let's search the filesystem timeline:
```
# grep tpack hnfc-fls.mactime.csv
Xxx Xxx 00 0000 00:00:00,4096,...b,d/drwxr-xr-x,0,0,108477,"/usr/include/netpacket"
Xxx Xxx 00 0000 00:00:00,2008,...b,r/rrw-r--r--,0,0,108478,"/usr/include/netpacket/packet.h"
Xxx Xxx 00 0000 00:00:00,4096,...b,d/drwxr-xr-x,0,0,31970,"/usr/lib/perl5/5.00503/i386-linux/netpacket"
Xxx Xxx 00 0000 00:00:00,1301,...b,r/rrw-r--r--,0,0,31971,"/usr/lib/perl5/5.00503/i386-linux/netpacket/packet.ph"
Xxx Xxx 00 0000 00:00:00,0,...b,d/drwxr-xr-x,500,100,8132,"/dev/tpack (deleted)"
Wed Feb 02 2000 15:39:43,1301,m...,r/rrw-r--r--,0,0,31971,"/usr/lib/perl5/5.00503/i386-linux/netpacket/packet.ph"
Tue Feb 29 2000 16:59:03,2008,m...,r/rrw-r--r--,0,0,108478,"/usr/include/netpacket/packet.h"
Sat Nov 04 2000 19:58:51,4096,m.c.,d/drwxr-xr-x,0,0,108477,"/usr/include/netpacket"
Sat Nov 04 2000 19:58:51,2008,..c.,r/rrw-r--r--,0,0,108478,"/usr/include/netpacket/packet.h"
Sat Nov 04 2000 20:01:23,4096,m.c.,d/drwxr-xr-x,0,0,31970,"/usr/lib/perl5/5.00503/i386-linux/netpacket"
Sat Nov 04 2000 20:01:23,1301,..c.,r/rrw-r--r--,0,0,31971,"/usr/lib/perl5/5.00503/i386-linux/netpacket/packet.ph"
Sun Nov 05 2000 10:39:24,1301,.a..,r/rrw-r--r--,0,0,31971,"/usr/lib/perl5/5.00503/i386-linux/netpacket/packet.ph"
Sun Nov 05 2000 10:39:42,2008,.a..,r/rrw-r--r--,0,0,108478,"/usr/include/netpacket/packet.h"
Wed Nov 08 2000 05:02:03,4096,.a..,d/drwxr-xr-x,0,0,31970,"/usr/lib/perl5/5.00503/i386-linux/netpacket"
Wed Nov 08 2000 05:02:04,4096,.a..,d/drwxr-xr-x,0,0,108477,"/usr/include/netpacket"
Wed Nov 08 2000 09:59:14,0,mac.,d/drwxr-xr-x,500,100,8132,"/dev/tpack (deleted)"
```

Perl network utilities, huh. 

_Wiggle it a little_

Sometimes the string you have doesn't match they way you have it. In this common situation check the case or disable case-sensitivity in your search. Try pieces of your keyword ( substrings or syllables) to see if your luck changes. Here I searched *snap* instead of *tsnap* and tripped over something.

```
# grep '.*snap\"$' hnfc-fls.mactime.csv 
Xxx Xxx 00 0000 00:00:00,3098,...b,r/rrwxr-xr-x,1010,100,109848,"/usr/man/.Ci/snap"
Sat Jun 03 2000 02:12:21,3098,m...,r/rrwxr-xr-x,1010,100,109848,"/usr/man/.Ci/snap"
Wed Nov 08 2000 09:51:55,3098,..c.,r/rrwxr-xr-x,1010,100,109848,"/usr/man/.Ci/snap"
Wed Nov 08 2000 09:56:04,3098,.a..,r/rrwxr-xr-x,1010,100,109848,"/usr/man/.Ci/snap"
```

Among a bunch of activity in "/usr/src/linux-2.2.14/include/linux/modules/" there's the creation, modification, change, and access of a folder in "/usr/man".

### What's that doing there ? 

The more familiar you arwe with a system the more your brain and tools can flag anomalies. As you begin to revview the evidence note anything that looks off and see if it's worth chasing. In this specific case knowing some Linux/UNIX sysadmin practices (or exhaustive familiarity with the FHS / ```man hier```) is a shortcut for me.

"/usr/man/.Ci/" looks pretty sketchy to me. The Leading dot in the folder name hides it from casual viewing and directory listing which is a classic hiding technique on Unix/Linux systems. To my memory there's no reason for their to be a folder here for the manual system, and also changes to the /usr filesystem should (under common practice) only be done by a sysadmin so there shoudl not be any user files anyywhere near "/usr/man".

### What's not there?

"The absence of evidence where it is expected is in itself evidence" --paraphrased forensic wisdom (need attrib)

Are the any files deleted from and possibly recoverable near "/usr/man/.Ci" ?

```
# grep /usr readme 
# fls -rd honeypot.hda5.dd > flrsd-usr.txt
```

```
# grep '.Ci' flrsd-usr.txt
d/d * 92757:    man/.Ci/ssh-1.2.27
r/r * 109861:   man/.Ci/named.tar
r/r * 109801:   man/.Ci/install-sshd1
r/r * 109802:   man/.Ci/install-sshd
r/r * 109803:   man/.Ci/install-named
d/d * 63098:    man/.Ci/bin
r/r * 109807:   man/.Ci/named.tgz
r/r * 109791:   man/.Ci/ssh-1.2.27.tar
r/r * 109816:   man/.Ci/.temp2
r/r * 109818:   man/.Ci/.temp3
r/r * 109853:   man/.Ci/.temp4
r/r * 109869:   man/.Ci/.temp5
r/r * 109849:   man/.Ci/ptyp
r/d * 109870:   man/.Ci/.temp6
r/r * 109861:   man/.Ci/in.ftpd
r/r * 109864:   man/.Ci/install-statd
r/r * 109865:   man/.Ci/nfs-utils-0.1.9.1-1.i386.rpm
r/r * 109866:   man/.Ci/wuftpd.rpm
r/r * 109867:   man/.Ci/install-wu
r/r * 109871:   man/.Ci/.temp7
r/r * 109872:   man/.Ci/.temp8
r/r * 109873:   man/.Ci/.temp9
r/r * 109874:   man/.Ci/.temp10
r/r * 109875:   man/.Ci/.temp11
r/r * 109876:   man/.Ci/.temp12
```

There were in this folder quite a collection of things including system packages (tgz and rpm), numbered temp files with hiding names (dots), a bin folder (scripts?) ... Well, that certainly looks like a lead worth chasing!
```
