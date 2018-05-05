---
title: File transfer with ping
tags:
- ping
- icmp
- file transfer
see_also:
  -
    link: Data Transfer over ICMP
    url: http://www.unixist.com/security/data-transfer-over-icmp/index.html
---
## Introduction

Anyone who ever deals with server managment knows the famous `ping` utility. 
Ping send **ICMP request** to a remote host, it's commonly used to test if a server is alive or to know his ip address. 
However ping options allow us to customize this requests in some way, then it becomes possible to transfer any type of data. 
For the purpose I test my script with different media types like png or mp3 and it worked perfectly.

## The idea

By default ping requests are formed with 98 bytes including 56 bytes of data and various headers. 
With the `-p` option, ping allows you to customize 16 of those 56 bytes:

[![ping test](/images/ping_test.png)](/images/ping_test.png)

Here is the request catched with `tcpdump` on the remote host:

[![ping capture](/images/ping-capture.png)](/images/ping-capture.png)

As you can see the submitted string repeats again and again until the end of the data request.
If you provide a string longer than 16 bytes it will be truncated. From here, we can **convert any data to hexa and send it through ping request**.
<!--more-->

## The POC

For my tests I used the following Anonymous image:

[![anonymous](/images/anonymous.jpg)](/images/anonymous.jpg)

This image is about 7Ko so the script sent near 1200 ping requests, which is alot... It's also time consuming but to be honest it's so fun :) Below the poc:

[![icmp transfer poc](/images/icmp-transfer-poc.png)](/images/icmp-transfer-poc.png)

[![icmp transfer capture](/images/icmp-transfer-capture.png)](/images/icmp-transfer-capture.png)

Note: the script also works if echo request has been disable on the remote host (with `icmp_echo_ignore_all` equal to `1`), 
but slower. Feel free to mail me if you want to take a look at the scripts.
