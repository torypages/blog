---
layout: post
title: Bash. Variables. Sed. Replace. Slashes
date: 2010-08-08 17:14:23.000000000 -04:00
categories:
- tech
tags:
- bash
- regex
- regular expression
- replace
- sed
- slashes
- text
- variable
- variables
type: post
---
Here is an example how to replace text containing slashes with new text that also contains slashes. It will ultimately convert "A cool website is http://www.yahoo.com" to "A cool website is http://www.google.com"
{% highlight bash %}#!/bin/bash
A="http://www.google.com";
B="http://www.yahoo.com";
C="A cool website is $B";
echo "A = $A";
echo "B = $B";
echo "C = $C";
A=`echo "$A" | sed 's/\//\\\\\//g'`
B=`echo "$B" | sed 's/\//\\\\\//g'`
echo "----------After Running Fixing----------";
echo "A = $A";
echo "B = $B";
echo "C = $C";
echo "-----------After running sed------------";
echo "$C" | sed "s/$B/$A/g"
{% endhighlight %}
