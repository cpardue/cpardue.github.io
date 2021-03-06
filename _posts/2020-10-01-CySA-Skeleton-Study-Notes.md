---
layout:     post
title:      CySA Skeleton Study Notes
date:       2020-10-01
summary:    A dump of the skeleton notes I jotted down while going through CySA training video course
categories: Certs
thumbnail: jekyll
tags:
 - Certs
---

cysanotes.txt purpose: jotted down notes to reference and look into based on the ItProTv CySA training course I underwent

## DAY ZERO

theharvester -d <domain> -b <bing,google,linkedin>

shodan search: org:<domain>

shodan: google dorking terms

STIX structured threats information expression

STIX is json method of sharing CTI

CTI cyber threat intelligence

TAXII trusted automated exchange of intelligence information

TAXII is a rest API to transfer http STIX CTI

oasis-open.github.io

SDO STIX Domain Objects are general domains of CTI

SRO TIX Relational Objects relate events and correlation between SROs

openIoC

openIoC is xml formatted

openIoC editor

MISP malware information sharing project

misp-project.org/features

intelligence management: STAXX, IBM X-Force Exchange

confidence levels

admiralty code

FM 2-22.3 HUMINT manual contains admiralty code table

CIA estimative language

known vs unknown vs known knowns vs unknown unknowns vs known unknowns vs unknown knowns

it's a 0-day until patch day n+1

portswigger has a 0day blog

APT

fireeye.com/apt-group.html

intelligence cycle phases

IR's intelligence requirements

cd /var/log && sudo grep -i "fail" *.log | grep "password"

## DAY ONE

commodity malware vs more advanced targetted malware

ISACs information sharing analysis centers

h-isac.org

fs-isac.com

a-isac.com

cisecurity.org/ms-isac

cisecurity.org/ei-isac

cisa.gov/critical-infrastructure-sectors

PSTN-based networks

attack frameworks

cyber kill chain lockheed martin

lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html

mitre att&ck matrix for enterprise

attack.mitre.org

mitre att&ck matrix navigator

diamond model of intrusion analysis

https://apps/dtic.mil/dtic/tr/fulltext/u2/a586960.pdf

####advers######

#infra<->capab##

#####vict#######

talos intelligence reputation center

talosintelligence.com/reputation_center

IoC

ioc + behavioral ttp + reputational ttp

modeling per the following

model adversary capability

mitre adversary capabilities

model total attack surface

model attack vectors of cyber, human, physical

model impact

model likelihood

WAF vs IDS,IPS

attribution

vulnerability management

emerging threats

asset criticality prioritization

nessus essentials version

openvas virtual appliance

qualys

baseline gathering

fluke networks, solarwinds, nagios

big 4 baselines, cpu | networking | memory | storage

mod security 3.0

webknight

setting up vulnerability feeds on vuln scanners

scoping the vuln scanner

scan types

credentialed vs non-credentialed

## DAY TWO

remediation inhibitors on legacy vs proprietary vs degraded functionality etc

owasp zed attack proxy free

burpsuite > extender tab > bapp store tab > tons of extensions!

nikto -h <url> -o outfile.htm

arachni (windows app)

nessus 

enum4linux -a <ip>

hydra -L ./userlist.txt -p <testpassword> -f ssh://<ip>

openvas

qualis

static analysis (code review)

https://samate.nist.gov/index.php/source_code_security_analyzers.html

above has GIANT list of binary scanners!!!

dynamic analysis 

immunity debugger

stress testing

reverse engineering

disassemblers 

ghidra

ida pro if you can hack it

fuzzing types

app ui

protocol 

file format

dumb and smart fuzzers

burp method: enter AAAA in field, locate request in burp, send to intruder, select field, select payload file (sql injection file), attack, view results

nmap

nmap -sn (host scan)

nmap -T4 (faster than default t3)

nmap -n (no dns)

nmap -sS is a default

nmap -sC (script scanning)

hping3

hping3 -1 (icmp mode)

hping3 -8 (--scan) known -S <host>

responder

llmnr poisoning

responder -I eth0 -v (now it's listening for events)

## DAY THREE

reaver for WPS attacks

aircrack-ng suite

airmon-ng to mon

airodump-ng to cap wpa handshake

aireplay-ng to deauth/speed above

pass the resultant .cap through cap2hccapx

pass the resultant .hccapx into hashcat

cloud assessment stuff

scoutsuite (github)

prowler (github)

pacu (github)

mobile stuff

byod tribbles

lack of encryption controls and remote wipe

legal issues if byod is the compromised device, can't forensics it

typical android thirdparty sideloaded attacks

same thing on ios

mdm hardware device features

emm more granular software control

uem unified more granular still

vmware airwatch

mobileiron

## DAY FOUR

PLC's

buffer overflows in hardcoded systems

hard codes creds in hardcoded systems

these are often just web interfaces for control

https://www.forescout.com/company/blog/vulnerabilities-in-building-automation-systems/

CANBUS controller area network bus

CANBUS is like an old ethernet hub

gain access to the CAN, you have access to the system

controller systems

ICS industrial control systems

DCS distributed control systems

SCADA supervisory controls and data aquisitions

HMI human machine interface

a lot of these are embedded proprietary interfaces, even though they use standards

MODBUS

these are often just web interfaces for control

http://dione.lib.unipi.gr/xmlui/bitstream/handle/unipi/11394/Evangeliou_1508.pdf?sequence=1&isAllowed=y

iot

embedded os vulnerabilities

system-on-a-chip

little embedded windows 7 devices are open to eternalblue, etc

https://www.vdoo.com/blog/vdoo+has-found-major-vulnerabilities-in-foscam-cameras

field-programmable gate arrays!

starbleed bug

CVSS 3.1 metrics

CVSS common vuln scoring system

0 = none

0.1+ = low

4.0+ = medium

5.0+ = medium

6.0+ = medium

7.0+ = high

8.0+ = high

9.0+ = critical

still have to look at likelihoods

CVSS base metrics

AV access vector

(P)hysical

(L)ocal (shell)

(A)djacent (same broadcast domain)

(N)etwork (routed)

AC access complexity

(H)igh (A HTB player)

(L)ow (A nullbyte.com reader)

PR priviledges required

(H)igh (privesc required)

(L)ow (non priv users)

(N)one (unauthenticated)

UI user interaction

(N)one

(R)equired

Scope

(U)nchanged (cannot spread)

(C)hanged (can spread)

CIA triad

(L)ow

(M)edium

(H)igh

https://www.first.org/cvss/specification-document

## DAY FIVE

public cloud threats

multi-tenancy resource share issues, vm popping

unencrypted data crossing multiple networks within the datacenter

private cloud threats

expensive enough that they will necessarilly jimp on something

aaaaaalllll support/monitoring goes to owner

private clouds turning into community clouds ie side-channel attacks due to lack of oversight

way less oversight

hybrid cloud threats

aggregation of threats of both public and private cloud infra

IAAA identity authorization authentication accounting

SaaS threats

easy to phish via fake login pages

PaaS containers

container popping

see owasp top 10 webapp for this

fast spin-up equals forgetting to change/harden defaults

permission issues via overlooking

IaaS full servers in datacenter

all infastructure threats via public IP

FaaS function as a service threats

Infrastructure as a Code threats 

terraform etc

API threats

API error/debugging enumeration

API key management threat

API key master keys | lack of rotation

API security and threats

unprotected cloud storage

xml attacks and mitigation

soap attacks <1.2

sql injection input sanitization

buffer overflow stack, heap, integer based

aslr

stack canary

dep

rce

directory traversal

privesc

password attacks

password spraying

credential stuffing

impersonation attacks vs phishing

mitm

session hijacking/replay

cross site scripting

rootkit

weak error handling or disclosing too much information

soap vulnerabilities

insecure direct object reference (modifying get request before it's received)

race conditions

https://lightningsecurity.io/blog/race+conditions/

https://cheatsheetseries.owasp.org/cheatsheets/AuthenticationCheatSheet.html

broken authentication

sensitive data exposure

insecure components

insufficient logging monitoring

weak/default configs

insecure functions

## DAY SIX

asset management

https://www.seton.com/asset+management/asset-tags.html

https://www.reftab.com

network architecture and segmentation

SDN

control plane

data plane

management plane

northbound

southbound

mininets

virtual private clouds within public cloud

VPN

serverless function-as-a-service

vlans for segmentation

jump-box to access admin consoles

IAM identity and access management

privilege mangement

DAC discretionary access control

DAC petty file perms/privs

MAC mandatory access controls

MAC role/group-based access

RBAC role based access control

ABAC attribute based access control

ABAC context/attribute based if/then rules

MFA

SSO

Federation (multi-organizational trusts)

VDI virtual desktop infrastructure

VDI non-persistence, reboot fresh from image each time

containerization

docker

docker pulling and running images

docker dockerfiles for orchestration

honeypots and active defense

active defense == trolling

intricately designed aggravations and annoyances to attackers

honeypots

opencanary

CASB cloud access security broker

CASB modes forward proxy

CASB modes reverse proxy

CASB is for huge infra and userbase

certificate management

always remove untrusted certs

don't use self-signed certs, users can't judge whether to trust a server or not

certs encrypt get/posts to prevent mitm

windows server role certificate management authority

windows cmd certutil

linux openssl

extrapolate public key from private key

ssh-keygen

ssh-keygen -t rsa -b 4096 -C user@domain

/etc/ssh/sshd_conf edits

ssh -i /path/to/key user@ip

hardware assurance

TPM

HSM hardware security modules

https://www.ncipher.com/products

antitamper 

FPGA

PUF physically uncloneable function

eFuse

trusted firmware updates

measured boot & attestation

checks hashes to ensure validity of configs

MEK media encryption key

KEK key encryption key (user's passwd)

AMD SME secure memory encryption

AMD SEV secure encrypted virtualization

Intel TXT trusted execution technology

Intel SGX software guard extension

secure enclave

secure area to store encryption information like crytpo keys

kernel and secure kernel store each other's pub/priv keys in secure enclaves to decrypt each other's comms before passing to kernel function

atomic executions

bus encryption

HDCP high definition content protection

software assurance and secure coding

SDLC

https://cheatsheetseries.owasp.org/cheatsheets/InputValidationCheat_Sheet.html

https://www.php.net/manual/en/function.htmlspecialchars.php

agile method vs waterfall method

input validation

output encoding

session management

parameterized queries

formalized methods to hunt edge case bugs

## DAY SEVEN

trend analysis

frequency against time

volume against time

statistical deviation, mean and standard deviation

baselining

metrics

alerts/detections vs alerts/response times

network metrics and host metrics

measuring user threat awareness

url analysis

[sub][hostname][resource/path][queryID]

http methods

GET resource request

PUT pushes files onto server

POST push resource data

HEAD gives common server info

? == start of query

& == value pairs

# == anchor tag

status codes 

200 GET/POST OK

201 PUT OK

300's redirect

400's client-side request invalid

500's server-side request invalid

useful attacking info

percent encoding after queryID

if ' are filtered out, try percent encoding instead

unreserved characters and reserved characters and unsafe characters

asciitable.com

can easily encode trings in burp, highlight>url encoding

dns analysis

https://www.ipvoid.com/dns+reputation/

DGA domain generation algorithm

bulletproof hosting

fast flux

https://github.com/baderj/domaingenerationalgorithms

https://www.alexa.com/topsites

dns analysis logging

look for NX domain errors

mitigation via blacklisting/whitelisting and recursive dns resolvers to fact-check other dns

## DAY EIGHT

packet analysis

basically just wireshark following tcp stream, viewing as raw when necessary

components in flow analysis

alerting for !baseline

flow collector probes

solarwinds

nagios

netflow format

https://openargus.org/using-argus

zeke/bro

MRTG multi-router traffic grapher

endpoint behavior analysis

known-good ehavior vs anomolous behavior

https://docs.microsoft.com/en-us/advanced-threat-analytics/what-is-ata

https://www.splunk.com/en_us/software/user-behavior-analytics.html

UEBA user entity behavior analysis

microsoft ATA advanced threat analytics

splunk user behavior analytics

sysinternals tools

sysinternals procmon

OSSEC is now Wazuk???

tripwire

EPP endpoint protection platforms

EDR endpoint protection response

pupy

malware analysis

they just go on about categories of malware for a while and it's annoying

dissecting malware

ida free

ghidra

packer for obfuscating

the reverse engineering is like, so effing basic in this video

log review

https://zecure.me/

https://demo.shadowd.zecure.org/login

always turn on auditing for failure and success

syslog

windows firewall does not log by default

logs located in windows\system32\logfiles\firewall\pfirewall.log

WAF logs

OSSEC log drilldown

kibana

sguil

impact analysis

components

money/reputation loss

event=<money&<rep

classification frameworks

taxonomy-based classification

impact-based classification

organizational vs localized events

immediate vs total impact

SIEM security incident and event management

OSSIM

Security Onion

kibana

rules and queries

pipes and logical operators

data enrichment, adding additional external info/threat feeds to alerts

## DAY NINE

email analysis

phishing, spearphishing, whaling, etc

BEC business email compromise

BEC legit compromised org email

view message details to see headers

body == payload

possible RCE via email client's html interpretation

SPF sender policy framework

SPF checks DNS against the sender domain

SPF <option>ALL options

SPF -ALL == reject it

SPF ~ALL == flag it

SPF +ALL == accept it

DKIM domain keys identified mail

DKIM uses PKI as checks

DKIM private key hashes

DKIM posts public key via DNS

DKIM then can go through PKI process to check mail against priv/pub and pub/priv key pair hashes for validity checks upon receiving the mail

DMARC domain based authentication reporting and conformance

file system permissions

windows file permissions

via admin cmd 

icacls <file>

icacls flags

icacles --help

linux file permissions

ls -l

drwxrwxrwx owner:group

-421421421

thus chmod 137 = ---x-wxrwx

chmod u+s sticky bits

blacklisting and whitelisting applications

blacklisting, don't-allow-these lists

whitelisting, only-allow-these lists

execution control

windows methods

SRP software restriction policies

SRP group policy controlled

applocker

applocker also via GPO

WDAC windows defender app control

WDAC code integrity policies

linux methods

apparmor

selinux

firewall configurations

multi-homed firewall

acl's

top down

left to right

implicit rules vs explicit rules

multicast/loopback traffic refered to as martians

blocking martians

reserved and r&r ranges refered to as bogons

blocking bogons

https://team+cymru.com/community-services/bogon-reference/

baseline firewall rules, basically block the hell out of internal traffic that shouldn't be coming in from WAN, ever

drop vs reject

egress filtering to screw over reverse shells

firewalking

ttl to firewall +1

firewalk tool

nmap -SA scan

data loss prevention

DLP is a data classification system, generally

office data loss prevention

DLP components

DLP policy server

DLP network agents

DLP endpoint agents

content types

structured data types

unstructured data types

remediations via alert vs block vs quarantine vs tombstone

NAC network access control

NAC filtering, physical port filtering

NAC IEEE 802.1x

PNAC port network access control

supplicant vs authenticator vs auth server

EPOL == EAPOL extensive authentication protocol over local area network

supplicant is the client pc

authenticator is some middleman

auth server is the actual man in charge

supplicant hooks to network

authenticator requests some form of authentication

authenticator passes that on to auth server

comes back down chain

NAC via health status policy checks

Windows Server NAP network access policy under Network Policy Server role

NAP is no longer current in windows server OS

time based controls

location based controls

role based controls

## DAY TEN

blackholes vs sinkholes

blackholes drop, simply do not process

cisco devices have a null 0 for routing, like /dev/null

sinkholes dump traffic into a drop box for analysis

sinkholing with DNS redirect traffic to <here>

<here> being 127.0.0.1 or an analyzing server

add known c2 domains to sinkhole server route

malware signatures

submit binaries to vendors for malware sampling

trend micro and virus total do this, there are others

malware classification

CARO computer antivirus research organization

http://www.caro.org/articles/naming.html

MAEK malware analysis enumeration and characterization

MAEK is STIX compliant

malware custom signatures

https://yara.readthedocs.io/en/v4.0.1/gettingstarted.html

------------------------------------------------------------>>>uh might be able to create yara rule to parse out php pages for bug hunting

------------------------------------------------------------>>>curl <page> && yara bugrules <page>

threat hunting components

methodology

establish a hypothesis

threat modeling

APTs that typically attack this industry

what are their TTPs and how to they attack

lean on SIEM

process analysis

analyze network traffic

marry CTI with internal logs

scripting concepts

syntax: 

declaration of binary (#!bin/bash)

contents line by line

$1 $2 $3 is a positional parameter

powershell

they just use the ise for a one liner :(

python

ruby

goland

wmic

AI artificial intelligence

can only do what it's programmed to do

ML machine learning

machine learning is a subset of AI

can learn the differentiations it is programmed to learn

can modify it's own decision making algorithms

DL deep learning

deep learning is a subset of machine learning

uses neural networks

continuous integration and continuous development

prevents stupid high level plans for how to attack issues

these methodologies have already been figured out

agile

development, test, staging, production

continuous delivery

continuous deployment

## DAY ELEVEN

incident response communication

communication bands may be compromised

have an out of bounds, unbreached line of communication

only communicate with trusted parties

mandatory legal communication

GDPR

IRP incident response plan

IR Playbook

response communication

c levels

IT management

regulatory bodies

law enforcement

HR

PR

data criticality factors

PII

PHI

SPI

Financial Information

IP

Corporate Information

High Value Asset

https://gdpr+info.eu/art-33-gdpr/

https://www.cisa.gov/sites/default/files/publications/CISAInsights+Cyber-SecureHighValueAssets_S508C.pdf

incident response phases

preparation

identification

containment

eradication

recovery

lessons learned

preparation:

have training and testing

preparation have a call escalation list

rpeparation have an incident form 

https://bok.ahima.org/PdfView?oid=76732

indentification:

event vs incident

severity classification

severity characteristics

containment:

containment is isolation

eradication:

mitigation

patching

compensating controls

hardening defficiencies

recovery:

re-imaging and validation

lessons learned:

what did right

what did wrong

how to improve each

after action report

update IR plan

summary report for c-levels

evidence retention for legal purposes

integrate the new IOC monitoring

IOC indicators of compromise

triaging incidents

networks, hosts, applications

network bandwidth

network beaconing outbound

network rogue devices

network irregular p2p

host resource consumption

host processes

host four horsemen

host log sizes

host linux pstree

host registry entries obfuscated

host scheduled tasks

application new users

application unexpected outputs

application unexpected outbound communication

application service interruption

application gaps in logs

## DAY TWELVE

digital forensics phases

identification, collection, analysis, reporting

identification

prevent contamination of the scene

ID the scope of the evidence

collection

use standard tools to collect evidence

sleuthkit

maintain chain of custody

name/date/time/stable container to maintain integrity

use repeatable methods and tools

create a report

things that become legal evidence will be siezed as evidence, be aware

digital forensics networks

save wireshark pcaps and tcpdumps

digital forensics endpoints

gather evidence from the endpoint

memory

https://www.magnetforensics.com/resources/magnet+ram-capture/

crashdumps

hibernation file

pagefiles

volitility tool

live acquisition is powered on machine

static acquisition is powered down machine

static may introduce issues or changes in state for the malware

https://tools.ietf.org/html/rfc3227

order of volitility

take images with a write blocker

dd to get identical

mobile forensics

is it locked or encrypted?

may have to use jtag

may have to take video of extraction method

vendor provided software

call data extraction

carrier provided logs

virtualization forensics

some caveats

may be able to dump live memory via hypervisor

save state files already imaged

unsupported filesystems, may be heavily fragmented

lost system logs

## DAY THIRTEEN

privacy vs security

not synonyms, there is a distinction between the two

privacy is a subsection of security

data governance for guidance and language

privacy element is outlined through data governance entities

privacy is how we control and oversee PII

governance is the mechanism of control via policy

GDPR

• The Sarbanes-Oxley Act (SOX)

• The Gramm-Leach-Bliley Act (GLBA)

• The Federal Information Security Management Act (FISMA)

• The Committee of Sponsoring Organizations of the Treadway Commission (COSO)

• The Health Insurance Portability and Accountability Act (HIPAA)

if you provide services to clients under an above umbrella, you must align with their policies

non-technical data controls

data governance and lifecycle of data

classification labels and signifies value of different types of created data

classification is attached to data to dictate how securely the data should be treated from a security standpoint

public data vs classified data

interal, official use only, secret, etc

confidentiality as preventing access to assets

confidentiality is applied according to data classification

of course integrity maintains the above

data typing for application of controls

encryption of data at each lifecycle phase

purpose limitation specifies the intended use

data minimization data should only be processed and stored to accomplish purposes A B and C

data sovereignty aligns data to geographies used in

## DAY FOURTEEN

data should have a shelf life

retention and retention policies

retention standards 

eDiscovery

storage limitation concerns hipa sox etc

ownership roles as a security control mechanism

roles of ownership

data owner is ultimate authority with how data is protected/used within the org

understand the logic of defining the role, not the individual

senior leaders as typical owner of organizational data

data stewards ensure data quality (labels, formats, compression)

data custodians handle managing and enforcing data backups etc

privacy officer has to do with purpose limitation, PII, PHI, SPI, disclosure, etc, can go to jail from legal standpoint

technical control of legal requirements

regulatory, statue, frameworks and protection levels

determined by geographics and industry, aka data sovereignty

always maintain legal counsel for data soveregnty

non-disclosure agreements as a non-technical control

NDA, SLA, OLA, MOU, ILA, RFP

NDA driven by contractual requirement landscape

there are secrecy acts dealing with gov types of data by the way

SLA stipulate between customer and provider for service coverage, uptime you are buying

ISA interconnection security agreement protects legally the protection of assets

data sharing and usage agreements for hipa and gdpr binding use of data

technical controls

the systems we can put into play to secure environment

mitigate and minimize risk

access controls 

more often than not software driven

ACL access control list

ACE access control entries

can also be physical in nature, such as doors or rfid locks

access control models

apply model of least privilege, just enough to do their specific job

ACL is a listing of all users

ACE is the specific permissions based

geographic access requirements

if not in this location then no access to that

subject requests access to object, what is location of subject and location of object determines access

safe harbor now known as privacy shield

encryption as applied to technical control

system of providing confidentiality and integrity

create integrity via hashing

encryption has to be constantly assessed vs retention policy, cannot be crackable by end of retention (sha1 crackable 7 years later

overlapping complementary controls as defense in depth

data at rest and data in transit should both be encrypted

data at rest, encrypted

data in transit, tls and ssl

encryption of session keys in memory

## DAY FIFTEEN

de-identification is basically removing PII, PHI, redacting such information so that elements cannot be identified

data masking is a possible application of de-identification such as asterisks in password fields or CC numbers

data masking should be irreversible in application

tokenization replaces data with a token, while the associated data is stored in a separate tokenization server

token string replaces the data in production database, so hackers in a db see token, as tokenized data is in other db

tokenization is just a sleight of hand where encryption can't be used from hardware or legal standpoint

aggregation/banding replaces unique values with a value range, age 21yrs-30yrs instead of age 25yrs

re-identification attack is reversal of shitty de-identification technique

K value is a value that represents how many individuals, where k-5 means 5 individuals within

DRM digital rights management

IRM individual rights management

ERM enterprise risk management

NIST SP-800-39

frame - high level context for kinds of risks being addressed and highest values acceptable, like a warning order

assess - identify risks associated with this individual risk

respond - mitigate each risk factor

monitor - evaluate effectiveness 

systems assessment process

people

tangible assets

intangible assets

procedures

MEF mission essential function

PBF primary business function

calculating and analyzing risk

• Probability is the chance of a threat being realized

• Magnitude is the impact of a successful exploit or a risk event

NOTE: Magnitude may be determined by factors such as the value of the asset or the cost of disruption if the asset is compromised.

Probability * Magnitude = Risk

therefore (decimal percentage * dollar value = risk)

100% probability that Chastain's $500 server dies and costs $1200 t&m = (1*$1700=$1700risk)

quantitative, qualitative, and semi-quantitative:

Quantitative Risk Calculation -

A quantitative assessment of risk attempts to assign concrete values to the elements of risk.

Probability is expressed as a percentage and magnitude as a cost (monetary) value, as in the following formula:

AV (Asset Value) x EF (Exposure Factor) = SLE (Single Loss Expectancy)

EF is the percentage of the asset's value that would be lost. 

The single loss expectancy (SLE) value is the financial loss that is expected from a specific adverse event. 

For example, if the asset value is $50,000 and EF is 5%, the SLE calculation is as follows:

$50,000 * .05 = $2,500

If you know or can estimate how many times this loss is likely to occur in a year, you can calculate the risk cost on an annual basis:

SLE (Single Loss Expectancy) x ARO (Annual Rate of Occurrence) = ALE (Annual Loss Expectancy)

Chastain's dead server costs $1700 every 3mo ($1700*4=SLE of $6800)

NOTE: The problem with quantitative risk assessment is that the process of determining and assigning these values can be complex and time consuming.

Qualitative analysis is generally scenario-based.

The qualitative approach seeks out people's opinions of which risk factors are significant.

Qualitative analysis uses simple labels and categories to measure the likelihood and impact of risk.

For example, impact ratings can be severe/high, moderate/medium, or low; and likelihood ratings can be likely, unlikely, or rare.

Semi-Quantitative Risk Calculation -

A semi-quantitative analysis method exists because it is impossible for a purely quantitative risk assessment to exist given that some issues defy numbers.

A semi-quantitative analysis attempts to find a middle ground between the risk analysis types to create a hybrid method.

analyzing risk

BIA business impact analysis assesses potential losses for a threat scenario

MDT maximum tolerable downtime longest that an outage can occure before irrecoverable business failure

RTO recovery time objective is amount of time it takes for service restore

WRT work recovery time is the time it takes to move from service restoration to completed validation testing and debrief

RPO recovery point objective is the amount of data loss a system can sustain, measured in time

## DAY SIXTEEN

Done
