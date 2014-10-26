---
layout: post
title: Reading Files by Line w/ Spaces
date: 2010-08-04 16:57:49.000000000 -04:00
categories:
- tech
tags:
- bash
- linux
- read file
type: post
---
{% highlight bash %}#!/bin/bash
SAVEIFS=$IFS;
IFS="\n";
while read LINE ; do
     echo "${LINE}";
done < myFileWithSpaces.txt;
IFS=$SAVEIFS;
{% endhighlight %}
