---
layout:     post
title:      TCM PEH Python Notes A
date:       2021-01-19
summary:    TheCyberMentor's Practical Ethical Hacking Course Python Notes block C
categories: Certs
thumbnail: jekyll
tags:
 - Certs
 - Python
---


<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Importing_Modules_in_Python3_0"></a>Importing Modules in Python3</h1>
<p class="has-line-data" data-line-start="1" data-line-end="6">#!/bin/python3<br>
import moduleName<br>
import sys #system funct &amp; parameters such as print(sys.version)<br>
import argv<br>
import datetime</p>
<h1 class="code-line" data-line-start=6 data-line-end=7 ><a id="Import_part_of_a_module_6"></a>Import part of a module</h1>
<p class="has-line-data" data-line-start="7" data-line-end="8">from datetime import datetime # to import part of a module</p>
<h1 class="code-line" data-line-start=8 data-line-end=9 ><a id="Import_with_alias_8"></a>Import with alias</h1>
<h2 class="code-line" data-line-start=9 data-line-end=10 ><a id="So_you_can_obfuscate_with_this_it_looks_like_9"></a>So you can obfuscate with this, it looks like</h2>
<p class="has-line-data" data-line-start="10" data-line-end="11">from datetime import datetime as dt # now print(datetime.now()) == print(dt.now())</p>
<h1 class="code-line" data-line-start=13 data-line-end=14 ><a id="Advanced_Strings_13"></a>Advanced Strings</h1>
<p class="has-line-data" data-line-start="14" data-line-end="35">my_name = “Heath”<br>
print(my_name[0]) # = H<br>
print(my_name[-1]) # = h<br>
sentence = “This is a sentence.”<br>
print(sentence[:4]) # prints This<br>
print(sentence[-9:-4])<br>
print(sentence.split()) # breaks out as individual words<br>
sent_split = sentence.split()<br>
sent_join = ‘ ’.join(sent_split)<br>
print(sent_join) # now it puts it all back together<br>
quote = “He said, ‘I quote myself’” # single quotes inside double quotes<br>
quote = “He said, \”I quote myself&quot;&quot; # escaped characters with <br>
2_mch_space = “                       Hello”<br>
print(2_mch_space.strip()) # outputs only Hello<br>
print (“A” in “Apple”) # looks for boolean truth, True or False, is True<br>
print(“a” in “Apple”) # outputs False<br>
letter = “A”<br>
word =“Apple”<br>
print(letter.lower() in word.lower()) # normalizes the chars for above boolean truths, YOU WILL USE THIS<br>
movie = “The Hangover”<br>
print(“My favorite movie is ().”.format(movie) # will insert movie var into brackets, much better than string concatenation</p>
<h1 class="code-line" data-line-start=37 data-line-end=38 ><a id="Dictionaries_37"></a>Dictionaries</h1>
<p class="has-line-data" data-line-start="38" data-line-end="42">drinks = (“wr”: 7, :of&quot;: 10, “ld”: 8)  # dictionary == key/value pairs {}<br>
key: value, key: value, key: value over and over<br>
employees = (“Fin”: [&quot;Bob, “Linda, ”Tina], “IT”: [&quot;Gene, “Lou”, “Ted”])<br>
print(employees) # so far just prints above as if a char str</p>
<h1 class="code-line" data-line-start=42 data-line-end=43 ><a id="Updating_Dictionaries_42"></a>Updating Dictionaries</h1>
<p class="has-line-data" data-line-start="43" data-line-end="49">employees[“Legal”] = [“Feng”] # adds to the dictionary<br>
employees.update({“Sales”: [“Sifl”, “Ollie”]}) # just another way to add to dictionary<br>
print(employees) # now fin, it, legal, and sales print out<br>
drinks[‘wr’} = 8<br>
print(drinks) # now drinks(wr: 8) instead of drinks(wr: 7)<br>
print(drinks.get(“wr”)) # prints wr and value</p>
