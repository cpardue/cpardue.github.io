---
layout:     post
title:      TCM PEH Recon Module Notes
date:       2021-01-19
summary:    TheCyberMentor's Practical Ethical Hacking Course Recon Module Notes
categories: Certs
thumbnail: jekyll
tags:
 - Certs
---

<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Passive_Information_Gathering_0"></a>Passive Information Gathering</h1>
<h2 class="code-line" data-line-start=1 data-line-end=2 ><a id="Location_Information_1"></a>Location Information</h2>
<p class="has-line-data" data-line-start="2" data-line-end="6">Thorough location recon<br>
Satellite images<br>
Drone recon<br>
Building layout (badge readers, break areas, security, fencing)</p>
<h2 class="code-line" data-line-start=6 data-line-end=7 ><a id="Job_Information_6"></a>Job Information</h2>
<p class="has-line-data" data-line-start="7" data-line-end="10">Thorough personnel recon<br>
Employees (names, job title, phone number, managers, etc)<br>
Pictures (badge photos, desk photos, computer photos, etc)</p>
<h2 class="code-line" data-line-start=11 data-line-end=12 ><a id="WebHost_note_most_of_this_uhis_active_not_passive_11"></a>Web/Host (note, most of this uh…is active, not passive)</h2>
<p class="has-line-data" data-line-start="12" data-line-end="21">target validation via:<br>
WHOIS, nslookup, dnsrecon<br>
finding subdomains via:<br>
google fu, dig, nmap, sublist3r, builtwith, netcat<br>
Fingerprinting via:<br>
nmap, wappalyzer, whatweb, builtwith, netcat<br>
Data breaches via:<br>
haveibeenpwned, breach-parse, weleakinfo<br>
data breaches is his most used method of gaining access these days by far</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="hunterio_0"></a><a href="http://hunter.io">hunter.io</a></h1>
<p class="has-line-data" data-line-start="1" data-line-end="3">is a website, realistically used quite a bit<br>
this is the first stop</p>
<ol>
<li class="has-line-data" data-line-start="3" data-line-end="4">sign in</li>
<li class="has-line-data" data-line-start="4" data-line-end="5">dashboard</li>
<li class="has-line-data" data-line-start="5" data-line-end="6">enter domain &amp; search</li>
<li class="has-line-data" data-line-start="6" data-line-end="12">lists findings and “Most common patter:”<br>
Most common pattern for my target is output as {f}{last}@domain<br>
Also listed job titles as found<br>
Also lists source each email was pulled from<br>
CAN EXPORT TO CSV!!!<br>
This already looks extremely useful for osint!</li>
</ol>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Breachparse_0"></a>Breach-parse</h1>
<p class="has-line-data" data-line-start="1" data-line-end="6"><a href="https://github.com/hmaverickadams/breach-parse">https://github.com/hmaverickadams/breach-parse</a><br>
it’s just a crappy script he made, requires downloading a 44GB data dump of breaches<br>
can possibly use this<br>
see <a href="https://www.troyhunt.com/the-773-million-record-collection-1-data-reach/">https://www.troyhunt.com/the-773-million-record-collection-1-data-reach/</a><br>
bigger version of breach collections at <a href="https://raidforums.com/Thread-Collection-1-5-Zabagur-AntiPublic-Latest-120GB-1TB-TOTAL-Leaked-Download">https://raidforums.com/Thread-Collection-1-5-Zabagur-AntiPublic-Latest-120GB-1TB-TOTAL-Leaked-Download</a></p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="theharvester_0"></a>theharvester</h1>
<p class="has-line-data" data-line-start="1" data-line-end="4">built into kali, so useful<br>
some of the data sources need API keys<br>
it’s not GREAT but it works and is always there</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Web_Information_Gathering_0"></a>Web Information Gathering</h1>
<p class="has-line-data" data-line-start="1" data-line-end="2">Scraping the domain is not enough, need all subdomains as well</p>
<p class="has-line-data" data-line-start="3" data-line-end="9">SubList3r<br>
apt install sublist3r<br>
sublist3r -h to get syntax<br>
sublist3r -d domain<br>
Will pick up 4th level subdomains without using any recursive flags<br>
If it’s slow then -t &lt;n&gt; where &lt;n&gt; = threads</p>
<p class="has-line-data" data-line-start="10" data-line-end="13"><a href="https://crt.sh">https://crt.sh</a><br>
*.domain<br>
This will enumerate all the certs, nice for subdomain enumeration</p>
<p class="has-line-data" data-line-start="15" data-line-end="19">OWASP Amass<br>
Install instructions at <a href="https://github.com/OWASP/Amass">https://github.com/OWASP/Amass</a><br>
Will do a lot more than Sublist3r<br>
It’s what bug bounty guys use</p>
<p class="has-line-data" data-line-start="20" data-line-end="23">tomnomnom’s http probe<br>
<a href="https://github.com/tomnomnom/httprobe">https://github.com/tomnomnom/httprobe</a><br>
Takes a list of domains (like Sublist3r output) and probes for alive/dead</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="What_its_built_with_0"></a>What it’s built with</h1>
<p class="has-line-data" data-line-start="1" data-line-end="2">You never know where a vulnerability will be</p>
<p class="has-line-data" data-line-start="3" data-line-end="5"><a href="https://builtwith.com">https://builtwith.com</a><br>
Enter domain and search</p>
<p class="has-line-data" data-line-start="6" data-line-end="13">Wappalyzer<br>
Webapp analyzer Firefox plugin<br>
Search Firefox extensions for Wappalyzer, add<br>
Go to domain<br>
Click the plugin, accept<br>
Will give you an immediate short listing of stuff<br>
WAPPALYZER IS ACTIVE RECON</p>
<p class="has-line-data" data-line-start="14" data-line-end="17">Whatweb<br>
whatweb &lt;url&gt;<br>
Will give even more info on version info</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Burp_Suite_0"></a>Burp Suite</h1>
<p class="has-line-data" data-line-start="1" data-line-end="2">A web proxy, you know</p>
<h1 class="code-line" data-line-start=4 data-line-end=5 ><a id="Initial_setup_4"></a>Initial setup</h1>
<p class="has-line-data" data-line-start="5" data-line-end="13">Start burp<br>
Open firefox<br>
Goto Preferences, Settings, enter 127.0.0.1:8080 for the Proxy<br>
Uh use FoxyProxy extension instead<br>
Go to <a href="https://burp">https://burp</a><br>
Allow cert permanently<br>
Click on CA Certificate, save<br>
Go back into Firefox Preferences, Privacy and Security, View Certificates, Import, select the CA Cert, Open, check both boxes, OK</p>
<h1 class="code-line" data-line-start=15 data-line-end=16 ><a id="Info_gathering_15"></a>Info gathering</h1>
<p class="has-line-data" data-line-start="16" data-line-end="20">It intercepts the web requests that are sent to and from webservers<br>
Target tab shows intercepted traffic, including linked API traffic and web plugins<br>
Use this info to enumerate the website by clicking through responses<br>
BurpSuite Pro is $399 per year, btw</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Google_Fu_0"></a>Google Fu</h1>
<p class="has-line-data" data-line-start="1" data-line-end="2">lmgtfy</p>
<p class="has-line-data" data-line-start="3" data-line-end="8">Search for “google search operators”<br>
site:&lt;term&gt; to search only within &lt;term&gt; site<br>
-&lt;term&gt; to remove &lt;term&gt; from results<br>
filetype:&lt;extension&gt; to search only for that doc type<br>
Combine these in single strings for best results</p>


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Utilizing_Social_Media_0"></a>Utilizing Social Media</h1>
<p class="has-line-data" data-line-start="2" data-line-end="5">Someone always posts employee group pics on Linkedin and Twitter<br>
Yields badge photos for fake badge models<br>
Yields hardware/software photos for intel</p>
<p class="has-line-data" data-line-start="6" data-line-end="8">Linkedin always yeilds people and positions<br>
Can scrape Linkedin Company People to generate list of emails by email format (f)(lastname)@(domain)</p>
