---
layout: post
title: How to make a simple (and possibly very annoying) marketing campaign based off of Twitter
date: 2013-07-09 15:34:03
permalink: /how-to-simple-marketing-campaign-twitter/
dsq_thread_id: 1484949896
---

Ingredients you will need:

  1. <a title="Twitter" href="https://twitter.com/login" target="_blank">Twitter Account</a>
  2. <a title="IFTTT" href="http://ifttt.com" target="_blank">IFTTT</a>
  3. <a title="Tweet-2-RSS" href="http://tweet-2-rss.appspot.com" target="_blank">Tweet-2-RSS</a>

So, you just wrote some cool new software or made a fancy new gizmo. You want to publicize it but come on, you don't know marketing, you're an engineer! Here is a simple and annoying recipe to get your product out there.<!--more-->

First, head over to <a title="Tweet-2-RSS" href="http://tweet-2-rss.appspot.com" target="_blank">Tweet-2-RSS</a> (shameless plug, <a title="How to get RSS or XML output from the Twitter API" href="http://mattdodge.net/twitter-rss-xml/" target="_blank">I created this service</a>) and hook up your Twitter account. Now let's say your software has something to do with **314** and the **Dodgers**. Use the Feed Builder to create an RSS feed that searches for these terms. Click "Tweets Matching a Phrase" and enter your phrase in the q parameter field. It will probably look something like this:

![Tweet2RSS Screenshot]({{site.url}}/img/tweet2rss.png "Tweet2RSS Screenshot")

Copy that Feed URL (you can test it if you want by clicking Test Feed) and head over to <a title="IFTTT" href="http://ifttt.com" target="_blank">IFTTT</a>. We are going to create a new recipe with an RSS feed as our input. Choose the "Feed" trigger and then "New feed item." Paste your Twitter RSS feed URL there. Now for the "then" part, we are going to post a tweet back at the person who originally tweeted.

Note: This is where it could get annoying slash spammy to people. Another option is to just email or text yourself when a tweet comes through, then you can decide what you want to do about it. This also cuts down on false positives. Both of these are set up as actions on IFTTT.

But assuming we're going to go ahead with the tweet back, choose Twitter as your Action Channel and then click "Post a Tweet." You can find the original tweet poster in the "Entry Author" field of IFTTT, so you may want to write something that looks like this:

![IFTTT Screenshot]({{site.url}}/img/ifttt.png "IFTTT Screenshot")

Create the Action and then save your recipe and you're done! Now every time someone sends a tweet talking about 314 and the Dodgers, they will get a response back from you with your link!
