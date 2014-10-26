---
layout: post
title: Script works but not via Cron
date: 2012-09-30 16:39:24.000000000 -04:00
categories: []
tags:
- bash
- cron
- crontab
- linux
- python
type: post
---
I spent more time sorting out a problem than I should have today. I was trying run some scripts from a cron job, they weren't running. They ran fine when run outside of cron, that is, they ran fine on their own or when I manually ran them, but the moment I tried to run them from cron they didn't work. My precise situation was a bash script that was calling some python scripts and some relative path references in the python scripts were breaking. I eventually realized that files were being created in my home directory that were supposed to be created in the directory containing the python scripts. The problem was that the directory that cron runs out of is different from where I was running it manually. That is, if I had ran "pwd" I would have not received the result I expected.

The fix was to "cd" into the the directory I wanted to run the scripts from at the beginning of my bash script. The result was something like this.
{% highlight bash %}#!/bin/bash
cd /home/tory/myScript/
python firstScript.py
python secondScript.py{% endhighlight %}
