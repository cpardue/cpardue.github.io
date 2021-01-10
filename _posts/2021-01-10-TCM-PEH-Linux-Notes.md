---
layout:     post
title:      TCM PEH Linux Notes
date:       2021-01-10
summary:    TheCyberMentor's Practical Ethical Hacking Course Linux Notes
categories: Certs
thumbnail: jekyll
tags:
 - Linux
 - Certs
---

These are the notes I've taken for TheCyberMentor Academy's Practical Ethical Hacking - Intro to Linux section
NOTE: Pasting my CherryTree notes into github without formatting is turning out to be some real hell.  
I really don't want to start typing "&nbsp" everywhere in my notes.  
The alternative is that my notes will look like crap when I post them into the blog.  
I am not altogether against it.  Below I've opted to paste my notes into an online page break converter.  
Maybe I should write a python tool to convert my raw notes into markdown, as practice.  
Maybe.  

<p class="has-line-data" data-line-start="0" data-line-end="4">new kali and sudo<br>
new kali uses non-privileged user<br>
use sudo to run as root now, duh<br>
su - to switch to root with kali’s pw, duh</p>
<p class="has-line-data" data-line-start="5" data-line-end="32">navigating filesystem<br>
pwd<br>
print present working directory<br>
cd …<br>
change dir up one dir<br>
ls -lha<br>
list contents (long, human readable, all)<br>
~<br>
shorthand for home dir<br>
mkdir newFolder<br>
make directory newFolder here<br>
echo “this text” &gt; newfile<br>
output “this text” overwritten into newfile<br>
cp /location/A/file /location/B/file<br>
copy file from A to B<br>
mv /path/A/file /path/B/FILE<br>
move and rename file from A to B<br>
updatebd<br>
update cache of locations and paths<br>
do this semi-frequently<br>
passwd<br>
change current user password<br>
enter twice to over-ride complexity rules<br>
man command<br>
manpage for command<br>
command --help<br>
abbreviated manpage (sometimes)</p>
<p class="has-line-data" data-line-start="33" data-line-end="57">Users and Privileges<br>
rwx/rwx/rwx owner group<br>
owner/group/all owner group<br>
rwx = 421 bits<br>
777 = rwxrwxrwx<br>
111 = r–r--r–<br>
123 = r—w-rw-<br>
chmod -switch file<br>
switch can be bits or switch<br>
switch can set sticky bit too<br>
set sticky bit = u+s<br>
remove sticky bit = u-s<br>
adduser<br>
check the man for the cool switches<br>
addgroup<br>
again check the man for the cool options<br>
deluser<br>
delete user<br>
usermod<br>
modify user<br>
passwd<br>
passwd -switch username<br>
su username<br>
switch to user username</p>
<p class="has-line-data" data-line-start="58" data-line-end="78">Common Network Commands<br>
ifconfig<br>
legacy ip addr<br>
iwconfig<br>
wireless<br>
ip addr<br>
replaced ifconfig<br>
ip addr | grep ‘inet ’<br>
ping -c n ip<br>
ping n times this ip<br>
arp -a<br>
check ip and mac directly associated<br>
netstat<br>
view connections<br>
netstat -ant<br>
tcp<br>
netstat -anu<br>
udp<br>
route<br>
prints routing table</p>
<p class="has-line-data" data-line-start="79" data-line-end="91">Installing and Updating Tools<br>
sudo apt-get update -y &amp;&amp; sudo apt-get upgrade -y<br>
sudo apt-get dist-upgrade -y<br>
sudo apt-get install pip3<br>
<a href="http://pimpmykali.sh">pimpmykali.sh</a><br>
rm -rf pimpmykali/<br>
git clone <a href="https://github.com/Dewalt-arch/pimpmykali">https://github.com/Dewalt-arch/pimpmykali</a><br>
cd pimpmykali<br>
sudo ./pimpmykali.sh<br>
select 0<br>
sudo apt install gedit -y<br>
sudo apt install flameshot -y</p>
<p class="has-line-data" data-line-start="92" data-line-end="109">Viewing, Creating, and Editing Files<br>
echo ‘string’ &gt; textfile<br>
echo ‘string’ &gt;&gt; textfile<br>
appends the data<br>
cat filename<br>
more filename<br>
less filename<br>
less is more than more<br>
head -n filename<br>
list first n lines of filename<br>
tail -n filename<br>
list last n lines of filename<br>
tail -f filename<br>
cats tail of filename perpetually, good for logs<br>
touch filename<br>
changes timestamp to time touched<br>
creates file that did not previously exist</p>
