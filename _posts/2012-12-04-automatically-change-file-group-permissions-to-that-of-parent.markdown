---
layout: post
title: Automatically change file group permissions to that of parent
date: 2012-12-04 15:06:18.000000000 -05:00
categories:
- tech
tags:
- "/var/www"
- bash
- commands
- group
- linux
- permissions
- ubuntu
- www-data
type: post
---
I was working with /var/www for some web stuff. The group and user owner of /var/www is www-data (Apache). I wanted to be able to  work in in /var/www with my own user. So, I added myself to the www-data group with the command "usermod -a -G www-data myUser" and then I ran "chmod g+s /var/www -R" which now means that every file created under /var/www becomes part of the /var/www group, even files created by me.
