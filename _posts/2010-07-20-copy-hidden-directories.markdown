---
layout: post
title: Copy Hidden Directories
date: 2010-07-20 20:41:37.000000000 -04:00
categories:
- tech
tags:
- bash
- linux
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _syntaxhighlighter_encoded: '1'
  _wp_old_slug: ''
author:
  login: admin
  email: wordpress@torypages.com
  display_name: admin
  first_name: Tory
  last_name: Law
---
I used this to duplicate the majority of my home folder
{% highlight bash %}~$ cp `ls -a | egrep '^\.[^.]'` ../newHome -r{% endhighlight %}
every output line of ls -a is fed into egrep and filtered based on lines that start with . but do not start with .. because it is required that the line start with a . because of ^\. the \ exists in order to treat the . literally and not as a wild card. Then [^.] says that the character following the first . at the beginning of the line cannot be a .   .  The, for each out that remains after the filter cp is ran using that output as the source and ../newHome as the destination. And finally, the -r does a recursive copy.

Caution: As I am finishing up this blog post I am wondering why my laptop is still griding away at the copy, it is because there was a ton in the trash folder which is located in .local.

Though this does work in theory for your home folder it didn't work so well, but, the idea still works for copying hidden files or even filtering the items that you wish to copy
