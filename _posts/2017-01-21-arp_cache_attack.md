---
layout: post
title: ARP attack classwork
category: Network Analysis
tag: imported, ittam
---

After looking at the tables with the MAC address for awhile I looked up the OUI online and substituted them in, hoping to catch something I'd missed. Indeed there was a third MAC from a third manufacturer in the discussion. All three OEMs make network gear as well as endpoint systems.

https://www.wireshark.org/tools/oui-lookup.html

<code>
$ tshark -n -r arp.pcap -q -z conv,ether | sed -e 's,00:21:70:c0:56:f0,Dell,g' -e 's,00:26:0b:31:07:33,Cisco,g' -e 's,00:25:b3:bf:91:ee,HP,g'
</code>

<pre>
================================================================================
Ethernet Conversations
Filter:<No Filter>
|       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
| Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
Dell    <-> HP         50     30653      61     15255     111     45908     4.646389000         6.2772
Dell    <-> Cisco         25     11978      28      4287      53     16265     0.000000000         0.4749
HP    <-> ff:ff:ff:ff:ff:ff          0         0       1        60       1        60    14.392559000         0.0000
================================================================================
</pre>

<code>
$ tshark -n -r arp.pcap -q -T fields -e eth.src -e eth.dst -e ip.src -e tcp.srcport -e ip.dst -e tcp.dstport -E header=y > arp-pcap.tsv 

$ sed -e 's,00:21:70:c0:56:f0,Dell,g' -e 's,00:26:0b:31:07:33,Cisco,g' -e 's,00:25:b3:bf:91:ee,HP,g' arp-pcap.tsv
</code>

<pre>
eth.src eth.dst ip.src  tcp.srcport     ip.dst  tcp.dstport
Dell    Cisco   172.16.0.107            12.153.20.41
Cisco   Dell    12.153.20.41            172.16.0.107
Dell    Cisco   172.16.0.107    45691   74.125.95.147   80
Cisco   Dell    74.125.95.147   80      172.16.0.107    45691
Dell    Cisco   172.16.0.107    45691   74.125.95.147   80
Dell    Cisco   172.16.0.107    45691   74.125.95.147   80
Cisco   Dell    74.125.95.147   80      172.16.0.107    45691
Cisco   Dell    74.125.95.147   80      172.16.0.107    45691
Dell    Cisco   172.16.0.107    45691   74.125.95.147   80

...

HP      Dell    12.153.20.41            172.16.0.107
Dell    HP      172.16.0.107            12.153.20.41
HP      Dell    12.153.20.41            172.16.0.107
Dell    HP      172.16.0.107            12.153.20.41
HP      Dell    12.153.20.41            172.16.0.107
Dell    HP      172.16.0.107            12.153.20.41
HP      Dell    12.153.20.41            172.16.0.107
Dell    HP      172.16.0.107            12.153.20.41
HP      Dell    12.153.20.41            172.16.0.107
HP      ff:ff:ff:ff:ff:ff
</pre>

We can look at the ARP specifically with simple display filters, and the substitutions help again. If (naively) the router is the Cusco device then it has been cut out by these ARP responses:

<pre>
$ tshark -n -r arp.pcap  -Y arp | sed -e 's,00:21:70:c0:56:f0,Dell,g' -e 's,00:26:0b:31:07:33,Cisco,g' -e 's,00:25:b3:bf:91:ee,HP,g'
 54   4.646389 HP -> Dell ARP 60 Who has 172.16.0.107? Tell 172.16.0.1
 55   4.646442 Dell -> HP ARP 42 172.16.0.107 is at Dell
 56   4.646455 HP -> Dell ARP 60 172.16.0.1 is at HP
165  14.392559 HP -> ff:ff:ff:ff:ff:ff ARP 60 Who has 172.16.0.1? Tell 172.16.0.105
</pre>

HTTP

<pre>
⟫ tshark -n -r arp.pcap  -q -Y http.request -T fields -e http.request.full_uri | head
http://www.google.com/
http://www.google.com/csi?v=3&s=webhp&action=&e=17259,18168,24483,25233,25460,25475,25511,25529,25585&ei=dNQ_TOejLY_6M_Sa8JwH&expi=17259,18168,24483,25233,25460,25475,25511,25529,25585&imc=1&imn=1&imp=1&rt=prt.30,xjsls.37,xjses.75,xjsee.89,ol.92,iml.45
http://www.google.com/csi?v=3&s=webhp&action=&e=17259,18168,24483,25233,25460,25475,25511,25529,25585&ei=dNQ_TOejLY_6M_Sa8JwH&expi=17259,18168,24483,25233,25460,25475,25511,25529,25585&imc=1&imn=1&imp=1&rt=
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=U&cp=1&pf=i&hl=en&source=hp&aq=f&aqi=&aql=&oq=U&gs_rfai=&fp=57d9c86769d1bf04&tch=1&ech=1&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK&cp=2&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK&gs_rfai=CqlB2e9Q_TMi1EI-GNKHCgNoMAAAAqgQFT9DM8J4&fp=57d9c86769d1bf04&tch=1&ech=2&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK%20&cp=3&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK+&gs_rfai=C8bMte9Q_TNiCGqX4MbrIia4KAAAAqgQFT9C0rKA&fp=57d9c86769d1bf04&tch=1&ech=3&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK%20b&cp=4&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK+b&gs_rfai=CXvV2fNQ_TI36AYuWMJe8sc4CAAAAqgQFT9AALr0&fp=57d9c86769d1bf04&tch=1&ech=4&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK%20bas&cp=6&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK+bas&gs_rfai=CId5GfNQ_TLutFIrAM6CPoYEKAAAAqgQFT9AUgUQ&fp=57d9c86769d1bf04&tch=1&ech=5&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK%20bask&cp=7&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK+bask&gs_rfai=CId5GfNQ_TLutFIrAM6CPoYEKAAAAqgQFT9AUgUQ&fp=57d9c86769d1bf04&tch=1&ech=6&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
http://www.google.com/complete/gsearch?hl=en&client=hp&expIds=17259,18168,24483,25233,25460,25475,25511,25529,25585&sugexp=ldymls&xhr=t&q=UK%20basketb&cp=10&pf=i&hl=en&source=hp&aq=f&aqi=g10&aql=&oq=UK+basketb&gs_rfai=Cl9tKfNQ_TP-UJ4-GNKHCgNoMAAAAqgQFT9BrvF4&fp=57d9c86769d1bf04&tch=1&ech=7&psi=dNQ_TOejLY_6M_Sa8JwH12792515729190
</pre>

The HTTP traffic all appears to be (classic HTTP) Google searches. The DNS traffic all goes to the 12 address and none of the responses look odd or reference any 172 addresses.
