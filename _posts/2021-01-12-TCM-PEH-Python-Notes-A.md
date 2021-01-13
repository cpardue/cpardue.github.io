---
layout:     post
title:      TCM PEH Python Notes A
date:       2021-01-10
summary:    TheCyberMentor's Practical Ethical Hacking Course Python Notes block A
categories: Certs
thumbnail: jekyll
tags:
 - Certs
 - Python
---


<h3 class="code-line" data-line-start=0 data-line-end=1 ><a id="Intro_to_Python_0"></a>Intro to Python</h3>
<h4 class="code-line" data-line-start=1 data-line-end=2 ><a id="python3_1"></a>(python3)</h4>
<p class="has-line-data" data-line-start="2" data-line-end="3">################</p>
<h3 class="code-line" data-line-start=5 data-line-end=6 ><a id="Start_of_a_Python_Script_if_pythonpy_5"></a>Start of a Python Script if ./python.py</h3>
<p class="has-line-data" data-line-start="6" data-line-end="8">#!/bin/python3<br>
rest of script goes here</p>
<h3 class="code-line" data-line-start=8 data-line-end=9 ><a id="not_necessary_if_you_run_with_python3_pythonpy_8"></a>not necessary if you run with $python3 <a href="http://python.py">python.py</a></h3>
<h3 class="code-line" data-line-start=11 data-line-end=12 ><a id="Commenting_11"></a>Commenting</h3>
<h1 class="code-line" data-line-start=12 data-line-end=13 ><a id="this_is_a_comment_12"></a>this is a comment</h1>
<h3 class="code-line" data-line-start=15 data-line-end=16 ><a id="Strings_15"></a>Strings</h3>
<p class="has-line-data" data-line-start="16" data-line-end="22">print(“The String”)<br>
print(“newline after this \n”)<br>
print{&quot;&quot;“this string runs<br>
multiple<br>
lines”&quot;&quot;)<br>
print(“this string is “+”concatenated”)</p>
<h3 class="code-line" data-line-start=24 data-line-end=25 ><a id="Math_24"></a>Math</h3>
<p class="has-line-data" data-line-start="25" data-line-end="33">print(50 + 50)<br>
print(50 - 50)<br>
print(50 * 50)<br>
print(50 / 50)<br>
print (50 + 50 - 50 * 50 / 50) # will PEMDAS it<br>
print(50 ** 50) # exponent 50 to 50th power<br>
print(50 % 6) # modulo<br>
print(50 // 6) # divide with no remainder</p>
<h3 class="code-line" data-line-start=35 data-line-end=36 ><a id="Variables_and_Methods_35"></a>Variables and Methods</h3>
<p class="has-line-data" data-line-start="36" data-line-end="37">quote = “This text” # stores the variable ‘quote’ of “this text”</p>
<h1 class="code-line" data-line-start=37 data-line-end=38 ><a id="so_yeah_you_dont_have_to_declare_the_variable_type_first_37"></a>so yeah you don’t have to declare the variable type first</h1>
<p class="has-line-data" data-line-start="38" data-line-end="39">print(quote) # prints the quote variables content</p>
<h1 class="code-line" data-line-start=39 data-line-end=40 ><a id="methods_are_basically_functions_available_for_an_object_like_in_Powershell_39"></a>methods are basically functions available for an object, like in Powershell</h1>
<p class="has-line-data" data-line-start="40" data-line-end="51">print(quote.upper()) # prints quote in uppercase<br>
print(quote.lower()) # lowercase<br>
print(quote.title()) # title case (capitalized)<br>
print(len(quote)) # prints char length of quote<br>
name = “Jacob” # string<br>
age = 45 # int<br>
gpa = 1.7 # float<br>
print(int(age)) # will print 45<br>
print(int(45.1)) # will print 45<br>
print(&quot;my name is “ + name + ” and i am &quot; + age) # this won’t work, can’t cat a str with int<br>
print(&quot;my name is “ + name + ” and i am &quot; + str(age)) # this will work</p>
<h1 class="code-line" data-line-start=51 data-line-end=52 ><a id="method_above_str_turns_int_into_str_51"></a>method above str() turns int into str</h1>
<p class="has-line-data" data-line-start="52" data-line-end="54">age += 1<br>
print(age) # outputs 46</p>
<h1 class="code-line" data-line-start=54 data-line-end=55 ><a id="now_age_will_increment_1_since_initialization_54"></a>now age will increment 1 since initialization</h1>
<h1 class="code-line" data-line-start=55 data-line-end=56 ><a id="therefore_age__46_55"></a>therefore age = 46</h1>
<p class="has-line-data" data-line-start="56" data-line-end="59">birthday = 1<br>
age =+ birthday # will increment again (by 1)<br>
print(age) # results now in 47</p>
<h3 class="code-line" data-line-start=61 data-line-end=62 ><a id="Functions_61"></a>Functions</h3>
<h1 class="code-line" data-line-start=62 data-line-end=63 ><a id="mini_programs_that_you_can_reuse_essentially_62"></a>mini programs that you can reuse, essentially</h1>
<p class="has-line-data" data-line-start="63" data-line-end="68">print(“look, print is a function)<br>
def who_am_i(): # the function definition syntax<br>
…name = “Heath” # four spaces indent, … = spacespacespacespace<br>
…age = 30 # … = 4 spaces, again<br>
…print(”&quot;my name is “ + name + ” and i am age &quot; + str(age))</p>
<h1 class="code-line" data-line-start=68 data-line-end=69 ><a id="calling_who_am_i_will_print_out_the_cated_phrase_with_those_variables_from_within_that_function_68"></a>calling who_am_i will print out the cat’ed phrase with those variables from within that function</h1>
<h1 class="code-line" data-line-start=69 data-line-end=70 ><a id="pretty_sure_defining_function_MUST_come_before_calling_function_unlike_c_69"></a>pretty sure defining function MUST come before calling function, unlike c++</h1>
<p class="has-line-data" data-line-start="70" data-line-end="84">def add_one_hundred(num):<br>
…print(num + 100) # again, periods = spaces<br>
add_one_hundred(400) # output ends up being 500, do the math, genius<br>
def add(x,y):<br>
…print(x + y)<br>
add(7,7) # outputs 14<br>
def multiply(x,y)<br>
…return x * y # output DOES return product of x,y but doesn’t know to print it out<br>
print(multiply(7,7)) # now it prints out the product<br>
def square_root(x):<br>
…print(x ** .5) # prints the inverse square, ie. square root<br>
square_root(64) # prints the root<br>
def NL():<br>
…print(&quot;\n&quot;) # new_line makes a…new line</p>
<h3 class="code-line" data-line-start=86 data-line-end=87 ><a id="Boolean_Expressions_86"></a>Boolean Expressions</h3>
<h1 class="code-line" data-line-start=87 data-line-end=88 ><a id="true_or_false_1_or_0_87"></a>true or false, 1 or 0</h1>
<p class="has-line-data" data-line-start="88" data-line-end="95">print(“Boolean expressions:\n”)<br>
bool1 = True<br>
bool2 = 3 * 3 == 9<br>
bool3 = False<br>
bool 4 = 3 * 3 != 9<br>
print(bool1,bool2,bool3,bool4) # output: True, True, False, False<br>
print(type(bool1)) # output: class ‘bool’</p>
<h1 class="code-line" data-line-start=95 data-line-end=96 ><a id="bools_make_more_sense_in_loops_and_conditional_statements_95"></a>bools make more sense in loops and conditional statements</h1>
<h3 class="code-line" data-line-start=98 data-line-end=99 ><a id="Relational_Operators_and_Boolean_Operators_98"></a>Relational Operators and Boolean Operators</h3>
<p class="has-line-data" data-line-start="99" data-line-end="109">greater_than = 7 &gt; 5 # this is True<br>
less_than = 5 &lt; 7 # this is True<br>
greater_than_equal_to = 7 &gt;= 7 # this is True<br>
less_than_equal_to = 7 &lt;= 7 # this is True<br>
test_and = (7 &gt; 5) and (5 &lt; 7) # T &amp; T = T<br>
test_and2 = (7 &gt; 5) and (5 &gt; 7) # T &amp; F = F<br>
test_or = (7 &gt; 5) or (5 &lt; 7) # T  || T = T<br>
test_or2 = (7 &gt; 5) or (5 &gt; 7) # T || F = T<br>
test_or3 = (7 &lt; 5) or (5 &gt; 7) # F || F = F<br>
test_not = not True # !T = F</p>
<h1 class="code-line" data-line-start=109 data-line-end=110 ><a id="google_truth_table_python_achieve_enlightenment_109"></a>google “truth table python”, achieve enlightenment</h1>
