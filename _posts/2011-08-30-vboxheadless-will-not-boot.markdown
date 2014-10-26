---
layout: post
title: VBoxHeadless will not boot
date: 2011-08-30 03:53:38.000000000 -04:00
categories:
- tech
tags:
- vboxheadless
- virtualbox
type: post
---
I'm running on some old hardware so perhaps that is the issue (P4) but I have had issues booting virtualbox machines. The machine would load grub, but after that I would just receive a blinking cursor. To fix this I had to modify some of the virtual CPU options
{% highlight bash %}VBoxManage modifyvm "vmName" --acpi on --ioapic on --pae on{% endhighlight %}
