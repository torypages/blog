---
layout: post
title: Apache Alias / Directory + Sharing Files Locally
date: 2010-08-01 19:02:36.000000000 -04:00
categories:
- tech
tags:
- apache2
- bash
- linux
- share files
- ubuntu
type: post
---
Here is how I shared a directory on a web sever so that people could simply download some files. I made a change to the /etc/apache2/sites-available/default file, which is actually the exact same file as the /etc/apache2/sites-enabled/default file. The file in the sites-enabled folder is just a symbolic link to the sites-available file. It is done this way so that one can simply run the a2ensite to enable the site or a2dissite to disable the site or course followed by an apache restart with the command "service apache2 restart." a2ensite and a2dissite can be though of as <strong>a</strong>pache<strong>2en</strong>able<strong>site</strong> and <strong>a</strong>pache<strong>2dis</strong>able<strong>site</strong>.
so, as I was saying. In the /etc/apache2/sites-available/default file I added the following within the <VirtualHost *:80> section: 
{% highlight bash %}    Alias /publicFiles/ "/home/tory/publicFiles"
    <Directory "/home/tory/publicFiles">
	       Options Indexes MultiViews FollowSymLinks
               AllowOverride None
               Order deny,allow
               Deny from all
               Allow from 192.168.1.0/24
    </Directory>{% endhighlight %}
This will only allow people on my local network to access the file via the address http://myServer/publicFiles/. When people go there they will be greeted with a plain file listing. Within the folder /home/tory/publicFiles I did things like:
{% highlight bash %} $ ln -s /mnt/coolDrive/coolFileToShare.jpg heySisterCheckThisOut.jpg{% endhighlight %}
 and of course all files need the proper permissions.... and no, that does not mean it is time to whip out the chmod 777 * -R ... lol 
