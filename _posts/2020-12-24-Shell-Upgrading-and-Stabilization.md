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
python3 -c 'import pty;pty.spawn("/bin/bash")'  //Imports a better python bash shell
export TERM=xterm  //gives term commands like Clear
Ctrl + Z  //backgrounds shell for a sec in order to...
stty raw -echo; fg  //  turn off terminal echo and allow arrows, autocomplete, ctrl+xyz, etc, then foreground
//if the shell dies, you no longer have terminal echo thus won't see it, try 'reset' command
{% endhighlight %}
