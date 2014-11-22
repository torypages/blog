---
layout: post
title:  "Using Eclipe with Python Behave"
date:   2014-11-22 15:06:00
categories: python eclipse behave bdd pydev
---

Two things I want to accomplish. Get rid of PyDev errors and use the Eclipse "run" functionality. 

Get rid of "unused wild import" `from behave import when, then` instead of `from behave import *` in `steps.py`

Get rid of `Unresolved Import: when` and `Unresolved Import: then` by adding "Behave" as a "Forced Builtin." To do this, `Window>Preferences>PyDev>Interperters>Python Interperter>Forced Builtins>New` Make sure that during that process your Python interpreter is setup correctly including the your libraries path. I had to close steps.py and re-open it before the errors went away.

Get rid of `Duplicated signature: step_impl`, for this I will just turn off error checking for duplicate signatures since this isn't really an error I make very often. To turn it off, `Window>Preferences>PyDev>Editor>Code Analysis>Others>Duplicated Signature>Ignore`

Setup the built in run. "Run>Run Configurations>Python Run" Create a new run. In Main>Project set your project. For the Main Module choose your version of "/home/tory/Desktop/demo/virtual_env/bin/behave" Inside the Arguments tab set the Working Direcotry, this is where your something.feature file exists. And now you should be set. Typically when you run "behave" outiside of the IDE you just type "Behave". This simply launches a Python executable with a shebang at the top. From the IDE (Eclipse) we are simply are calling the Behave executable using a python executable instead of relying on the shebang. We also have to specify the current working directory.