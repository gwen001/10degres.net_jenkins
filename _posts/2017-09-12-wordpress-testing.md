---
title: Wordpress testing
tags:
- wordpress
see_also:
  -
    link: OneLogin authentication bypass on WordPress sites
    url: https://hackerone.com/reports/136169
  -
    link: Image Gallery SQL Injection
    url: http://local.10degres.net:4000/image-gallery-sql-injection/
---
Here is the way I usually follow to test a Wordpress install.

## Information gathering

Get basic informations with [WPScan](https://wpscan.org/){:class="flashlink" target="_blank"}:
```
wpscan -r --enumerate u --url http://www.example.com
```

If it can't retrieve the user list, try to use provided script `stop_user_enumeration_bypass.rb`
For each user found, try to brute with basic passwords:
```
wpscan -r --url http://www.example.com --wordlist /Wordlists/Passwords/best1050.txt --username <username>
```

If the version of Wordpress has been found, download it from the official archive directory:
[https://wordpress.org/download/release-archive/](https://wordpress.org/download/release-archive/){:target="_blank"}

Open the tested website and take a look at the source to find those strings:
`wp-content/themes` and `wp-content/plugins`
This could reveal more themes/plugins.

<!--more-->

## Replication

For each themes found, download them from the official archive directory:
[https://wordpress.org/themes/](https://wordpress.org/themes/){:target="_blank"}  
Try to download the exact same version from the SVN repository (ex: [https://themes.svn.wordpress.org/matala/](https://themes.svn.wordpress.org/matala/){:target="_blank"})  
If you can't find it, try to download the version before and after.
If the theme can't be found or if it's not free, try to find it from Google or on some illegal website (take care of backdoors).

For each plugins found, download them from the official archive directory:
[https://wordpress.org/plugins/](https://wordpress.org/plugins/){:target="_blank"}  
Try to download the exact same version from the Trac browser (ex: [https://themes.svn.wordpress.org/matala/](https://plugins.trac.wordpress.org/browser/anual-archive/){:target="_blank"})  
If you can't find it, try to download the version before and after.
If the plugin can't be found or if it's not free, try to find it from Google or on some illegal website (take care of backdoors).

Once you have everything, open you local dashboard of your local install, and enable all themes and plugins you have downloaded.

## Find an exploit, the easy way

For each themes and plugins found, check on [Exploit Database](https://www.exploit-db.com/){:class="flashlink" target="_blank"} if there is an existing exploit:  
[![wordpress exploitdb](/images/wordpress-exploitdb.png)](/images/wordpress-exploitdb.png)  
If yes, try them all.

## Low hanging fruits

Open a terminal and find all files you've got with the themes and the plugins:
```
find <your_install_path>/wp-content/themes/ -type f
find <your_install_path>/wp-content/plugins/ -type f
```
Save the result in a file and inject in Burp Intruder on your target. Check the response code, this will give you a good idea if you have downloaded the right things or not (you should get a lot of 200).

Check twice the result of PHP files, this could reveal a Full Path Disclosure:
[![wordpress full path disclosure](/images/wordpress-fpd.png)](/images/wordpress-fpd.png)  

If you don't get anything, retry with all PHP files, even the Wordpress core.
Most of the time WPScan will detect it but it's worth the hit to try!
```
find <your_install_path> -type f -name "*.php"
```

## Code analysis

Time to dig deeper! For each themes/plugins, use regexp to locate dangerous functions.
Test everything on your local install before testing your target.

SQL statements:
```
egrep -ri 'select|insert|update|delete' . | grep -i ".php" | egrep -i "select.*FROM|insert.*INTO|update.*SET|delete.*FROM"
egrep -ri '\->prepare' . | grep -i ".php" | egrep -i '\->prepare'
egrep -ri '\$_SERVER|\$_REQUEST|\$_GET|\$_POST|\$_COOKIE' . | grep -i ".php" | egrep -i '\$_SERVER\[|\$_REQUEST\[|\$_GET\[|\$_POST\[|\$_COOKIE\['
```

File system interaction:
```
egrep -ri 'fopen|file_get_contents|file_put_contents|fread|fwrite' . | grep -i ".php" | egrep -i "fopen\s*\(|file_get_contents\s*\(|file_put_contents\s*\(|fread\s*\(|fwrite\s*\("
```

Command execution:
```
egrep -ri 'eval|system|exec|passthru' . | grep -i ".php" | egrep -i "eval\s*\(|system\s*\(|exec\s*\(|passthru\s*\("
egrep -ri 'assert|create_function|preg_replace' . | grep -i ".php" | egrep -i "assert\s*\(|create_function\s*\(|preg_replace\s*\("
```

File inclusion:
```
egrep -ri 'include|require' . | grep -i ".php" | egrep -i "include\s*\(|include_once\s*\(|require\s*\(|require_once\s*\("
```

Custom headers:
```
egrep -ri 'header|setcookie|content-disposition' . | grep -i ".php" | egrep -i "header\s*\(|setcookie\s*\(|content-disposition\s*\("
```

## Boring stuff

Finally, you can use the same technics you use on other website. Try to find endpoints from Google, Wayback Machine, etc... and test SQLi, XSS and so on...
Far from exhaustive, you also need to check authentication deeper, specially if oauth is part of the party.
