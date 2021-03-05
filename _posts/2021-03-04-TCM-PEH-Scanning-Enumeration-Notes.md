---
layout:     post
title:      TheCyberMentor PEH Course Scanning/Enumeration Notes
date:       2021-03-04
summary:    TheCyberMentor Practical Ethical Hacker Course, Scanning and Enumeration notes
categories: certs
thumbnail: cogs
tags:
 - certs
---
<p class="has-line-data" data-line-start="0" data-line-end="1">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=2 data-line-end=3 ><a id="ScanningEnumeration_2"></a>Scanning/Enumeration</h1>
<p class="has-line-data" data-line-start="4" data-line-end="5">&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="6" data-line-end="7">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=8 data-line-end=9 ><a id="Kioptrix_8"></a>Kioptrix</h1>
<p class="has-line-data" data-line-start="10" data-line-end="12"><a href="https://www.vulnhub.com/entry/kioptrix-level-1-1,22/">https://www.vulnhub.com/entry/kioptrix-level-1-1,22/</a><br>
it’s a vulnhub box, level 1</p>
<p class="has-line-data" data-line-start="13" data-line-end="15"><a href="https://www.abatchy.com/2017/02/oscp-like-vulnhub-vms">https://www.abatchy.com/2017/02/oscp-like-vulnhub-vms</a><br>
oscp-like vulnhub boxes</p>
<p class="has-line-data" data-line-start="16" data-line-end="18"><a href="https://www.vmware.com/uk/products/workstation-player/workstation-player-evaluation.html">https://www.vmware.com/uk/products/workstation-player/workstation-player-evaluation.html</a><br>
to download vmware workstation free</p>
<p class="has-line-data" data-line-start="19" data-line-end="21"><a href="https://www.linuxlookup.com/howto/install_vmware_workstation_or_vmware_player_bundle_file">https://www.linuxlookup.com/howto/install_vmware_workstation_or_vmware_player_bundle_file</a><br>
instructions for installing a vmware .bundle file</p>
<p class="has-line-data" data-line-start="22" data-line-end="26">1. Download Kioptrix lvl 1<br>
2. Download and install vmware<br>
3. Unrar kioptrix lvl 1<br>
4. import into vmware, edit settings, boot</p>
<p class="has-line-data" data-line-start="27" data-line-end="30">currently 192.168.0.71<br>
arp scan<br>
sudo arp-scan -l</p>
<p class="has-line-data" data-line-start="31" data-line-end="33">netdiscover<br>
sudo netdiscover -r 192.168.0.0/24</p>
<p class="has-line-data" data-line-start="34" data-line-end="35">&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="36" data-line-end="37">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=38 data-line-end=39 ><a id="Nmap_38"></a>Nmap</h1>
<h2 class="code-line" data-line-start=40 data-line-end=41 ><a id="scanning_with_nmap_40"></a>scanning with nmap</h2>
<p class="has-line-data" data-line-start="41" data-line-end="45">losers call it “network mapper”<br>
stealth scanning (-sS) is by default<br>
it used to be stealthy, but any IDS will pick it up<br>
it’s trying to make a connection via SYN -&gt;, then loljk RST -&gt;</p>
<p class="has-line-data" data-line-start="46" data-line-end="48">sudo nmap -p- -A<br>
scans all ports (-p-) with aggressive scan (-A) but you already know this</p>
<h2 class="code-line" data-line-start=49 data-line-end=50 ><a id="lets_do_some_diffs_49"></a>let’s do some diff’s</h2>
<h3 class="code-line" data-line-start=50 data-line-end=51 ><a id="nmap_192168071_50"></a>nmap 192.168.0.71</h3>
<p class="has-line-data" data-line-start="51" data-line-end="60">output:<br>
Not shown: 994 closed ports<br>
PORT STATE SERVICE<br>
22/tcp open ssh<br>
80/tcp open http<br>
111/tcp open rpcbind<br>
139/tcp open netbios-ssn<br>
443/tcp open https<br>
1024/tcp open kdm</p>
<h3 class="code-line" data-line-start=61 data-line-end=62 ><a id="nmap_p_192168071_61"></a>nmap -p- 192.168.0.71</h3>
<p class="has-line-data" data-line-start="62" data-line-end="71">output:<br>
Not shown: 65529 closed ports<br>
PORT STATE SERVICE<br>
22/tcp open ssh<br>
80/tcp open http<br>
111/tcp open rpcbind<br>
139/tcp open netbios-ssn<br>
443/tcp open https<br>
1024/tcp open kdm</p>
<h3 class="code-line" data-line-start=72 data-line-end=73 ><a id="nmap_sV_192168071_72"></a>nmap -sV 192.168.0.71</h3>
<p class="has-line-data" data-line-start="73" data-line-end="82">output:<br>
Not shown: 994 closed ports<br>
PORT STATE SERVICE VERSION<br>
22/tcp open ssh OpenSSH 2.9p2 (protocol 1.99)<br>
80/tcp open http Apache httpd 1.3.20 ((Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)<br>
111/tcp open rpcbind 2 (RPC #100000)<br>
139/tcp open netbios-ssn Samba smbd (workgroup: MYGROUP)<br>
443/tcp open ssl/https Apache/1.3.20 (Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b<br>
1024/tcp open status 1 (RPC #100024)</p>
<h3 class="code-line" data-line-start=83 data-line-end=84 ><a id="nmap_A_192168071_83"></a>nmap -A 192.168.0.71</h3>
<p class="has-line-data" data-line-start="84" data-line-end="124">output:<br>
Not shown: 994 closed ports<br>
PORT STATE SERVICE VERSION<br>
22/tcp open ssh OpenSSH 2.9p2 (protocol 1.99)<br>
| ssh-hostkey:<br>
| 1024 b8:74:6c:db:fd:8b:e6:66:e9:2a:2b:df:5e:6f:64:86 (RSA1)<br>
| 1024 8f:8e:5b:81:ed:21:ab:c1:80:e1:57:a3:3c:85:c4:71 (DSA)<br>
|_ 1024 ed:4e:a9:4a:06:14:ff:15:14:ce:da:3a:80:db:e2:81 (RSA)<br>
|<em>sshv1: Server supports SSHv1<br>
80/tcp open http Apache httpd 1.3.20 ((Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)<br>
| http-methods:<br>
|</em> Potentially risky methods: TRACE<br>
|_http-server-header: Apache/1.3.20 (Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b<br>
|<em>http-title: Test Page for the Apache Web Server on Red Hat Linux<br>
111/tcp open rpcbind 2 (RPC #100000)<br>
| rpcinfo:<br>
| program version port/proto service<br>
| 100000 2 111/tcp rpcbind<br>
| 100000 2 111/udp rpcbind<br>
| 100024 1 1024/tcp status<br>
|</em> 100024 1 1024/udp status<br>
139/tcp open netbios-ssn Samba smbd (workgroup: yMYGROUP)<br>
443/tcp open ssl/https Apache/1.3.20 (Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b<br>
|_http-server-header: Apache/1.3.20 (Unix) (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b<br>
|_http-title: 400 Bad Request<br>
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=–<br>
| Not valid before: 2009-09-26T09:32:06<br>
|_Not valid after: 2010-09-26T09:32:06<br>
|<em>ssl-date: 2021-02-03T04:56:14+00:00; +1h01m49s from scanner time.<br>
| sslv2:<br>
| SSLv2 supported<br>
| ciphers:<br>
| SSL2_RC4_128_EXPORT40_WITH_MD5<br>
| SSL2_RC4_64_WITH_MD5<br>
| SSL2_RC4_128_WITH_MD5<br>
| SSL2_DES_192_EDE3_CBC_WITH_MD5<br>
| SSL2_RC2_128_CBC_WITH_MD5<br>
| SSL2_DES_64_CBC_WITH_MD5<br>
|</em> SSL2_RC2_128_CBC_EXPORT40_WITH_MD5<br>
1024/tcp open status 1 (RPC #100024)</p>
<p class="has-line-data" data-line-start="125" data-line-end="129">Host script results:<br>
|_clock-skew: 1h01m48s<br>
|_nbstat: NetBIOS name: KIOPTRIX, NetBIOS user: &lt;unknown&gt;, NetBIOS MAC: &lt;unknown&gt; (unknown)<br>
|_smb2-time: Protocol negotiation failed (SMB2)</p>
<h3 class="code-line" data-line-start=130 data-line-end=131 ><a id="nmap_scriptvuln_192168071_130"></a>nmap --script=vuln 192.168.0.71</h3>
<p class="has-line-data" data-line-start="131" data-line-end="228">output:<br>
Not shown: 994 closed ports<br>
PORT STATE SERVICE<br>
22/tcp open ssh<br>
80/tcp open http<br>
|_http-csrf: Couldn’t find any CSRF vulnerabilities.<br>
|<em>http-dombased-xss: Couldn’t find any DOM based XSS.<br>
| http-enum:<br>
| /test.php: Test page<br>
| /icons/: Potentially interesting directory w/ listing on ‘apache/1.3.20’<br>
| /manual/: Potentially interesting directory w/ listing on ‘apache/1.3.20’<br>
|</em> /usage/: Potentially interesting folder<br>
|_http-stored-xss: Couldn’t find any stored XSS vulnerabilities.<br>
|_http-trace: TRACE is enabled<br>
111/tcp open rpcbind<br>
139/tcp open netbios-ssn<br>
443/tcp open https<br>
|_http-aspnet-debug: ERROR: Script execution failed (use -d to debug)<br>
|<em>http-csrf: Couldn’t find any CSRF vulnerabilities.<br>
|<em>http-dombased-xss: Couldn’t find any DOM based XSS.<br>
|<em>http-stored-xss: Couldn’t find any stored XSS vulnerabilities.<br>
| ssl-ccs-injection:<br>
| VULNERABLE:<br>
| SSL/TLS MITM vulnerability (CCS Injection)<br>
| State: VULNERABLE<br>
| Risk factor: High<br>
| OpenSSL before 0.9.8za, 1.0.0 before 1.0.0m, and 1.0.1 before 1.0.1h<br>
| does not properly restrict processing of ChangeCipherSpec messages,<br>
| which allows man-in-the-middle attackers to trigger use of a zero<br>
| length master key in certain OpenSSL-to-OpenSSL communications, and<br>
| consequently hijack sessions or obtain sensitive information, via<br>
| a crafted TLS handshake, aka the “CCS Injection” vulnerability.<br>
|<br>
| References:<br>
| <a href="https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224">https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224</a><br>
| <a href="http://www.cvedetails.com/cve/2014-0224">http://www.cvedetails.com/cve/2014-0224</a><br>
|</em> <a href="http://www.openssl.org/news/secadv_20140605.txt">http://www.openssl.org/news/secadv_20140605.txt</a><br>
| ssl-dh-params:<br>
| VULNERABLE:<br>
| Transport Layer Security (TLS) Protocol DHE_EXPORT Ciphers Downgrade MitM (Logjam)<br>
| State: VULNERABLE<br>
| IDs: BID:74733 CVE:CVE-2015-4000<br>
| The Transport Layer Security (TLS) protocol contains a flaw that is<br>
| triggered when handling Diffie-Hellman key exchanges defined with<br>
| the DHE_EXPORT cipher. This may allow a man-in-the-middle attacker<br>
| to downgrade the security of a TLS session to 512-bit export-grade<br>
| cryptography, which is significantly weaker, allowing the attacker<br>
| to more easily break the encryption and monitor or tamper with<br>
| the encrypted stream.<br>
| Disclosure date: 2015-5-19<br>
| Check results:<br>
| EXPORT-GRADE DH GROUP 1<br>
| Cipher Suite: TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA<br>
| Modulus Type: Safe prime<br>
| Modulus Source: mod_ssl 2.0.x/512-bit MODP group with safe prime modulus<br>
| Modulus Length: 512<br>
| Generator Length: 8<br>
| Public Key Length: 512<br>
| References:<br>
| <a href="https://weakdh.org">https://weakdh.org</a><br>
| <a href="https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000">https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000</a><br>
| <a href="https://www.securityfocus.com/bid/74733">https://www.securityfocus.com/bid/74733</a><br>
|<br>
| Diffie-Hellman Key Exchange Insufficient Group Strength<br>
| State: VULNERABLE<br>
| Transport Layer Security (TLS) services that use Diffie-Hellman groups<br>
| of insufficient strength, especially those using one of a few commonly<br>
| shared groups, may be susceptible to passive eavesdropping attacks.<br>
| Check results:<br>
| WEAK DH GROUP 1<br>
| Cipher Suite: TLS_DHE_RSA_WITH_DES_CBC_SHA<br>
| Modulus Type: Safe prime<br>
| Modulus Source: mod_ssl 2.0.x/1024-bit MODP group with safe prime modulus<br>
| Modulus Length: 1024<br>
| Generator Length: 8<br>
| Public Key Length: 1024<br>
| References:<br>
|</em> <a href="https://weakdh.org">https://weakdh.org</a><br>
| ssl-poodle:<br>
| VULNERABLE:<br>
| SSL POODLE information leak<br>
| State: VULNERABLE<br>
| IDs: BID:70574 CVE:CVE-2014-3566<br>
| The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other<br>
| products, uses nondeterministic CBC padding, which makes it easier<br>
| for man-in-the-middle attackers to obtain cleartext data via a<br>
| padding-oracle attack, aka the “POODLE” issue.<br>
| Disclosure date: 2014-10-14<br>
| Check results:<br>
| TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>
| References:<br>
| <a href="https://www.securityfocus.com/bid/70574">https://www.securityfocus.com/bid/70574</a><br>
| <a href="https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566">https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566</a><br>
| <a href="https://www.imperialviolet.org/2014/10/14/poodle.html">https://www.imperialviolet.org/2014/10/14/poodle.html</a><br>
|</em> <a href="https://www.openssl.org/~bodo/ssl-poodle.pdf">https://www.openssl.org/~bodo/ssl-poodle.pdf</a><br>
|_sslv2-drown: ERROR: Script execution failed (use -d to debug)<br>
1024/tcp open kdm</p>
<p class="has-line-data" data-line-start="229" data-line-end="248">Host script results:<br>
|<em>samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: ERROR: Server returned less data than it was supposed to (one or more fields are missing); aborting [14]<br>
| smb-vuln-cve2009-3103:<br>
| VULNERABLE:<br>
| SMBv2 exploit (CVE-2009-3103, Microsoft Security Advisory 975497)<br>
| State: VULNERABLE<br>
| IDs: CVE:CVE-2009-3103<br>
| Array index error in the SMBv2 protocol implementation in srv2.sys in Microsoft Windows Vista Gold, SP1, and SP2,<br>
| Windows Server 2008 Gold and SP2, and Windows 7 RC allows remote attackers to execute arbitrary code or cause a<br>
| denial of service (system crash) via an &amp; (ampersand) character in a Process ID High header field in a NEGOTIATE<br>
| PROTOCOL REQUEST packet, which triggers an attempted dereference of an out-of-bounds memory location,<br>
| aka “SMBv2 Negotiation Vulnerability.”<br>
|<br>
| Disclosure date: 2009-09-08<br>
| References:<br>
| <a href="http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103">http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103</a><br>
|</em> <a href="https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103">https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103</a><br>
|_smb-vuln-ms10-054: false<br>
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: ERROR: Server returned less data than it was supposed to (one or more fields are missing); aborting [14]</p>
<p class="has-line-data" data-line-start="249" data-line-end="251">is it possible to create a script which only runs -A against open ports from the -p- scan?<br>
our job is to scan for open ports, then try to exploit them.&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="252" data-line-end="253">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=254 data-line-end=255 ><a id="Enumerating_HTTPS_254"></a>Enumerating HTTP/S</h1>
<h2 class="code-line" data-line-start=256 data-line-end=257 ><a id="Enumerating_HTTP_and_HTTPS_256"></a>Enumerating HTTP and HTTPS</h2>
<h3 class="code-line" data-line-start=257 data-line-end=258 ><a id="think_about_it_from_the_POV_of_an_attacker_257"></a>think about it from the POV of an attacker</h3>
<p class="has-line-data" data-line-start="258" data-line-end="265">develop this methodology over time<br>
when you see a website, what are the basics you’re looking for?<br>
service enumeration<br>
back end directories<br>
source code<br>
vulnerability scanning<br>
any sort of information that can be divulged</p>
<h3 class="code-line" data-line-start=266 data-line-end=267 ><a id="Go_to_the_website_on_all_ports_available_80_8080_443_etc_266"></a>Go to the website (on all ports available (80, 8080, 443, etc))</h3>
<p class="has-line-data" data-line-start="267" data-line-end="271">If you see a default web page, this is an automatic FINDING<br>
Always write this up in the report as an information disclosure<br>
Tells you about the architecture and hygiene<br>
404 is an information disclosure as well if lists version, hostname, port</p>
<p class="has-line-data" data-line-start="272" data-line-end="273">Click around in the web page a little</p>
<p class="has-line-data" data-line-start="274" data-line-end="275">View source</p>
<p class="has-line-data" data-line-start="276" data-line-end="277">Perform a vulnerability scan</p>
<p class="has-line-data" data-line-start="278" data-line-end="279">Dig through the results for anything of value, enumerate and note</p>
<h2 class="code-line" data-line-start=280 data-line-end=281 ><a id="Nikto_280"></a>Nikto</h2>
<p class="has-line-data" data-line-start="281" data-line-end="286">Be wary to not get yourself blocked by a WAF, like an IPS would block nmap -sS scans<br>
nikto -h <a href="http://IP">http://IP</a><br>
anything that lists as outdated is a finding to notate on report<br>
any directories found are findings, but dirbust later for full lists<br>
look for juicy stuff (remote buffer overflow to shell) for separate exploitation phase notes</p>
<h2 class="code-line" data-line-start=287 data-line-end=288 ><a id="Dirbuster_287"></a>Dirbuster</h2>
<p class="has-line-data" data-line-start="288" data-line-end="295">dirbuster&amp;<br>
enter <a href="http://ip">http://ip</a>:port<br>
browse to /usr/share/wordlists/dirbuster/(pick one that makes sense)<br>
alternately, use /usr/share/seclists/<br>
add file extensions (php, asp, aspx) that make sense for whatever the server runs<br>
add file extensions of txt, zip, rar, pdf, docx, etc as well<br>
it will find stuff for you to investigate, of course</p>
<h2 class="code-line" data-line-start=296 data-line-end=297 ><a id="Burpsuite_intro_296"></a>Burpsuite intro</h2>
<p class="has-line-data" data-line-start="297" data-line-end="305">start foxyproxy<br>
start burp<br>
intercept one request<br>
send to Repeater<br>
edit the sent requests directly through Repeater and see response in real time (GET/POST etc)<br>
go to target<br>
set scope to each target and port<br>
check server headers for information disclosure of webserver</p>
<h2 class="code-line" data-line-start=306 data-line-end=307 ><a id="Dirb_306"></a>Dirb</h2>
<p class="has-line-data" data-line-start="307" data-line-end="308">also exists</p>
<h2 class="code-line" data-line-start=309 data-line-end=310 ><a id="Gobuster_309"></a>Gobuster</h2>
<p class="has-line-data" data-line-start="310" data-line-end="311">my fav so far&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="312" data-line-end="313">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=314 data-line-end=315 ><a id="Enumerating_SMB_314"></a>Enumerating SMB</h1>
<h2 class="code-line" data-line-start=316 data-line-end=317 ><a id="Enumerating_SMB_316"></a>Enumerating SMB</h2>
<p class="has-line-data" data-line-start="317" data-line-end="323">port 139<br>
SMB = file share<br>
Like those C:\Scans folders<br>
nmap -A will run enumeration scripts against SMB by default<br>
Try to connect to the machine to see what’s in it<br>
spin up Metasploit with $msfconsole</p>
<blockquote>
<p class="has-line-data" data-line-start="323" data-line-end="332">search smb<br>
The auxiliary/scanner||fuzzer modules are for enumeration<br>
use auxiliary/scanner/smb/smb_version<br>
show options<br>
set &lt;option&gt; for each &lt;option&gt;<br>
(Yes we all know this already, but I have to take notes)<br>
there’s &gt;show advanced options?<br>
run<br>
Paste the output into your notes</p>
</blockquote>
<p class="has-line-data" data-line-start="333" data-line-end="339">My enum4linux and/or smbclient are not working. I am receiving “Protocol negotiation failed: NT_STATUS_IO_TIMEOUT”. How do I resolve?<br>
<strong>Resolution</strong>:<br>
On Kali, edit /etc/samba/smb.conf<br>
Add the following under global:<br>
client min protocol = CORE<br>
client max protocol = SMB3</p>
<h3 class="code-line" data-line-start=340 data-line-end=341 ><a id="smbclient_340"></a>smbclient</h3>
<p class="has-line-data" data-line-start="341" data-line-end="350">Smbclient to connect via anonymous shares if available<br>
Because you never know what you’ll find until you actually look<br>
#smbclient -L \\&lt;ip&gt;\<br>
-L lists shares<br>
press enter at password prompt<br>
then look at the shares listed<br>
#smbclient \\&lt;ip&gt;\&lt;the shares listed$&gt;<br>
So for SMB sometimes you get lucky, sometimes you don’t<br>
But SMB is always important for AD stuff later&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="351" data-line-end="352">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=353 data-line-end=354 ><a id="Enumerating_SSH_353"></a>Enumerating SSH</h1>
<h2 class="code-line" data-line-start=355 data-line-end=356 ><a id="Enumerating_SSH_355"></a>Enumerating SSH</h2>
<p class="has-line-data" data-line-start="356" data-line-end="358">Copy and paste your nmap scan ssh info into exploit-db<br>
Basically as soon as you test a single ssh login, you’re actively in exploit phase</p>
<p class="has-line-data" data-line-start="359" data-line-end="367">Problem:<br>
Unable to negotiate… No matching key exchange method found. Their offer: &lt;keyexchoffer&gt;<br>
Solution:<br>
#ssh &lt;ip&gt; -oKexAlgorithms=+&lt;keyexchoffer&gt;<br>
Unable to negotiate…No matching cipher found. Their offer: &lt;ciper&gt;<br>
#ssh &lt;ip&gt; -oKexAlgorithm=+&lt;keyexchoffer&gt; -c &lt;cipher&gt;<br>
Now you can connect<br>
I’ve run into this on htb often.</p>
<p class="has-line-data" data-line-start="368" data-line-end="369">That’s it for ssh.&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="370" data-line-end="371">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=372 data-line-end=373 ><a id="Researching_Vulnerabilities_372"></a>Researching Vulnerabilities</h1>
<h2 class="code-line" data-line-start=374 data-line-end=375 ><a id="Researching_Vulnerabilities_374"></a>Researching Vulnerabilities</h2>
<p class="has-line-data" data-line-start="375" data-line-end="376">Identifying and researching potential vulnerabilities</p>
<p class="has-line-data" data-line-start="377" data-line-end="384">Take all the scan output results you’ve pasted into notes<br>
Target the low hanging fruit:<br>
port 80<br>
port 443<br>
port 139<br>
other web ports<br>
port 22</p>
<p class="has-line-data" data-line-start="385" data-line-end="388">If you see potential RCE anything, that’s the juciest fruit to start with.<br>
But be thorough.<br>
Look up vulnerabilities, take notes, move on.</p>
<p class="has-line-data" data-line-start="389" data-line-end="393">Notes vuln syntax:<br>
port# - “Potentially vulnerable to &lt;vuln name&gt;” (links)<br>
port# - “Potentially vulnerable to &lt;vuln name&gt;” (links)<br>
port# - “Potentially vulnerable to &lt;vuln name&gt;” (links)</p>
<p class="has-line-data" data-line-start="394" data-line-end="395">Check this stuff against CVE listings and add listings as subnotes.</p>
<h2 class="code-line" data-line-start=396 data-line-end=397 ><a id="Searchsploit_396"></a>Searchsploit</h2>
<p class="has-line-data" data-line-start="397" data-line-end="404">#searchsploit &lt;protocol or service&gt;<br>
You can be too specific with searchsploit<br>
#searchsploit Samba 2.2.3<br>
= No results<br>
#searchsploit Samba 2<br>
= Lots of results<br>
&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="405" data-line-end="406">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=407 data-line-end=408 ><a id="Notes_So_Far_407"></a>Notes So Far</h1>
<p class="has-line-data" data-line-start="409" data-line-end="421">–Site<br>
----hostname<br>
------enumeration<br>
--------nmap<br>
------------port/result<br>
------------port/result<br>
--------nikto<br>
------------page/result<br>
------exploitation<br>
--------blah blah<br>
------findings<br>
------------one page per finding plus screenshots&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="422" data-line-end="423">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=424 data-line-end=425 ><a id="MassScan_424"></a>MassScan</h1>
<h2 class="code-line" data-line-start=426 data-line-end=427 ><a id="Scanning_with_Masscan_426"></a>Scanning with Masscan</h2>
<p class="has-line-data" data-line-start="427" data-line-end="432">Masscan was built to scan the entire internet really fast<br>
It’s a port scanner, built into kali2.0<br>
This is an Internet-scale port scanner. It can scan the entire Internet in under 5 minutes, transmitting 10 million packets per second, from a single machine.<br>
Its usage (parameters, output) is similar to <code>nmap</code>, the most famous port scanner. When in doubt, try one of those features – features that support widespread scanning of many machines are supported, while in-depth scanning of single machines aren’t.<br>
NOTE: masscan uses its own <strong>ad hoc TCP/IP stack</strong>. Anything other than simple port scans may cause conflict with the local TCP/IP stack.</p>
<p class="has-line-data" data-line-start="433" data-line-end="434">#masscan -p1-65535 192.168.1.50</p>
<p class="has-line-data" data-line-start="435" data-line-end="438">By default it will force -sS -Pn options<br>
–rate 1000 speeds it up considerably<br>
&lt;/div&gt;</p>
<p class="has-line-data" data-line-start="439" data-line-end="440">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=441 data-line-end=442 ><a id="Metasploit_441"></a>Metasploit</h1>
<h2 class="code-line" data-line-start=443 data-line-end=444 ><a id="Scanning_with_Metasploit_443"></a>Scanning with Metasploit</h2>
<blockquote>
<p class="has-line-data" data-line-start="445" data-line-end="452">search portscan<br>
You’re looking for the syn scanner<br>
show options and fill in requirements<br>
this is kinda slow, doesn’t give you that much detail<br>
You can multi-thread it<br>
it’s just another auxiliary module you should know you can use<br>
&lt;/div&gt;</p>
</blockquote>
<p class="has-line-data" data-line-start="453" data-line-end="454">&lt;div class=“page”&gt;</p>
<h1 class="code-line" data-line-start=455 data-line-end=456 ><a id="Nessus_455"></a>Nessus</h1>
<h2 class="code-line" data-line-start=457 data-line-end=458 ><a id="Scanning_with_Nessus_457"></a>Scanning with Nessus</h2>
<p class="has-line-data" data-line-start="458" data-line-end="459">Typically you will start a Nessus scan, then move to enumeration, then return to the Nessus scan once it’s complete</p>
<h3 class="code-line" data-line-start=460 data-line-end=461 ><a id="Download_and_Install_Nessus_460"></a>Download and Install Nessus</h3>
<p class="has-line-data" data-line-start="461" data-line-end="470">Download Nessus x64<br>
Install it<br>
Navigate to it (<a href="http://localhost:8834">http://localhost:8834</a>)<br>
It will compile plugins, which will take some time<br>
Install whichever license (Essentials)<br>
Get activation code, copy/paste<br>
Need a new activation code with each new installation if Essentials<br>
Let it finish installing<br>
Log in</p>
<h3 class="code-line" data-line-start=471 data-line-end=472 ><a id="Run_a_Scan_with_Nessus_471"></a>Run a Scan with Nessus</h3>
<p class="has-line-data" data-line-start="472" data-line-end="479">Scan &gt; Templates &gt; Basic Scan<br>
Name the scan job<br>
Enter targets to scan against<br>
Do port scan of all ports via options<br>
Set scan type<br>
Go ahead and look through the options<br>
Start scan</p>
<h3 class="code-line" data-line-start=480 data-line-end=481 ><a id="Review_Nessus_Scan_Results_480"></a>Review Nessus Scan Results</h3>
<p class="has-line-data" data-line-start="481" data-line-end="488">View results<br>
Click the Vulnerabilities tab<br>
Click Settings cog, disable Grouping<br>
View installed version, and recommended version to patch<br>
Sift through the results and report on what’s important and potential for exploitation<br>
Download nessus file<br>
There are tools to turn a .nessus file into an excel file</p>
<h3 class="code-line" data-line-start=489 data-line-end=490 ><a id="Converting_nessus_File_489"></a>Converting .nessus File</h3>
<p class="has-line-data" data-line-start="490" data-line-end="492">Into CSV<br>
<a href="https://github.com/levyjm/NessusConverter">https://github.com/levyjm/NessusConverter</a></p>
<p class="has-line-data" data-line-start="493" data-line-end="498">Into Excel 2003<br>
<a href="https://seclists.org/pen-test/2006/Apr/82">https://seclists.org/pen-test/2006/Apr/82</a><br>
You can use the XML version of the Nessus report and just directly import it<br>
into Excel (at least in 2003).<br>
In Excel:</p>
<ul>
<li class="has-line-data" data-line-start="498" data-line-end="499">Drop down the “Data” menu on the toolbar</li>
<li class="has-line-data" data-line-start="499" data-line-end="500">Point to “XML” then click “Import”</li>
<li class="has-line-data" data-line-start="500" data-line-end="501">Click “Ok” a couple times; all the results are imported nicely into Excel via SOME method, there are tools out there that do this</li>
</ul>
<h2 class="code-line" data-line-start=501 data-line-end=502 ><a id="Ungroup_the_scan_results_501"></a>Ungroup the scan results</h2>
<p class="has-line-data" data-line-start="502" data-line-end="506">-Click Scans<br>
-Click Vulnerabilities tab<br>
-Click Settings gear<br>
-Click Disable Groups</p>
<p class="has-line-data" data-line-start="507" data-line-end="510">Specifically tell the Client to review the Nessus results (excel format it)<br>
Always verify, never trust your vulnerability scanner<br>
Don’t provide a screenshot of a nessus finding, provide a screenshot of the proof.</p>
<p class="has-line-data" data-line-start="511" data-line-end="514">As a pentester, own two programs:<br>
1. Nessus<br>
2. Burpsuite&lt;/div&gt;</p>
