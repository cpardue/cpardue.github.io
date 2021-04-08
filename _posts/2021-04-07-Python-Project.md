---
layout:     post
title:      Python Project
date:       2021-04-07
summary:    Python Project Basic Update
categories: projects
thumbnail: cogs
tags:
 - python
 - projects
 - homelab
---
## Python Project  
Everyone and there mother says I need to know Python.  
So, after going through the buffer overflow portion of the course I'm in, I'm taking a break to automate recon scans with python.  
Below is theunfinished code and where I'm having issues so far.  
I wrote out basic pseudocode and have been writing out the program from the pseudocode.  
Once the initial 4 scans work and create output files properly, then I'll break each scan into a function and just call the functions.  
  
{% highlight ruby %}
################################################
####### Written by Chris Pardue in 2021 ########
########## https://cpardue.github.io ###########
############## Written in Python3 ##############
################################################
# !#/bin/python3 # wrong path, not universal for others

# import stuff
import os, sys

# clear the terminal
os.system("clear")

# initial declarations and housekeeping

# check for root
# This script must be run as root!
if not os.geteuid()==0:
    sys.exit('This script must be run as root!')

# ask user for input_folder_name
# assign input_folder_name to folder_name
print("I need to create a folder in this dir for scan results...")
folder_name = input("Enter a folder name :")
print(folder_name)

# ask user for input_target_IP
# assign input_target_IP to target_IP
print("I need a target IP address to scan...")
target_IP = input("Enter target IP : ")
print(target_IP)

# functions

# function make_folder()
# mkdir ./folder_name
os.system("mkdir " + folder_name)
# echo "...created folder_name to house scan results."
print("...Created folder " + folder_name + " to hold scan results.")



# function nmap_first()
# touch ./folder_name/1_nmap_sweep.txt
os.system("touch ./" + folder_name + "/1_nmap_sweep.txt")
# echo "created file 1_nmap_sweep.txt within folder_name"
print("...Created 1_nmap_sweep.txt within " + folder_name)
# echo "starting initial nmap scan and dumping into 1_nmap_sweep.txt"
print("Starting initial nmap scan and dumping into 1_nmap_sweep.txt...")
# nmap -Pn -p- -oN ./folder_name/1_nmap_sweep.txt target_IP
nmap_sweep = os.system("nmap -Pn -p- -T5 -oN ./" + folder_name + "/1_nmap_sweep.txt " + target_IP)
print(nmap_sweep) # print out the ongoing scan
print("...DONE!\n") # when above is done, print done and move on
# pull ports marked open, create either a new file or an array open_ports
print("Processing open ports into open_ports.txt...")
get_ports_open = os.system("cat " + folder_name + "/1_nmap_sweep.txt | grep ^[:0-9:] | grep open | awk '{print $1}' | sed 's/\/tcp//g' > ./ports_open.txt")
print(get_ports_open)
print("...DONE!\n")
{% endhighlight %}
Note: The above is performing an all port scan at default speed.  The idea is that it's much faster to do a non-aggressive scan of all ports, then do an aggressive scan of only the open ports.  This way, I don't have to worry about missing something in a fast scan, which most certainly WILL miss something.  
{% highlight ruby %}
# pull ports marked open and http, and create either a new file or an array http_ports
print("Processing open http ports into ports_http.txt...")
get_ports_http = os.system("cat " + folder_name + "/1_nmap_sweep.txt | grep ^[:0-9:] | grep http* | awk '{print $1}' | sed 's/\/tcp//g' > ./ports_http.txt")
print(get_ports_http)
print("...DONE!\n")
# when done, print ...DONE
{% endhighlight %}
The above lines use bash to pull open ports from the above nmap scan, create a separate textfile of the open ports, then do the same for the open http ports.  
{% highlight ruby %}
# function nmap_second()
# touch ./folder_name/2_nmap_scan.txt
os.system("touch ./" + folder_name + "/2_nmap_scan.txt")
# echo "created file 2_nmap_scan.txt within folder_name"
print("...Created file 2_nmap_scan.txt within " + folder_name)
# echo "starting second nmap scan and dumping into 2_nmap_scan"
print("Starting second nmap scan and dumping into 2_nmap_scan...")
# nmap -Pn -A --script=vuln -p open_ports -oN ./folder_name/2_nmap_scan.txt target_IP
nmap_scan = os.system("nmap -Pn -A --script=vuln -p $(tr '\n' , <./ports_open.txt) -oN ./" + folder_name + "/2_nmap_scan.txt " + target_IP)
print(nmap_scan) # print out the ongoing scan
print("...DONE!\n") # when above is done, print done and move on
# when done, print ...DONE
{% endhighlight %}
Above performs an aggressive nmap scan on only the open ports, also runs the vulnerability scan on said ports, then outputs to a separate file.  
{% highlight ruby %}
# function gobuster()
# for loop for each http port in ports_http.txt until ports_http.txt endline
ports_http = open("ports_http.txt","r")
# Repeat for http port in open_http.txt
for line in ports_http:
    # touch ./folder_name/3_gobuster_scan.txt
    os.system("touch ./" + folder_name + "/3_gobuster_scan_" + line + ".txt")
    # echo "created file 3_gobuster_scan.txt within folder_name"
    print("...Created file 3_gobuster_scan_" + line + ".txt within " + folder_name)
    # echo "starting gobuster scan and dumping into 3_gobuster_scan"
    print("Starting gobuster directory scan of " + line + " and dumping into 3_gobuster_scan_" + line + "...")
    # gobuster dir -w /usr/share/seclists/Discovery/Web-Content/big.txt -u target_IP > ./folder_name/3_gobuster_scan.txt
    gobuster_scan = os.system("gobuster dir -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://" + target_IP + ":" + line + " > 3_gobuster_scan_*.txt")
    print(gobuster_scan)
    print("...DONE!\n")
    os.system("mv ./3_gobuster_scan_*.txt " + folder_name + "/3_gobuster_scan_" + line + ".txt")
    # when done, print ...DONE
ports_http.close() # end gobuster scans
{% endhighlight %}
The above block does a gobuster scan on each http port and SHOULD output the results to a separate file, but I can't get that part to work in the way that I'm approaching it.  
{% highlight ruby %}
# function wfuzz()
ports_http = open("ports_http.txt","r")
# Repeat for http port in open_http.txt
for line in ports_http:
    # touch ./folder_name/4_wfuzz_scan.txt
    os.system("touch ./" + folder_name + "/4_wfuzz_scan_" + line + ".txt")
    # echo "created file 4_wfuzz_scan.txt within folder_name"
    print("...Created file 4_wfuzz_scan_" + line + ".txt within " + folder_name)
    # echo "starting wfuzz scan and dumping into 4_wfuzz_scan.txt"
    print("Starting wfuzz subdomain scan and dumping into 4_wfuzz_scan_" + line + ".txt...")
    # wfuzz -c -f sub-fighter -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u 'http://target_IP' -H "Host: FUZZ.target_IP" > ./folder_name/4_wfuzz_scan.txt
    wfuzz_scan = os.system("wfuzz -c -f sub-fighter -w /usr/share/seclists/Discovery/DNS/subdomains-topmillion-110000.txt -u \'http://" + target_IP + ":" + line + "\' -H \"Host: FUZZ." + target_IP + "\"")
    print(wfuzz_scan)
    print("...DONE!\n")
    os.system("mv ./sub-fighter ./" + folder_name + "/4_wfuzz_scan_" + line + ".txt")
    # when done, print ...DONE
ports_http.close() # end wfuzz scans
{% endhighlight %}
The above performs a wfuzz subdirectory scan of each open http port, and SHOULD output the results to a separate file but again, this method I'm using is not working.  The files are created and named for each port, but they are empty.
{% highlight ruby %}
# clean up the root folder ports_open.txt and ports_http.txt
print("Cleaning up files...")
# cleanup = os.system("rm -rf ports_*.txt")
# print(cleanup)
# print("...DONE!\n")
# function report()
{% endhighlight %}
This last bit is cleanup, a vague notion that I believe I should be doing either as the program steps through, or all at the end.  Right now it's commented out so that I can see what's going on.  
  
  The script needs a lot of work, still.  I'm hoping that as I fumble my way through figuring out how to save scan output to a textfile, I will also have stumbled upon some other python-ish solutions for more of what is now basically just bash commands.  We'll see.  
