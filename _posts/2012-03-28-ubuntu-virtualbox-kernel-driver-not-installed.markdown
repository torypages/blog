---
layout: post
title: ubuntu virtualbox kernel driver not installed
date: 2012-03-28 14:42:49.000000000 -04:00
categories:
- tech
tags: []
type: post
---
I ran into some issues with getting Virtualbox to run. I'm not sure why it stopped working, it seems like it might have been because a new kernel was installed, I'm not sure. However, I fixed it, it seems as if I was missing the new kernel headers. Bellow you will find the two commands I think fixed the situation, hwoever, there were a few others I ran in the process of troubleshooting.
{% highlight bash %}# apt-get install linux-headers-$(uname -r)
# /etc/init.d/vboxdrv setup
{% endhighlight %}
