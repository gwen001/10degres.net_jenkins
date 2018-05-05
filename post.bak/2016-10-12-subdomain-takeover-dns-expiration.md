---
title: Subdomain takeover - DNS expiration
tags:
- dns
- subdomain
- tools
see_also:
  -
    link: Explanation by Peter Yaworski
    url: https://www.youtube.com/watch?v=aurAbCcF-lk
---
One quick and easy way to make cash in bug bounty job is subdomain takeover.
The goal is to thief a forgetted/unused subdomain of your target and put a PoC in place.
If you are able to do that, that means that instead of a plain text file, an attacker could replicate the true site of the victim and deals with phishing.
This way he could trick users and even the employees of the company to grab useful datas like credentials, this can also have really huge impact on the companies reputation.

First of all you have to find a list of subdomains of your target.
To perform that task, you simply can use a tool like [TheHarvester](/theharvester/) or [DNSRecon](https://github.com/darkoperator/dnsrecon){:class="flashlink" target="_blank"}.
<!--more-->

Then for each subdomains you should check if it is an alias or not, I personnally use the command `host`:

~~~bash
$ host rencontres.leparisien.fr
rencontres.leparisien.fr is an alias for www.pointscommuns.com.
www.pointscommuns.com has address 217.70.188.38
~~~

if yes, and if the alias destination is an external domain, then you should check this expiry date of this domain.
Because if this domain has expired, that means an attacker could buy it through a registrar like Gandi for a small amount.
Then put a fake look alike website in place and finally start social engineering by impersonating the vulnerable company.

To perform all those tests, I wrote a PHP script that takes a subdomains list as an argument.
Usage is:

```
Usage: php dnsexpire.php [OPTIONS] -f <subdomain|input file>

Options:
    -a  set alert for result output, default=30 days
    -f  subdomains list source file
    -h  print this help

Examples:
    php dnsexpire.php -f example.com
    php dnsexpire.php -a 10 -f dns.txt
```

Output is:

[![DNS expiration PHP tool](/images/dnsexpire-example.jpg)](/images/dnsexpire-example.jpg)

The code is available on my [GitHub repository](https://github.com/gwen001/dnsexpire){:class="flashlink" target="_blank"} so give it a try!