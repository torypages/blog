---
layout: post
title: Facebook + webclient + xmpp
date: 2012-12-05 18:58:26.000000000 -05:00
categories:
- tech
tags:
- chat
- facebook
- linux
- openfire
- PHP
- python
- ubuntu
- xmpp
type: post
---
I don't necessarily like how we are becoming as dependant on web services as we are. Perhaps I have some tinfoil hat syndrome symptoms, but putting total control in other people's hands doesn't seem right. Web services certainly have their place, and I will certainly continue to use them, but where possible and appropriate, it would be nice to take control again. The decision, in part, depends on expectations. Some things are meant to be public, or there is an expectation that certain actions might be public or not totally private, and in these scenarios, web services are more appropriate. Given this idea of when web services are appropriate, it seems to me that instant messaging is a place where the user should have a fair amount of control. The things I say in an instant messaging session are things that I expect to be between me and the other party or parties whom I know are involved. This is why I am becoming uncomfortable with Facebook chat. Comparatively, unlike with other uses of Facebook, I am not comfortable with my instant messages being data mined. As such, I wanted to see if there is a way where I could reduce my dependence on Facebook chat.

My idea is that I could create an alternative way for people to contact me. The alternative should be easy to use as to not burden the user attempting to contact me. It must to a degree, integrate with Facebook since that is where people are, and that is how people would often attempt to start an instant messaging session with me. Some Facebook integration would reduce the need for people to register entirely new accounts, and put up with the hassle of doing so. It also serves as a great way for me to authenticate who they are. If users were registering new accounts, I would have to take it on faith that they are who they say they are. By linking with Facebook I get the assurance that the person I am communicating with, is the person behind the Facebook account which I have grown to trust. Now, of course this creates the possibility that someone has been a fraud for many years, but, there is only so much I can do.

To accomplish this, I set out to create a web XMPP client which could use Facebook authentication which would then communicate with my XMPP instant messaging account. The rest of this post will detail many of the experiences I had. It will include much if not all of the code to make this happen, however the code will be altered at times for this post. The code isn't final and I don't consider it to be of production quality but it does get the job done, and for personal experiments, this is often good enough.

<a href="https://dl.dropboxusercontent.com/s/09x6n79f32m6pv6/systemMap24.png"><img class="alignleft size-full wp-image-1535" style="float: left; margin-right: 10px;" title="systemMap2" src="https://dl.dropboxusercontent.com/s/09x6n79f32m6pv6/systemMap24.png" alt="" width="465" height="931" /></a>The diagram here is meant to give a general overview of what is going on. It isn't a super precise diagram, I only included the details that I thought were important. There are also certainly important aspects which I have chosen to entirely not include, like third-party libraries that serve as key components to the system. The numbers listed, do not necessarily indicate the order in which items are processed.


<ol style="margin-left: 490px;">
<li style="margin-bottom: 15px;">Actual movement between pages occurs here. If the user is not logged in they are redirected to Facebook.</li>
<li style="margin-bottom: 15px;">User is sent back, index.php is reloaded but this time being logged in.</li>
<li style="margin-bottom: 15px;">Creates user if does not exist.</li>
<li style="margin-bottom: 15px;">Web browser moves to a new webpage. Also sends Facebook user ID.</li>
<li style="margin-bottom: 15px;">chat.py instantiates a listener</li>
<li style="margin-bottom: 15px;">When listener receives a message it writes in the chat log.</li>
<li style="margin-bottom: 15px;">chat.py includes chat.js</li>
<li style="margin-bottom: 15px;">When the message button on chat.py is pressed chat.js pushes the message to send.py so that it can sent.</li>
<li style="margin-bottom: 15px;">Periodically chat.js checks with chatReader.py to see if there are new messages to display.</li>
<li style="margin-bottom: 15px;">chatReader.py monitors the chat log to see if there is anything new.</li>
<li style="margin-bottom: 15px;">Periodically chat.js tells pidAlive.py that the session is still open.</li>
<li style="margin-bottom: 15px;">pidAlive.py makes note of the fact that the session is still alive.</li>
<li style="margin-bottom: 15px;">chat.py includes style.css.</li>
<li style="margin-bottom: 15px;">cron periodically run  pidKiller.py</li>
<li style="margin-bottom: 15px;">pidKiller.py checks with PID records to see the status of chat sessions.</li>
<li style="margin-bottom: 15px;">Upstart tells fbReplyBot.py to run.</li>
</ol>
<div style="clear: both; height: 0px"></div>
<!--more-->
<h2 style="padding-top:25px;">index.php</h2>
index.php is the landing page for the web application. This is the first page that people will reach when visiting the web application. My preference would have been to use Python for the entire application, but due to the fact that this part of the application requires access to the Facebook API it was easier for me to simply use PHP here. This was in part because I have worked with the PHP Facebook API before. I know there is the unofficial <a href="https://github.com/pythonforfacebook/facebook-sdk/" target="_blank">Python Facebook API</a> and in fact I have used it before, and it works well, but not in a web setting. I am only thinking about it now, but there is also the Facebook Javascript API which perhaps could have been used. This is something to consider for a hypothetical version 2. But as I have mentioned a few times on this blog, I don't know Javascript, I just use it from time to time. The frequency that this happens make me think I should just learn the basics.

In the usual new user process, the user comes to the page and is redirected to a login page controlled by Facebook. The user is then redirected back to this page, however, this time they are logged in. Each and every person who uses this application becomes registered with my local <a href="http://www.igniterealtime.org/projects/openfire/" target="_blank">Openfire XMPP server</a>. The user is not aware of this fact. The user's Openfire account is linked with their Facebook account. If the Facebook account has not already been linked to an Openfire account, the link will be made, if so, this linking process will be skipped over. The final function of this script is to notify the user of the consequences of me using a self-signed SSL certificate. Ultimately, potentially scaring some people with the self-signed certificate is better than not using SSL and is better than me paying the money to get a proper certificate. This part of the program also showed me how to do some browser detection.

{% highlight php %}
<?php
require './facebook-php/src/facebook.php';
// setting info for Facebook application
$facebook = new Facebook(array(
  'appId'  : '222',
  'secret' : '22',
));
// Get User ID
$user = $facebook->getUser();
// We may or may not have this data based on whether the user is logged in.
//
// If we have a $user id here, it means we know the user is logged into
// Facebook, but we don't know if the access token is valid. An access
// token is invalid if the user logged out of Facebook.
if ($user) {
  try {
    // Proceed knowing you have a logged in user who's authenticated.
    $user_profile = $facebook->api('/me');
  } catch (FacebookApiException $e) {
    error_log($e);
    $user = null;
  }
}
// if user is logged in
if ($user) {
	// check if not registered
		// register
		$fbId = $user_profile['id'];
		$fbFname = $user_profile['first_name'];
		$fbLname = $user_profile['last_name'];
		$fbName = "$fbFname%20$fbLname";
		// generate password for user
		$password = censored
		$secret = "censored";
                // attempt to make new account on openfire server
		// the line below requires the User Service plugin on Openfire
                $url = "http://censored:9090/plugins/userService/userservice? \
                type=add&amp;secret=$secret&amp;username=fb_$fbId&amp;password=$password&amp;name=$fbName";
		$curl_handle = curl_init();
		curl_setopt( $curl_handle, CURLOPT_URL, $url );
		curl_setopt( $curl_handle, CURLOPT_RETURNTRANSFER, true ); // Fetch the contents too
		$html = curl_exec( $curl_handle ); // Execute the request
		curl_close( $curl_handle );
		$onlyGet = //security mechanism for maintaining session, sort of like a token
		$toPost = array(
			"fbId" : $fbId,
			"fbName" : "$fbFname $fbLname",
			"onlyGet" : $onlyGet
		);
                // The block directly below doesn't really matter and I could
                // probably remove it except maybe the last else. Whether the
                // user already exists or doesn't, it doesn't really change
                // anything. The final else could be important because unlike
                // with the first two, there is no user at this point, and
                // the rest of the application will not work. As I am
                // preparing this for the web, I am thinking I will just
                // redirect the user to a generic error page. I am not too
                // concerned about this, because the chances of the final if
                // statement being triggered is unlikely.
		if (trim($html) == "<result>ok</result>"){
			//header("location:$chatUrl");
                        //do nothing
		} elseif (trim($html) == "<error>UserAlreadyExistsException</error>") {
			//header("location:$chatUrl");
                        //do nothing
		} else {
			echo "I don't really know what happened...";
                        // in hindsight, I probably forgot to address this. Job for V2
		}
} else {
  // not logged in, go to facebook to login
  $loginUrl = $facebook->getLoginUrl();
  header("Location:$loginUrl");
}
?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
     "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html>
	<head>
		<style>
		    // deleted to save space
		</style>
		<script src="jquery-1.8.3.min.js"></script>
		<script language="JavaScript">
                // lesson on browser detection
		$(document).ready(function(){
			var isChrome = /Chrome/.test(navigator.userAgent) \
                                    && /Google Inc/.test(navigator.vendor);
		        var isSafari = /Safari/.test(navigator.userAgent) \
                                    && /Apple Computer/.test(navigator.vendor);
		if ($.browser.mozilla == true){
    			$("#chrome").hide();
    			$("#safari").hide();
    			$("#ie").hide();
    			$("#opera").hide();
    		}
    		else if (isChrome){
    			$("#firefox").hide();
    			$("#safari").hide();
    			$("#ie").hide();
    			$("#opera").hide();
    		}
    		else if (isSafari){
    			$("#firefox").hide();
    			$("#chrome").hide();
    			$("#ie").hide();
    			$("#opera").hide();
    		}
    		else if ($.browser.opera == true){
    			$("#firefox").hide();
    			$("#chrome").hide();
    			$("#ie").hide();
    			$("#safari").hide();
    		}
    		else if ($.browser.msie == true){
    			$("#firefox").hide();
    			$("#chrome").hide();
    			$("#opera").hide();
    			$("#safari").hide();
    		}
		});
		</script>
	</head>
<body>
<!-- warn people about the self signed certificate -->
<div id="notice">
	Please be advised that you will likely have to allow
        for security exception as described below,
        <a href="#" onclick="document.frm.submit();">click here to continue</a>.
</div>
<div id="chrome">
	<h1>Chrome</h1>
	<img src="images/chromeWarn1.png" />
</div>
<div id="firefox">
	<h1>Firefox</h1>
	<img src="images/ffWarn1.png" />
	<img src="images/ffWarn2.png" />
	<img src="images/ffWarn3.png" />
</div>
<div id="safari">
	<h1>Safari</h1>
	<img src="images/safari.png" />
	<div id='original'>
		This image originally appeared
                at http://magma.maths.usyd.edu.au/magma/faq/sslcertificate
	</div>
</div>
<div id="opera">
	<h1>Opera</h1>
	<img src="images/operaWarn.png" />
</div>
<div id="ie">
	<h1>Inernet Explorer</h1>
	Okay, I really wasn't in the mood to accommodate
        Internet Explorer, so just go
        <a href="http://magma.maths.usyd.edu.au/magma/faq/sslcertificate#sec_5">
        here</a> if you need information on it, or just go
        <a href="http://www.google.com/chrome">here</a> and save yourself.
</div>
<form action='https://example.com/webxmpp/chat.py' method='post' name='frm'>
<?php
//just a way to pass on some data to the next page
     foreach ($toPost as $name : $value) {
         echo "<input type='hidden' name='".$name."' value='".$value."'>";
     }
?>
</form>
</body>
</html>
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">chat.py</h2>
This is really the main part of the application. It is the final destination for a user's web browser and where the chat session occurs. But of course this script connects to other scripts which in turn connect to even more scripts and so on and so forth.

The first thing the script does is receive information sent to it from index.php. It then deals with the handling of listener.py. Every chat session needs an instance of listener.py because this is how messages are received, it listens for messages that are sent to it, that is, specifically, it listens for messages that I send from my XMPP client back to the web client application (the system being described in this blog post). Though, perhaps the situation is better described by saying that every active user needs one instance of listener.py, if there are multiple windows open to the chat application, there is still a need for only one instance of listener.py. The second function this script performs is the handling of listener.py instances. It checks to see if any existing listener.py instances are active, and if not it starts one.

Every instance of listener.py is represented by Linux/the operating system by a <a href="http://en.wikipedia.org/wiki/Process_identifier" target="_blank">Process ID</a>, a PID. This application keeps track of the PID, which user created the PID, and whether the user still has chat windows open that require listener.py. It does this by creating a file in "./pids/" for each instance of listener.py using the PID as the filename. Within each of these files, labelled with the PID of the listener.py instance has a user ID on the first line, and a timestamp on the second line. With all this information I can know what instances of listener.py are active, who owns them, and whether it is time to close the instance. Knowing the ownership of the listener.py instances, or the ownership of the PIDs at this point is important because if the application finds that there is already an active listener.py instance for the user it will not create a new one.

Finally, the code below provides the necessary HTML for the chat window as well as providing some hidden form fields in order to facilitate the movement of data between other parts of the application.

{% highlight python %}
#!/usr/bin/python3.2
print ("Content-Type: text/html")
print ("")
import cgi
import cgitb; cgitb.enable()
import hashlib
import subprocess
import time
import os
# accept data from index.php
formData = cgi.FieldStorage()
fbId = formData.getvalue("fbId")
fbName = formData.getvalue("fbName")
onlyGet = formData.getvalue("onlyGet")
# security mechanism, the expected value should be one of the values on index.php
onlyGetExpected = censored
if onlyGetExpected == onlyGet:
	password = # openfire user password
	jstoken = # a token of sorts
	pidToken = # another token
	# check for existing listeners
	doesPidExist = False
	for root, dirs, files in os.walk("./pids/"):
		for pidFileName in files:
			pidFile = open("./pids/" + pidFileName)
			lines = pidFile.readlines()
			pidToken2 = lines[0][:-1]
			if pidToken == pidToken2:
				doesPidExist = True
				listenPid = pidFileName
	jid = censored + "@xmpp.example.com"
        # create a listener.py instance using subprocess.Popen
        # TODO: fix the fact that this path will not tolerate
        # the application being moved.
	if doesPidExist == False:
		listener = subprocess.Popen(['python3.2', \
                   '/var/www/webxmpp/listener.py', '-j', jid,\
                    '-p', password], stdout=subprocess.PIPE,\
                    stderr=subprocess.STDOUT)
		listenPid = listener.pid
	f = open("./pids/" + str(listenPid), 'w')
	toWrite = pidToken + "\n" + str(time.mktime(time.gmtime()))
	f.write(toWrite)
	f.close()
	print("""
	<html>
		<head>
			<link type="text/css" rel="stylesheet" href="style.css" />
			<script src="jquery-1.8.3.min.js"></script>
			<script language="JavaScript" src="chat.js" ></script>
		</head>
		<body>
                        <div id="chatbox"></div>
			<form name="message" action="">
			<input name="usermsg" type="text" id="usermsg" size="63" />
			<input name="submitmsg" type="submit"  id="submitmsg" value="Send" />
                        <!-- the rest is data I need to share with other -->
                        <!-- parts of the system -->
                        <input name="jsfbid" id="jsfbid" type="hidden" value="%(fbId)s" />
			<input name="jsjid" id="jsjid" type="hidden" value="%(jid)s" />
			<input name="jsname" id="jsname" type="hidden" value="%(jsname)s" />
			<input name="jstoken" id="jstoken" type="hidden" value="%(jstoken)s" />
			<input name="jspid" id="jspid" type="hidden" value="%(jspid)s" />
		        </form>
		</body>
	</html>
	""" % dict(fbName=fbName, fbId=fbId, jid=jid, jsname=fbName, jstoken=jstoken, jspid=listenPid))
	# the line above actually shows a pretty neat way of including data
        # inside a large string. You'll be able to see the dictionary keys
        # are dispersed through the python string
else:
        # invalid token of sorts
	print ("something went wrong") #someone might actually be cheating!
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">chat.js</h2>
This is the Javascript file that accompanies chat.py. It runs several pieces of code periodically feeding information to and from chat.py and the chat window contained within chat.py and other parts of the application. It facilitates the sending of a message when a message is typed and the send button is clicked by taking what is in the new message box located on chat.py and sending it to send.py which will be explained later. This script also monitors the chat log and continuously updates the chat window in chat.py with the chat log's contents. The chat log contains the entire chat. Not only does it contain the entire chat, it contains everything that is inside the chat window in chat.py. In this case, the chat window is specifically referring to the portion of the screen where all previous messages are displayed. And it containing everything that is in the window, also means that it includes HTML. The chat log is not simply just text, but a mixture of text and HTML. The final function of chat.js is to inform the system that listener.py instances should stay alive. It does this by periodically calling pidAlive.py which updates the timestamp of the appropriate PID record located in "./pids/" as mentioned earlier.
{% highlight javascript %}
// jQuery Document
$(document).ready(function(){
	//If user submits the form
	$("#submitmsg").click(function(){
		var clientmsg = $("#usermsg").val();
		var jsfbid = $("#jsfbid").val();
		var jsname = $("#jsname").val();
		var jstoken = $("#jstoken").val();
		var forAjax = {text: clientmsg, fbid: jsfbid, jsname: jsname, jstoken: jstoken};
		// submit data to the send script
		$.post("send.py", forAjax);
                // clear the chatbox
		$("#usermsg").attr("value", "");
		return false;
	});
	//Load the file containing the chat log into the chatbox
	function loadLog(){
		//var menuId = $("ul.nav").first().attr("id");
		var jstoken = $("#jstoken").val();
		var jsfbid = $("#jsfbid").val();
		var request = $.ajax({
			  url: 'chatReader.py',
			  type: "POST",
			  data: {jsjid : $('#jsjid').val(), jstoken: jstoken, jsfbid: jsfbid},
			  dataType: "html"
		});
		request.done(function(html) {
		    var oldHeight = $('#chatbox')[0].scrollHeight;
		    $("#chatbox").html(html)
                    $(function() {
		        var height = $('#chatbox')[0].scrollHeight;
		        if (oldHeight != height){
		            $('#chatbox').animate({scrollTop: height});
	                }
		    });
	        });
        }
        setInterval (loadLog, 1000);	// every second
        function keepalive(){
	    var jsfbid = $("#jsfbid").val();
	    var jspid = $("#jspid").val();
	    var forAjax2 = {fbid: jsfbid, pid: jspid};
	    $.post("pidAlive.py", forAjax2);
        }
        setInterval (keepalive, 180000) // every 3 minutes
});
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">send.py</h2>
The code in this file has been adapted from <a href="http://sleekxmpp.com/getting_started/sendlogout.html" target="_blank">here</a>. I have left many of the original comments intact. This script takes a message and sends it to <i>my</i> XMPP client (not the application being described in this blog post) and makes an entry in the chat log.
{% highlight python %}
#!/usr/bin/python3.2
print ("Content-Type: text/html")
print ("")
import cgi
import cgitb; cgitb.enable()
import sleekxmpp
import hashlib
class SendMsgBot(sleekxmpp.ClientXMPP):
    def __init__(self, jid, password, recipient, message):
        sleekxmpp.ClientXMPP.__init__(self, jid, password)
        # The message we wish to send, and the JID that
        # will receive it.
        self.recipient = recipient
        self.msg = message
        # The session_start event will be triggered when
        # the bot establishes its connection with the server
        # and the XML streams are ready for use. We want to
        # listen for this event so that we we can initialize
        # our roster.
        self.add_event_handler("session_start", self.start)
    def start(self, event):
        """
        Process the session_start event.
        Typical actions for the session_start event are
        requesting the roster and broadcasting an initial
        presence stanza.
        Arguments:
            event -- An empty dictionary. The session_start
                     event does not provide any additional
                     data.
        """
        self.send_presence()
        self.get_roster()
        self.send_message(mto=self.recipient,
                          mbody=self.msg,
                          mtype='chat')
        # Using wait=True ensures that the send queue will be
        # emptied before ending the session.
        self.disconnect(wait=True)
if __name__ == '__main__':
    # get the information sent by chat.js
    form = cgi.FieldStorage()
    message = form.getvalue("text")
    fbId = form.getvalue("fbid")
    fbName = form.getvalue("jsname")
    jsToken = form.getvalue("jstoken")
    jstokenExpected = censored
    if jsToken == jstokenExpected:
        jid = censorredUserName + "@xmpp.example.com"
        # write to the chatlog
        f = open('./chats/' + jid, 'a')
        f.write(fbName + ": " + message + "")
        f.close()
        password = censored
        to = censoredMeTory
        message = fbName + ": " + message
        xmpp = SendMsgBot(jid, password, to, message)
        xmpp.register_plugin('xep_0030') # Service Discovery
        xmpp.register_plugin('xep_0199') # XMPP Ping
        if xmpp.connect():
            xmpp.process(block=True)
            print("Done ")
        else:
            # this is pretty useless. I should consider writing this
            # error to the chat log.
            print("Unable to connect.")
    else:
         print ('error') #shady business, bad token
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">chatReader.py</h2>
This script will be called by chat.js. It reads the contents of the chat logs and outputs back their contents. It would technically be possible for chat.js to read the chat log directly, but this extra step is necessary for security purposes.
{% highlight python %}
#!/usr/bin/python3.2
print ("Content-Type: text/html")
print ("")
import cgi
import cgitb; cgitb.enable()
import hashlib
import os
# get data sent by chat.js
form = cgi.FieldStorage()
jsjid = form.getvalue("jsjid")
token = form.getvalue("jstoken")
fbid = form.getvalue("jsfbid")
tokenExpected = censored
# simple read and output the chat file assuming security requirements are met
if token == tokenExpected:
    if os.path.isfile('chats/' + jsjid):
        f = open('chats/' + jsjid, 'r')
        lines = f.readlines()
        for line in lines:
            print (line)
    else:
        print ("no chatting yet")
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">pidAlive.py</h2>
This interacts with the PID records stored under "./pids/" by updating their timestamp. It takes the contents of the PID record and re-writes them with the updated timestamp. This script is ran periodically by chat.js. Without this, listener.py instances would be killed by pidKiller.py even if the browser is still open with the chat session.
{% highlight python %}
#!/usr/bin/python3.2
print ("Content-Type: text/html")
print ("")
import cgi
import cgitb; cgitb.enable()
import hashlib
import os
import time
form = cgi.FieldStorage()
pid = form.getvalue("pid")
fbId = form.getvalue("fbid")
pidTokenExpected = censored
if os.path.isfile("./pids/" + pid):
    f = open("./pids/" + pid, 'r')
    lines = f.readlines()
    token = lines[0][:-1]
    f.close()
    if token == pidTokenExpected:
        toWrite = "" + token + "\n" + str(time.mktime(time.gmtime()))
        f = open("./pids/" + pid, 'w')
        f.write(toWrite)
        f.close
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">pidKiller.py</h2>
This is ran periodically by cron. It looks through all the files in "./pids/" which represent active instances of listener.py. If the timestamp in the PID record is too old, it kills the instance of listener.py associated with the PID and removes the PID record. It checks to make sure that the PID it is about to kill is actually associated with an instance of listener.py to make sure that other system processes are not inappropriately killed. If somehow there is a PID record of a non-existent PID, the PID record is still removed.
{% highlight python %}
#!/usr/bin/python3.2
# this should be on a cronjob for every 5 minutes
import time
import os
from subprocess import Popen, PIPE
# need this because we are running with cron
myPath = os.path.dirname(os.path.abspath(__file__))
for root, dirs, files in os.walk(myPath + "/pids/"):
    for f in files:
        if f == '.htaccess':
            continue
        pidFile = open(myPath + "/pids/" + f)
        lines = pidFile.readlines()
        stamp = lines[1]
        currentTime = time.mktime(time.gmtime())
        delta = float(currentTime) - float(stamp)
        if delta &amp;gt; 4*60:
            p = Popen('ps -p '+ f +' -o cmd', shell=True, stdout=PIPE, stderr=PIPE)
            out, err = p.communicate()
            command = out[4:37].decode("utf-8")
            os.remove(myPath + "/pids/" + f)
            expectedCommand = "python3.2 "+ myPath + "/listener.py"
            if command == expectedCommand:
                Popen(['kill', f])
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">fbReplyBot.py</h2>
The code below is nearly a one-to-one duplicate of <a href="http://sleekxmpp.com/getting_started/echobot.html" target="_blank">this</a>. Originally the code received XMPP messages and sent what was received back to the sender. In this case, the code receives messages and then sends back a reply with instructions on how to use the application described in this blog post.
{% highlight python %}
#!/usr/bin/python3.2
# -*- coding: utf-8 -*-
"""
    SleekXMPP: The Sleek XMPP Library
    Copyright (C) 2010  Nathanael C. Fritz
    This file is part of SleekXMPP.
    See the file LICENSE for copying permission.
"""
# http://en.wikipedia.org/wiki/MIT_License
import os
import sys
cmd_folder = os.path.dirname(os.path.abspath(__file__))
if cmd_folder not in sys.path:
        sys.path.insert(0, cmd_folder)
import logging
import getpass
from optparse import OptionParser
import sleekxmpp
# Python versions before 3.0 do not use UTF-8 encoding
# by default. To ensure that Unicode is handled properly
# throughout SleekXMPP, we will set the default encoding
# ourselves to UTF-8.
if sys.version_info < (3, 0):
    reload(sys)
    sys.setdefaultencoding('utf8')
else:
    raw_input = input
class EchoBot(sleekxmpp.ClientXMPP):
    """
    A simple SleekXMPP bot that will echo messages it
    receives, along with a short thank you message.
    """
    jid = ""
    def __init__(self, jid, password):
        sleekxmpp.ClientXMPP.__init__(self, jid, password)
        # The session_start event will be triggered when
        # the bot establishes its connection with the server
        # and the XML streams are ready for use. We want to
        # listen for this event so that we we can initialize
        # our roster.
        self.add_event_handler("session_start", self.start)
        # The message event is triggered whenever a message
        # stanza is received. Be aware that that includes
        # MUC messages and error messages.
        self.add_event_handler("message", self.message)
        self.jid = jid
    def start(self, event):
        """
        Process the session_start event.
        Typical actions for the session_start event are
        requesting the roster and broadcasting an initial
        presence stanza.
        Arguments:
            event -- An empty dictionary. The session_start
                     event does not provide any additional
                     data.
        """
        self.send_presence()
        self.get_roster()
    def message(self, msg):
        """
        Process incoming message stanzas. Be aware that this also
        includes MUC messages and error messages. It is usually
        a good idea to check the messages's type before processing
        or sending replies.
        Arguments:
            msg -- The received message stanza. See the documentation
                   for stanza objects and the Message stanza to see
                   how it may be used.
        """
        if msg['type'] in ('chat', 'normal'):
               # tell people to use this application
               msg.reply("Thanks for contacting me, ...").send()
if __name__ == '__main__':
    # Setup the command line arguments.
    optp = OptionParser()
    # Output verbosity options.
    optp.add_option('-q', '--quiet', help='set logging to ERROR',
                    action='store_const', dest='loglevel',
                    const=logging.ERROR, default=logging.INFO)
    optp.add_option('-d', '--debug', help='set logging to DEBUG',
                    action='store_const', dest='loglevel',
                    const=logging.DEBUG, default=logging.INFO)
    optp.add_option('-v', '--verbose', help='set logging to COMM',
                    action='store_const', dest='loglevel',
                    const=5, default=logging.INFO)
    # JID and password options.
    optp.add_option("-j", "--jid", dest="jid",
                    help="JID to use")
    optp.add_option("-p", "--password", dest="password",
                    help="password to use")
    opts, args = optp.parse_args()
    # Setup logging.
    logging.basicConfig(level=opts.loglevel,
                        format='%(levelname)-8s %(message)s')
    opts.jid = "censord@chat.facebook.com"
    opts.password = censored
    # Setup the EchoBot and register plugins. Note that while plugins may
    # have interdependencies, the order in which you register them does
    # not matter.
    xmpp = EchoBot(opts.jid, opts.password)
    xmpp.register_plugin('xep_0030') # Service Discovery
    xmpp.register_plugin('xep_0004') # Data Forms
    xmpp.register_plugin('xep_0060') # PubSub
    xmpp.register_plugin('xep_0199') # XMPP Ping
    # If you are working with an OpenFire server, you may need
    # to adjust the SSL version used:
    # xmpp.ssl_version = ssl.PROTOCOL_SSLv3
    # If you want to verify the SSL certificates offered by a server:
    # xmpp.ca_certs = "path/to/ca/cert"
    # Connect to the XMPP server and start processing XMPP stanzas.
    if xmpp.connect():
        # If you do not have the dnspython library installed, you will need
        # to manually specify the name of the server if it does not match
        # the one in the JID. For example, to use Google Talk you would
        # need to use:
        #
        # if xmpp.connect(('talk.google.com', 5222)):
        #     ...
        xmpp.process(block=True)
        print("Done")
    else:
        print("Unable to connect.")
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">upstart script</h2>
This simply starts fbReplyBot.py when the operating system boots, or to be more precise, when the network card starts.
{% highlight bash %}
# /etc/init/facebookBot.conf
description "Tells people to talk to me via other means than Facebook"
# start when the network card starts
start on (local-filesystems and net-device-up IFACE=eth0)
stop on shutdown
# Automatically Respawn:
respawn
respawn limit 1 60
script
    # Not sure why $HOME is needed, but we found that it is:
    export HOME="/var/www/webxmpp"
    exec $HOME/fbReplyBot.py &amp;gt;&amp;gt; /dev/null
end script
{% endhighlight %}
<h2 style="clear: both; padding-top:25px;">Other Things Learned</h2>
I learned a lot about <a href="http://inkscape.org/" target="_blank">Inkscape</a> ("an Open Source vector graphics editor") in creating the flow diagram in this blog post. I've always struggled finding good diagramming tools for Linux. I have researched the topic plenty and have yet to find something that has met my needs. If I had to, I could resort to cloud based diagramming tools, but, that feels like cheating to me, an admission that the Linux ecosystem lacks in some way. This was basically my first time using Inkscape. It feels like a fairly promising diagramming tool. The big revelation I had with Inkscape allowing me to recognize it as a powerful diagraming tool, was discovering the cloning functionality. In Inkscape, you can clone something which you have drawn, making several duplicates from a master. Then you can modify the master, such as the colour and have the changes be reflected in the cloned children. This is a huge plus when you have created a bunch of objects then want to change the colour of them. That said, there was a quirky aspect of its functionality that almost caused me to lose confidence. When resizing children the border thickness is scaled along with the resizing, that is, when you made the cloned child smaller, the border got skinnier, and when you made the child larger, the border got larger, but I wanted the border width to stay the same. There is an option in Inkscape that promises just this. It says that it maintains the boarder thickness when the object is scaled and I figured it would work for everything. However, it turns out that it does not work for cloned objects. The general way I overcame this was by drawing the boarders separately, but still making use of duplicating capabilities so that it wasn't a completely manual process. Additionally, there was a strange aspect that an arrow head on a line would not naturally match the colour of the line. To make it match according to <a href="http://wiki.inkscape.org/wiki/index.php/FAQ#How_do_I_change_the_color_of_markers_.28e.g._arrow_ends.29.3F" target="_blank">this</a> you have to go to: Extensions > Modify Path > Color Markers to Match Stroke. I used this, and it works, but it did cause me some confusion as to why this doesn't happen by default.

Also, just as I was finishing up this post, wordpress did <a href="http://www.torypages.com/blog/wp-content/uploads/2012/12/disaster-622x398.png" target="_blank">this</a> to my sourcecode, adding in br's all over the place, new paragraphs, replacing characters with HTML encoding etc. This is the type of thing that just makes you want to give up or leave it looking bad. But, I went back and I think I corrected everything. If you see some random br's or &nbsp;'s anywhere this is why. It also drives home the point as to why many developers refrain from using visual designers, because of mess-ups like these.

<h2 style="clear: both; padding-top:25px;">Resources and Thanks</h2>
I wouldn't have been able to do all this without help from other people on the Internet. Below is a list of many of the resources that helped me in the creation of this application.
<ul>
<li><a href="http://net.tutsplus.com/tutorials/javascript-ajax/how-to-create-a-simple-web-based-chat-application/" target="_blank">net tuts+ - How to Create A Simple Web-based Chat Application</a> - this taught me the concept of how to create a section on a  webpage, a box, that could have scrolling content in it that updates. It gave me the idea of including HTML in the chat log, and then simply displaying the content of the chat log into a scrollable area/div. If it weren't for this I would have attempted to creat additional elements on the page when each new message is created at either end of the conversation. Admittedly  describing what I learned here was a bit difficult, and I'm not sure if I did a good job at it, but this was a useful resource!</li>
<li><a href="http://sleekxmpp.com/" target="_blank">SleekXMPP</a> - Without this project my project may simply not exist. This allowed me to connect to my Openfire/XMPP server from Python. This really shows one of the strengths of object oriented programming. I was able to simply drop in, or include the functionality of SleekXMPP and suddenly I was able to connect to my Openfire/XMPP server, something I'm not sure I would have ever been able to do otherwise since it is my guess that the programming involved in doing so would be over my head.</li>
<li><a href="http://www.igniterealtime.org/projects/openfire/" target="_blank">Openfire</a> - This is the XMPP chat server I am using. It plays a critical role in this application.</li>
<li><a href="http://linuxformat.co.uk/wiki/index.php/Inkscape_-_cloning_and_tiling" target="_blank">Inkscape - cloning and tiling - wiki tutorial</a> - This is probably the primary reason why my flow diagram above looks good. It introduced me to cloning in Inkscape.</li>
<li><a href="http://stackoverflow.com/a/6984168" target="_blank">Stackoverflow - info on Curl</a> - This showed me how to make a web request and receive the response of the request using Curl.</li>
<li><a href="http://stackoverflow.com/a/12625944" target="_blank">Stackoverflow - info on browser detection</a> - This showed me some of the techniques I used for browser detection.</li>
<li><a href="https://www.digitalocean.com/community/articles/how-to-create-a-ssl-certificate-on-apache-for-ubuntu-12-04" target="_blank">DigitalOcean - SSL Apache Ubuntu 12.04</a> - This helped me setup SLL on my Apache server.</li>
<li><a href="http://forrestbao.blogspot.ca/2009/08/hello-world-of-webhttp-python.html" target="_blank">This blog</a> for refreshing me on how to use Python and CGI for web programming.</li>
<li>And many of the tools that I used: <a href="http://www.ubuntu.com/" target="_blank">Ubuntu</a>, <a href="http://httpd.apache.org/" target="_blank">Apache</a>, <a href="http://php.net/" target="_blank">PHP</a>, <a href="http://www.python.org/" target="_blank">Python</a>, <a href="http://www.eclipse.org/" target="_blank">Eclipse IDE</a>, <a href="http://www.pidgin.im/" target="_blank">Pidgin chat</a></li>
<li>and many others I have neglected.</li>
</ul>
<h2 style="clear: both; padding-top:25px;">Demonstration</h2>
Below are two videos. The first is a demonstration of the chat application. The second is a demonstration of some of my new Inkscape skills. There is some strange glitches in the second demo, I suspect it is just because I am having some mouse issues in the virtual machine I was using.

<iframe width="420" height="315" src="//www.youtube.com/embed/2nQaEEIjwQo" frameborder="0" allowfullscreen></iframe>

<iframe width="420" height="315" src="//www.youtube.com/embed/DJE5b8wbu2U" frameborder="0" allowfullscreen></iframe>
