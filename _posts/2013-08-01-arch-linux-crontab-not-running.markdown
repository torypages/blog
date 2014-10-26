---
layout: post
title: Arch Linux Crontab Not Running
date: 2013-08-01 12:56:56.000000000 -04:00
categories:
- tech
tags:
- arch
- arch linux
- cron
- crontab
- linux
type: post
---
My cron was not running, it appears that the cron service is not enabled by default on a new Arch Linux install. I enabled it with:
{% highlight text %} systemctl start cronie
systemctl start cronie.service{% endhighlight %}
