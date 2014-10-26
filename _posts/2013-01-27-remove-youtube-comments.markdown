---
layout: post
title: Remove Youtube Comments
date: 2013-01-27 10:08:51.000000000 -05:00
categories:
- tech
tags:
- comments
- python
- python 3.2
- youtube
type: post
---
I recently wrote a Python 3.2 script which will remove your Youtube comments automatically for you. It works reasonably well, but it did require me to run it a few times in order to get everything. It uses the Google/Youtube API to accomplish the deletion, but some "hackery" was needed in order to find the comments which need to be deleted. This "hackery" entails scraping Google web search results, something which is not entirely kosher. As such, you may find that Google will temporarily block your requests being made from this script, as they did to me at one point. This is fine because the proper method of access Google search remained open and I was in no rush since this is just for personal use, so I just waited it out. I have since inserted a delay command in the script to make the requests look a little more human like, but of course that does cause the script to take longer. I have not included the Google app id I used to run this script, you will have to create your own at https://code.google.com/apis/console/. You will also have to update the script to use your own Youtube username. The places in the code that require your attention most have exclamation marks in the comments. I used this script on Ubuntu 12.10 with curl installed. The script has been formatted for this blog, if an error arises it may be around a long string that was made to occur on multiple lines after the fact.
{% highlight python %}import lxml.html
import urllib.request
import urllib.parse
import json
import webbrowser
import time
import sys
import subprocess
##### get authenticated #####
client_id = "847147874147.apps.googleusercontent.com" ## need to get your own !!!!!!!!!
client_secret = "92dGuyKje6DncGo8EOe8GYKJ" ## need to get your own !!!!!!!!!
apiKey = "MIzjSyQmSZ3U18vmD1esxF-gQU2ZmXKdf5FQoCW" ## need to get your own !!!!!!!!!
url = "https://accounts.google.com/o/oauth2/auth?client_id=" +\
      "{0}&amp;response_type=code&amp;scope=https://www.googleapis.com/auth/youtube".format(client_id) +\
      "&amp;redirect_uri=urn:ietf:wg:oauth:2.0:oob"
webbrowser.open(url)
time.sleep(2) # required for clean output display
code = input('paste in your code: ')
values = {"code" : code,
          "client_id" : client_id,
          "client_secret" : client_secret,
          "grant_type" : "authorization_code",
          "redirect_uri" : "urn:ietf:wg:oauth:2.0:oob"}
url = "https://accounts.google.com/o/oauth2/token"
data = urllib.parse.urlencode(values).encode('utf-8')
request = urllib.request.Request(url, data=data,\
                           headers={"Host": "accounts.google.com",\
                           "Content-Type": "application/x-www-form-urlencoded"})
try:
    page = urllib.request.urlopen(request)
except:
    print("Was not able to authenticate, quitting.")
    sys.exit(0)
authJson = json.loads(page.read().decode('utf-8'))
access_token = authJson['access_token']
refresh_token = authJson['refresh_token'] #unused
if not access_token:
    print ("was not able to authenticate, quitting.")
    sys.exit(0)
########## get links from google #########
username = "INSERT_YOUR_USERNAME_HERE!!!!!!!!!!!!!" ## !!!!!!!!!!!!!!!!!!
query = "site:youtube.com%2Fall_comments+-site%3Am.youtube.com%20" + username
start = 0 #page count, goes by 10s
user_agent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/535.20' +\
       ' (KHTML, like Gecko) Ubuntu/12.04 Chromium/18.0.1025.168 Chrome/18.0.1025.168 Safari/535.19'
linkArray = []
while True:
    url = "https://www.google.ca/search?q=" + query + "&amp;start=" + str(start)
    page = urllib.request.urlopen(urllib.request.Request(url, headers={'User-Agent': user_agent}))
    pageStr = page.read().decode('utf-8')
    html = lxml.html.fromstring(pageStr)
    result = html.xpath('//a[@class="l"]')
    if not result:
        break;
    else:
        start = start + 10
        time.sleep(10) # be nice to google
    for i in result:
        link = i.get('href')
        linkArray.append(link)
###### get comment ids ######
commentLinkArray = []
for i in linkArray:
    try:
        videoId = i[38:]
        url = "http://gdata.youtube.com/feeds/api/videos/{0}/comments?alt=json".format(videoId)
        commentJson = json.loads(urllib.request.urlopen(url).read().decode('utf-8'))
        for x in commentJson['feed']['entry']:
            if x['author'][0]['name']['$t'] == username:
                print ("sssss")
                commentLinkArray.append(x['link'][2]['href'])
    except:
        pass
##### Perform the deletion ######
for i in commentLinkArray:
    print ("Deleting:", i)
    call = ["/usr/bin/curl", "-X", "DELETE", "{0}".format(i),
                             "-H", "Authorization: Bearer {0}".format(access_token),
                             "-H", "X-GData-Key: key={0}".format(apiKey),
                             "-H", "GData-Version: 2",
                             "-H", "Content-Type: application/atom+xml",
                             "-H", "Host: gdata.youtube.com",
                             "-o", "/dev/null"]     ## not sure if this did anything
    subprocess.call(call)
{% endhighlight %}
