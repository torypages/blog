---
layout: post
title: SSH keys still asking for password
date: 2013-03-02 15:03:26.000000000 -05:00
categories:
- tech
tags:
- linux
- ssh
type: post
---
After attempting to setup an SSH key-pair for passwordless logins, I was still being asked for a password. It turns out that the remote system was taking issue with the fact that the remote user's home directory (as defined in /etc/passwd) was not in the standard /home location. After changing the home location (via /etc/passwd) they key-pair worked. I'm sure there is a way to allow for the use of a non-standard home location and SSH key-pairs, but, I worked around the issue instead.
