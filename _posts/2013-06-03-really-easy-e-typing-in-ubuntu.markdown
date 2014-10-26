---
layout: post
title: Really easy é typing in Ubuntu
date: 2013-06-03 22:28:06.000000000 -04:00
categories:
- tech
tags:
- compose key
- "é"
- french
- key map
- keyboard layout
- linux
- special characters
- ubuntu
type: post
---
I do have my compose key setup so that typing an é can be a matter of pressing <composekey> + e + ' but é seems to come up enough with French that having an even easier way to type it would be handy. So, I did what I have shown below. Now I can type an é by pressing <composekey> + e + e (one after another). It is the same amount of keystrokes, but considerably easier. After adding the following content to the following files I needed to log out and back in again.
~/.XCompose
{% highlight bash %}include "%L"   # import the default Compose file for your locale
<Multi_key> <e> <e>     : "é"{% endhighlight %}
~/.profile
{% highlight bash %}export GTK_IM_MODULE="xim"{% endhighlight %}</composekey></composekey>
