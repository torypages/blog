---
layout: post
title: Recursive Chown with Exceptions
date: 2012-09-13 19:37:30.000000000 -04:00
categories:
- tech
tags:
- bash
- chown
- exec
- find
- linux
- ubuntu
type: post
---
I needed to apply a recursive chown command to my home directory today. The problem is that I have a very large external file system that is mounted in my home directory, one that must remain mounted. I did not want the chown to be applied to the remote file system and chown doesn't have functionality built into it to perform advanced recursive actions, like, recursive except with x. Then I remembered the -exec switch on the find command and realized that -exec is a great way to get recursive functionality for any command that your want to run with advanced recursive functionality. In my case, I used the -mount switch to limit the execution to the local filesystem only, but other regular expression conditions, or other filtering mechanisms could be used too! This is an important and flexible lesson. It's really quite obvious in hindsight, and probably obvious to a Linux sysadmin guru, but in any event, here it is.
{% highlight bash %}find * -mount -exec chown tory:tory {} ;\{% endhighlight %}
