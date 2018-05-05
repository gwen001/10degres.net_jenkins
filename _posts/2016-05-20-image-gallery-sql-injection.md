---
title: Image Gallery SQL Injection
tags:
- wordpress
- sql injection
see_also:
  -
    link: Huge-IT website
    url: http://huge-it.com/wordpress-gallery/
---
While I was working on a famous bug bounty program, WPScan returns me the list of the plugins configured on the Wordpress install.
Here is what I found in one of them: [Image Gallery by Huge-IT](https://wordpress.org/plugins/gallery-images/){:class="flashlink" target="_blank"}.

WPScan output, no issues known:

~~~
[+] Name: gallery-images - v1.8.6  
 |  Location: https://[REDACTED]/wp-content/plugins/gallery-images/  
 |  Readme: https://[REDACTED]/wp-content/plugins/gallery-images/readme.txt  
~~~

After a fast search on [exploit-db.com](https://www.exploit-db.com/){:target="_blank"} with no success, I finally decided to download it and read the code to find vulnerabilites by myself.
Since the readme was available, I was able to confirm the version of the plugin.

I was looking for two kind of vulnerabilities: file upload and sql injection.
First thing I did was to locate PHP files, and grepping the result to find `Content-Disposition` header:

~~~bash
$ find . -name "*.php*" | xargs grep -i header
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:		/*HEIGHT FROM HEADER.PHP*/
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./Front_end/gallery_front_end_view.php:							header("Location:".$actual_link."");
./admin/gallery_func.php:			header('Location: admin.php?page=gallerys_huge_it_gallery&id='.$rowsldccs->id.'&task=apply');
./admin/gallery_view.php:	header('Location: admin.php?page=gallerys_huge_it_gallery&id='.$row->id.'&task=apply');
./admin/gallery_view.php:	<div id="gallery-header">
~~~
<!--more-->

Nothing interesting here...

Then, I noticed that the "non admin" requests where mainly coded in two files: `gallery-images.php` and `Front_end/gallery_front_end_view.php`.
Luckily I started to read the first one which contains the hole.

In the function `huge_it_image_gallery_ajax_callback`, the variable `$huge_it_ip` is used multiple times in the following form:

~~~php
$wpdb->prepare("SELECT `image_status`,`ip` FROM ".$wpdb->prefix."huge_itgallery_like_dislike WHERE image_id = %d AND `ip` = '".$huge_it_ip."'",(int)$row->id);
~~~

This variable is defined at the top of the file:

~~~php
if(!empty($_SERVER['HTTP_CLIENT_IP'])){
	$huge_it_ip=$_SERVER['HTTP_CLIENT_IP'];
}
elseif(!empty($_SERVER['HTTP_X_FORWARDED_FOR'])){
	$huge_it_ip=$_SERVER['HTTP_X_FORWARDED_FOR'];
}
else{
	$huge_it_ip=$_SERVER['REMOTE_ADDR'];
}
~~~

Unfortunately HTTP headers cannot be trusted, even ip address.
So I started to test the injection in `X-Forwarded-For` header and it worked perfectly on my local install.
However the site I was testing seems to had a firewall who drop this header. 
I then tested the injection in `Client-Ip` header and it passed !

Note that the plugin must be enabled to be able to exploit the injection.
Finally here is the full request in Burp Suite:

~~~
POST /wp-admin/admin-ajax.php HTTP/1.1
Host: local.wordpress.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:38.0) Gecko/20100101 Firefox/38.0 Iceweasel/38.8.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Referer: http://local.wordpress.com/wp-admin/admin.php?page=gallerys_huge_it_gallery&task=edit_cat&id=1
Content-Length: 89
Client-Ip: 123.123.123.123
Cookie: wordpress_3e0ed6e299d95d3a38b8516e25f1b1e2=admin%7C1463913228%7CYRWrxl5s69SoSkkXyMBFnXzSt2dSINk63ojC6F0mcWJ%7Cb28be90ae8a75452f047cb21fcc84f42a9a801123af24c1f77bb3f19c880147b; wordpress_test_cookie=WP+Cookie+check; wordpress_logged_in_3e0ed6e299d95d3a38b8516e25f1b1e2=admin%7C1463913228%7CYRWrxl5s69SoSkkXyMBFnXzSt2dSINk63ojC6F0mcWJ%7C243730f21ebdc724e25dd292d6c8d1773510dd572a0a39abc88b379852181f1f; wp-settings-1=libraryContent%3Dbrowse%26editor%3Dtinymce; wp-settings-time-1=1463740430; PHPSESSID=e5a63r274m6mvcfbmblgacrvq0
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache

action=huge_it_video_gallery_ajax&task=load_images_content&galleryid=1&page=1&perpage=100
~~~

The parameter `task` can be one of the following:  
load_images_content  
load_images_lightbox  
load_image_justified  
load_image_thumbnail  
load_blog_view

And the result:
[![sqlmap injection](/images/image-gallery-sql-injection.png)](/images/image-gallery-sql-injection.png)

A Full Path Disclosure is also available if you call `http://www.example.com/wp-content/plugins/gallery-images/gallery-images.php`

I sent an email to Huge-IT who released a fix the same day. Fast enough :)
