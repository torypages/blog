---
layout: post
title: Samba, Symbolic Links, Windows XP
date: 2010-06-08 16:15:44.000000000 -04:00
categories:
- tech
tags: []
type: post
---
The other day I noticed that I was unable to follow a symbolic link in a Samba share definition. I fixed this by adding `unix extensions = no` to the [global] section in /etc/samba/smb.conf as well as making my share definition (in the same file) to look like this

{% highlight bash %}
[homes]
   comment = Home Directories
   browseable = yes
   writable = yes
   valid users = %S
   wide links = yes
{% endhighlight %}
