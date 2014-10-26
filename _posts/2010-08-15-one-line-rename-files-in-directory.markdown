---
layout: post
title: One Line, Rename Files in Directory
date: 2010-08-15 08:13:10.000000000 -04:00
categories:
- tech
tags:
- bash
type: post
---
{% highlight bash %} $ ls | while read i; do mv "$i" "some prefix $i"; done;{% endhighlight %}
the read part of the while waits for input and takes the input and stores it in "i". The while loop gets it's input from ls. At each line of ls, the current line is sent to the read.
