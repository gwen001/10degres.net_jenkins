---
title: Actarus code release
tags:
- actarus
- information gathering
- bug bounty
- tools
- recon
---
Actarus is a custom tool that can perform automatic recon and store all datas in a database. 
Afterwards you could consult/search keywords in it to find vulnerabilities or at least entry points.

After some months of inactivity, I finally decided to publicly release the source code of Actarus.
I started this project to learn Symfony, now I hate it. Maybe someone will give him another chance to grow up.

<!--more-->

Below a quick list of Actarus features:

- project managment
- server managment
- domain managment
- host (subdomain) managment
- task managment, priority, autokill
- automatic recon
- result interpretation and callback
- alert managment and automatic generation
- technology managment and gathering
- multi processing
- clustering
- HackerOne interaction

<br>
  
Here is a video that shows the web gui:
[![video actarus](http://10degres.net/images/actarus_video_preview.jpg)](https://www.youtube.com/watch?v=_u1-L0YjI7g){:target="_blank"}
  
<br>
  
Here is the git repository.
https://github.com/gwen001/actarus
[https://github.com/gwen001/actarus](https://github.com/gwen001/actarus){:class="flashlink" target="_blank"}
  
<br>

I also created a Virtual machine with VirtualBox for people who want to test the project but not install/configure it (yes, it's pain in the ass).  
[Download the VM](http://10degres.net/assets/actarus.ova){:class="flashlink" target="_blank"}

<br>

I really think that this tool could help bounty hunters in their daily task.
I had the opportunity to test it on 3 dedicated servers at the same time and the result was awesome.
30 task paralellized for 1 week, nights and days, the amount of data collected was huge but because of the interpreter and the search engines, it's pretty easy to extract Wordpress install, svn repositories, directory listing or whatever you are looking for.
