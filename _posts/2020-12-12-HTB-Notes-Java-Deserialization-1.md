---
layout:     post
title:      Hackthebox Notes Java Deserialization Part 1
date:       2012-12-12
summary:    First run of poking on the next easiest medium-difficulty machine available on Hackthebox
categories: CTF
thumbnail: cogs
tags:
 - CTF
 - Hackthebox
---
{% highlight ruby %}
current IP 10.10.10.214  
ran nmap -sT -sV -A -p- -oN nmapsT --script=vuln <ip>  
idk if i can do --script=vuln in conjunction with -sV or -A, let's see  
looks like it runs pretty well  
results:     
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)  
| vulners:   
|   cpe:/a:openbsd:openssh:8.2p1:   
|       CVE-2020-15778  6.8     https://vulners.com/cve/CVE-2020-15778  
|       CVE-2020-12062  5.0     https://vulners.com/cve/CVE-2020-12062  
|_      CVE-2020-14145  4.3     https://vulners.com/cve/CVE-2020-14145  
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))  
| http-csrf:   
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.10.10.214  
|   Found the following possible CSRF vulnerabilities:   
|       
|     Path: http://10.10.10.214:80/  
|     Form id:   
|_    Form action:   
|_http-dombased-xss: Couldn't find any DOM based XSS.  
| http-fileupload-exploiter:   
|     
|     Couldn't find a file-type field.  
|     
|_    Couldn't find a file-type field.  
| http-internal-ip-disclosure:   
|_  Internal IP Leaked: 127.0.1.1  
|_http-server-header: Apache/2.4.41 (Ubuntu)  
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.  
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)  
| vulners:   
|   cpe:/a:apache:http_server:2.4.41:   
|       CVE-2020-11984  7.5     https://vulners.com/cve/CVE-2020-11984  
|       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927  
|       CVE-2020-9490   5.0     https://vulners.com/cve/CVE-2020-9490  
|       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934  
|_      CVE-2020-11993  4.3     https://vulners.com/cve/CVE-2020-11993    
checking http page  
interesting, it's a json parser with input/output form  
input/output form has two options:   
	Beautify (the input)   
	Validate (the input)  
ran <script>alert(1)</script> into Validate option  
got the following output:    
Validation failed: Unhandled Java exception:  
com.fasterxml.jackson.core.JsonParseException:   
Unexpected character ('<' (code 60)):   
expected a valid value (number, String, array, object, 'true', 'false' or 'null')    
this is prob the way in, but keep enumerating http  
checked http source...nothing special  
ran OWASP ZAP Proxy against http...nothing special  
ran LEGION against ip...stuck at final nmap?? circle back if necessary  
ran gobuster with seclists/*/directory-list-2.3-big.txt...nothing special  
circling back to the Unexpected character  
googled the full unhandled exception string...nothing special, mostly stack overflow Solution posts  
googled json validator cve...got some interesting CVEs on different JSON validator packages  
re-checked ZAP spider output, found jquery 3.2.1 being used  
googled jquery CVE  
found CVE-2020-11022  
loaded up msfconsole  
searched for jquery  
found exploit/unix/webapp/jquery_file_upload  
what the hell may as well run it real quick  
used exploit  
set options  
ran exploit...could not find target (TARGETURI, found none from fuzzing)  
returning to actually read CVEs  
also googling json beautify cve  
CVE 2019-19507 keeps popping up  
located and reading through https://www.cvedetails.com/vulnerability-list/vendor_id-15866/product_id-42991/Fasterxml-Jackson-databind.html  
also found CVE-2019-14439  
not sure if any specific CVE found has a specific exploit or PoC for this box, or if I'll have to research and roll my own  
appears that Java Deserialization is the hot topic and I am unsure if this is like SQLi where I just need to know how it works and experiment with it  
been reading for an hour about java deserialization to get a basic understanding of it  
found resource https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html  
pause for the night to spend time with wife  
{% endhighlight %}
