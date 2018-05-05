---
title: Document Metadata
tags:
- information gathering
- metadata
---
Metadata are informations stored in a document itself but not easy to find for common mortals. 
Those infos usually are: file name/type/size, author, organization, created date, last modified date and so on... 
But sometimes there are extra infos that could be very interesting from a hacker point of view like email, phone number, username, geoloc and even local ip address.

<!--more-->

Let's take a look at a photo of the beautiful Scarlett Johansson. 
First I will use the nice online tool [Jeffrey's Exif Viewer](http://regex.info/exif.cgi){:class="flashlink" target="_blank"} wich will output dozen infos:

[![exif viewer 1](/images/exif-viewer-1.png)](/images/exif-viewer-1.png) 
[![exif viewer 2](/images/exif-viewer-2.png)](/images/exif-viewer-2.png) 
[![exif viewer 4](/images/exif-viewer-4.png)](/images/exif-viewer-4.png) 
[![exif viewer 3](/images/exif-viewer-3.png)](/images/exif-viewer-3.png)

From here what usefull informations did I get ?

- the filename
- the title of the image (headline)
- the name of the author in different ways
- the date and the time it was taken in different ways also
- the resolution
- the image size in pixel
- the file size
- the credit/copyright
- the company name
- a caption describing the event
- many parameters about colors
- alot of datas that are not related to security

Ok nothing special here and nothing very serious, the only thing we can be sure is that the author has been very cautious with all details of the image, looks like copyright is important for him :)

Let's try another file that I intentionnally set up with "secret" metadata. 
This time I will use the command line [ExifTool](http://www.sno.phy.queensu.ca/~phil/exiftool/){:class="flashlink" target="_blank"} wich can be used to read, set and edit metadata. 
This tool support about 150 different file format and is by default installed in Kali Linux.

[![exiftool](/images/exiftool.png)](/images/exiftool.png)

There I got the following infos:

- the file name and it's size
- the document title
- the creation date and the last modified date
- the name of the last writer
- and the most important, the originial author wich looks like more than a username than a full name

By using a Google Dorks like `site:www.leparticulier.fr filetype:pdf` and after a bit of scripting combined with ExifTool, I was able to find about 15 usernames. 
With only this information in my hands I won't be able to penetrate their system but it's a really good start while performing the information gathering phase of a pentest. 
Plus you will probably be confronted to vulnerabilities who require a valid user to be exploited...

An awesome tool to automate metadata mining is FOCA. 
This program can search office documents of a specified site and extract all metadata to finally map the local network of the company.

&gt; [Foca demo](https://www.youtube.com/watch?v=XVjZEijbekw){:class="flashlink" target="_blank"} at DEFCON 18

Anyway you should be able to remove the metadata of your document with the application you used to create it or with online tools, follow the link below.

&gt; [How to remove EXIF Metadata](http://www.makeuseof.com/tag/3-ways-to-remove-exif-metadata-from-photos-and-why-you-might-want-to/){:class="flashlink" target="_blank"}
