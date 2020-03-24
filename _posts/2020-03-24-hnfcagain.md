---
layout: post
title: HNFC Again (Again)
category: education,dfir
author: adricnet
---

To prepare for the _Linux Forensics_ class Hal is giving this week, I had a go at an old favourite, the original Honey Net Forensic Challenge (HNFC) image set. My [mirror](http://dfirfiles.net/chals/) is at dfirfiles, see it [hosted](https://web.archive.org/web/20190716094835/http://old.honeynet.org/challenge/) on the Internet Archive, and check out https://forensicswiki.xyz/wiki/index.php?title=Forensic_corpora for more.

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
## Analysis


### Bash History


### Try the tail ?


### What's that doing there ? 
