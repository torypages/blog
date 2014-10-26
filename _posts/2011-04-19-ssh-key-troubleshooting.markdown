---
layout: post
title: SSH Key Troubleshooting
date: 2011-04-19 18:57:31.000000000 -04:00
categories: []
tags: []
type: post
---
I was having issues setting up an SSH key pair, something I have done a few times before and isn't too complicated. I kept on receiving
{% highlight bash %}Permission denied (publickey){% endhighlight %}
It turns out the folder on the remote server I was placing the public key in was wrong. The issue was that the users home folder was unusual. The username was cookieMonster yet the user's home folder was located at /home/cookie_monster instead of what I had expected, /home/cookieMonster I realized this as I was browsing the /etc/passwd file.
