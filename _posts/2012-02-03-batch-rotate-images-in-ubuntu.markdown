---
layout: post
title: Batch Rotate Images in Ubuntu
date: 2012-02-03 17:54:47.000000000 -05:00
categories:
- tech
tags:
- command
- image
- imagemagic
- jpg
- linux
- rotate
- ubuntu
type: post
---
For whatever reason I wasn't able to quickly find something to batch rotate some images in Ubuntu, so, here is the command I ran in the folder that contained the images I wished to rotate after creating a folder named 'resized.' Not to worry about the program trying to rotate the folder, it will just error. This also requires <a hfre="https://help.ubuntu.com/community/ImageMagick" target="_blank">ImageMagick</a> to be installed
{% highlight bash %}for i in `ls`; do convert $i -rotate 90 rotated/$i; done;{% endhighlight %}
