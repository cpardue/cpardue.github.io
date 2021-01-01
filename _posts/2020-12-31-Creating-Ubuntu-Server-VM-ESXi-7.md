---
layout:     post
title:      Creating Ubuntu Server VM in ESXi 7.0
date:       2020-12-31
summary:    Un-necessarily detailed step-by-step of creating an Ubuntu Server VM in ESXi
categories: Homelab
thumbnail: jekyll
tags:
 - Homelab
 - Virtualization
 - ESXi
 - Linux
 - Ubuntu
---

Creating Ubuntu Server VM in ESXi 7  
Un-necessarily detailed step-by-step of creating an Ubuntu Server VM in ESXi

Step One - Download the Media  

Download ISO from https://ubuntu.com/download/server  
Log into ESXi  
Access Storage>Datastore  
Click Datastore Browser  
Click Upload, browse to ISO, and upload  
Wait for the "1 file(s)" in top right of Datastore Browser to reach 100%  

Step Two - Provision the VM  

Log into ESXi  
Access Virtual Machines  
Click Create/Register VM  
Select Create a new virtual machine, click Next  
Enter a VM Name (WEB-NIX-C2 for me), select ESXi Version compatability (ESXi 7.0), select OS family (Linux), select OS version (Ubuntu Linux 62-bit), click Next  
Select desired Datastore, click Next  
Select CPU (1), Memory (2048MB for me), HDD (25GB for me), Network Adapter, CD/DVD (Datastore's ubuntu ISO), click Next  
Click Finish  

Step Three - Install the OS  

Log into ESXi  
Access Virtual Machines > The new VM  
Click Power On  
Click Console > Open (whichever) Console  
Follow the prompts as they appear within the Ubuntu Installation  
NOTE: You will have to navigate via arrows or Tab/Shift+Tab, and Enter  
Select Language  
Select Continue Without Updating  
Select Keyboard Type  
Enter a static IP  
Enter Proxy Address, if any  
Enter Mirror Address, if you have any specific mirrors you want to use  
Use Entire Disk  
Review Guided Disk info, select Done  
Enter Profile info  
Install OpenSSH Server  
Select any Server Snaps you want (none for me)  
At this point, the system will begin installing
Reboot when prompted  
In ESXi, Edit the VM, and make sure that the CD/DVD Connected checkbox is now unchecked, then Save  
In the VM Console, go ahead and follow the reboot prompt all the way through  


Step Four - Configure the OS  

Open a shell and run the following commands blindly  
sudo apt-get -y && sudo apt-get upgrade -y  

Check passwd to make sure only Root has a 0 UID  
awk -F: '($3=="0"){print}' /etc/passwd  

Check shadow for accounts with empty password  
cat /etc/shadow | awk -F: '($2==""){print $1}'

If there are any stock accounts that need to be locked down then lock them  
passwd -l <accountName>  

Lock down shared memory  
sudo echo 'tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0' >> /etc/fstab && sudo tail -2 /etc/fstab  

Lock down SSH  
sudo nano /etc/ssh/sshd_config  
Edit the Protocol line with 2  
Edit the AllowUsers line with <yourUser>@<yourLanNetworkPrefix>.*  
Edit the PermitRootLogin line with no  
Edit the PermitEmptyPasswords line with no  
Edit the X11Forwarding line with no  

Lock down SU (by creating a base configuration)  
sudo groupadd admin  
sudo usermod -a -G admin <yourUser>  
sudo dpkg-statoverride --update --add root admin 4750 /bin/su  

Install Fail2Ban  
sudo apt-get install fail2ban -y  
sudo nano /etc/fail2ban/jail.local  
Edit in the following lines:  
[sshd]  
enabled = true  
port = 22  
filter = sshd  
logpath = /var/log/auth.log  
maxretry = 3  

Harden against IP Spoofing  
sudo nano /etc/hosts.conf  
Edit in the following:  
order bind,hosts  
nospoof on  

Reboot to restart and apply most of the systemctl changes above  
sudo reboot -h now  
