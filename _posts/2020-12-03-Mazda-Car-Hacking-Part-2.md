---
layout:     post
title:      Mazda CX 5 Car Hacking - Part 2
date:       2020-12-03 
summary:    Gaining a root shell in the Johnson Controls Infotainment system
categories: Research
thumbnail: jekyll
tags:
 - Research
---

I had 10 minutes tonight of just me, my car, and a keyboard, to test out methods I've researched for gaining a shell on the car.  Below are the steps I followed to gain a root shell to the system, this will work for any Mazda with a JCI infotainment system and un-patched firmware.  

-Got in car

-Started car

-Waited for infotainment system to finish booting

-Pressed/held 'Music+Favorites+Mute' for about 10sec

-Debug menu popped up

-Pressed/held 'DEL' for about 20sec until I saw "JCI Test Mode Active" output

-Entered '11', pressed 'ENTER'

-Entered JCI Test Mode Scripts page

-Pressed 'Next' to see Terminal button option

-Pressed 'Terminal' button option

-Was greeted to a pleasant ash shell!

-Plugged keyboard into top USB port (bottom will probably work as well)

-Ran 'whoami'

-Output: 'root'

YES!!  ROOT SHELL!!  

I poked around the file system.  

-Ran 'which vi'

-'vi' is installed.  

-Ran 'ls /etc/ssh'

-'/etc/ssh/sshd_config' is present.

-Tried to 'cat /etc/ssh/sshd_config' but couldn't enter "_" character via keyboard

-Tried to 'cat /etc/ssh/sshd*' but couldn't enter "*" either

-This could be the keyboard or it could be the shell.  

-I poked around the filesystem a little more.  

-Noted that there are more binaries located under '/jci/bin'.  

-At this point my 10 minute window was up so I exited the shell by entering exit.  

-Backed out of all the menu options via exit buttons, and didn't even have to restart the car to return to normal function.  
Drove home.  
<br>
<br>
# Next Steps: 

-Tweak and install MZD-AIO scripts.  

-Find out if nc is installed, for future use.  

-Find out if cron is installed, for future use.  

-Start exploring the wifi possibilities.  

-If wifi can bridge to home wifi, then investigate installation of routersploit.  
