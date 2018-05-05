---
title: DVWA - XSS reflected
tags:
- dvwa
- xss
- training
see_also:
  -
    link: OWASP Top 10
    url: https://www.owasp.org/index.php/Top_10_2013-Top_10
  -
    link: OWASP XSS description
    url: https://www.owasp.org/index.php/Top_10_2013-A3-Cross-Site_Scripting_(XSS)
  -
    link: Filter Evasion Cheat Sheet
    url: https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet
  -
    link: A (very) comprehensive tutorial on cross-site scripting
    url: http://excess-xss.com/
  -
    link: Damn Vulnerable Web Application
    url: /damn-vulnerable-web-application/
---
According to OWASP Top 10, Cross-Site Scripting aka XSS takes the 3rd place in the more common and important web vulnerabilities list. 
Her primary goal is to spoof the session of another user by stealing his cookie id, usually a privileged user like an admin. 
You can train XSS in Damn Vulnerable Web Application, here are some tests you can perform.

## Low

```php
<?php
if( !array_key_exists ("name", $_GET) || $_GET['name'] == NULL || $_GET['name'] == '' ) {  
  $isempty = true;  
} else {  
  $html .= '<pre>';  
  $html .= 'Hello ' . $_GET['name'];  
  $html .= '</pre>';  
}
?>
```

This code output the `name` parameter without any filter so it's very vulnerable to XSS!
<!--more-->
If you provide a single name it works perfectly but if you insert any HTML code it will be interpreted: 

[![DVWA XSS reflected low](/images/dvwa_xss_reflected_low0.png)](/images/dvwa_xss_reflected_low0.png) 

That means you can also use JavaScript: 

[![DVWA XSS reflected low](/images/dvwa_xss_reflected_low1.png)](/images/dvwa_xss_reflected_low1.png) 

## Medium 

In the second level, the parameter is sanitized by removing the HTML tag `<script>` :

```php
<?php
if(!array_key_exists ("name", $_GET) || $_GET['name'] == NULL || $_GET['name'] == ''){
  $isempty = true;
} else {
  $html .= ' <pre>';
  $html .= 'Hello ' . str_replace('<script>', '', $_GET['name']);
  $html .= '</pre>';
}
```

Is that enough? Of course not because the text you provide can have many different forms. 
The test is not even case insensitive. 
So you can simply use the same payload as previously and just add an uppercase letter or some few useless characters like white spaces:

[![DVWA XSS reflected medium](/images/dvwa_xss_reflected_low2.png)](/images/dvwa_xss_reflected_low2.png)

## High

The final level is a good example of how to protect your site. 
Before echoing the `name` the script escapes it with `htmlspecialchars()`. 
According to [PHP htmlspecialchars](http://php.net/manual/fr/function.htmlspecialchars.php "PHP documentation"){:class="flashlink" target="_blank"}, 
this function converts all special characters to HTML entities. 
`< ` will be converted to `&lt;`, `>` to `&gt;` and so on... 
So the HTML or JavaScript code won't run.

When a XSS is found, no matter the code you submit, it will be evaluated. 
You can then perform a redirection:

```javascript
<script>document.location='http://10degres.net'</script>
```

Deface the site:

```javascript
<script>document.write('H@ck3d by true l33t r0x0r')</script>
```

Or display cookies...

```javascript
<script>alert(document.cookie)</script>
```
