---
layout: post
title: Facebook Wall JavaScript Delete Bot
date: 2012-08-12 14:34:31.000000000 -04:00
categories:
- tech
tags:
- delete
- facebook
- javascript
- JQuery
- JS
- timeline
type: post
---
This will be very quick, I cannot dedicat much time to this sort of thing at the moment, I have already spent too much time on it. But, as I have shown <a href="http://www.torypages.com/blog/?p=1086">before</a> I have interest in being able to delete old Facebook wall posts. This is because conversations we have in person are largely ephemeral and I think Facebook should be too, or at least have the ability to be.

I have discovered a new technique to do this, different from my previous shaky attempt this one works very well. This time I have created a JavaScript bot that uses some JQuery that runs in a <a title="FireQuery" href="http://firequery.binaryage.com/">FireQuery</a> console which is a modification of a popular Firefox extension named FireBug. When doing this, one must remember to click "JQuerif"y so that the script I have written has access to the JQuery library.


<a href="https://dl.dropboxusercontent.com/s/wnf2wqv3j18ritb/Screenshot-from-2012-08-12-144221.png"><img class=" wp-image-1171 aligncenter" title="FireQuery" src="https://dl.dropboxusercontent.com/s/pbov1a0c69zg8jv/Screenshot-from-2012-08-12-144221-622x131.png" alt="" width="373" height="79" /></a>

To show where the script goes I have include the image below (but of course you'd be on Facebook):

<a href="https://dl.dropboxusercontent.com/s/6re9tpa4svd7yky/Screenshot-from-2012-08-12-144453.png"><img class=" wp-image-1172 aligncenter" title="Script Location" src="https://dl.dropboxusercontent.com/s/3hw0wihmg1xi2ny/Screenshot-from-2012-08-12-144453-622x416.png" alt="" width="373" height="250" /></a>


This script is to be run when you are on the Facebook "activity log" page. Within that page you could select, for example, <del>"All," "Your Posts," "Post by others, etc"</del> (I can only confirm it for "your posts," if it comes across an item which you can't delete it will not work) in order to have a bit of extra control over what is deleted.

This post has been brief, details have been left out, but I did want to share this code so that it is out there in case anyone wants to build on it. If I had infinite time I would put this in a Greasemonkey script, and wrap it into some sort of easy to use user interface.

Do note! I am definitely not a JavaScript developer. Despite all the languages I have written, and all the programming courses that I have taken, I have never made a serious attempt to learn JavaScript. In fact, I am very confused by it. For example, being able to have loops and delays in this script presented major difficulties which would have been a breeze in the other languages I have coded in. I am baffled by JavaScript, I am surprised the below works, I am positive there are better ways of writing this, and I suspect there are useless aspects of this script.

Also note, you need to scroll down in your activity log, and reveal all the posts which you want to be deleted in order for them to be deleted. The script does not automatically load them in for you.

Lastly, how do you know when the script ends? It crashes. hehe
{% highlight javascript %}
var to1, to2, to3, to4, to5;
// $jq is the method of accessing JQuery after "JQurifying"
function myStart(){
    task1();
}
function task1(){
    try {
        $jq('a[ajaxify*="show_story_options"]:first').attr('id', 'toryUnique1');
        document.getElementById("toryUnique1").click();
    } catch (err) {
        window.clearTimeout(to1); //no idea if these are needed
        window.clearTimeout(to2);
        window.clearTimeout(to3);
        window.clearTimeout(to4); // something dint' work right, just
        window.clearTimeout(to5); // start over. If something didn't
        myStart();                // work right it is probably because
        return;                   // a delay was too short
    }
    to1 = window.setTimeout(function(){
        task2();
    }, 800); //800 millisecond delay
}
function task2(){
    try {
        $jq("span:contains(Delete)").parent().click();
        $jq("a:contains(Delete)").attr('id', 'toryUnique2');
    } catch (err) {
        window.clearTimeout(to1);
        window.clearTimeout(to2);
        window.clearTimeout(to3);
        window.clearTimeout(to4);
        window.clearTimeout(to5);
        myStart();
        return;
    }
    to2 = window.setTimeout(function(){
        task3();
    }, 800);
}
function task3(){
    try {
        document.getElementById("toryUnique2").click();
    } catch (err) {
        window.clearTimeout(to1);
        window.clearTimeout(to2);
        window.clearTimeout(to3);
        window.clearTimeout(to4);
        window.clearTimeout(to5);
        myStart();
        return;
    }
    to3 =  window.setTimeout(function(){
        task4();
    }, 400);
}
function task4(){
    try {
        $jq('input[value="Delete"]').attr('id', "toryUnique3");
	document.getElementById("toryUnique3").click();
    } catch (err) {
        window.clearTimeout(to1);
        window.clearTimeout(to2);
        window.clearTimeout(to3);
        window.clearTimeout(to4);
        window.clearTimeout(to5);
        myStart();
        return;
    }
    to4 = window.setTimeout(function(){
        myStart();
    }, 2000);
}
myStart();
{% endhighlight %}
