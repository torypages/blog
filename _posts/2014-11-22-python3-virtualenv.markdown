---
layout: post
title:  "Creating a Python3 virtual environment"
date:   2014-11-22 15:00:00
categories: python python3 virtualenv
---

{% highlight bash %}
virtualenv -p /usr/bin/python3 virtual_env
{% endhighlight %}

Where virtual_env is arbitrary.

Also may be required,

{% highlight bash %}
sudo apt-get install python-pip
sudo pip install virtualenv
{% endhighlight %}

