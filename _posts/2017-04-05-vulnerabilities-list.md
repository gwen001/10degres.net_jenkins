---
title: Vulnerabilities list
tags:
- resources
see_also:
  -
    link: OWASP
    url: https://www.owasp.org/index.php/Main_Page
  -
    link: Common Weakness Enumeration
    url: https://cwe.mitre.org/data/index.html
  -
    link: PortSwigger list
    url: https://portswigger.net/knowledgebase/Issues/
  -
    link: Bug Bounty forum
    url: https://bugbountyforum.com/resources/
---
Here is a non exhausted list of vulnerabilities that I use as a reminder with links for reference.
It's based on many different [resources](/resources/) available on the Internet.
<!--more-->

[SQL injection aka SQLi](/vuln-sql-injection/)  
[Cross-site scriptting aka XSS](/vuln-cross-site-scripting/)  
[Subdomain takeover](/vuln-subdomain-takeover/)  
[Relative path overwrite / Path-relative style sheet import](/vuln-relative-path-overwrite/)  
[Cross-site request forgery aka CSRF](/vuln-cross-site-request-forgery/)  
[Clickjacking](/vuln-clickjacking/)  
[Cross-origin resource sharing aka CORS](/vuln-cross-origin-resource-sharing/)  


## Cookies
[SSL cookie without secure flag set](https://portswigger.net/knowledgebase/Issues/details/00500200_sslcookiewithoutsecureflagset){:target="_blank"}  
[Cookie scoped to parent domain](https://portswigger.net/knowledgebase/Issues/details/00500300_cookiescopedtoparentdomain){:target="_blank"}  
[Duplicate cookies set](https://portswigger.net/knowledgebase/Issues/details/00400a00_duplicatecookiesset){:target="_blank"}  
[Cookie without HttpOnly flag set](https://portswigger.net/knowledgebase/Issues/details/00500600_cookiewithouthttponlyflagset){:target="_blank"}  
[Cookie manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500b00_cookiemanipulationdombased){:target="_blank"}  
[Cookie manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500b01_cookiemanipulationreflecteddombased){:target="_blank"}  
[Cookie manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500b02_cookiemanipulationstoreddombased){:target="_blank"}  


## Headers manipulation
[HTTP response header injection aka CRLF](https://portswigger.net/knowledgebase/Issues/details/00200200_httpresponseheaderinjection){:target="_blank"}  
[Referer-dependent response](https://portswigger.net/knowledgebase/Issues/details/00400100_refererdependentresponse){:target="_blank"}  
[X-Forwarded-For dependent response](https://portswigger.net/knowledgebase/Issues/details/00400110_xforwardedfordependentresponse){:target="_blank"}  
[User agent-dependent response](https://portswigger.net/knowledgebase/Issues/details/00400120_useragentdependentresponse){:target="_blank"}  


[Ajax request header manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500c00_ajaxrequestheadermanipulationdombased){:target="_blank"}  
[Ajax request header manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500c01_ajaxrequestheadermanipulationreflecteddombased){:target="_blank"}  
[Ajax request header manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500c02_ajaxrequestheadermanipulationstoreddombased){:target="_blank"}  

[Cacheable HTTPS response](https://portswigger.net/knowledgebase/Issues/details/00700100_cacheablehttpsresponse){:target="_blank"}  
[Multiple content types specified](https://portswigger.net/knowledgebase/Issues/details/00800100_multiplecontenttypesspecified){:target="_blank"}  
[Content type incorrectly stated](https://portswigger.net/knowledgebase/Issues/details/00800400_contenttypeincorrectlystated){:target="_blank"}  
[Content type is not specified](https://portswigger.net/knowledgebase/Issues/details/00800500_contenttypeisnotspecified){:target="_blank"}  


## Code injection
[PHP code injection](https://portswigger.net/knowledgebase/Issues/details/00100c00_phpcodeinjection){:target="_blank"}  
[Serialized object in HTTP message](https://portswigger.net/knowledgebase/Issues/details/00400900_serializedobjectinhttpmessage){:target="_blank"}  
[Server-side JavaScript code injection](https://portswigger.net/knowledgebase/Issues/details/00100d00_serversidejavascriptcodeinjection){:target="_blank"}  
[Perl code injection](https://portswigger.net/knowledgebase/Issues/details/00100e00_perlcodeinjection){:target="_blank"}  
[Ruby code injection](https://portswigger.net/knowledgebase/Issues/details/00100f00_rubycodeinjection){:target="_blank"}  
[Python code injection](https://portswigger.net/knowledgebase/Issues/details/00100f10_pythoncodeinjection){:target="_blank"}  
[Expression Language injection](https://portswigger.net/knowledgebase/Issues/details/00100f20_expressionlanguageinjection){:target="_blank"}  
[Unidentified code injection](https://portswigger.net/knowledgebase/Issues/details/00101000_unidentifiedcodeinjection){:target="_blank"}  
[Server-side template injection](https://portswigger.net/knowledgebase/Issues/details/00101080_serversidetemplateinjection){:target="_blank"}  
[SSI injection](https://portswigger.net/knowledgebase/Issues/details/00101100_ssiinjection){:target="_blank"}  
[Client-side template injection](https://portswigger.net/knowledgebase/Issues/details/00200308_clientsidetemplateinjection){:target="_blank"}  
[JavaScript injection (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200320_javascriptinjectiondombased){:target="_blank"}  
[JavaScript injection (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200321_javascriptinjectionreflecteddombased){:target="_blank"}  
[JavaScript injection (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200322_javascriptinjectionstoreddombased){:target="_blank"}  

[Client-side JSON injection (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200370_clientsidejsoninjectiondombased){:target="_blank"}  
[Client-side JSON injection (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200371_clientsidejsoninjectionreflecteddombased){:target="_blank"}  
[Client-side JSON injection (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200372_clientsidejsoninjectionstoreddombased){:target="_blank"}  


## XML manipulation
[XML injection](https://portswigger.net/knowledgebase/Issues/details/00100700_xmlinjection){:target="_blank"}  
[XML external entity injection](https://portswigger.net/knowledgebase/Issues/details/00100400_xmlexternalentityinjection){:target="_blank"}  
[XPath injection](https://portswigger.net/knowledgebase/Issues/details/00100600_xpathinjection){:target="_blank"}  
[Client-side XPath injection (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200360_clientsidexpathinjectiondombased){:target="_blank"}  
[Client-side XPath injection (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200361_clientsidexpathinjectionreflecteddombased){:target="_blank"}  
[Client-side XPath injection (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200362_clientsidexpathinjectionstoreddombased){:target="_blank"}  
[XML entity expansion](https://portswigger.net/knowledgebase/Issues/details/00400700_xmlentityexpansion){:target="_blank"}  


## HTTP method
[HTTP PUT method is enabled](https://portswigger.net/knowledgebase/Issues/details/00100900_httpputmethodisenabled){:target="_blank"}  
[HTTP TRACE method is enabled](https://portswigger.net/knowledgebase/Issues/details/00500a00_httptracemethodisenabled){:target="_blank"}  



## HTML5
[HTML5 web message manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500e00_html5webmessagemanipulationdombased){:target="_blank"}  
[HTML5 web message manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500e01_html5webmessagemanipulationreflecteddombased){:target="_blank"}  
[HTML5 web message manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500e02_html5webmessagemanipulationstoreddombased){:target="_blank"}  
[HTML5 storage manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500f00_html5storagemanipulationdombased){:target="_blank"}  
[HTML5 storage manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500f01_html5storagemanipulationreflecteddombased){:target="_blank"}  
[HTML5 storage manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500f02_html5storagemanipulationstoreddombased){:target="_blank"}  


## Information exposure

[ASP.NET tracing enabled](https://portswigger.net/knowledgebase/Issues/details/00100280_aspnettracingenabled){:target="_blank"}  
[ASP.NET debugging enabled](https://portswigger.net/knowledgebase/Issues/details/00100800_aspnetdebuggingenabled){:target="_blank"}  
[ASP.NET ViewState without MAC enabled](https://portswigger.net/knowledgebase/Issues/details/00400600_aspnetviewstatewithoutmacenabled){:target="_blank"}  
[Email addresses disclosed](https://portswigger.net/knowledgebase/Issues/details/00600200_emailaddressesdisclosed){:target="_blank"}  
[Private IP addresses disclosed](https://portswigger.net/knowledgebase/Issues/details/00600300_privateipaddressesdisclosed){:target="_blank"}  
[Private key disclosed](https://portswigger.net/knowledgebase/Issues/details/00600550_privatekeydisclosed){:target="_blank"}  
[Database connection string disclosed](https://portswigger.net/knowledgebase/Issues/details/00600080_databaseconnectionstringdisclosed){:target="_blank"}  
[Source code disclosure](https://portswigger.net/knowledgebase/Issues/details/006000b0_sourcecodedisclosure){:target="_blank"}  
[Directory listing](https://portswigger.net/knowledgebase/Issues/details/00600100_directorylisting){:target="_blank"}  


## File path manipulation
[File path traversal](https://portswigger.net/knowledgebase/Issues/details/00100300_filepathtraversal){:target="_blank"}  
[File path manipulation](https://portswigger.net/knowledgebase/Issues/details/00100b00_filepathmanipulation){:target="_blank"}  
[Local file path manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200350_localfilepathmanipulationdombased){:target="_blank"}  
[Local file path manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200351_localfilepathmanipulationreflecteddombased){:target="_blank"}  
[Local file path manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200352_localfilepathmanipulationstoreddombased){:target="_blank"}  


## Password related
[Cleartext submission of password](https://portswigger.net/knowledgebase/Issues/details/00300100_cleartextsubmissionofpassword){:target="_blank"}  
[Password returned in later response](https://portswigger.net/knowledgebase/Issues/details/00400200_passwordreturnedinlaterresponse){:target="_blank"}  
[Password submitted using GET method](https://portswigger.net/knowledgebase/Issues/details/00400300_passwordsubmittedusinggetmethod){:target="_blank"}  
[Password returned in URL query string](https://portswigger.net/knowledgebase/Issues/details/00400400_passwordreturnedinurlquerystring){:target="_blank"}  
[Password field with autocomplete enabled](https://portswigger.net/knowledgebase/Issues/details/00500800_passwordfieldwithautocompleteenabled){:target="_blank"}  
[Password value set in cookie](https://portswigger.net/knowledgebase/Issues/details/00500900_passwordvaluesetincookie){:target="_blank"}  


## DDOS
[Denial of service (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500d00_denialofservicedombased){:target="_blank"}  
[Denial of service (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500d01_denialofservicereflecteddombased){:target="_blank"}  
[Denial of service (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500d02_denialofservicestoreddombased){:target="_blank"}  


## Others

[Out-of-band resource load (HTTP)](https://portswigger.net/knowledgebase/Issues/details/00100a00_outofbandresourceloadhttp){:target="_blank"}  
[WebSocket hijacking (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200340_websockethijackingdombased){:target="_blank"}  
[WebSocket hijacking (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200341_websockethijackingreflecteddombased){:target="_blank"}  
[WebSocket hijacking (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00200342_websockethijackingstoreddombased){:target="_blank"}  
[LDAP injection](https://portswigger.net/knowledgebase/Issues/details/00100500_ldapinjection){:target="_blank"}  
[SMTP header injection](https://portswigger.net/knowledgebase/Issues/details/00200800_smtpheaderinjection){:target="_blank"}  
[Os command injection](https://portswigger.net/knowledgebase/Issues/details/00100100_oscommandinjection){:target="_blank"}  

[Flash cross-domain policy](https://portswigger.net/knowledgebase/Issues/details/00200400_flashcrossdomainpolicy){:target="_blank"}  
[Silverlight cross-domain policy](https://portswigger.net/knowledgebase/Issues/details/00200500_silverlightcrossdomainpolicy){:target="_blank"}  

[External service interaction (DNS)](https://portswigger.net/knowledgebase/Issues/details/00300200_externalserviceinteractiondns){:target="_blank"}  
[External service interaction (HTTP)](https://portswigger.net/knowledgebase/Issues/details/00300210_externalserviceinteractionhttp){:target="_blank"}  
[External service interaction (SMTP)](https://portswigger.net/knowledgebase/Issues/details/00300220_externalserviceinteractionsmtp){:target="_blank"}  

[Cross-domain POST](https://portswigger.net/knowledgebase/Issues/details/00400500_crossdomainpost){:target="_blank"}  
[Input returned in response (stored)](https://portswigger.net/knowledgebase/Issues/details/00400b00_inputreturnedinresponsestored){:target="_blank"}  
[Input returned in response (reflected)](https://portswigger.net/knowledgebase/Issues/details/00400c00_inputreturnedinresponsereflected){:target="_blank"}  
[Suspicious input transformation (reflected)](https://portswigger.net/knowledgebase/Issues/details/00400d00_suspiciousinputtransformationreflected){:target="_blank"}  
[Suspicious input transformation (stored)](https://portswigger.net/knowledgebase/Issues/details/00400e00_suspiciousinputtransformationstored){:target="_blank"}  
[Cross-domain Referer leakage](https://portswigger.net/knowledgebase/Issues/details/00500400_crossdomainrefererleakage){:target="_blank"}  
[Cross-domain script include](https://portswigger.net/knowledgebase/Issues/details/00500500_crossdomainscriptinclude){:target="_blank"}  
[Session token in URL](https://portswigger.net/knowledgebase/Issues/details/00500700_sessiontokeninurl){:target="_blank"}  

[File upload functionality](https://portswigger.net/knowledgebase/Issues/details/00500980_fileuploadfunctionality){:target="_blank"}  

[Long redirection response](https://portswigger.net/knowledgebase/Issues/details/00400800_longredirectionresponse){:target="_blank"}  
[Open redirection](https://portswigger.net/knowledgebase/Issues/details/00500100_openredirection){:target="_blank"}  
[Open redirection (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500110_openredirectiondombased){:target="_blank"}  
[Open redirection (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500111_openredirectionreflecteddombased){:target="_blank"}  
[Open redirection (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00500112_openredirectionstoreddombased){:target="_blank"}  

[Link manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501000_linkmanipulationdombased){:target="_blank"}  
[Link manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501001_linkmanipulationreflecteddombased){:target="_blank"}  
[Link manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501002_linkmanipulationstoreddombased){:target="_blank"}  
[Document domain manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501100_documentdomainmanipulationdombased){:target="_blank"}  
[Document domain manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501101_documentdomainmanipulationreflecteddombased){:target="_blank"}  
[Document domain manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501102_documentdomainmanipulationstoreddombased){:target="_blank"}  
[DOM data manipulation (DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501200_domdatamanipulationdombased){:target="_blank"}  
[DOM data manipulation (reflected DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501201_domdatamanipulationreflecteddombased){:target="_blank"}  
[DOM data manipulation (stored DOM-based)](https://portswigger.net/knowledgebase/Issues/details/00501202_domdatamanipulationstoreddombased){:target="_blank"}  

[HTML does not specify charset](https://portswigger.net/knowledgebase/Issues/details/00800200_htmldoesnotspecifycharset){:target="_blank"}  
[HTML uses unrecognized charset](https://portswigger.net/knowledgebase/Issues/details/00800300_htmlusesunrecognizedcharset){:target="_blank"}  

[SSL certificate](https://portswigger.net/knowledgebase/Issues/details/01000100_sslcertificate){:target="_blank"}  
[Unencrypted communications](https://portswigger.net/knowledgebase/Issues/details/01000200_unencryptedcommunications){:target="_blank"}  
[Strict transport security not enforced](https://portswigger.net/knowledgebase/Issues/details/01000300_stricttransportsecuritynotenforced){:target="_blank"}  
[Mixed content](https://portswigger.net/knowledgebase/Issues/details/01000400_mixedcontent){:target="_blank"}  
