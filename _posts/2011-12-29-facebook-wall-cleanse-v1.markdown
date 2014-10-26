---
layout: post
title: Facebook Wall Cleanse V1
date: 2011-12-29 05:26:12.000000000 -05:00
categories:
- tech
tags:
- comments
- delete
- facebook
- imacros
- posts
- privacy
- wall
type: post
---
This is an early iteration of code which allows you cleanse your Facebook wall. This is necessary because of the new Facebook Timeline which allows your past history to be searched more efficiently. This information was always there it is just now easier to get at. I had attempted to write a nice script using the Facebook API which would delete posts automatically but that proved to be impossible since the API does not allow applications to delete posts from other applications, and since no application I would create would be the same as the Facebook web application deletion through this method was not going to be possible. As result, I have resorted to iMacros a web browser extension for Firefox (and other browsers) that records mouse movements. Due to the fact that deleting content can be a fairly dynamic task some further scripting was required than what could be achieved simply through recording mouse clicks. This was done by implementing some javascript. 

The code below is no doubt un-finished, I am largely writing it for myself but I would like others to be able to benefit if they are attempting to do similar stuff. The code below should remove all comments made in 2011 and 2010. These are the only dates because I believe comments are a relatively new feature, and at the time of writing it is not yet 2012.

I have been working on similar scripts for a few days now, my motivator in posting now is to show my method of handling the dynamic nature of the dates on the activity log. When running macros with no extra scripting it would be the case that the macro would run out of comments to delete due to the fact that later dates were not being loaded automatically. The javascript that I have written handles that issue.

The 999999999 and the YOUR_FACEBOOK_HANDLE will need to be replaced with information which is unique to you and you need to have the Facebook Timeline to use this.

{% highlight bash %}
var i, s, errorCount, dateTry;
errorCount = 0;
dateTry = 0;
var dateTryArray=new Array();
dateTryArray[0]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=12&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Dec\n";
dateTryArray[1]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=11&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Nov\n";
dateTryArray[2]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=10&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Oct\n";
dateTryArray[3]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=9&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Sept\n";
dateTryArray[4]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=8&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Aug\n";
dateTryArray[5]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=7&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jul\n";
dateTryArray[6]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=6&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jun\n";
dateTryArray[7]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=5&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:May\n";
dateTryArray[8]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=4&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Apr\n";
dateTryArray[9]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=3&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Mar\n";
dateTryArray[10]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=2&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Feb\n";
dateTryArray[11]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2011&amp;log_filter=cluster_116&amp;month=1&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jan\n";
dateTryArray[12]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=12&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Dec\n";
dateTryArray[13]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=11&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Nov\n";
dateTryArray[14]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=10&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Oct\n";
dateTryArray[15]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=9&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Sept\n";
dateTryArray[16]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=8&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Aug\n";
dateTryArray[17]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=7&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jul\n";
dateTryArray[18]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=6&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jun\n";
dateTryArray[19]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=5&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:May\n";
dateTryArray[20]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=4&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Apr\n";
dateTryArray[21]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=3&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Mar\n";
dateTryArray[22]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=2&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Feb\n";
dateTryArray[23]="TAG POS=1 TYPE=A ATTR=AJAXIFY:/ajax/timeline/all_activity/show_year.php?profile_id=999999999&amp;year=2010&amp;log_filter=cluster_116&amp;month=1&&HREF:#&&REL:async&&CLASS:fbTimelineLogMonthLink&&TXT:Jan\n";
while (dateTry <= 23)
{
	if (errorCount > 2)
	{
		dateTry += 1;
		errorCount = 0;
	}
	macro = "CODE:";
	macro += "URL GOTO=http://www.facebook.com/YOUR_FACEBOOK_HANDLE?sk=allactivity\n";
	macro += "TAG POS=1 TYPE=SPAN ATTR=CLASS:uiButtonText&&TXT:All\n";
	macro += "WAIT SECONDS=1\n";
	macro += "TAG POS=1 TYPE=SPAN ATTR=CLASS:itemLabel<SP>fsm&&TXT:Comments\n";
	macro += "WAIT SECONDS=3\n";
	macro += dateTryArray[dateTry] + "\n";
	macro += "WAIT SECONDS=1\n";
	macro += "TAG POS=1 TYPE=I ATTR=CLASS:mrs<SP>defaultIcon<SP>customimg<SP>img<SP>sp_8cd3nf<SP>sx_253349&&TXT:\n";
	macro += "SET !TIMEOUT_PAGE 1\n";
	macro += "SET !ERRORIGNORE YES\n";
	macro += "WAIT SECONDS=1\n";
	macro += "TAG POS=1 TYPE=SPAN ATTR=CLASS:itemLabel<SP>fsm&&TXT:Delete<SP>comment...\n";
	macro += "SET !ERRORIGNORE NO\n";
	macro += "SET !TIMEOUT_PAGE 10\n";
	macro += "WAIT SECONDS=1\n";
	macro += "TAG POS=1 TYPE=INPUT:BUTTON ATTR=NAME:ok&&VALUE:Delete\n";
	i = iimPlay(macro);
	/* Check for error */
	if (i < 0) {
	    errorCount += 1;
	};
}
{% endhighlight %}
And again, this code is in its very early stages. Depending on time constraints it may get developed further, I do not know. At first I was just curious about how to do this, but then I decided to do it for myself after becoming upset over the fact that Facebook would not allow me to easily do this through their API. This is really the first time I have felt betrayed by Faceook's privacy policy.
