---
title: Subdomain enumeration
image:
  feature: images/subdomain-enumeration-aquatone.jpg
tags:
- recon
- subdomain
- bug bounty
- information gathering
- tools
see_also:
  -
    link: The Art of Subdomain Enumeration
    url: https://blog.sweepatic.com/art-of-subdomain-enumeration/
  -
    link: A penetration tester’s guide to sub-domain enumeration
    url: https://blog.appsecco.com/a-penetration-testers-guide-to-sub-domain-enumeration-7d842d5570f6
---
A friend recently asked me what methods I use to find subdomains.
To be honest I was confused, like *"oooohhh so much, brute force mmm... zone transfer and... brute for... wait Google and mmm... many other tools!"*
What a shame that I was so inaccurate after so much time spent to look for subdomains.
Time to dig a little bit!
After I wrote a list of the most popular methods, I tried to make a list of some tools and online resources to exploit them.
Of course this list is far from exhaustive, there are many new stuff every day, but it's still a good start :)
<!--more-->


## Methods

__Brute force__  
The easiest way. Try millions and millions words as subdomains and check which ones are alive with a forward DNS request.

__Zone transfer aka AXFR__  
Zone transfer is a mechanism that administrators can use to replicate DNS databases but sometimes the DNS is not well configured and this operation is allowed by anyone, revealing all subdomains configured.  

__DNS cache snooping__  
DNS cache snooping is a specific way to query a DNS server in order to check if a record exists in his cache.

__Reverse DNS__  
Try to find the domain name associated with an IP address, it's the opposite of Forward DNS. 

__Alternative names__  
Once the first round of your recon is finished, apply permutations and transformations (based on another wordlist maybe?) to all subdomains discovered in order to find new ones.

__Online DNS tools__  
There are many websites that allow to query DNS databases and their history.

__SSL Certificates__  
Request informations about all certificates linked to a specific domain, and obtain a list of subdomains covered by these certificates.

__Search engines__  
Search for a specific domain in your favourite search engine then minus the discovered sudomains one by one `site:example.com -www -dev`

__Technical tools/search engines__  
More and more companies host their code online on public platform, most of the time these services have a search bar.

__Text parsing__  
Parse the HTML code of a website to find new subdomains, this can be applied to every resources of the company, office documents as well.

__VHost discovery__  
Try to find any other subdomain configured on the same web server by brute forcing the HTTP `Host` header.


## Tools

[Altdns](https://github.com/infosec-au/altdns){:class="flashlink" target="_blank"}: alternative names brute forcing  
[Amass](https://github.com/caffix/amass){:class="flashlink" target="_blank"}: brute force, Google, VirusTotal, alt names  
[aquatone-discover](https://github.com/michenriksen/aquatone){:class="flashlink" target="_blank"}: Brute force, Riddler, PassiveTotal, Threat Crowd, Google, VirusTotal, Shodan, SSL Certificates, Netcraft, HackerTarget, DNSDB  
[BiLE-suite](https://github.com/sensepost/BiLE-suite){:class="flashlink" target="_blank"}: HTML parsing, alt names, reverse DNS  
[blacksheepwall](https://github.com/tomsteele/blacksheepwall){:class="flashlink" target="_blank"}: AXFR, brute force, reverse DNS, Censys, Yandex, Bing, Shodan, Logontube, SSL Certificates, Virus Total  
[Bluto](https://github.com/RandomStorm/Bluto){:class="flashlink" target="_blank"}: AXFR, netcraft, brute force  
[brutesubs](https://github.com/anshumanbh/brutesubs){:class="flashlink" target="_blank"}: enumall, Sublist3r, Altdns  
[cloudflare_enum](https://github.com/mandatoryprogrammer/cloudflare_enum){:class="flashlink" target="_blank"}: Cloudflare DNS  
[CTFR](https://github.com/UnaPibaGeek/ctfr){:class="flashlink" target="_blank"}: SSL Certificates  
[DNS-Discovery](https://github.com/m0nad/DNS-Discovery){:class="flashlink" target="_blank"}: brute force  
[DNS Parallel Prober](https://github.com/lorenzog/dns-parallel-prober){:class="flashlink" target="_blank"}: DNS resolver  
[dnscan](https://github.com/rbsec/dnscan){:class="flashlink" target="_blank"}: AXFR, brute force  
[dnsrecon](https://github.com/darkoperator/dnsrecon){:class="flashlink" target="_blank"}: AXFR, brute force, reverse DNS, snoop caching, Google  
[dnssearch](https://github.com/evilsocket/dnssearch){:class="flashlink" target="_blank"}: brute force  
[domained](https://github.com/reconned/domained){:class="flashlink" target="_blank"}: Sublist3r, enumall, Knockpy, SubBrute, MassDNS, recon-ng  
[enumall](https://github.com/jhaddix/domain){:class="flashlink" target="_blank"}: recon-ng -> Google, Bing, Baidu, Netcraft, brute force  
[Fierce](https://github.com/mschwager/fierce){:class="flashlink" target="_blank"}: AXFR, brute force, reverse DNS  
[Knockpy](http://github.com/guelfoweb/knock){:class="flashlink" target="_blank"}: AXFR, virustotal, brute force  
[MassDNS](https://github.com/blechschmidt/massdns){:class="flashlink" target="_blank"}: DNS resolver  
[Second Order](https://github.com/mhmdiaa/second-order){:class="flashlink" target="_blank"}: HTML parsing  
[Sonar](https://github.com/jrozner/sonar){:class="flashlink" target="_blank"}: AXFR, brute force  
[SubBrute](https://github.com/TheRook/subbrute){:class="flashlink" target="_blank"}: brute force  
[Sublist3r](https://github.com/aboul3la/Sublist3r){:class="flashlink" target="_blank"}: Baidu, Yahoo, Google, Bing, Ask, Netcraft, DNSdumpster, VirusTotal, Threat Crowd, SSL Certificates, PassiveDNS  
[theHarvester](https://github.com/laramies/theHarvester){:class="flashlink" target="_blank"}: reverse DNS, brute force, Google, Bing, Dogpile, Yahoo, Baidu, Shodan, Exalead  
TXDNS: alt names (typo/tld)  
[vhost-brute](https://github.com/gwen001/vhost-brute){:class="flashlink" target="_blank"}: vhost discovery  
[VHostScan](https://github.com/codingo/VHostScan){:class="flashlink" target="_blank"}: vhost discovery  
[virtual-host-discovery](https://github.com/jobertabma/virtual-host-discovery){:class="flashlink" target="_blank"}: vhost discovery  


## Online DNS tools

[https://hackertarget.com/](https://hackertarget.com/){:target="_blank"}  
[http://searchdns.netcraft.com/](http://searchdns.netcraft.com/){:target="_blank"}  
[https://dnsdumpster.com/](https://dnsdumpster.com/){:target="_blank"}  
[https://www.threatcrowd.org/](https://www.threatcrowd.org/){:target="_blank"}  
[https://riddler.io/](https://riddler.io/){:target="_blank"}  
[https://api.passivetotal.org](https://api.passivetotal.org){:target="_blank"}  
[https://www.censys.io](https://www.censys.io){:target="_blank"}  
[https://api.shodan.io](https://api.shodan.io){:target="_blank"}  
[http://www.dnsdb.org/f/](http://www.dnsdb.org/f/){:target="_blank"}  
[https://www.dnsdb.info/](https://www.dnsdb.info/){:target="_blank"}  
[https://scans.io/](https://scans.io/){:target="_blank"}  
[https://findsubdomains.com/](https://findsubdomains.com/){:target="_blank"}  
[https://securitytrails.com/dns-trails](https://securitytrails.com/dns-trails){:target="_blank"}  

[https://crt.sh/](https://crt.sh/){:target="_blank"}  
[https://certspotter.com/api/v0/certs?domain=example.com](https://certspotter.com/api/v0/certs?domain=example.com){:target="_blank"}  
[https://transparencyreport.google.com/https/certificates](https://transparencyreport.google.com/https/certificates){:target="_blank"}  
[https://developers.facebook.com/tools/ct](https://developers.facebook.com/tools/ct){:target="_blank"}  


## Search engines

[http://www.baidu.com/](http://www.baidu.com/){:target="_blank"}  
[http://www.yahoo.com/](http://www.yahoo.com/){:target="_blank"}  
[http://www.google.com/](http://www.google.com/){:target="_blank"}  
[http://www.bing.com/](http://www.bing.com/){:target="_blank"}  
[https://www.yandex.ru/](https://www.yandex.ru/){:target="_blank"}  
[https://www.exalead.com/search/](https://www.exalead.com/search/){:target="_blank"}  
[http://www.dogpile.com/](http://www.dogpile.com/){:target="_blank"}  
[https://www.zoomeye.org/](https://www.zoomeye.org/){:target="_blank"}  
[https://fofa.so/](https://fofa.so/){:target="_blank"}  


## Technical tools/search engines

[https://github.com/](https://github.com/){:target="_blank"}  
[https://gitlab.com/](https://gitlab.com/){:target="_blank"}  
[https://www.virustotal.com/fr/](https://www.virustotal.com/fr/){:target="_blank"}  


## DNS cache snooping


```
nslookup -norecursive domain.com
```

```
nmap -sU -p 53 --script dns-cache-snoop.nse --script-args 'dns-cache-snoop.mode=timed,dns-cache-snoop.domains={domain1,domain2,domain3}' <ip>
```	


## Others online resources

[https://ask.fm/](https://ask.fm/){:target="_blank"}  
[http://logontube.com/](http://logontube.com/){:target="_blank"}  
[http://commoncrawl.org/](http://commoncrawl.org/){:target="_blank"}  
[http://www.sitedossier.com/](http://www.sitedossier.com/){:target="_blank"}  

