---
layout: post
title: List all URLs to YouTube user's videos
date: 2012-09-29 21:37:46.000000000 -04:00
categories: []
tags:
- python
- script
- youtube
type: post
---
Here is a python script which will list all the URLs associated with a YouTube user's videos
{% highlight python %}# this outputs all the video URLs for a particular YouTube user
from urllib.request import urlopen
import xml.etree.ElementTree as etree
ytUserName = "7sagelsat"
next = "https://gdata.youtube.com/feeds/api/users/%s/uploads" % ytUserName
while next:
    xml = urlopen(next)
    tree = etree.parse(xml)
    root = tree.getroot()
    # get all the links on the xml page
    for i in root.findall('{http://www.w3.org/2005/Atom}entry'):
        print (i.find('{http://www.w3.org/2005/Atom}link[@rel="alternate"]').attrib['href'][:-22])
    try:
        # find the next xml page if available
        next = root.find('{http://www.w3.org/2005/Atom}link[@rel="next"]').attrib['href']
    except:
        # we are at the end
        next = ""
print ("\nfinished"){% endhighlight %}
