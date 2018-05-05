---
title: Kick the bucket
tags:
- aws
- s3
- bucket
see_also:
  -
    link: Documentation of S3 commands
    url: http://docs.aws.amazon.com/cli/latest/reference/s3/
  -
    link: Documentation of s3api
    url: http://docs.aws.amazon.com/cli/latest/reference/s3api/index.html
---
I already wrote [a post about Amazon S3 buckets](/playing-with-s3-buckets/) but they became so popular these last weeks  that many people explain what is a bucket, what is the danger and how to exploit misconfiguration. My goal here is more: how/where to find those vulnerable buckets.

First I assume you already know the basics, if not, you can read the excellent article from [Frans Rosen on Detectify](https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/){:target="_blank" class="flashlink"}.
<!--more-->


## How a bucket name looks like?
After analyzing alot of buckets, here are the most common pattern I found:

- &lt;main_name&gt;&lt;separator&gt;&lt;word&gt;
- &lt;word&gt;&lt;separator&gt;&lt;main_name&gt;
- &lt;subdomain&gt;

Where `main_name` usually is the name of the company or the domain and `separator` usually is a dot `.` or a dash `-`, example:

- 10degres-static
- prod.10degres
- assets.10degres.net

It's also very common to find multiple "level" of separation, example:

- 10degres-backups-2016
- dev.www.10degres.net

Pretty rare, but sometimes separators are mixed:

- img-dev.10degres.net
- static.10-degres.net


## How to access a bucket?

There is 4 ways to access a bucket:
- subdomain of s3.amazonaws.com, ex: `https://xxxxxxxxxx.s3.amazonaws.com`
- subdirectory of s3.amazonaws.com, ex: `https://s3.amazonaws.com/xxxxxxxxxx`
- subdomain of Cloudfront, ex: `https://xxxxxxxxxx.cloudfront.net`
- `awscli` the command line environment tool for AWS, ex: `aws s3 ls s3://xxxxxxxxxx`

From here you probably already guessed the next chapter...


## How to find buckets?

To Find buckets, you can use the tools you already use when you perform recon on a new program/domain/host:

- sudbomain discovery: Sublist3r, dnsrecon, altdns, [threadcrowd.org](https://www.threatcrowd.org/searchApi/v2/domain/report/?domain=cloudfront.net){:target="_blank"}...
![Sudbomain discovery with theharvester](/images/s3bucket-theharvester.png)

- subdirectory discovery: dirb, wfuzz, gobuster...
![Subdirectory discovery witf WFuzz](/images/s3bucket-wfuzz.png)

- both: Google, Burp Suite and many others...
![Subdomain discovery with Burp Suite](/images/s3bucket-burp.png)


## Dedicated tools

Some dedicated tools have been released to discover buckets:

- [Bucket Finder](https://digi.ninja/projects/bucket_finder.php){:target="_blank"} by DigiNinja
- [laszyS3](https://github.com/nahamsec/lazys3){:target="_blank"} by Jobert Abma and Ben Sadeghipour
- [bucketlist](https://github.com/michenriksen/bucketlist){:target="_blank"} by Michen Riksen
- [s3-bucket-finder](https://github.com/gwen001/s3-buckets-finder){:target="_blank"} by me  
I also wrote [a small script](/assets/cloudfront.txt){:target="_blank"} that try to determine what is behind a Cloudfront subdomain.


## Additionnal notes

Even if a bucket is read only, you can report it if the datas available are hot, take a look at Hackerone where the researcher wrote a [report to Mapbox](https://hackerone.com/reports/202725){:target="_blank" class="flashlink"} and got a nice reward.
<br><br>
Even if a bucket is empty, you can also report it, the danger still here, a hacker could use the place to store hacked datas (movies, software...) or serve malicious files.
<br><br>
A bucket can be readable from the command line tool `awscli` but not with your browser, try both way.
<br><br>
All files contained in a bucket can have different permissions, test them all.
<br><br>
A bucket can be configured to serve only one region, you will get a very specific message in that case, so test all region to find the good one ([see the region list](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region){:target="_blank" class="flashlink"}).
<br><br>
Some buckets are not reachable via `http`, you should prefer `https`.
<br><br>
At this time, there is no real way to know the owner of a bucket. The only thing you can do, if you have access to the ACL (via the command line method `get-bucket-acl`), is to compare the owner of two different buckets. So take care when/who you send a report.
