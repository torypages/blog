---
layout: post
title: Get Last Column of Each Row of Tab Delimited File
date: 2010-08-25 08:11:17.000000000 -04:00
categories:
- tech
tags:
- bash
- delimited
- extract
- tab
type: post
---
I had a file that was of this form
<pre>aaa bbb ccc hhh
aaa hhh
nnn vvv hhh
vvv aaa eee aaa hhh</pre>
Where each one of those spaces is a tab. I required the last column of each row. That is, I needed each field where "hhh" exists. And this was solved with the following code!
{% highlight bash %} $ cat tabDelimitedFile.txt | awk -F\\t '{print $NF}'{% endhighlight %}
