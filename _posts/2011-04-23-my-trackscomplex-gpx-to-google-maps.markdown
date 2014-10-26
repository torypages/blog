---
layout: post
title: '"My Tracks"/complex .gpx to Google Maps'
date: 2011-04-23 19:00:59.000000000 -04:00
categories:
- tech
tags: []
type: post
---
I probably have an upcoming project where I have to take gps tracking data generated from a program named "My Tracks" that is available in the Android app store and plot it on a Google map. From my preliminary testing Google maps doesn't like having complex gps data imported into it. In fact, I wasn't able to get it to work. I was able to get the data to import in to Google Earth but this was not idea since Google Earth only has the satellite view while I want a nice clean city map. So, in my preliminary exploration in search of a solution I have been looking at the Google api that allows me to do overlays on the maps. Particularly the part that allows me to draw lines. <a href="http://code.google.com/apis/maps/documentation/javascript/overlays.html#Polylines" target="_blank">http://code.google.com/apis/maps/documentation/javascript/overlays.html#Polylines</a> So this is pretty neat. But I will need to convert my gps entries that looks like
{% highlight text %}<trkpt lat="42.678785" lon="-25.46783"><ele>101</ele><fix>3d</fix><time>2010-01-03T10:48:41Z</time><hdop>4</hdop></trkpt>{% endhighlight %}
into something that looks like
{% highlight javascript %} new google.maps.LatLng(42.678785, -25.46783){% endhighlight %}
Simple enough with a bit of sed
{% highlight bash %}cat coordinates.gpx | sed -e 's/<trkpt lat="/new google.maps.LatLng(/' | sed -e 's/" lon="/,/' | sed -e 's/"><ele>.*/),/' > insertIntoJs.txt{% endhighlight %}
Be sure to strip out the extra lines from the beginning and end of the gpx file before running the command. Also, there will be an extra comma at the end of the result that will need to be removed.
