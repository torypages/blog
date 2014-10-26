---
layout: post
title: Character Issues and MySQL
date: 2010-08-13 19:23:05.000000000 -04:00
categories:
- tech
tags:
- bash
- chacters
- language
- MYSQL
type: post
---
A lot of times I work with databases with French characters or other characters and I finally found a solution that has helped in my particular case thus it may help in yours. It is to specify the the character encoding right on the command line with 
{% highlight bash %}--default-character-set=utf8{% endhighlight %}
so, a command might look like
{% highlight bash %} $ mysql -u Tory -pMyPassword --default-character-set=utf8 < commandsToRun.sql {% endhighlight %}
