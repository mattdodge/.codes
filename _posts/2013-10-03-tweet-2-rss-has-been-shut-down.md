---
layout: post
title: Tweet-2-RSS has been shut down
date: 2013-10-03 09:56:25
permalink: /tweet-2-rss-has-been-shut-down/
dsq_thread_id: 1821560515
header-img: "img/sad-twitter.jpg"
header-bg-color: "#4295c8"
---
A few months back I created a service that would allow people to get tweets or other content from the Twitter API in RSS or XML format. It ended up growing a moderate amount and amassed over 2,500 users. Well, some frustrating and disappointing news came through yesterday; the Twitter API team has suspended the Tweet-2-RSS application for violation of the Terms of Service. They did so without any warning and it was certainly a surprise to me. Here is the relevant quote from their <a title="Twitter API TOS" href="https://dev.twitter.com/terms/api-terms" target="_blank">API Terms</a> (Section I.4):<!--more-->

> You will not attempt or encourage others to sell, rent, lease, sublicense, redistribute, or syndicate access to the Twitter API or Twitter Content to any third party without prior written approval from Twitter. If you provide downloadable datasets of Twitter Content or an API that returns Twitter Content, you may only return IDs (including tweet IDs and user IDs).

They consider returning the body of the tweet in any form "re-syndication" of their content. Their "solution" is to return a tweet ID and have the end user make their own API call to obtain the actual tweet. Obviously this is a horrible solution for current users as it would require you to register your own Application and make a custom API call. If you could do that, you wouldn't be using this service at all.

There are dozens of uses for getting RSS/XML Twitter data; use in existing RSS readers, XML-only plugins, and more. Unfortunately, it seems that the Twitter API team doesn't recognize them or care. Their lowly customer service rep who was on my case specifically called it out (and even tried to get me to rat out some similar services):

> Services which provide XML/RSS output of Twitter Content are disallowed per our API Terms of Service, unless they only return IDs. If you have encountered other services which engage in this activity please let us know and we will investigate them for compliance with our Rules.

I "politely" responded that I would not snitch on fellow developers who are simply trying to make the web a better place and that they could do their own homework to find them.

To the users of Tweet-2-RSS, I apologize for the sudden suspension. It came as a surprise to me as well. Even though Twitter can't seem to do some simple Googling, there are other services out there that will accomplish the same thing. However, be warned that they are most likely in violation of the same terms. For now, Tweet-2-RSS will just return some empty XML results so your plugins won't "break" or throw exceptions. It will just look like there is no content. To those of you who donated and helped out with the hosting costs: I greatly appreciate the support and help. I will be refunding your donations considering the service is no longer operating. Wait for an email from me.

This isn't the end of the road though. I understand the need for a service like this and I am currently working with similar RSS services to come up with a workaround. It may take a few weeks but I'm confident that a solution will arise, it may just be a hacky one. If you are interested in being involved let me know via email or a comment below.
