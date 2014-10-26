---
layout: post
title: POCO Install/Compile Error Fix
date: 2013-01-31 19:50:30.000000000 -05:00
categories:
- tech
tags:
- c++
- poco
type: post
---
I was in the process of installing the <a href="http://pocoproject.org/">POCO Libraries</a> on Ubuntu 12.10 for C++ and while compiling I received this error: ODBC.make:49: *** No ODBC library found. Please install unixODBC or iODBC or specify ODBCLIBDIR and try again. I fixed it with:
{% highlight bash %}apt-get install unixodbc-dev libmysqlclient-dev{% endhighlight %}
