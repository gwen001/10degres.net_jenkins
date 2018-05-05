---
title: Damn Vulnerable Web Application
tags:
- dvwa
- captcha
- csrf
- xss
- brute force
- sql injection
- command execution
- training
- file upload
- path traversal
---
**DVWA** is a PHP/MySQL web application that is intentionally vulnerable. 
The goal is to learn common web vulnerabilities and improve your security skills by training yourself on your own server. 
3 levels are available (low, medium and high) to perform those following attacks :

- Bruce Force
- Command Execution
- CSRF
- Captcha
- File Inclusion
- SQL Injection (plus Blind)
- File Upload
- XSS

The lowest level is usually pretty easy to bypass but the high level as a best practice presents the right way to protect your application.

<!--more-->

The installation is pretty easy, you simply need to extract the zip archive found on [DVWA official website](http://www.dvwa.co.uk/ "Damn Vulnerable Web Application"){:class="flashlink" target="_blank"} 
in the root directory of your web server. 
You will then have to configure a dedicated database because DVWA comes with two small tables.

A [full tutoriel](http://www.funinformatique.com/dvwa-testez-vos-competences-en-hacking/ "DVWA turoriel"){:class="flashlink" target="_blank"} (fr) is available and you could find a lot 
of videos on [Youtube](https://www.youtube.com/results?search_query=dvwa "DVWA on Youtube"){:class="flashlink" target="_blank"} about how to exploit the vulnerabilities.

[![DVWA](/images/dvwa.png)](/images/dvwa.png)
