---
layout: post
title: UnicodeEncodeError
date: 2012-10-02 11:47:39.000000000 -04:00
categories: []
tags:
- error
- python
type: post
---
I was getting this error
{% highlight bash %}UnicodeEncodeError: 'ascii' codec can't encode character u'\xae' in position 44817:
ordinal not in range(128){% endhighlight %}
when trying to dump some output from Python. Since I was only temporarily trying to dump some output I found that surrounding my offending content with repr(), that is, "print (repr(offendingContent))" while printing solved the problem good enough for the context.
