---
layout: post
title: Count Number of Lines Given by any Command
date: 2010-08-01 19:22:12.000000000 -04:00
categories:
- tech
tags:
- bash
- cat
- count
- linux
- ls
- newlines
- wc
type: post
---
Here is how you count the number of files outputted from any command using the ls command as an example
{% highlight bash %} $ ls -A | wc -l{% endhighlight %}
This is a bit curious actually because ls -A doesn't necessarily print each file on each line but the line count is still equal to the number of files. I presume that this is because bash is deciding not create newlines where new line characters exist. 
There is a simple way to test to see if this is true. If we run
{% highlight bash %} $ ls -A > test.txt {% endhighlight %}
The output of ls -A will be sent into a file named test.txt instead of the wc program. After having done that we can run
{% highlight bash %} $ cat -E test.txt{% endhighlight %}
which will represent each newline character with a $. And, as I have just found out, and as you'll be able to see, there is a $ after each file. Thus, when you run ls -A the newline characters are there, the shell just decides to not necessarily print each file to a newline
