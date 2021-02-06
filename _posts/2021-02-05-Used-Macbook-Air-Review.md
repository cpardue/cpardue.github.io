---
layout:     post
title:      Used Macbook Air Review
date:       2021-02-05
summary:    Used Macbook Air Kali Netbook
categories: homelab
thumbnail: cogs
tags:
 - homelab
---

## Used Macbook Air Review  

### Procurement  
Last year I had been searching for an ultra-lightweight netbook with greater than an I3 and 8GB of RAM, to use around the house for homelab stuff.  
I ended up procuring a used mid-2013 Macbook Air from my wife instead of spending money on yet another laptop.  The problem was, it didn't work.  
I could boot it to recovery mode, but couldn't boot into the OS.  
I decided that this must be a corrupted OS issue.  
I booted into the Mac recovery environment and tried to re-install OSX via wifi.  It failed because it could not detect disk.  
I decided that ok, this must be a dead disk.  
I replaced the disk (turned out to be a proprietary NVMe) with an aftermarket NVMe from Amazon, and installed OSX from wifi.  
While looking around within the mac, I decided that Liveoverflow is right, Macs do have great build quality.  So I decided to keep it.  

## User Review of Used Macbook Air  
This mid-2013 Macbook Air happens to have an I5, 8GB RAM, and a 512 NVMe.  The Wifi card is a proprietary connection.  
Booting up is fine, as fast as any decent laptop.  But it's small and light.  
Navigating around is fine, as fast as any decent laptop again.  It's OSX, so...  
I had good luck installing Brew, and a host of pentesting tools via Brew packages, and Burp, and Virtualbox with a kali vm running, and it was all fine.  Just fine.  Only fine.  
It was super annoying trying to figure out how to import new modules into MSF5 through Kali documentation, when I'm on a Macbook working out of /opt/ directories.  
I had to re-download wordlists, constantly download, move, and reconfigure programs, and using it as a pure pentesting platform was just a real pain.  
I set up my Kali Virtualbox VM to auto load upon boot, but I was then just using this laptop as a pure VM host, which is largely useless because I'm running two OS' on one machine's resources.  Oooh I can snapshot the VM though.  Oooh well I shouldn't rm -rf /.  
I decided to make some improvements to the Macbook Air to offset these liabilities and bring out the assets.   

## How To Imrpove Your Used Macbook Air  
-Pre-req's  
-USB  
-Wifi  
-USB wifi card  
Go to https://www.kali.org/downloads/ and download a Kali ISO.  
Press Command+Spacebar, search for Terminal, and open it.  
Run the command _diskutil list_  
Note the drives listed.  
Plug in a blank USB.  
Run the command _diskutil list_  
Note the new drive listed.  It will probably be /dev/disk2.  
Unmount your USB to write an ISO to it by running the command _diskutil unmountDisk /dev/disk2_ where /dev/disk2 is whatever your actual USB ended up being.  
_cd_ to your Kali ISP download directory.  
Write the ISO to the USB by running the command _sudo dd if=kaliisofilename.iso of=/dev/disk2orwhatever bs=1m_  
Shutdown your Macbook by running the command _sudo shutdown -h now_  
Boot your Macbook into Startup Manager by pressing the power button, as soon as you hear the chime, press and hold the Option key, releasing once in menu.  
Select your USB and double-click.  
Select Install.  
Follow the prompts all the way through to install Kali with a full guided partition.  
Once Kali is installed, plug in your USB wifi card.  This is because you have to install the Mac Wifi Card drivers manually, isn't that fun?  
Connect to your wifi with the auxiliary USB card.  
Run the command _lspci | grep Wireless_ to find your Broadcom chipset.  
Check it against https://tutorialforlinux.com/2020/05/14/how-to-install-broadcom-driver-in-kali-gnu-linux/ to be sure of whether you need to install WI or B43 drivers.  
If WI, run the commands _sudo apt install broadcom-sta-dkms_ then _sudo reboot -h now_.  
If B43, run the commands _sudo apt install firmware-b43-installer_ then _sudo reboot -h now_.  
Remove the USB wifi card and connect your onboard wifi card to your wifi.  
Open a terminal and run the following commands  
1. sudo apt-get clean  
2. sudo apt-get update -y  
3. sudo apt-get upgrade -y  
4. sudo apt-get dist-upgrade -y  
5. sudo apt-get install git -y  
6. cd /etc/ssh/  
7. dpkg-reconfigure openssh-server  
8. sudo passwd root  
9. (enter a new root password)  
10. sudo apt install seclists -y
11. sudo apt-get install openvpn -y  

So now you have the absolute best upgrades done for your Macbook Air.  Thank me later.  
Install Discord, Flameshot, Obsidian, Remmina, whatever else you want for your Macbook Air needs.  
Enjoy those silky smooth colors, that fast terminal command action, and the complete lack of having to rePATH everything from Python version installs to /opt/*/Metasploit-Framework directories.  
I've run Kali on so many different machines now, and this old Macbook Air is easily the most comfy, cool, and responsive Kali host I have used yet.  
The I5 and 8GB in this tiniest of form factors is heaven, and this Mac track pad has a gorgeous feel, like touching silk all day.  
I give the used Macbook Air a user review rating of 10 out of 10.  
