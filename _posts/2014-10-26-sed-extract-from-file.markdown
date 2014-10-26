---
layout: post
title:  "Extract text with sed"
date:   2014-10-26 10:23:00 
categories: sed 
---

Here I search for the contents of all src (from an HTML tag) attributes in the files in a folder.

{% highlight bash %}
sed -n 's/.*src="\([^"]*\).*/\1/p' *
{% endhighlight %}
