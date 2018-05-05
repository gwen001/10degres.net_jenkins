---
title: The bug bounty program that changed my life
image:
  feature: images/pp-rce-1.png
tags:
- sqli
- xss
- subdomain
- rce
- upload
- repository
see_also:
  -
    link: LevelUP 2017 videos
    url: https://www.youtube.com/watch?v=BEaMhs9LmoY&list=PLIK9nm3mu-S5InvR-myOS7hnae8w4EPFV&index=11
  -
    link: My endpoints extractor
    url: https://github.com/gwen001/pentest-tools/blob/master/extract-endpoints.php
---
This is a real story or not, that occured in mid 2017 or not, about a private program or not, on Hackerone or not, believe me or not, but it changed my life.
I would like to thanks all the people from this company I talked with.
They were very nice with me, very fast to fix the bugs and I always got the rewards in less than 7 days, frequently the day of the report, even for the smallest bugs.
Thanks to them, I wish we could find more program like this one.
<!--more-->

The money was not the only good thing I earned during this period, my reputation on Hackerone also grew up very fast.
From here I received many private invitations, sometimes 3 or 4, up to 5 a week, and we all know that private programs are very important for hunters.
I also learned alot because it was the very first time that I was able to play with such issues, I mean in real life, not on a training platform, so it was really really formative.

Some people were mad on me because I didn't want to share my findings in this program.
Come on guys...
Following each others on Twitter doesn't mean I want/can disclose the details of the issues I found in a private program, specially if you're also working on...  
It would be like cutting the branch of the tree I'm sitting on.
I hope you understand that.  
Kisses :)


## Introduction

Had a hard a time to contact the company, the kool [@Geekboy](https://twitter.com/emgeekboy){:class="flashlink" target="_blank"} helped me to enter in the game, thanks to him.

So I joined with 1 subdomain takeover on Cloudfront and 2 SQL injection on a minor subdomain, not bad at all, promising.


## First hit, well, "official" hit...

A big scan with Dirb on another subdomain revealed a `.git` directory.
Using the program [gitpillage](https://github.com/koto/gitpillage){:class="flashlink" target="_blank"}, I got some interesting files:  
[![Private program git listing](/images/pp-git-listing.png)](/images/pp-git-listing.png)  

That could be enough to write a report but since it was PHP code and since I can read PHP, I jumped in the source (PHP guru will recognize the very old Zend Studio 5.5 here xD).
[![Private program git code](/images/pp-git-code.png)](/images/pp-git-code.png)

I quickly found some calls to `exec()`, that allow a developper to execute system commands.
By tracking the creation of the command string, I had notice that a POST parameter was used without any sanitization.
Simplified, the code looked like this:
```php
<?php
    $image->upload( $_FILES['img']['tmp_name'], $_POST );

    class image {
        public function upload( $file, $args ) {
            $this->create_base_name( $args );
            $full_path = $this->upload_dir.$this->base_name;
            $command = sprintf( 'chmod 777 %s', $full_path );
            exec( $command );
        }
    
        private function create_base_name( $args ) {
            $this->base_name = sprintf( '%s%s%s', $this->user_id, $args['p'], $this->suffix );
        }
    }
?>
```

What would you try then? RCE natürlich! The PoC:
[![Private program rce](/images/pp-rce-1.png)](/images/pp-rce-1.png)

W00t w00t bounty time :)


## Replay

Some weeks later, and some others issues later (CORS/CSRF/source code disclosure/rate limit), another SQL injection popped on the main domain.  
[![Private program sqli](/images/pp-sqli.png)](/images/pp-sqli.png)

Since I said to the company to take care about the vulnerable module where I found the RCE, 1 month later I came back in the same place and tried to reproduce the bug.
Was fixed, the vulnerable parameter wasn't anymore.
But what about all others and what about the other scripts in the repository?

KA-CHING! Got 3 more RCE.

One of them required 3 parameters in his input:
- path
- filename
- contents

This script wrote the content `contents` in the file `filename` created in the directory `path`.
The perfect backdoor. Congrats captain Obvious!  

[![Private program rce](/images/pp-rce-2.png)](/images/pp-rce-2.png)


## Bouncing from a RCE to another RCE

Is that fair or not? Well I don't know but while I was "connected" to the server of the company, and since I knew that they have a functionality to handle images,
I checked the version of the library ImageMagick.
[![Private program imagetragick](/images/pp-imagetragick-1.png)](/images/pp-imagetragick-1.png)

If you're hunter you probably already heard about the famous [ImageTragick](https://imagetragick.com/){:class="flashlink" target="_blank"}, 
version prior to 6.9.3-9 are prone to multiple vulnerabilities including RCE.
Here is the PoC:
[![Private program imagetragick](/images/pp-imagetragick-2.png)](/images/pp-imagetragick-2.png)

The company was nice enough to reward those two RCE the same day.


## My favorite

After I had seen the LevelUP 2017 video from the awesome [@zseano](https://twitter.com/zseano){:class="flashlink" target="_blank"}, 
kudos to him, where he explains how to find bugs by extracting datas from Javascript files, I decided to write my own program to extract endpoints and other juicy infos from source code.
Once done, I downloaded all JS files I found from my target, and ran my script.
Needless to say that hundreds of new endpoints popped. 
One of them catched my eyes:
`/[REDACTED].php?[REDACTED]=[REDACTED]&template=[REDACTED].tpl`

When you're a web developper, you know that the extension `.tpl` is commonly used for HTML code rendered by template engines.
Could it be a filename as a parameter???
I immediately tried the value `/etc/passwd`:

[![Private program local file disclosure](/images/pp-lfd-1.png)](/images/pp-lfd-1.png)

Ok, this is a valid bug. 
At first, I though it was a Local File Inclusion so I tried to upgrade to RCE with some tools like fimap, dotdotpwn or Kadimus and then manually but nope...
Since I didn't know the path of the application on the server, I was not able to include any PHP file.

When I find a serious bug like this, I always send the report the same day.
First because I want to avoid a stupid duplicate, second because the company has to be aware of it and fix ASAP!

The day after, the bug was still here. 
Hmmm, well well well...
Ding Dang!
My brain had a flash, why to not try to include the PHP endpoint itself?!
Something like:
`/the_vulnerable_script.php?[REDACTED]=[REDACTED]&template=the_vulnerable_script.php`
Guess what? I got the whole source code and it was not interpreted, so actually this was a Local File Disclosure.  

[![Private program local file disclosure](/images/pp-lfd-2.png)](/images/pp-lfd-2.png)

Jumping from include to include, I got some config files with database credentials, API keys of 3rd party services and so on...
After digging for a while, I finally got the Graal aka the hard coded `autoload` file. 
Basically it's an array with the classname as the keys and the filepath as the values, here is a basic example:
```php
<?php
$t_autoload = [
	'Auth' => '/var/www/project/lib/User/Auth.php',
	'User' => '/var/www/project/lib/User/User.php',
	'Article' => '/var/www/project/lib/Article/Article.php',
	'Comment' => '/var/www/project/lib/Article/Comment.php'
];
?>
```

In a nutshell (Yaworsk favorite 3 words), I could access to the whole source of the application.
Not a RCE but still a nice catch :)
Some minutes later I got a script running and grabbing the files:

[![Private program local file disclosure](/images/pp-lfd-3.png)](/images/pp-lfd-3.png)

[![Private program local file disclosure](/images/pp-lfd-4.png)](/images/pp-lfd-4.png)

Game over!  

I stayed in touch with the company all along the exploitation of this bug. 
I stopped my script after a few minutes, I though that would not be fair/polite and legal to download everything. 


## The last but not the least

Using my previous findings, I had some more endpoints in my hands. 
From here I found 1 more SQL injection, 4 XSS, and another nice one: account takeover.

In this request, changing my email by the email of any other user will immediately update his password by the one provided. 
No confirmation needed, I didn't even need to be authenticated.  

[![Private program account takeover](/images/pp-ato.png)](/images/pp-ato.png)


## Conclusion

"I got the swag and it's pumping out my ovaries."  
[https://www.youtube.com/watch?v=PAjYwryn2qY](https://www.youtube.com/watch?v=PAjYwryn2qY){:class="flashlink" target="_blank"}

Great thanks to [@Hacker0x01](https://twitter.com/Hacker0x01){:class="flashlink" target="_blank"} and every bug bounty platforms to make this possible.

