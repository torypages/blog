---
layout: post
title: Bash Read File
date: 2010-07-23 19:04:53.000000000 -04:00
categories:
- tech
tags:
- bash
- linux
- read file
type: post
---
Here is the code snippet that I will use in the future when I want to read in a file line by line
{% highlight bash %}#!/bin/bash
while read LINE ; do
    echo $LINE;
done < file.txt;{% endhighlight %}
The following is a possibility too, but, any variable used on the inside of the while loop will not be available on the outside. This is because below the while loop is executed in a sub-shell because of the pipe.
{% highlight bash %}#!/bin/bash
cat file.txt |
while read LINE ; do
    echo $LINE
done{% endhighlight %}
