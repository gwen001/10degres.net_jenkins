---
title: DNS enumeration with Host
tags:
- pentest
- information gathering
- dns
see_also:
  -
    link: Zone transfer on Wikipedia
    url: http://en.wikipedia.org/wiki/DNS_zone_transfer
---
While performing a pentest the information gathering phase is the most important, it's the key of a successful test. 
The DNS is very great source of informations, whith some simple queries you will be able to grab usefull datas about the domain you are targeting. 
The `host` command is a very powerful DNS lookup utility which is present in all Linux distribution.

For the examples, I will use a domain which allows that kind of query at this moment.

## Basic usage

As the `man` says, `host` is normally used to convert names to IP addresses and vice versa:

[![DNS enumeration host basic](/images/dns-enumeration-host-basic.png)](/images/dns-enumeration-host-basic.png)

If the domain doesn't exist, you will meet that message: `Host pmolkijn.de not found: 3(NXDOMAIN)`

If the ip doesn't point anywhere, you will get this message: `Host 91.121.139.2 not found: 3(NXDOMAIN)`

<!--more-->

## Forward

From here you can perform a brute force based on a dictionnary to find subdomains. 
Imagine a text file containing those few keywords:

mail, demo, test, admin, blog

With 3 lines of shell code, you could loop to query all derived subdomains:

```
#!/bin/bash

for name in $(cat subdomains.txt) ; do
  host $name.leparisien.fr |grep "has address" |cut -d " " -f 1,4
done
```

Will output:

[![DNS enumeration host forward](/images/dns-enumeration-host-forward.png)](/images/dns-enumeration-host-forward.png)

## Reverse

Of course you could query the discovered ip address but you can also query all ip in the same network to find more subdomains. Here is a simple shell script:

```
#!/bin/bash

for ip in $(seq 0 254) ; do
  host 160.92.126.$ip |grep "leparisien.fr" |cut -d " " -f 1,5
done
```

Will output:

[![DNS enumeration host reverse](/images/dns-enumeration-host-reverse.png)](/images/dns-enumeration-host-reverse.png)

## Zone transfer

Finally the best technique is to try a zone transfer.
Zone transfer is a mechanism available for administrators to replicate DNS databases across a set of DNS servers.

With the `-t` option you can grab specific informations about the domain: name server (ns), exchange mail (mx), alias (cname), etc.:

[![DNS enumeration host zone transfer](/images/dns-enumeration-host-zone-transfer-1.png)](/images/dns-enumeration-host-zone-transfer-1.png)

Then, for each name server found we will try to perform a zone transfer for the domain of the example:

[![DNS enumeration host zone transfer](/images/dns-enumeration-host-zone-transfer-2.png)](/images/dns-enumeration-host-zone-transfer-2.png)

Et voilà! My screen is to small but more than 100 unique subdomains have been found.
If the zone transfer fails, you will get this message: `; Transfer failed.`

There is many tools to deal with DNS: `dig`, `dnsenum`, `dnsrecon` can also do the trick. 
My favorite and probably the easiest is [Fierce](http://ha.ckers.org/fierce/ "Fierce"){:class="flashlink" target="_blank"}.
