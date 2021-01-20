---
layout:     post
title:      TCM PEH Python Notes B
date:       2021-01-17
summary:    TheCyberMentor's Practical Ethical Hacking Course Python Notes block B
categories: Certs
thumbnail: jekyll
tags:
 - Certs
 - Python
---


<h3 class="code-line" data-line-start=0 data-line-end=1 ><a id="Conditional_Statements_0"></a>Conditional Statements</h3>
<h1 class="code-line" data-line-start=1 data-line-end=2 ><a id="if_the_conditions_are_met_then_this_result_1"></a>if the conditions are met, then this result</h1>
<p class="has-line-data" data-line-start="3" data-line-end="10">def drink(money):<br>
…if money &gt;= 2:<br>
…,return “You got yourself a drink”<br>
…else:<br>
…,return “No drink for you”<br>
print(drink(3)) # outputs You got yourself a drink<br>
print(drink(1)) # outputs No drink for you</p>
<p class="has-line-data" data-line-start="11" data-line-end="23">def alcohol(age,money):<br>
…if (age &gt;= 21) and (money &gt;= 5):<br>
…,return “We’re getting a drink”<br>
…elif (age &gt;= 21) and (money &lt;5):<br>
…,return “Come back with more money”<br>
…elif (age &lt;21) and (money &gt;= 5):<br>
…,return “Nice try, kid”<br>
…else:<br>
…,return “Too poor and too young”<br>
print(alcohol(21,5)) # gets a drink<br>
print(alcohol(21,4)) # come back more money<br>
print(alcholo(20,4)) # too poor, too young</p>
<h3 class="code-line" data-line-start=25 data-line-end=26 ><a id="Lists_25"></a>Lists</h3>
<h1 class="code-line" data-line-start=26 data-line-end=27 ><a id="lists_are_just_an_array_26"></a>lists are just an array</h1>
<h1 class="code-line" data-line-start=27 data-line-end=28 ><a id="lists_live_within_brackets_27"></a>lists live within brackets</h1>
<p class="has-line-data" data-line-start="28" data-line-end="39">movies = [“when harry met sally”, “the hangover”, “perks of being wallflower”]<br>
print(movies[1]) # outputs 2nd item in the array, because 0 = 1st index<br>
print(movies[0:2]) # will print 1st, 2nd, 3rd<br>
print(movies[1:]) # will print entire array after the 2nd index<br>
print(movies[:1]) # will print entire array up to 2nd index (“when harry…” only)<br>
print(movies[-1)) # grabs last item on the list<br>
print(len(movies))<br>
movies.append(“jaws”) # will append jaws to end of array from this point forward<br>
movies.pop() # this will delete last item on array<br>
print(movies) # won’t have jaws anymore<br>
movies.pop(0) # this will delete 1st index of array</p>
<h3 class="code-line" data-line-start=41 data-line-end=42 ><a id="Tuples_41"></a>Tuples</h3>
<h1 class="code-line" data-line-start=42 data-line-end=43 ><a id="tuples_are_an_immutable_array_uses_brackets_42"></a>tuples are an immutable array, uses brackets</h1>
<p class="has-line-data" data-line-start="43" data-line-end="45">grade = {“a”, “b”, “c”, “f”} # now that it’s defined, it stays that way<br>
print(grades{3}) # will still print 4th index in the tuple</p>
<h3 class="code-line" data-line-start=47 data-line-end=48 ><a id="Loops_47"></a>Loops</h3>
<p class="has-line-data" data-line-start="48" data-line-end="51">ports = [21, 22, 23,80,3389]<br>
for x in ports:<br>
…print(x) # prints each port</p>
<p class="has-line-data" data-line-start="52" data-line-end="56">y = 1<br>
while y &lt; 10:<br>
…print(y)<br>
…y += 1 # keeps going until y = 10</p>
