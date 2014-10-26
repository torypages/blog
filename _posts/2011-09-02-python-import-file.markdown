---
layout: post
title: Python import file
date: 2011-09-02 15:12:38.000000000 -04:00
categories:
- tech
tags:
- import
- python
- snippet
type: post
---
Avoid installing modules and just import them
{% highlight python %}
import os, sys
 cmd_folder = os.path.dirname(os.path.abspath(__file__))
 if cmd_folder not in sys.path:
     sys.path.insert(0, cmd_folder)
import moduleName
{% endhighlight %}
Source: <a href="http://stackoverflow.com/questions/279237/python-import-a-module-from-a-folder">http://stackoverflow.com/questions/279237/python-import-a-module-from-a-folder</a>
