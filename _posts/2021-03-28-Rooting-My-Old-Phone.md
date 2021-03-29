---
layout:     post
title:      Rooting My Old Phone for "Educational" Purposes
date:       2021-03-28
summary:    Rooting My Old LG K20 Plus Phone for "Educational" Purposes
categories: homelab
thumbnail: cogs
tags:
 - homelab
 - pwned
---
<p class="has-line-data" data-line-start="0" data-line-end="1">I’m going to level with you, I had no idea what I was doing when I started this process earlier today.  I just happened to google “&lt;my old phone model&gt; nethunter install”, saw a specific thread, and jumped right in.</p>
<h3 class="code-line" data-line-start=1 data-line-end=2 ><a id="Basic_Overview_of_Rooting_1"></a>Basic Overview of Rooting</h3>
<p class="has-line-data" data-line-start="2" data-line-end="3">Or rather, what I’ve learned about it so far:</p>
<p class="has-line-data" data-line-start="4" data-line-end="14">Phone goes into Developer Mode.<br>
Phone USB Debugging is enabled.<br>
Phone is connected to a toolkit called ADB (Android Debugging Bridge) via a PC.  (Think of this as a simple hardware serial interface.)<br>
ADB is used to place phone into Bootloader mode.  (Think of this as a BIOS Boot handoff environment.)<br>
ADB is used to unlock the phone.<br>
ADB is used to flash a recovery image to the phone.  (This will be the new second stage BIOS environment.)<br>
The phone is then booted into the recovery environment, to install new packages or flavors of Android.<br>
Among new packages installed, install SuperSu, which roots the phone.<br>
I performed the steps above without much issue.<br>
Now comes the annoying and fun stuff.<br>
I only rooted this phone to install Kali Nethunter on it.  So, on to the next task.</p>
<h3 class="code-line" data-line-start=14 data-line-end=15 ><a id="Kali_Nethunter_Install_14"></a>Kali Nethunter Install</h3>
<p class="has-line-data" data-line-start="15" data-line-end="38">I found a custom Kernel for Nethunter builds on the LG K20 on a random forum. (<a href="https://forum.xda-developers.com/t/unofficial-howto-kali-nethunter-lg-k20-plus-mp260-modified-kernel.3839799/">https://forum.xda-developers.com/t/unofficial-howto-kali-nethunter-lg-k20-plus-mp260-modified-kernel.3839799/</a>)<br>
I mean, literally, I found this one single Kernel build.<br>
The Nethunter Build requires a flashed and unlocked phone that already has an amd64 OS on it, ie. a 64-bit build.<br>
I flashed TWRP firmware to the phone to root it.  Ok, so that part is completed.  After poking around, I learned how to navigate around to various images on internal storage or the SD Card.<br>
So, I plastered a bunch of 64-bit images to the SD Card so that I could rapidly install them on top of each other in case some work and some don’t.<br>
I immediately began testing installs, and kept getting the same failed output of ERROR:255.<br>
“255” sounded like a suspicious number for an error code (bits are maxed out like hex FF:FF:FF, so it must be THE most general error) so I went ahead and googled it once I confirmed that at least 2 images were outputing the same error code.<br>
Turns out, ERROR:255 is the result of attempting to install a 64 bit OS with firware that can only install/compile 32-bit images.  Ugh.<br>
I tracked down firmware (bootloader in “phone rooting” terminology) named PBRP, which handles 64-bit images.<br>
So I flashed PBRP to the phone’s bootloader, then successfully installed LineageOS 15 and GoogleApps 8.1.  Ok, so that part is completed (again).<br>
I then immediately attempted an install of the Nethunter Kernel, and a generic version of nethunter itself, in that order.<br>
The Kernel compiled just fine, but Nethunter exited with an error, stating explicitly (nice!) that I need to set up the phone first. Roger that.<br>
I went through initial set up, then booted back to the bootloader and installed Kernel and Nethunter itself again, rebooted.<br>
My phone’s LG splash image at boot came up and…stayed up.  For over 30min.  I did a hard reboot and had same effect.<br>
After close examination of the above linked XDA Thread, I saw that the author was using LineageOS 14.  Okay.<br>
I went ahead and re-flashed LineageOS 14.1 and GoogleApps 7.1 Micro (I’m catching on).<br>
I then waited for LineageOS to initialize, went through the initial setup, and booted back into the new PBRP environment.<br>
I held my breath, installed the custom Kernel, then the Nethunter build, watching apprehensively as the Nethunter terminal output progressed BEYOND where it was erroring out and just kept continuing on.<br>
“IT’S WORKING,” I exclaimed to my wife.  My poor wife, at this point, has endured my interrupting her periodically for multiple hours while I’ve tried to make headway on this phone thing.<br>
“Oh that’s good”, she tells me.  No.  No no no.  This is excellent.<br>
I have been purposelessly installing Kali Linux on everything for maybe 6 years now, similar to those who install Doom on everything.  (<a href="https://www.reddit.com/r/itrunsdoom/">https://www.reddit.com/r/itrunsdoom/</a>)<br>
I have been dreaming of installing Nethunter on a phone since nethunter came out, and have just not yet had the clear willingness or resources to accomplish this task.<br>
Now that I have nethunter on a phone, I can place it back in a drawer and feel truly accomplished.</p>
<h3 class="code-line" data-line-start=40 data-line-end=41 ><a id="Update_40"></a>Update:</h3>
<p class="has-line-data" data-line-start="41" data-line-end="50">The day I posted this, which is the day after installing Nethunter, I have been updating and playing with it.<br>
Good Lord this is awesome.  I have been playing with cSploit.<br>
I will probably update PGP keys and upgrade to the newest version this week so that I can access and download apps from the Nethunter App Store, or download the latest generic image and install it.<br>
I want to access the Nethunter App store and see what new stuff is in there.<br>
Is there a Responder port???  That would be so great!<br>
Is there an OWASP ZAP Proxy port???<br>
Can I do vlan hopping with this thing???<br>
Can I install and properly use Wifi Pumpkin???<br>
The possible applications of this innoculous phone are very exciting!</p>
