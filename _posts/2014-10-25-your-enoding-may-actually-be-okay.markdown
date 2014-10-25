---
layout: post
title:  "Your encoding may be okay, you may just not know it."
date:   2014-10-25 18:49:00
categories: python encoding python2.7
---

I recently realized that dumping a Python dictionary to the terminal may give different results compared to printing a particular value to the screen. It's best to demonstrate by example using the Python repl.


{% highlight python %}
>>> dict = {"str": "à"}
>>> print(dict)
{'str': '\xc3\xa0'}
>>> print(dict['str'])
à
>>> repr(dict['str'])
"'\\xc3\\xa0'"
{% endhighlight %}

As you can see dumping the dictionary to the terminal does not display the special characters. Because of this I spent far too long (40 minutes?) trying to figure out why the source of my text was faulty, when in fact it was simply the way I was displaying the text that was faulty.

Lets try the same code using python 3.4

{% highlight python %}
>>> dict = {"str": "à"}
>>> print(dict)
{'str': 'à'}
>>> repr(dict['str'])
"'à'"
{% endhighlight %}

As we can see here Python 3.4 has no problem handling these characters.
