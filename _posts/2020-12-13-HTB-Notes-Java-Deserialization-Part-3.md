---
layout:     post
title:      Hackthebox Notes Java Deserialization Part 3
date:       2020-12-13
summary:    Third run on the next easiest medium-difficulty machine available on Hackthebox
categories: CTF
thumbnail: cogs
tags:
 - CTF
 - Hackthebox
 - rooted
---
{% highlight ruby %}
back from meeting  
extended machine time  
hit up 10.10.10.214 in browser, webserver is up  
started netcat listener again  
dropped exploit code into json validator again  
SimpleHTTPServer still up  
machine is taking a while to complete the shell this time  
it's because i was sending exploit with netcat port instead of simplehttpserver's port  
so once the code runs in server,  
server pulls inject.sql from simplehttpserver,  
inject.sql points a shell toward netcat listener.  
boom i have a shell again  
on to enumeration  

ran ps aux | grep root:  
/sbin/init auto automatic-ubiquity noprompt  
/usr/sbin/sshd -D [listener]  

ran ls /bin/:  
there's a shell script in here, timer_backup.sh  

ran ls -lha /bin/:  
ha timer_backup.sh is owned by pericles  

ran less /bin/timer_backup.sh:  
zip -r website.bak.zip /var/www/html && mv website.bak.zip /root/backup.zip  
i did this same thing as a secondary backup for a webserver once, uh oh...  
how can i use this to get root  

looking at cron files, each runs as root  
looking at crontab, everything in cron runs as root  
i can't view cron.hourly directory contents but i assume it runs timer_backup.sh  
maybe i can cat root flag into pericles directory  
echo'ed "cat /root/root.txt > /home/pericles/root.txt"  successfully  
waiting on that  

sshd is running as root, script prob runs as root  
i can prob sshd in as root if this fails  
i should have added a catch error to the timer_backup.sh i guess  
still waiting on initial timer_backup.sh command  

just checked /home/pericles, root.txt exists  
cat /home/pericles/root.txt  
IT'S A FLAG  
active box rooted in 26hrs, world record for me  
{% endhighlight %}
