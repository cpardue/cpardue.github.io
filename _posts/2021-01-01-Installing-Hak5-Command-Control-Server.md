---
layout:     post
title:      Installing the Hak5 Command Control Server
date:       2021-01-01
summary:    Installing the Hak5 Command & Control Server 
categories: Homelab
thumbnail: jekyll
tags:
 - Homelab
 - ESXi
 - Linux
 - Ubuntu
---

"Order" the C2 binary from the Hak5 shop, so that you can receive your Community Key via email

Download and Transfer to your Webserver:  
Download the binary at https://downloads.hak5.org/cloudc2  

Now scp that file over to your webserver:  
Install Putty if you haven't yet  
Navigate to your Putty.exe folder  
Click the Explorer's Navbar, type in CMD, press enter (This just opens a cmd shell at that folder)  
>pscp -scp thec2binary.zip yourubuntuuser@ubuntuIP:/target/path/  

Install it:  
Access your webserver (via console or SSH)  
cd to the binary's directory  
mkdir a new c2 binary directory  
mv the zipped binary into that directory  
cd into that directory  
unzip the c2 binary  
execute the binary with ./binaryname -hostname hostnameOrIP  
Copy that token that comes up  
Elsewhere on your network, navigate to that webserver's hostname:8080  
You should now be at the C2 Setup Web Page  
Enter your Token from the install script, and your License from the download email  
Finish editing in info, then click Complete  
Log in  
Go to Menu > Settings > Account > Theme  
Change it to Dark theme, you are not a psychopath  
Go to Menu > Settings > Server > Loot Collection Limit  
Change it to suite your webserver storage  

That's it, you're done.  
You can now start adding devices.  
