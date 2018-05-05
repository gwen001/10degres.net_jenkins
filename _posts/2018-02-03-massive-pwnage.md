---
title: Massive pwnage
image:
  feature: images/massive-xss.png
tags:
- aws
- bucket
- subdomain
- xss
- heroku
- cloudfront
see_also:
  -
    link: Top 500 Most Important XSS Script Cheat
    url: https://gbhackers.com/top-500-important-xss-cheat-sheet/
---
There is different ways to hunt for vulnerabilities, depending of your knowledge, your skills, your expectation and how you like to chase.
I personally love programming and, as a true developper, I'm lazy, so I love to automate things.
By writing some piece of code or by combining multiple tools, you can find a lot of interesting stuff.
This article is about some scripts/tricks I wrote/use to perform massive tests:

- XSS with PhantomJS
- Heroku subdomain takeover
- Amazon S3 buckets theft
<!--more-->


## XSS with PhantomJS

The first thing to do is to get a list of urls to test, it's like finding endpoints when you perform the recon of a new target.
And since even an url without any parameter can be vulnerable, it doesn't matter if our urls have one or several or none.

Possible sources:
- using the [scanner INURLBR](https://github.com/googleinurl/SCANNER-INURLBR){:class="flashlink" target="_blank"}, you can retrieve a list of urls from many search engines, a simple dork as `inurl:"&"` would return many results
- a list of subdomains in scope of different bug bounty programs
- the list of the most famous xxx websites in whatever country or all around the world
- the history of the Wayback Machine
- ...

The second thing you need is a good list of payloads.
You could use the awesome [Brutal Cheat Sheet](https://leanpub.com/xss){:class="flashlink" target="_blank"} list or grab another one on the internet or create your own.
Another good one is the list from RSnake because it contains payloads with a call of an external Javascript, using [XSS Hunter](https://xsshunter.com/app){:class="flashlink" target="_blank"} here could be a good idea, you never know...

Finally you need a program to automatically test all the urls you grabbed.
You could probably use Burp Suite but I'm not sure it would be the best tool to test XSS.
@brutelogic created the magic extension [Knoxss](https://knoxss.me/){:class="flashlink" target="_blank"}, you can use the free version.
However it becomes very complicated when you deal with thousands of urls, I already tried to script it but I was faced to the rate limit of the firewall.
So I finally created my own script, it was originally based on string comparaison but I recently changed my mind and I implemented PhantomJS which is much more reliable.

```
$ php testxss.php --prefix --suffix --sos --threads=10 --verbose=2
--phantom=/usr/local/bin/phantomjs --payload=brute-full.txt --urls=urls.txt
```
[![massive XSS](/images/massive-xss.png)](/images/massive-xss.png)


## Heroku subdomain takeover

I recently discovered [DNSTrails](https://dnstrails.com/){:class="flashlink" target="_blank"}, "The World's Largest Repository of historical DNS data". 
Basically it's a huge database of DNS history.  

After reading the report [#296907](https://hackerone.com/reports/296907){:class="flashlink" target="_blank"} on Hackerone, I had the idea to check the IPs of Heroku.
I checked my log files and I extracted all IPs I had, linked to `herokuapp.com`.
I have 44 address in my list, they probably own much more but that was enough for a single test.

Calling the API of DNSTrails, you can retrieve many domains and subdomains configured with those DNS: `https://app.securitytrails.com/api/search/by_type/ip/<ip>`
Looping through the 44 IPs, I got +30k (sub)domains. Doh!

We all know that a misconfigured Heroku service leads to this page:
[![Heroku error](/images/heroku-error.png)](/images/heroku-error.png)

It would be impossible (or very very long) to manually test all domains. Fortunately Michael Henriksen wrote [Aquatone](https://github.com/michenriksen/aquatone){:class="flashlink" target="_blank"}.
This tool can check if a given list of subdomains is vulnerable to subdomain takeover, it supports services like: Heroku, Cloudfront, Zendesk, Uservoice and many others...
It comes very handy when you deal with so much datas.  

```
$ aquatone-takeover --domain herokuapp.com
```
[![massive subdomain takeover Aquatone](/images/massive-subto-aquatone.png)](/images/massive-subto-aquatone.png)

I also wrote my own script to test some 3rd party services a long time ago, so I also injected my list in it, the result was pretty much the same.

```
$ for d in $(cat domains.txt) ; do php 3rdparty.php -s heroku -d $d ; done
```
[![massive subdomain takeover 3rd party services](/images/massive-subto-3rdps.png)](/images/massive-subto-3rdps.png)

Let's say 10% of the domains are vulnerable.  
[![massive subdomain surprise](/images/massive-subto-surprise.png)]()


## Amazon S3 buckets theft

Here I started to query Threat Crowd with every IP I was able to link to Amazon Service using [https://bgp.he.net/](https://bgp.he.net/){:target="_blank"} and some other resources.
`https://www.threatcrowd.org/searchApi/v1/api.php?type=ip&query=<ip>`  
I locally stored the json result and more than 3000 requests later, I got 90k+ unique (sub)domains.

Ok, things change, so probably some of them are not linked to Cloudfront anymore but hey! It's worth to try!
So I wrote a quick script that basically requests every domain in the list and check the response from the server.

```
$ php cloudfront.php domains.txt
```
[![massive bucket cloudfront](/images/massive-bucket-cloudfront.png)](/images/massive-bucket-cloudfront.png)

After extracting all buckets found, about 37000, I ran my tool to test their permissions, maybe some of them are writable? Really??

About 2300 buckets are world writable, and for 1950 of them any user could alter the permissions, so everything is possible.

[![massive bucket vulnerable](/images/massive-bucket-vulnerable.png)](/images/massive-bucket-vulnerable.png)


## Conclusion

I don't know how far this can goes. I think there is no limit to automation when combined with a good imagination and some technical skills, you can always dig deeper and deeper. 
The problem is, as a hunter, how would you monetize the time you spend on this?  
  
2 years ago, when I started bug bounty, I was confident enough to think that companies would be happy to be warned of that kind of problems.
But the responses were very different from a firm to another. Sometimes you get a "WTF?? Who the hell are you? What are you doing with my website?!", then take care.
Sometimes a single "Thank you", no matter if the issue is fixed or not, and sometimes nothing, silence, they simply ignore you...	

As far as I can see, chasing in the wild is fun, you can learn alot, but you won't make money this way.


## Tools

[The script waybackurls.py](https://gist.github.com/mhmdiaa/adf6bff70142e5091792841d4b372050){:class="flashlink" target="_blank"}  
[My tool: testxss](https://github.com/gwen001/testxss/){:class="flashlink" target="_blank"}  
[My tool: 3party-services](https://github.com/gwen001/3rdparty-services){:class="flashlink" target="_blank"}  
[The Cloudfront checker script](http://10degres.net/assets/cloudfront.txt){:class="flashlink" target="_blank"}  
[My tool: s3-buckets-finder](https://github.com/gwen001/s3-buckets-finder){:class="flashlink" target="_blank"}  
