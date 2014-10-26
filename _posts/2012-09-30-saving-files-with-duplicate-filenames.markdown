---
layout: post
title: Saving files with Duplicate Filenames
date: 2012-09-30 17:12:16.000000000 -04:00
categories: []
tags:
- filename
- python
type: post
---
If I want to save a file, but make sure to not overwrite files, and instead append a number to the end of the file name using Python I may want to write something like this:
{% highlight python %}#in this case I want to take a title and use it as a filename
filename = title
counter = 1
while os.path.isfile(filename):
    filename = title + " " + str(counter)
    counter = counter + 1
# write a file using the filename variable as a filename
{% endhighlight %}
