---
layout: post
title:  "Move files in directory containing a string."
date:   2014-10-26 13:12:00
categories: bash linux 
---

Move all files containing the text "status: draft" to another folder.

{% highlight python %}
for i in `grep -l "status: draft" *`; do mv $i ~/Documents/jekyllDrafts/; done;
{% endhighlight bash %}
