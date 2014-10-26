---
layout: post
title: Rhythmbox Wouldn't Start and Libre.fm
date: 2011-01-22 12:51:18.000000000 -05:00
categories:
- tech
tags:
- libre.fm
- Rhythmbox
- ubuntu
type: post
---
Yesterday Rhythmbox stopped working for me, soon after starting up it would lock up. I searched on the net, no one else seemed to be having the issue. I removed it, purged it, I re-installed it, I removed some of the data files associated with it and no dice. But, this morning I decided to just go ahead and run (without checking what was in there)
{% highlight bash %}$ rm .local/share/rhythmbox/ -r {% endhighlight %}
Now, this probably killed an ant with a hammer and I probably deleted things that I didn't need to, but I don't care, I didn't have the patience to figure it out properly, but hey, it worked. I'll probably have to reinstall some plugins, but at least I have a working music player again
<b>Update: </b> It seems like the cause may have been libre.fm. It seems as if the libre.fm website is down or has disappeared. I had a plugin that would perform audioscrobbing with libre.fm it is possible that the plug-in was trying to connect to libre.fm and failing. 
<b>Update: </b> Libre.fm is functional again, I don't know what that means for the plugin.
