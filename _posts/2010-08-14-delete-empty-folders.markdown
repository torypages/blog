---
layout: post
title: Delete Empty Folders
date: 2010-08-14 07:21:57.000000000 -04:00
categories:
- tech
tags:
- bash
type: post
---
This command will delete all the empty folders in your current directly and all its children.
{% highlight bash %} $ find . -depth -type d -empty -exec rmdir "{}" \;{% endhighlight %}
<ul>
<li>The "<strong>.</strong>" specifies that we want to search from our current directory.</li>
<li>"<strong>-depth</strong>" gives a recursive <em>effect</em> to this.
	</li>
<li>The "<strong>-type d</strong>" specifies that we only want to return directories.</li>
<li>"<strong>-empty</strong>" finds regular files and folders but the "-type d" part makes it so that we only find directories.</li>
<li>The items found after "<strong>-exec</strong>" are to be executed each time an item is found. </li>
<li>"<strong>rmdir</strong>" removes only empty directories. One could also write "rm  -r" but that is less safe since it will also remove directories that are not empty.</li>
<li>The " <strong>"{}"</strong> " acts sort of like a variable that holds the name of the item that was found. The double quotes around it may not be needed, but, it didn't hurt the execution of the command when I ran it.</li>
<li>And, the "<strong>\;</strong>" finishes it all off.</li>
</ul>
