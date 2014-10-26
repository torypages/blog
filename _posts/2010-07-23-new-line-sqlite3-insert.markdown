---
layout: post
title: New Line + sqlite3 + Insert + Bash
date: 2010-07-23 20:48:52.000000000 -04:00
categories:
- tech
tags:
- bash
- insert
- line
- linebreak
- linux
- newline
- sqlite
- sqlite3
type: post
---
In a bash script was having no luck with 
{% highlight bash %}MYTEXT="my text \n to be insert with two \n new lines";
sqlite3 mydb.db "INSERT INTO myTable VALUES('value1', 'value2', '$MYTEXT')";{% endhighlight %}
because it was literally putting the \n into the database when it should have literally been a new line. This was solved by
{% highlight bash %}MYTEXT=`echo -e "my text \n to be insert with two \n new lines"`;
sqlite3 mydb.db "INSERT INTO myTable VALUES('value1', 'value2', '$MYTEXT')";{% endhighlight %}
the -e on the echo allows for the new line characters to be interpreted as new lines as opposed to just a backslash and an n.
