---
layout: post
title: SSHFS + Nautilus + Delete Folders with Files
date: 2012-10-26 18:45:43.000000000 -04:00
categories:
- tech
tags:
- autofs
- bash
- delete
- folder
- nautilus
- sshfs
- ubuntu
type: post
---
I am using autofs in conjunction with SSHFS to mount some remote folders. When attempting to remove/delete non-empty folders from the mounted folder using Nautilus I ran into issues. I would get "Error removing file: Operation not permitted" or "Error while deleting." The workaround I used to overcome this issue was to create a folder named .Trash in the mounted folder with 1777 permissions. One aspect to note here is that if you mounted a folder located on the remote file system at /mnt/content/a/ for example, and you are trying to remove a folder named "coffee" located at /mnt/content/a/b/c/d/e/coffee/ on the remote server .Trash needs to exist in /mnt/content/a/ <span style="text-decoration: underline;">not</span> /mnt/content/a/b/c/d/e/. But! Then the files will be left in .Trash, thus, you will need to account for that, perhaps through a cronjob.
