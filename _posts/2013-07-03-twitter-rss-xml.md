---
layout: post
title: How to get RSS or XML output from the Twitter API
date: 2013-07-03 17:35:33
permalink: /twitter-rss-xml/
dsq_thread_id: 1464418721
---

Short answer: you can't. Well, I should say you couldn't!

I recently discovered that Twitter was no longer offering XML as a response format for their API calls. No big deal, XML is clunky, verbose, and I hate every XML library that was ever written. I was ready to move on until a few days later I realized that <a href="http://ifttt.com" target="_blank">IFTTT</a> had also pulled Twitter as an input source for recipes. However, RSS was still there; this made the lack of an XML/RSS option from the Twitter folks slightly annoying now.

So I decided I would make one. Normally I would save an idea like this for a hackathon or something but in this case I had a couple recipes I wanted to make ASAP so I figured let's give it a shot.

The service is called <a title="Tweet-2-RSS" href="http://tweet-2-rss.appspot.com" target="_blank">Tweet-2-RSS</a> and is powered by Google App Engine. Source code is of course provided <a href="https://github.com/mattdodge/Tweet-2-RSS" target="_blank">on GitHub</a>.

<!--more-->

You can read more on the landing page but basically what it does is take calls from the Twitter v1.1 API (thats the annoying one that requires pesky OAuth headers every time) and converts the JSON to XML (spec'd to RSS 2.0 standards) and returns that. It also takes care of all of the OAuth for every request for you. I've recently encountered several WordPress and jQuery plugins that used the deprecated Twitter API. No need for those anymore, just use an RSS plugin instead and hook up to one of these feeds.

Hope you enjoy! Comment below or for issues/feature requests use GitHub.
