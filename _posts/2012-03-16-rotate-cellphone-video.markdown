---
layout: post
title: Rotate Cellphone Video
date: 2012-03-16 17:48:50.000000000 -04:00
categories:
- tech
tags:
- arch
- arch linux
- ffmpeg
- linux
- mp4
- rotate
type: post
published: true
---
For whatever reason every time I have shot a portrait mode video with my Motorola Atrix cellphone. Today the issue came up again and here is the command I finally used with acceptable success.
{% highlight bash %}ffmpeg -i 2012-03-16_15-12-41_472.mp4 -vcodec libx264 -preset medium -crf 24 \
 -threads 0 -vf transpose=1 -acodec copy output.mkv{% endhighlight %}
It isn't perfect, but, it is good enough, I hope this helps
