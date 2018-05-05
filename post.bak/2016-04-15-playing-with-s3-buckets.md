---
title: Playing with S3 buckets
tags:
- aws
- s3
- bucket
see_also:
  -
    link: Managing your bucket access with ACLs
    url: https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html
  -
    link: Demo video from Pete Yaworks
    url: https://www.youtube.com/watch?v=_x5VKuFjvrk
  -
    link: The initial report on Hackerone
    url: https://hackerone.com/reports/128088
---
Amazon Simple Storage Service aka S3 is a cloud storage for the Internet. 
You first create a bucket and you can then upload any number of objects (photos, videos, documents etc.) to it.
However if the permissions (ACL) are not well settled, bad things can happen.

Recently disclosed by Hackerone, a misconfiguration in their Amazon Web Services S3 buckets allowed any authenticated user to write in there.
From here an attacker could upload a malicious file waiting for someone open it, or overwrite existing files. 

When you crawl a website, you can you can check the presence of S3 by intercepting calls to `amazonaws.com`.
The bucket call can have different look:
`https://<aws_region>.amazonaws.com/<bucket_name>/<file_path>`  
or:  
`https://<bucket_name>.amazonaws.com/<file_path>`

Once you get the bucket name, you can execute many tests using **awscli** to check his permissions.
If you try to access to a bucket who doesn't exist, you'll get this message:

```bash
$ aws s3 ls s3://gwen001-azertyuiop  
A client error (NoSuchBucket) occurred when calling the ListObjects operation: The specified bucket does not exist
```

If you try to execute a command you are not allowed to, you'll then get something like this:

```bash
$ aws s3 ls s3://gwen001-test000/
A client error (AccessDenied) occurred when calling the ListObjects operation: Access Denied
```
<!--more-->
For the purpose of this article I created some buckets with differents permissions:

|Bucket |Grantee|Read |Write|get ACL|set ACL| 
|-------|-------|-----|-----|-------|-------|
|test000|-|-|-|-|-|
|test001|me|yes|yes|yes|yes|
|test002|Everyone|yes|-|-|-|
|test003|Everyone|yes|yes|-|-|
|test004|Everyone|yes|yes|yes|-|
|test005|Everyone|yes|yes|yes|yes|

Using my brain and bit of Bash, I wrote a script based on a wordlist to discover and test my buckets.
The wordlist: 
`test000 test001 testme test002 test003 test004 test005 testx`

The script:  
https://github.com/gwen001/pentest-tools/blob/master/s3-buckets-force.sh

The output:
[![s3 bucket test](/images/s3-bucket-test.png)](/images/s3-bucket-test.png)

Finally 6 buckets has been found, which is correct, and the permissions has also been tested.

When `Edit Permissions` is allowed for `Everyone`, this is the deadly zone but don't underestimate the power of read only.
From a pentester point of view this is a very good start to gather informations about your business and for a hacker it's a great source to leak your datas. 


