---
layout:     post
title:      CEH v10 Study Notes Quick Review
date:       2019-12-05
summary:    A raw dump of my CEH v10 quick-review notes txt
categories: Certs
thumbnail: jekyll
tags:
 - Certs
---

*def-in-depth layers ACLs==Application Layer

atkr captured emails w/Network Sniffer

victm<-station->svr==Man-in-the-Middle

DNS+CACHE==POISONING

adverts==Adware

*DDOS+SERVICE==Application Layer

passwd atk+HASH+probability+"same is used for other keys"==Birthday Attack

!chnged login creds==Default Passwords and Settings

multi svrs+multi passwds+"overcome"==Single Sign-On (SSO)

PKI+issue&verify==Certificate Authority

inspct+alrt==Intrusion Detection System(IDS)

blck websites==Internet Content Filter

*access on behalf of others==Implicit Authorization

analyze pckts==Network Protocol Analyzer

colored part of eye==Iris Scanning

*packet filtering+firewall==Network Layer

no redundancy==RAID 0

RAID 0==pure striping

S/MIME==RSA

PGP==Application Layer

*VPN+offices+third-party==Hub-and-Spoke

cmposite sig-based anlsys=="Multiple packet analysis required to detect attack signatures"

composite:noun: a thing made up of several parts

blood vssls back of eyes==Retina Scanner

detect+deflect+study==Honeypot

*fw+tcp handshke+dtrmn sess'n legitmcy==Circuit Level Gateway

*disble services+Debianflav==update-rc.d -f <service> remove

cardholder==PCI

TCP.flags==0x000==NULL scan attempts

*tcp.flags==0x003==Syn/Fin DDOS attempts

*actions for patching the vulns==Remediation Phase

IPsec Transport Mode crypts the PAYLOAD

*same Name&SSID==Ad Hoc

encrypts entire communication including...==TACACS+

*default deny==Prudent Policy

cloud privacy==ISO/IEC 27018

*intnt'l FI prosecuted under GLBA

*EFS could only encrypt files that follow NTFS

*false pos rate==False Positive/False Positive+True Negative

SYMMETRIC (32braids)

=========

 3des*

 2fish

 Blowfish

 Rc*

 Aes*

 Idea

 Des*

 Serpent

ASYMMETRIC (deerqp)

==========

 Diffie helmen

 Ecliptic curve

 Elgamal

 Rsa

 Quantum

 Pki/pgp

 

 Wireless Encryption read here

 WEP -> FAST -> RC4 -> 24 BIT IV's

 WPA -> RC4 -> TKIP -> 48 BIT IV's

 WPA2 -> AES -> CCMP -> 128 BIT

 

nmap 192.168.0-50

nmap 192.168.0.1-254

nmap <ip> -p 1-65535

nmap -sS tcp syn

nmap -sF tcp fin 

nmap -sX xmas scan FUP(FinUrgPsh)

nmap -sT tcp connect (most reliable)

nmap -sU udp (icmp error comes back if port closed)

nmap -T0-5 urgency rating, slow to insane == 0 to 5

nmap -F fastscan(100ports)

nmap -oX output to xml

nmap -O os guess

nmap -sV service vers guess

nmap -sI idle scan

 

OS fingerprinting == the various ways OS' implement the stack

detecting attacker on dif subnet == direct TTL probes technique

rootkit before virtual machine == hypervisor level rootkit

steganography in freq domain == transform domain technique

lawful intercept (law enforcement) == hides from all but most privileged users

passive sniffing == through a hub

54Mbps == 802.11a

WEP Tkip changes for ea. 10,000 packets

half-open == Syn Stealth

website defaced through a DNS cache poisoning

RSA has integer multiplication, 4-bit working registers

SHA512 == 128-bit

buffer overflow + evade IDS == replace NOPS with equivalent f()'s

host public facing servers with Screened Subnet

evasion+TTL == Insertion Attack

detected honeypot == all services deny handshake

virus+modifies directory links == cluster virus

WEP IV's == 24-bit

unsolicited transmissions for bluejacking (steal connection)

UNIX enumeration == #showmount

IDS+fixed behavior of users == anomaly based

