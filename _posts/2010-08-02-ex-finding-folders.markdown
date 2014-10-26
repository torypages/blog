---
layout: post
title: Finding Folders
date: 2010-08-02 08:32:38.000000000 -04:00
categories:
- tech
tags:
- bash
- find
- linux
type: post
---
This command will find all folders in your current directory and deeper where the folder name has the word "tea", and then later followed by "cup" with anything in between. This will also ignore case.
{% highlight bash %}$ find ./ -iname *tea*cup* -type d{% endhighlight %}
<ul>
This will match folder names such as
<li>tea cup</li>
<li>TeA and Cup</li>
<li>wickedTeaCUP</li>
<li>super.tea.in.my.cup.right.now</li>
</ul>
