---
layout: post
title: Custom Home Folder Sub-Folders
date: 2010-07-20 21:50:11.000000000 -04:00
categories:
- tech
tags:
- '10.04'
- home folder
- linux
- lucid
- path
- ubunu
type: post
---
In my case I don't like th folders in my home folder being capitalized. In order to change what your home folder sub-folders are you need to edit `~/.config/user-dirs.dirs` and from there it is pretty self explanatory. You'll have to manually create the folders I believe. And then restart Nautilus with `killall nautilus` it will start up back again automatically. All thanks to this posting goes to <a href="http://www.howtogeek.com/howto/17752/use-any-folder-for-your-ubuntu-desktop-even-a-dropbox-folder/">http://www.howtogeek.com/howto/17752/use-any-folder-for-your-ubuntu-desktop-even-a-dropbox-folder</a>
