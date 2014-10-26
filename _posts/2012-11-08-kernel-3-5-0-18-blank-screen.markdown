---
layout: post
title: kernel 3.5.0-18 blank screen
date: 2012-11-08 09:22:51.000000000 -05:00
categories:
- tech
tags:
- '12.10'
- driver
- drivers
- linux
- nvidia
- ubuntu
type: post
---
I updated to kernel 3.5.0-18 with my Ubuntu 12.10 install and after restarting it would not boot, I would get a blank screen when I feel as though I should have been getting the login manager. It seems this was an issue with the Nvidia drivers. The following (followed by a restart) is what seemed to get things working for me.
{% highlight bash %}apt-get purge nvidia-current
apt-get purge nvidia-settings
apt-get install linux-headers-3.5.0-18
apt-get install linux-headers-3.5.0-18-generic
apt-get install nvidia-current nvidia settings{% endhighlight %}
I may have not had to install two sets of headers, but, it wasn't worth my time determining this.
