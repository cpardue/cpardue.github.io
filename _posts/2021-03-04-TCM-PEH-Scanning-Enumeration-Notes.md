---
layout:     post
title:      Title Here
date:       2021-02-28
summary:    Summary Here
categories: Category
thumbnail: cogs
tags:
 - tag
---
<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="Scanning_with_Nessus_0"></a>Scanning with Nessus</h2>
<p class="has-line-data" data-line-start="1" data-line-end="2">Typically you will start a Nessus scan, then move to enumeration, then return to the Nessus scan once it’s complete</p>
<h3 class="code-line" data-line-start=3 data-line-end=4 ><a id="Download_and_Install_Nessus_3"></a>Download and Install Nessus</h3>
<p class="has-line-data" data-line-start="4" data-line-end="13">Download Nessus x64<br>
Install it<br>
Navigate to it (<a href="http://localhost:8834">http://localhost:8834</a>)<br>
It will compile plugins, which will take some time<br>
Install whichever license (Essentials)<br>
Get activation code, copy/paste<br>
Need a new activation code with each new installation if Essentials<br>
Let it finish installing<br>
Log in</p>
<h3 class="code-line" data-line-start=14 data-line-end=15 ><a id="Run_a_Scan_with_Nessus_14"></a>Run a Scan with Nessus</h3>
<p class="has-line-data" data-line-start="15" data-line-end="22">Scan &gt; Templates &gt; Basic Scan<br>
Name the scan job<br>
Enter targets to scan against<br>
Do port scan of all ports via options<br>
Set scan type<br>
Go ahead and look through the options<br>
Start scan</p>
<h3 class="code-line" data-line-start=23 data-line-end=24 ><a id="Review_Nessus_Scan_Results_23"></a>Review Nessus Scan Results</h3>
<p class="has-line-data" data-line-start="24" data-line-end="31">View results<br>
Click the Vulnerabilities tab<br>
Click Settings cog, disable Grouping<br>
View installed version, and recommended version to patch<br>
Sift through the results and report on what’s important and potential for exploitation<br>
Download nessus file<br>
There are tools to turn a .nessus file into an excel file</p>
<h3 class="code-line" data-line-start=32 data-line-end=33 ><a id="Converting_nessus_File_32"></a>Converting .nessus File</h3>
<p class="has-line-data" data-line-start="33" data-line-end="35">Into CSV<br>
<a href="https://github.com/levyjm/NessusConverter">https://github.com/levyjm/NessusConverter</a></p>
<p class="has-line-data" data-line-start="36" data-line-end="41">Into Excel 2003<br>
<a href="https://seclists.org/pen-test/2006/Apr/82">https://seclists.org/pen-test/2006/Apr/82</a><br>
You can use the XML version of the Nessus report and just directly import it<br>
into Excel (at least in 2003).<br>
In Excel:</p>
<ul>
<li class="has-line-data" data-line-start="41" data-line-end="42">Drop down the “Data” menu on the toolbar</li>
<li class="has-line-data" data-line-start="42" data-line-end="43">Point to “XML” then click “Import”</li>
<li class="has-line-data" data-line-start="43" data-line-end="44">Click “Ok” a couple times; all the results are imported nicely into Excel</li>
</ul>
