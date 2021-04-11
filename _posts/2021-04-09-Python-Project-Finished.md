---
layout:	post
title:	Python-Project-Finished
date:	2021-04-09
summary:	Python Project Finished and notes on what solutions worked in the code.
categories:	python
thumbnail:	cogs
tags:
 - python
---
## TLDR
A few days, maybe a week ago I started trying to script out an automated recon scanner in Python 3, based off ideas I'm getting from a TheCyberMentor course.  
The idea:  
- Create one python script to somehow do the following  
- nmap scan of a host  
- Based off the results of nmap scan, do a gobuster scan of only the http ports  
- Based off the results of nmap scan, do a subdir wfuzz of only the http ports  
- Save results of each scan in a new folder, named by me, so that I can use it with HTB  
This morphed into:  
- Name a folder to dump scans into  
- Ask for a hostname/IP  
- Run two nmap scans to save time.  One -p-, then feed the open ports into a longer -A --script=vuln scan.  
- Run a gobuster scan  
- Run a wfuzz scan  
In the end I made it work.  Code below with brief explanations.  Might be useful if you're rolling your own.  

## The Code  
I made a cool splash in MOTD ASCII art format.  Script clears the terminal, then presents motd.  
{% highlight ruby %}
################################################
####### Written by Chris Pardue in 2021 ########
########## https://cpardue.github.io ###########
############## Written in Python3 ##############
################################################
# !#/bin/python3 # wrong path, not universal for others

import os
import sys
import subprocess

os.system("clear") # clear the terminal
print("#########################################################")
print("#     ______ ______ _   __ ______ ____   ____ ______    #")
print("#    / ____// ____// | / // ____// __ \ /  _// ____/    #")
print("#   / / __ / __/  /  |/ // __/  / /_/ / / / / /         #")
print("#  / /_/ // /___ / /|  // /___ / _, _/_/ / / /___       #")
print("#  \____//_____//_/ |_//_____//_/_|_|/___/_\____/____   #")
print("#    / ___/ / ____//   |   / | / // | / // ____// __ \  #")
print("#    \__ \ / /    / /| |  /  |/ //  |/ // __/  / /_/ /  #")
print("#   ___/ // /___ / ___ | / /|  // /|  // /___ / _, _/   #")
print("#  /____/ \____//_/  |_|/_/ |_//_/ |_//_____//_/ |_|    #")
print("# by cpardue circa 2021     ver1.0    cpardue.github.io #")
print("#########################################################")
print(" from github.com/cpardue/projects/tree/main/GenericRecon ")
print("\n")

{% endhighlight %}
## First Tasks
The first thing the script does is check to see if it was run as root.  If not, it exits.  It then asks you for a name for a new folder, then makes that folder.  It then asks for a hostname/ip, then saves that input as target_IP for later use.  
{% highlight ruby %}

if not os.geteuid()==0: # check for root
    sys.exit('This script must be run as root!')
print("We'll need to dump scans into a folder...")
folder_name = input("Create a new folder name :") # ask user for input_folder_name
print(folder_name)
print("We'll need a target hostname or IP to scan...")
target_IP = input("Enter target hostname or IP : ") # ask user for input_target_IP
print(target_IP)
os.system("mkdir " + folder_name) # mkdir ./folder_name
print("...Created folder " + folder_name + " to hold scan results.")

{% endhighlight %}
## Initial faster nmap scan  
The next thing the script does is start a fast all-port nmap scan and saves the output under the new folder.  At get_ports_open, it then reads the nmap scan results, greps through the following pipeline: Line starts with a number, contains the word "open", prints the first column of that line, removes "/tcp", saves to ports_open.txt.   
It then basically does the same thing for http ports.  
{% highlight ruby %}

os.system("touch ./" + folder_name + "/1_nmap_sweep.txt") # initalizing textfile, maybe not necessary
print("...Created 1_nmap_sweep.txt within " + folder_name)
print("Starting initial nmap scan and dumping into 1_nmap_sweep.txt...")
nmap_sweep = os.system("nmap -Pn -p- -T5 -oN ./" + folder_name + "/1_nmap_sweep.txt " + target_IP) # nmap all port sweep
print(nmap_sweep) # output it to the terminal
print("...DONE!\n")
print("Processing open ports into open_ports.txt...")
get_ports_open = os.system("cat " + folder_name + "/1_nmap_sweep.txt | grep ^[:0-9:] | grep open | awk '{print $1}' | sed 's/\/tcp//g' > ./ports_open.txt") # filter scan by lines staring with #'s, then "open", then print first column, then remove "/tcp", place in new file
print(get_ports_open)
print("...DONE!\n")
print("Processing open http ports into ports_http.txt...")
get_ports_http = os.system("cat " + folder_name + "/1_nmap_sweep.txt | grep ^[:0-9:] | grep http | awk '{print $1}' | sed 's/\/tcp//g' > ./ports_http.txt") # do same as above but with "http" instead of "open"
print(get_ports_http)
print("...DONE!\n")

{% endhighlight %}
## Nmap aggressive scan  
The script then starts the second, longer, more aggressive nmap scan of only the ports listed in ports_open.txt.  I found a cool "-p $(tr '\n' , <./ports_open.txt)" line on stack overflow which tells nmap to scan ports from a file line by line.  This works very well.  
{% highlight ruby %}

os.system("touch ./" + folder_name + "/2_nmap_scan.txt") # initializing textfile, maybe not necessary
print("...Created file 2_nmap_scan.txt within " + folder_name)
print("Starting second nmap scan and dumping into 2_nmap_scan...")
nmap_scan = os.system("nmap -Pn -A --script=vuln -p $(tr '\n' , <./ports_open.txt) -oN ./" + folder_name + "/2_nmap_scan.txt " + target_IP) # run aggressive and NSE vulnerability scan against only the open ports
print(nmap_scan) # print out the ongoing scan
print("...DONE!\n") # when above is done, print done and move on

{% endhighlight %}  
## Gobuster scan  
The script then moves on to gobuster.  It uses the seclists raft-large wordlist to fuzz for directories found in ports_http.txt.  
This one was tricky for me to figure out.  
I had to open ports.http.txt, then start a FOR loop: for each line in the file, do a gobuster scan.  
Python was opening the lines in ports_http.txt as an array (list), and I couldn't just concatenate the lines with the os.system commands because the os.system commands are strings.  I kept getting a "you can't concatenate list with str" error and had to figure out how to make both the list and string values either all list, or all strings.  It was easier to re-declare everything as strings, then concatenate each of these individual declarations.  
I also ran into the issue of having line breaks in the ports_http.txt file, from back where I told nmap to pull the http ports from the initial scan and paste them into a new ports_http.txt file.  So, I also had to google and implement a line replace in the for loop, replacing "\n" for "" in each line iteration.  
The loop then renames and moves the scan result at the end of each for loop, placing the actual port # in the name of the resulting scan so that you don't just end up with multiple copies of "3_gobuster_scan.txt".  
{% highlight ruby %}

ports_http = open("ports_http.txt","r") # open ports_http.txt and read it
for line in ports_http: # for each http port in file...
    line_without_breaks = line.replace("\n", "") # remove line breaks which are messing up gobuster_scan
    print("Starting gobuster...")
    gobuster_scan1 = 'gobuster dir -e -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -u http://' # breaking up system commands with variables
    gobuster_scan2 = ':'
    gobuster_scan3 = ' > output.txt' # we're saving the output as output.txt
    gobuster_scan = os.system(gobuster_scan1 + target_IP + gobuster_scan2 + line_without_breaks + gobuster_scan3) # reassembling commands and variables as string
    print("moving output.txt to 3_gobuster_" + line_without_breaks + ".txt")
    os.system("mv ./output.txt " + folder_name + "/3_gobuster_scan_" + line_without_breaks + ".txt") # move/rename output.txt to folder_name
    print("Done with loop of " + line_without_breaks)
ports_http.close() # close ports_http.txt reading when last line is iterated through

{% endhighlight %}
## Wfuzz scan  
So the wfuzz scan does the exact same thing as the gobuster scan above, in the same manner.  
A tricky bit that I discovered after running a few wfuzz scans, that I didn't think of because I have rarely used it for HackTheBox so far, is that the wfuzz output gives you the http response code for every single fuzz done, that you just grep through or sort afterward.  So, I looked up all the response codes, figured that ehhh, maybe I don't need any of the 500 responses, which is perhaps a mistake, and added "--sc 200,201,202,203,204,205,206" to tell wfuzz to only output the 200 responses.  
{% highlight ruby %}

ports_http = open("ports_http.txt","r") # open ports_http.txt and read it
for line in ports_http: # for each http port in file...
    line_without_breaks = line.replace("\n", "") # remove line breaks which are messing up gobuster_scan
    print("Starting wfuzz...")
    wfuzz_scan1 = 'wfuzz -c --sc 200,201,202,203,204,205,206 -f suboutput.txt -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H Host:FUZZ.'# breaking up system commands with variables
    wfuzz_scan2 = ':' # will use this one twice
    wfuzz_scan3 = ' -u http://'
    wfuzz_scan4 = ' > output.txt' # we're saving the output as output.txt
    wfuzz_scan = os.system(wfuzz_scan1 + target_IP + wfuzz_scan2 + line_without_breaks + wfuzz_scan3 + target_IP + wfuzz_scan2 + line_without_breaks) # reassembling commands and variables as string
    print("moving suboutput.txt to 3_wfuzz_" + line_without_breaks + ".txt")
    os.system("mv ./suboutput.txt " + folder_name + "/3_wfuzz_scan_" + line_without_breaks + ".txt") # move/rename output.txt to folder_name
    print("Done with loop of " + line_without_breaks)
ports_http.close() # close ports_http.txt reading when last line is iterated through

{% endhighlight %}
## Cleanup
The last part of the script just deletes the ports_open.txt and ports_http.txt files.  
I thought maybe I should move them into the folder the the script creates, like the other scan outputs, but decided that this is stupid if I'm already going to be reading the scan results anyway.  
In the future, I still plan on parsing through results and combining them into some sort of quick reference textfile.  That's for later, not now, I guess.  
{% highlight ruby %}

print("Cleaning up files...") # clean up the root folder ports_open.txt and ports_http.txt
cleanup = os.system("rm -rf ports_*.txt")
print(cleanup)
print("...DONE!\n")

{% endhighlight %}

## Final Thoughts  
This was interesting, and moved on much faster than I thought it would.  While I do review python code for CTF stuff, and do read about it, I have never made anything in python before in my life, not even a "Hello World".  This was a smooth-enough first project.  
Now that I know for a fact how some of the loops work, and have some code to look back on that will jog my memory, I intend to start leveraging python more in practice, not just in mental games and "oh what-if" scenarios.  
It's just too simple to not do.  I mean it's basically just C++ psuedocode, and finding/using the "while" method was cool, I bet there's more stuff out there than while and for loops.  
