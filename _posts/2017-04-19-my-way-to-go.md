---
title: My way to go
see_also:
  -
    link: The Bug Hunters Methodology by Jason Haddix
    url: https://github.com/jhaddix/tbhm
---

## Project
* Find Amazon s3 buckets:  
`s3-buckets-bruteforce /opt/SecLists/mine/s3-buckets.txt <project>-`  
if found: `s3-buckets-extractor <bucket>`  
* Explore GitHub account:  
`github-search -o <project> -r 1000 -s password`  
`gitrob analyze quizlet --no-color --no-server`  

<!--more-->

## Domain
* Check expiration date: `whois <domain>`  
* Find subdomains:  
  * Google: `site:<domain> -www`  
  * Test zone transfer: `dnsrecon -t axfr -d <domain>`  
  * Brute force:  
  `dnsrecon -t brt -D /opt/SecLists/Discovery/DNS/subdomains-top1mil-20000.txt <domain>`  
  `sublist3r -d <domain>`  
  `altdns -i dns.txt -o /tmp/perm -w /opt/SecLists/Discovery/DNS/subdomains-top1mil-20000.txt -r -s final.txt`  
  * and others  
  `theharvester -d <domain> -b all -l 1000 -n`  
  `subthreat <domain>`  
  `crtsh <domain>`  


## Server
* Find open ports:  
`portscan_nc <ip> 1 65535`  
`nmap -A -T4 -sS -p 1-65535 --open <ip> -Pn`  
`nmap -A -T4 -sU --top-ports=1000 --open <ip> -Pn`  
  * 21/tcp:  
  test anonymous login: `nmap -p 21 --script ftp-anon <ip> -Pn`  
  try brute force:  
  `hydra -F -L /opt/SecLists/Usernames/top_shortlist.txt -P /opt/SecLists/Passwords/best1050.txt ftp://<ip>`  
  `patator ftp_login host=<ip> user=root password=FILE0 0=/opt/SecLists/Passwords/passwords_john.txt`  
  * 22/tcp: try brute force root password:  
  `hydra -F -l root -P /opt/SecLists/Passwords/best1050.txt ssh://<ip>`  
  `patator ssh_login host=<ip> user=root password=FILE0 0=/opt/SecLists/Passwords/passwords_john.txt`  
  * 25/tcp: test smtp user enumeration:  
  `smtp-user-enum -p 25 -M VRFY -U /opt/SecLists/metasploit/unix_users.txt -t <ip>`  
  * 161/tcp:  
  `onesixtyone <ip>`  
  * 3306/tcp: try brute force root password  
  `hydra -F -l root -P /opt/SecLists/Passwords/best1050.txt mysql://<ip>`  
  `patator mysql_login host=<ip> user=root password=FILE0 0=/opt/SecLists/Passwords/passwords_john.txt`  
  * 3389/tcp: try brute force Administrator password  
  `hydra -F -l Administrator -P /opt/SecLists/Passwords/best1050.txt rdp://<ip>`  
  * if web, perform same test than a host.  


## Host
* Try most common Google dorks  
* Check SSL certificate: `testssl --color 0 -U <host>`  
* Test CRLF: `testcrlf -o <host> -p http`  
* Test CORS: `testcors -o <host> -p http`  
* Test open redirect: `ultimate-open-redirect -t http://<host> -z 10degres.net`  
* Find technologies:  
`wappalyzer http://<host>`  
`whatweb -v http://<host>`  
* Quick test:  
`dirb http://<host> /opt/SecLists/mine/myhardw.txt -S`  
* Find directories:  
`nikto -h http://<host>`  
`dirb http://<host> /opt/SecLists/dirb/big.txt -S`  
`wfuzz -c -z file,/opt/SecLists/dirb/big.txt --hc 404 http://<host>/FUZZ`  
* Find XSS and SQL injection point:  
Google: `site:<host> inurl:"&"`  
Lynx: `lynx -dump http://www.google.com/search?q=site:<host>+inurl:"&"&num=5000`  
wget: `wget --random-wait -r -l4 --spider -D <host> http://<host> -o ouput.txt`  


## Application
