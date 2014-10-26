---
layout: post
title: Libreoffice Grammar and Spelling Checker
date: 2012-09-13 22:40:10.000000000 -04:00
categories:
- tech
tags:
- AfterTheDeadline
- grammar check
- LanguageTool
- libreoffice
- linux
- spell check
type: post
---
I had some problems with the Libreoffice 3.6.0.1 spellchecker and grammar checker in Writer today. I tried to install the <a href="http://extensions.libreoffice.org/extension-center/languagetool" target="_blank">LanguageTool</a> extension but it was giving me permission issues that I wasn't able to quickly fix. I had upgraded from an earlier version of Libreoffice today and I read that the old user profile might be causing an issue so I also removed ~/.config/libreoffice to no avail. I then removed and purged libreoffice-* and reinstalled. I was then able to install the language tool, as well as the hunspell-en-ca (Canadian English) Ubuntu package. As of this evening, I have a very nice Libreoffice installation, complete with grammar and spell checking! On a side note, I do recognize that this was more troublesome than it should have been, spell checking and grammar checking should be pretty standard, and something that every user makes use of, including those less tolerant than me. But, what can I say? Beggars can't be choosers, it's libre and gratis. Lastly, <a href="http://afterthedeadline.com/" target="_blank">afterthedeadline.com</a> is a language tool I use for this blog, and it works pretty well. It can also be used with Libreoffice but I am hesitant to do so. It is a cloud service that checks your language use, and since unlike my blog, usually the content I produce in Libreoffice is private it constitutes a privacy/security threat. It is possible to run your own After the Deadline server but that is a fair amount of effort for spelling and grammar and in addition it takes up a fair amount of system resources.

Update: Strangely, a friend just asked me about this, a few days after I had this problem. I supplied him with some instructions that I think would be useful for people who might end up here. The following should probably get you a good Libreoffice install. I haven't tested them, but they should be pretty close. There are some unnecessary commands here, but, I'm just sort of choosing the nuclear option to kill an ant.

{% highlight bash %}
 apt-get remove libreoffice-*
apt-get purge libreoffice-*
rm ~/.config/libreoffice/ -r
add-apt-repository ppa:libreoffice/libreoffice-prereleases
apt-get update
apt-get dist-upgrade
apt-get install libreoffice
apt-get install hunspell-en-ca
{% endhighlight %}
Then install this plugin <a href="http://extensions.libreoffice.org/extension-center/languagetool" rel="nofollow" target="_blank">http://extensions.libreoffice.org/extension-center/languagetool</a> and restart Libreoffic<wbr>e.</wbr>
Update: In Ubuntu 13.04 with Libreoffice 4.0.2.2 and Language Tool 2.1 I needed to install libreoffice-java-common in order to install Language Tool.
