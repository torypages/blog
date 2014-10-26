---
layout: post
title: redshift / gtk-redshift wont start
date: 2012-12-28 20:49:51.000000000 -05:00
categories:
- tech
tags:
- '12.10'
- desktop
- gtk-redshift
- launcher
- linux
- redshift
- ubuntu
- Unity
type: post
---
In my Ubuntu 12.10 install redshift / gtk-reshift would not start. When ran from the terminal the following is the output that I would receive:
{% highlight python %}Started Geoclue provider `Geoclue Master'.
Using provider `geoclue'.
** (process:911): WARNING **: Could not get location, 3 retries left.
** (process:911): WARNING **: Could not get location, 2 retries left.
** (process:911): WARNING **: Could not get location, 1 retries left.
** (process:911): WARNING **: Provider does not have a valid location available.{% endhighlight %}
It seems to be looking for something which determines my current geo coordinates and failing to find it. With the -l switch you can supply your coordinates manually. You can get your coordinates by googling "your city longitude latitude." In order for you to be able to open Redshift from the Unity dash with this switch / manual entry, you need to update the launcher file for the application. As root I updated the exec value of /usr/share/applications/gtk-redshift.desktop to be "Exec=gtk-redshift -l 43:79" and my problem was solved.
