---
layout: post
title: Python3 - all threads complete, return data example
date: 2012-12-22 21:42:16.000000000 -05:00
categories: []
tags: []
type: post
---
This is simply a demonstration of threading in Python.  In addition to threading it also has awareness of when the threads are complete and be able to return data.
Code:
{% highlight python %}#!/usr/bin/env python3
import time, threading, queue
def print_t(name, delay, q):
    q.put("I am data from " + name)
    for i in range(1,10):
        time.sleep(delay)
        print (name)
q1 = queue.Queue()
q2 = queue.Queue()
t1 = threading.Thread(target=print_t, args=("First Thread", 1, q1))
t2 = threading.Thread(target=print_t, args=("Second Thread", 2, q2))
t1.start()
t2.start()
t1.join()
print ("First Thread complete")
t2.join()
print ("All Threads complete")
print (q1.get())
print (q2.get()){% endhighlight %}
Output:
{% highlight python %}First Thread
First Thread
Second Thread
First Thread
First Thread
Second Thread
First Thread
First Thread
Second Thread
First Thread
First Thread
Second Thread
First Thread
First Thread complete
Second Thread
Second Thread
Second Thread
Second Thread
Second Thread
All Threads complete
I am data from First Thread
I am data from Second Thread{% endhighlight %}
