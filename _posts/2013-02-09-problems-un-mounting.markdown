---
layout: post
title: Problems Un-mounting
date: 2013-02-09 12:42:05.000000000 -05:00
categories:
- tech
tags:
- linux
type: post
---
I had some problems un-mounting an sshfs mount. I did a "umount -fl" (force, lazy) and tried re-mounting, but I continued to have issues. I fixed the issue with a combination of un-mounting and clearing the mount entry from /etc/mtab which entailed simply opening /etc/mtab in a text editor and removing the relevant line entry.
