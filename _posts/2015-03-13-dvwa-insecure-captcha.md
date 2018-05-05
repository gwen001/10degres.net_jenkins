---
title: DVWA - Insecure CAPTCHA
tags:
- dvwa
- captcha
---
Captchas are usually used to prevent robots to make an action instead of humans. 
It should add an extra layer of security but badly configured it could lead to unauthorized access...

When you try to submit the form without providing a captcha code, you get the following error:

[![dvwa captcha error](/images/dvwa-captcha-error.png)](/images/dvwa-captcha-error.png)

## Low

Try to submit an empty password and take a look to the HTTP request and her parameters, you can notice the strange variable `step`:

[![dvwa captcha low](/images/dvwa-captcha-low.png)](/images/dvwa-captcha-low.png)

This variable is the step in the change password functionnality. 
So if you simply change it to `2` and replay the request with this new value, it works perfectly.

<!--more-->

## Medium

In this level another step has been added. 
After submitting your new password and the good captcha you'll have to confirm your wish by submitting another form:

[![dvwa captcha medium](/images/dvwa-captcha-medium.png)](/images/dvwa-captcha-medium.png)

Again, if you check the parameters of this second request, you can notice a new field called `passed_captcha` set to `true`. 
Now if you merge the both requests and apply the same method viewed in the first level, you are able to change your password within only one request:

[![dvwa captcha medium](/images/dvwa-aptcha-medium.png)](/images/dvwa-captcha-medium.png)

## High

As usual the highest level is well configured and cannot be bypassed. 
The check is done within only one step by calling the method `captcha_check_answer()`. 
Plus note how the SQL query is protected from injection with `mysql_real_escape_string()`.
