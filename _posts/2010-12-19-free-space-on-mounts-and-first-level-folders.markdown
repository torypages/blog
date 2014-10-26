---
layout: post
title: Free Space on Mounts and First Level Folders
date: 2010-12-19 15:56:57.000000000 -05:00
categories:
- tech
tags: []
type: post
---
On a server I work with I have multiple physical volumes mounted under /mnt. Frequently I want to know the amount of free space available on each volume in addition to the first level contents that exist on the volume. The following command when run from /mnt or probably any place where your volumes are mounted will display the required information.
{% highlight bash %}for i in `ls`; do echo -ne "\E[1;31m$i "; df -h -P | grep `echo -n \`pwd\`; echo -n "/$i"` | awk '{print $4}'; tput sgr0; ls $i --color=never; echo ""; done;{% endhighlight %}

<ol>The steps that are taken are as such:
<li>obtain a list of the contents in your current working directory</li>
<li>for each item on the list just obtained, get its absolute path. The absolute path is needed because this is what will be used to look up the space available on the df output</li>
<li>now that we have the absolute path for a particular mounted volume, display the relevant column and row from the df output to get the proper available space</li>
<li>echo the folder name and the available space left in bold red. The bold red is why the "\E[1;31m" craziness exists</li>
<li>finally, list the first level contents of each drive without any special colouring</li>
</ol>
I am not sure how this will translate to other situations, but, to make it a bit easier to understand my situation I may have several drives mounted like this: /mnt/sda1 /mnt/sdb3 /mnt/sdc2 and so on.
