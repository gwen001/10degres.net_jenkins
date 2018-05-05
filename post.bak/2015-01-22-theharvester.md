---
title: theHarvester
tags:
- pentest
- information gathering
see_also:
  -
    link: Introduction to pentesting
    url: /introduction-to-pentesting/
---
Information gathering is the first and the most important step of a penetration test. 
More informations you will grab, easier the exploit will be. 

Developed by Christian Martorella [theHarvester](https://code.google.com/p/theharvester/){:class="flashlink" target="_blank"} is very usefull for this task. 
It's a python script that will help you to find user emails and subdomains of a given domain by merely parsing search engines results. 
The following data sources are supported :

- bing,
- google, googleCSE, googleplus, google-profiles
- jigsaw  
- linkedin  
- people123  
- pgp  
- shodan  
- twitter 

theHarvester is by default installed on Kali Linux. Basic usage is:
`theharvester -d <domain> -b <source>`

<!--more-->

Some options are available to tweak your request:

-d: the domain you are looking for  
-b: source or `all`  
-f: output&nbsp;file (html and xml)  
-l: limit the number of results used for each source  
-s: start result number  
-h: query Shodan with&nbsp;each discovered hosts  
-n: perform a reverse dns lookup for each range of ip address discovered  
-c: perform a brute force search (can't make it work anyway...)

Example:

[![theHarvester](/images/theharvester_1.png)](/images/theharvester_1.png)
