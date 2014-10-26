---
layout: post
title: Ubuntu Firewire Audio Kino
date: 2012-09-20 10:01:21.000000000 -04:00
categories: []
tags: []
type: post
published: true
---
I was capturing some digital video through firewire on Ubuntu 12.04 with Kino and there was no sound. Starting Kino with this comman remmedied the situation.
{% highlight bash %}padsp kino{% endhighlight %}
The error I was receiving was this:
{% highlight bash %}ALSA lib pcm.c:2217:(snd_pcm_open_noupdate) Unknown PCM /dev/dsp
Could not open ALSA device "/dev/dsp": No such file or directory{% endhighlight %}
