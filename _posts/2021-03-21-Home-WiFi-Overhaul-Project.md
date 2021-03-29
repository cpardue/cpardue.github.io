---
layout:     post
title:      Home WiFi Overhaul Project
date:       2021-03-21
summary:    Home WiFi Overhaul Project with Unifi UAP AC Pro's and a UCK
categories: homelab
thumbnail: cogs
tags:
 - homelab
---
<p class="has-line-data" data-line-start="1" data-line-end="2">Today, I am pretending that I am at work!</p>
<h3 class="code-line" data-line-start=4 data-line-end=5 ><a id="Background_4"></a>Background</h3>
<p class="has-line-data" data-line-start="5" data-line-end="11">My home wifi has consisted of a single vulnerable WiFi AP given to us by our ISP.<br>
My family lives in a 2 story house which is shaped like a box.<br>
There are no drops in this house, this one AP must fuel the entire 2 story house.<br>
The ISP Modem is at one end of the house.<br>
Problem: We need wifi to extend from one side of the house to the other, with full coverage.<br>
Solution: Upgrade to two Unifi UAP AC Pro’s, installed so that radio saturation is achieved.</p>
<h3 class="code-line" data-line-start=12 data-line-end=13 ><a id="Scope_12"></a>Scope</h3>
<p class="has-line-data" data-line-start="13" data-line-end="20">-Task 1: Order two (2) Unifi UAP AC PRO’s and one (1) Unifi Cloud Key<br>
-Task 2: Research and plan network and installation points for APs<br>
-Task 3: Prototype new wifi network in lab<br>
-Task 4: Install APs<br>
-Task 5: Install Cloud Key<br>
-Task 6: Test new wifi network<br>
-Task 7: Wrap-Up Meetings and Conclusion</p>
<h3 class="code-line" data-line-start=22 data-line-end=23 ><a id="Task_1_22"></a>Task 1:</h3>
<p class="has-line-data" data-line-start="23" data-line-end="30">Order two (2) Unifi UAP AC PRO’s and one (1) Unifi Cloud Key<br>
Necessary funds have been allocated for ordering APs and Cloudkey via a COVID Stimulous Check<br>
-2 AP’s ordered<br>
-1 UCK ordered<br>
-2 AP’s received<br>
-1 UCK received<br>
Task 1 completed</p>
<h3 class="code-line" data-line-start=31 data-line-end=32 ><a id="Task_2_31"></a>Task 2:</h3>
<p class="has-line-data" data-line-start="32" data-line-end="46">Research and plan network and installation points for APs<br>
-Googled UAP AC PRO Wireless Bridge, found article (<a href="https://michaelryom.dk/using-ubiquiti-ac-pro-as-a-wireless-bridge/?doing_wp_cron=1617032959.3260259628295898437500">https://michaelryom.dk/using-ubiquiti-ac-pro-as-a-wireless-bridge/?doing_wp_cron=1617032959.3260259628295898437500</a>)<br>
-Noted that “Wireless Bridge” == “Mesh”<br>
-Noted that Mesh is possible with all purchased hardware<br>
-Googled UAP AC PRO Radio Patterns, found article (<a href="https://help.ui.com/hc/en-us/articles/115005212927#summary">https://help.ui.com/hc/en-us/articles/115005212927#summary</a>)<br>
-UAP AC PRO pattern image below:<br>
<img src="https://help.ui.com/hc/article_attachments/115014562308/UAP-AC-Pro-Overall_-_Summary_Plot5ghz.png" alt="Pattern"><br>
-Image is overlayed on itself as shown below:<br>
<img src="https://help.ui.com/hc/article_attachments/115022437707/image8.png" alt="Overlay"><br>
-Drew up how APs can be installed with Mesh to cover necessary areas in house<br>
-Install points and coverage shown below:<br>
<img src="https://i.imgur.com/yBu65hs.png" alt="Coverage"><br>
-Worst case scenario, with inverse square and interference, this will maximize coverage<br>
Task 2 completed</p>
<h3 class="code-line" data-line-start=47 data-line-end=48 ><a id="Task_3_47"></a>Task 3:</h3>
<p class="has-line-data" data-line-start="48" data-line-end="82">Prototype new wifi network in lab<br>
-Installed Unifi Controller on lab machine<br>
-Logged into Controller<br>
-Set up network to point to home GW, no DHCP, home GW will hand out DHCP<br>
-Set up same SSID and PSK as current<br>
-Enabled Meshing<br>
-Enabled rolling updates<br>
-Checked through settings, no further settings to set at this time<br>
-Saved config<br>
-Hooked up PoE adapters (NOTE: I need a QUIET rackmount PoE switch)<br>
-Powered on UCK<br>
-Logged in with default ubnt:ubnt, changed password<br>
-Imported config<br>
-Logged back into UCK<br>
-Checked configuration, all is OK<br>
-Updated UCK firmware<br>
-Updated Controller software<br>
-Printed and applied label for AP01<br>
-Printed and applied label for AP02<br>
-Hooked up AP01 and AP02 to network<br>
-Viewed Devices in UCK<br>
-Devices checked in, pending adoption<br>
-Adopted devices successfully<br>
-Updated firmware on both APs<br>
-Enabled meshing on AP02<br>
-Unplugged AP02 from network<br>
-AP02 successfully downlinking to AP01<br>
-Moved AP02 to other side of house<br>
-Connected phone to AP02<br>
-Tested Wifi Only, WAN is up<br>
-Scanned network, can see all internal devices, Trusted LAN is up<br>
-Saved UCK config<br>
-Unplugged all devices<br>
Task 3 completed</p>
<h3 class="code-line" data-line-start=83 data-line-end=84 ><a id="Task_4_83"></a>Task 4:</h3>
<p class="has-line-data" data-line-start="84" data-line-end="92">Install APs<br>
-Routed cables for AP01 and mounted to wall<br>
-AP01 powered on<br>
-Routed cables for AP02 and mounted to ceiling<br>
-AP02 powered on<br>
-Spun up old controller<br>
-APs are checked into old controller<br>
Task 4 completed</p>
<h3 class="code-line" data-line-start=93 data-line-end=94 ><a id="Task_5_93"></a>Task 5:</h3>
<p class="has-line-data" data-line-start="94" data-line-end="103">Install Cloud Key<br>
-Plugged UCK into PoE passthrough port on Firewall<br>
-Logged into Firewall, assigned new network to interface, enabled interface<br>
-Hit new UCK IP in browser, UCK is up<br>
-Googled unifi ap set inform ip, found result (<a href="https://community.spiceworks.com/how_to/9692-manually-setting-the-controller-address-for-a-unifi-ap">https://community.spiceworks.com/how_to/9692-manually-setting-the-controller-address-for-a-unifi-ap</a>)<br>
-Opened Terminal on each AP<br>
-Ran command $set-inform http://&lt;newIP&gt;:8080/inform<br>
-AP01 checked into UCK<br>
-AP02 checked into UCK</p>
<h3 class="code-line" data-line-start=104 data-line-end=105 ><a id="Task_6_104"></a>Task 6:</h3>
<p class="has-line-data" data-line-start="105" data-line-end="123">Test new wifi network<br>
-Unplugged old WiFi AP and powered off<br>
-Opened laptop<br>
-Laptop has automatically authenticated with the SSID and PSK<br>
-Pinged 8.8.8.8, WAN is up<br>
-Pinged <a href="http://google.com">google.com</a>, WAN DNS is up<br>
-Pinged internal hosts, Trusted LAN is up<br>
-Browsed to pages in browser, networking appears OK<br>
-Logged into UCK<br>
-Viewed Devices, AP01<br>
-Viewed Connected Devices, all Wifi devices in this area are connected<br>
-Moved laptop upstairs<br>
-Viewed AP02 devices<br>
-Laptop has not yet migrated to AP02<br>
-Moved laptop to far end of house<br>
-Laptop has migrated to AP02<br>
-Signal strength is excellent<br>
Task 6 completed</p>
<h3 class="code-line" data-line-start=124 data-line-end=125 ><a id="Task_7_124"></a>Task 7:</h3>
<p class="has-line-data" data-line-start="125" data-line-end="129">Wrap-Up Meetings and Conclusion<br>
-Updated family on progress<br>
Task 7 completed<br>
Project closed</p>
