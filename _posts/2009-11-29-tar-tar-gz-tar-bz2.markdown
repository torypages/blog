---
layout: post
title: Extract/Create .tar .tar.gz .tar.bz2
date: 2009-11-29 00:01:46.000000000 -05:00
categories:
- tech
tags:
- ".tar"
- ".tar.bz2"
- ".tar.gz"
- linux
type: post
---
I have always been dumbfounded as to how to extract and create .tar .tar.gz and .tar.bz2 but I don't know why because there really isn't much to it.

First off what are these things? .tar is simply a bundle of files packaged into one. And .tar.gz and .tar.gz2 are compressed packages that can be considered the same for most peoples purposes. However, I believe .tar.gz2 can compress files further than .tar.gz but takes longer.

Checkout  for more information check out <a title="http://en.wikipedia.org/wiki/Comparison_of_file_archivers" href="http://en.wikipedia.org/wiki/Comparison_of_file_archivers" target="_blank">http://en.wikipedia.org/wiki/Comparison_of_file_archivers</a>

To extract and create you use the "tar" command combined with some options. The options are as follows and are combined together to create a single statement.

x = e<strong>x</strong>tract
v = show what the computer is doing (<strong>v</strong>erbose)
f  = you are providing a <strong>f</strong>ilename
z = dealing with .tar.g<strong>z</strong>
j = dealing with .tar.bz2
c = <strong>c</strong>reate
<span style="text-decoration: underline;">Extraction of .tar.</span>
<pre><code>tar xvf toBeExtracted.tar</code></pre>
As you type this command out. Type "tar" then "-" for options, "x" for extract, "v" to show me what is going on also known as 'verbose' and "f" because I am supplying a file name.
<span style="text-decoration: underline;">Extraction of .tar.gz</span>
<pre><code>tar xzf toBeExtracted.tar.gz</code></pre>
same thing as before but with "z" added to it. Perhaps you could think "z" for zipped. With "v" for verbose left off this time.
<span style="text-decoration: underline;">Extraction of .tar.bz2</span>
<pre><code>tar xvjf toBeExtracted.tar.bz2</code></pre>
Similar to above, but instead of "z" it is "j". It is always nice to have a story to remember something, and apparently .bz2 was invented by Julian Seward, so, "j" for Julian.
<span style="text-decoration: underline;">Creation of .tar</span>
<pre><code>tar cfv resultingFile.tar folderOrFileToBeCompressed</code></pre>
"c" for create
<span style="text-decoration: underline;">Creation of .tar.gz</span>
<pre><code>tar cfvz  resultingFile.tar.gz folderOrFileToBeCompressed</code></pre>
<span style="text-decoration: underline;">Creation of .tar.bz2</span>
<pre><code>tar cfvj resultingFile.tar.bz2 folderOrFileToBeCompressed</code></pre>
<span style="text-decoration: underline;">Summary</span>
In sum, use the switches as listed above in combination (order is important) to achieve your goal. Once you study these commands a little bit you will feel far more comfortable with them.
For more information check out the manual page with the following command.
<pre><code>man tar</code></pre>
