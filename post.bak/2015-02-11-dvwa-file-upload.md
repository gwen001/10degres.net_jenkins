---
title: DVWA - File upload
tags:
- dvwa
- file upload
see_also:
  -
    link: Introduction to DVWA
    url: http://blog.10degres.net/damn-vulnerable-web-application/
  -
    link: My favourites browser extensions
    url: /browser-extensions/
  -
    link: OWASP Unrestricted File Upload
    url: https://www.owasp.org/index.php/Unrestricted_File_Upload
  -
    link: File upload on InfoSec
    url: http://resources.infosecinstitute.com/file-upload-vulnerabilities/
---
A very useful aspect of PHP is the ability to manage file uploads. 
Allowing users to send a file opens a whole can of worms, so be careful when allowing this fonctionnality. 
If wrong protected it could result of a full control of the server. 
With DVWA you can learn effective defense.

## Low

```php
if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {
    $html .= '<pre>';
    $html .= 'Your image was not uploaded.';
    $html .= '</pre>';
} else {
    $html .= '<pre>';
    $html .= $target_path . ' succesfully uploaded!';
    $html .= '</pre>';
}
```

The first level is the easiest because it has absolutly no protection against malicious file upload. 
Choose a file - in my case a PHP shell - and submit the form:

[![DVWA file upload](/images/dvwa-file-upload_1.png)](/images/dvwa-file-upload_1.png)

<!--more-->

The script has been successfully uploaded and the path displayed, now you can easily call it:

[![DVWA file upload](/images/dvwa-file-upload-2.png)](/images/dvwa-file-upload-2.png)

## Medium

```php
if (($uploaded_type == "image/jpeg") && ($uploaded_size < 100000)){
  if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {
    $html .= '<pre>';
    $html .= 'Your image was not uploaded.';
    $html .= '</pre>';
  } else {
    $html .= '<pre>';
    $html .= $target_path . ' succesfully uploaded!';
    $html .= '</pre>';
  }
}
else {
  echo '<pre>Your image was not uploaded.</pre>';
}
```

In the next level, the script checks the size of the uploaded file, which is useless because a PHP backdoor can be under 1ko, and the mime type by allowing only jpeg image, which is a good idea. 
If you try to upload anything else you will be faced to the following error:

[![DVWA file upload](/images/dvwa-file-upload-3.png)](/images/dvwa-file-upload-3.png)

To bypass this test, you need to change the mime type of the file you choosed. 
There is plenty of browser extensions available to perform that kind of tricks, I personally like [LiveHTTPHeaders](http://livehttpheaders.mozdev.org/ "LiveHTTPHeaders"){:class="flashlink" target="_blank"} for Firefox which is very easy to use. 
After submitting your file, you can view, modify and replay the request. 
Locate the `Content-type` header and replace it by: `image/jpeg`

[![DVWA file upload](/images/dvwa_file_upload_4.png)](/images/dvwa_file_upload_4.png)

Submit the new request, et voilà! It works like a fucking charm!

## High

```php
if (($uploaded_ext == "jpg" || $uploaded_ext == "JPG" || $uploaded_ext == "jpeg" || $uploaded_ext == "JPEG") && ($uploaded_size < 100000)){
  if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {					
    $html .= '<pre>';
    $html .= 'Your image was not uploaded 1.';
    $html .= '</pre>';
  } else {
    $html .= '<pre>';
    $html .= $target_path . ' succesfully uploaded!';
    $html .= '</pre>';
  }
}
```

Here the check of the mime type has been replaced by an extension check. 
That does not mean you can only upload jpeg image but only a file with an allowed jpeg extension. 
Of course if you upload a PHP script renamed with a jpeg extension your browser will throw an error when trying to display the image:

[![DVWA file upload](/images/dvwa-file-upload-5.png)](/images/dvwa-file-upload-5.png)

It would be also possible to bypass this if you could upload an `.htaccess` but there is no way in this level, it's well secured.
