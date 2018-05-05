---
title: Null Byte Injection
tags:
- php
- null byte
see_also:
  -
    link: OWASP description
    url: https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_Null_Byte_Injection
  -
    link: Another article about Null byte injection
    url: http://blog.benjaminwalters.net/?p=22139
---
I recently read a nice article on [InfoSec Institute](http://resources.infosecinstitute.com/null-byte-injection-php/ "Null byte injection"){:class="flashlink" target="_blank"} 
(again) about Null byte injection. 
However I met some problems to make it works in a real situation so I decided to write my own.

First of all, this vulnerability has been **fully patched in PHP 5.3.4** (until someone else find another buggy function...), that means you need to install an old version of PHP. 
Grab it in [PHP releases archive](http://php.net/releases/ "PHP releases"){:class="flashlink" target="_blank"}. 
After compiled and configured, it's very important to set **magic_quotes_gpc=Off** in the `php.ini` (it won't work with `ini_set` in the script itself).

Then imagine an `index.php` like this:

```php
<?php
include( '/var/www/pages/'.$_GET['p'].'.php' );
?>
```

And a `/pages/store.php` like this:

```php
<?php
echo 'This is the store !';
?>
```

If you call it directly it will works but in that case `index.php` is usually used to display common stuffs like header and footer. 
<!--more-->
So the basic use is:

[![Null Byte Injection](/images/null_byte_injection_1.png)](/images/null_byte_injection_1.png)

Since `index.php` already concatenate `.php` with the `p` parameter, you'll get that kind of error if you set it manually:

[![Null Byte Injection](/images/null_byte_injection_2.png)](/images/null_byte_injection_2.png)

Notice the double extension `.php.php`, that means the site might be vulnerable. 
An easy tool to determine the PHP version of your target is a browser extension like [Wappalyzer](https://wappalyzer.com/ "Wappalyzer"){:target="_blank"} 
for [Firefox/Iceweasel](https://addons.mozilla.org/fr/firefox/addon/wappalyzer/ "Wappalyzer for Firefox/Iceweasel"){:target="_blank"} 
or [Chrome](https://chrome.google.com/webstore/detail/wappalyzer/gppongmhjkpfnbhagpmjfkannfbllamg "Wappalyzer for Chrome"){:target="_blank"}.

The next step is to add the null byte:

[![Null Byte Injection](/images/null_byte_injection_3.png)](/images/null_byte_injection_3.png)

Because we got the same result as the "normal" usage, now that's sure, the target is vulnerable! You can inject everything. 
Of course the goal of this attack is to retrieve sensitive files:

[![Null Byte Injection](/images/null_byte_injection_5.png)](/images/null_byte_injection_5.png)

Keep in mind that the path of the required file must be relative of the current script (`index.php` in my case) otherwise it will throw an error as a file not found:

[![Null Byte Injection](/images/null_byte_injection_4.png)](/images/null_byte_injection_4.png)

Here is the error you'll get if you fight against a patched PHP:

[![Null Byte Injection](/images/null_byte_injection_7.png)](/images/null_byte_injection_7.png)

And the error you'll get if `magic_quotes_gpc` is enable:

[![Null Byte Injection](/images/null_byte_injection_6.png)](/images/null_byte_injection_6.png)
