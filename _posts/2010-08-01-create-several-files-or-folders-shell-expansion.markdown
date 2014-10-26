---
layout: post
title: Create Several Files or Folders - Shell Expansion
date: 2010-08-01 18:34:57.000000000 -04:00
categories:
- tech
tags:
- bash
- linux
- shall expansion
type: post
---
Today I learned a new method of shell expansion that I thought was pretty cool.
Here is an example of how to create files by the names of: file1, file2, file3, file4 and file5
{% highlight bash %} $ touch file{1,2,3,4,5} {% endhighlight %}
and do create 5 folders in the same way you could
{% highlight bash %} $ mkdir folder{1,2,3,4,5} {% endhighlight %}
 essentially the touch or mkdir command runs 5 times, one for each of the coma separated values within the squigly brackets. So with that said by no means is this limited to making files and folders. You could for example move several files into a single folder
{% highlight bash %} $ mv file{1,2,3,4,5} someFolder {% endhighlight %}
{% highlight bash %} $ mv {julia,matt,kattie}\'s\ work peoples\'\ work{% endhighlight %}
