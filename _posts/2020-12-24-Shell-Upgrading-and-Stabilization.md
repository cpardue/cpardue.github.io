---
layout:     post
title:      Shell Upgrading and Stabilization
date:       2020-12-24
summary:    Quick note on stabilizing a crappy reverse shell
categories: CTF
thumbnail: cogs
tags:
 - CTF
---
I've been participating in this year's Tryhackme.com Adent of Cyber cybersecurity event.  
Thought I better drop some notes on stabilizing a shell so I don't forget.  
Reverse shells are crappy and have no features.  Try Ctrl+a, up-arrow, or even pressing back-arrow to correct a pasted in one-liner.  It won't work.  
So here is the method I've found for upgrading and stabilizing a reverse shell.  

{% highlight ruby %}
python3 -c 'import pty;pty.spawn("/bin/bash")'  
export TERM=xterm  
Ctrl + Z  
stty raw -echo; fg  
//if the shell dies, try 'reset' command
{% endhighlight %}
