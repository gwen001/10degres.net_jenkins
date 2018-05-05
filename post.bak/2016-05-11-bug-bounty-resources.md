---
title: Bug bounty resources
tags:
- bug bounty
- resources
see_also:
  -
    link: The unofficial HackerOne disclosure timeline
    url: http://h1.nobbd.de/index.php?start=0
---
In my opinion the best way to learn hacking and security is to read public disclosure. 
It's a great resources of tips and tools to use to make your life easier.

Here are some blogs I frequently visit from bounty hunters themself. 
They explain their findings, why it occurs, how they were able to exploit and sometimes how much they win. 
I visit them once a week and I also follow their writer on Twitter to not miss the bugs they don't review.   

Ben Sadeghipour, plays with many various vulns:  
[http://archive.nahamsec.com/](http://archive.nahamsec.com/){:class="flashlink" target="_blank"}

Filedescriptor, I don't know who is this guy but his profil on HackerOne is truely awesome:  
[https://blog.innerht.ml/](https://blog.innerht.ml/){:class="flashlink" target="_blank"}

Sean Melia, swings between the first and the second place on HackerOne:  
[https://seanmelia.wordpress.com/](https://seanmelia.wordpress.com/){:class="flashlink" target="_blank"}

Jack Whitton, is a true XSS jedi:  
[https://whitton.io/](https://whitton.io/){:class="flashlink" target="_blank"}

Nir Goldshlager, CEO of Break Security:  
[http://www.breaksec.com/](http://www.breaksec.com/?page_id=6002){:class="flashlink" target="_blank"}

<!--more-->

To stay aware of new bugs who run in wild I follow some other blogs about web security. 
Those are professional blogs, they always try to sell you something but whatever, the goal here is to learn.
Most of the time they also explain how to protect agains the attacks, wich is a very good point to add in a bug report.  

PortSwigger Web Security Blog  
[http://blog.portswigger.net/](http://blog.portswigger.net/){:class="flashlink" target="_blank"}

Sucuri Blog  
[https://blog.sucuri.net/](https://blog.sucuri.net/){:class="flashlink" target="_blank"}

Finally here are some of my favorites issues, my current top 15, the ones I like to read again and again to understand the vulnerability 
and try to discern the state of mind of the hacker who found it. 
It's also a good way to improve your report skill and see the way hackers communicates with security teams to keep good feelings. 

[XSS on OAuth authorize/authenticate endpoint](https://hackerone.com/reports/87040){:target="_blank"}  
[Remote Code Execution on Shopify](https://hackerone.com/reports/73567){:target="_blank"}  
[DNS Misconfiguration](https://hackerone.com/reports/1509){:target="_blank"}  
[XSS in the all widgets of shopifyapps.com](https://hackerone.com/reports/119250){:target="_blank"}  
[Private program activity timeline information disclosure](https://hackerone.com/reports/116029){:target="_blank"}  
[AWS S3 bucket writeable for authenticated aws users](https://hackerone.com/reports/128088){:target="_blank"}  
[uber.com may RCE by Flask Jinja2 Template Injection](https://hackerone.com/reports/125980){:target="_blank"}  
[CSV Injection in business.uber.com](https://hackerone.com/reports/126109){:target="_blank"}  
[Reflected XSS on developer.uber.com via Angular template injection](https://hackerone.com/reports/125027){:target="_blank"}  
[Pixel flood attack](https://hackerone.com/reports/390){:target="_blank"}  
[Stored XSS in archive.uber.com Due to Injection of Javascript:alert(0)](https://hackerone.com/reports/126906){:target="_blank"}  
[XSS In archive.uber.com Due to Mime Sniffing in IE](https://hackerone.com/reports/126197){:target="_blank"}  
[CRLF Injection in developer.uber.com](https://hackerone.com/reports/125984){:target="_blank"}  
[Reflected XSS via Unvalidated / Open Redirect in uber.com](https://hackerone.com/reports/125791){:target="_blank"}  
[Reflected XSS via Livefyre Media Wall in newsroom.uber.com](https://hackerone.com/reports/134061){:target="_blank"}  

Each Time I learn a new kind of issue, I try to reproduce it on on my local lab, then I try it on security programs I'm currently working on.
Sometime it works, sometimes not but bug bounty is also about patience.

Happy reading !
