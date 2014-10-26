---
layout: post
title: Redirect Within Body
date: 2010-07-15 09:42:42.000000000 -04:00
categories:
- tech
tags:
- development
- html
- javascript
- web
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _syntaxhighlighter_encoded: '1'
  _wp_old_slug: ''
author:
  login: admin
  email: wordpress@torypages.com
  display_name: admin
  first_name: Tory
  last_name: Law
---
If you do not have access to the head of a webpage it is useful to know how to do a redirect within the body. Here is an example:
{% highlight html %}
<html>
     <head>
          <title> test </title>
     </head>
     <body>
          <script language="javascript">
               location.replace('http://www.torypages.com');
          </script>
     </body>
</html>
{% endhighlight %}
