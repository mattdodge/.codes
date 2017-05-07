---
layout: post
title: 'SF API Hack Day - Hacking Email Inboxes'
date: 2013-06-25 08:42:52
permalink: /sf-api-hack-day-hacking-email-inboxes/
dsq_thread_id: 1484932977
---

I attended [San Francisco API Hack Day](http://apihackday.com/ "API Hack Day") last weekend and had an awesome time. There were some really cool ideas thrown around and I didn't get mugged taking the bus home at 10 PM so it was a great day.

I ended up teaming up with [Crystal Rose](http://crystalrose.me "Crystal Rose") ([@crystalrose](http://twitter.com/crystalrose "@crystalrose")) and we made an email hack that I think is pretty cool, so I thought I'd share the idea here. We called it "Hack my Email" (which we named with about 15 minutes to go in the competition) and the idea is to open source email filtering and prioritization **before** it gets to your inbox. Here we are presenting:

<a href="http://mattdodge.net/wp-content/uploads/2013/06/hack_my_email.jpg" rel="image_group"><img class="aligncenter size-medium wp-image-227" alt="hack_my_email" src="http://mattdodge.net/wp-content/uploads/2013/06/hack_my_email-450x600.jpg" width="450" height="600" srcset="http://mattdodge.net/wp-content/uploads/2013/06/hack_my_email-450x600.jpg 450w, http://mattdodge.net/wp-content/uploads/2013/06/hack_my_email.jpg 768w" sizes="(max-width: 450px) 100vw, 450px" /></a>

<!--more-->

We all get tons of email, so much so that we go and unsubscribe from as many things as possible just to cut down. Personally, I use Gmail's Priority Inbox and find that it actually works pretty well. But what I also found was that you really can't customize what Gmail marks as priority or not. You can set up "filters" in Gmail but these are limited and actually require the email to get to your inbox before the rules apply. This may seem obvious but even when mail is in my regular inbox (not priority) I still go and look at it. Maybe I want all of my sports digest emails to come to me at once at the end of the day. Or maybe I want shopping deals to come on a weekend when I may actually go shopping. Everybody can probably come up with some sort of rule that they wish they had but that can't be achieved through a standard Gmail filter.

That's where Hack My Email comes in. What we did is hook up to the [ContextIO](http://context.io "ContextIO") and [SendGrid](http://sendgrid.com "SendGrid") APIs as inputs. You can use ContextIO to hook to an existing email address and SendGrid to hook into a domain's email server (can you say corporate monitoring?).** **When an email is received to your input email address, it is picked up by the platform and run through a filter. It could really be any filter you want, it's **open source**! The filter could queue the message for later, mark it as spam, or make use of the [Gmail Plus Sign Address Trick](http://lifehacker.com/144397/instant-disposable-gmail-addresses "GMail Plus Sign Address Trick"). The idea is that you could apply as simple (don't email me after midnight) or as complex of an algorithm (learning or NLP) as you want here, something that is currently not possible.**
  
** 

For the Hackathon demo, I created a relatively simple algorithm that would determine the sender's [Klout](http://klout.com "Klout") and prioritize based off of that. The idea being if someone with a high Klout Score sends me an email, I would like to mark it as important. To do this, we take the from address, cross-reference the [FullContact API](http://www.fullcontact.com/developer/ "FullContact API") to grab the twitter handle, then hit the Klout API to get the Klout Score. If that score exceeds my customizable threshold, mark the email as a "high klout" email so I don't miss it. It's probably not the most practical email filtering algorithm but it does demonstrate how something being open source can provide access to a vast array of options that aren't possible through Gmail Filters. It also didn't hurt that the Klout API guys ([Mashery](http://mashery.com "Mashery")) were there and were giving out Jamboxes as prizes, here we are with ours!

<a href="http://mattdodge.net/wp-content/uploads/2013/07/mashery_jambox.jpg" rel="image_group"><img class="aligncenter size-medium wp-image-225" alt="mashery_jambox" src="http://mattdodge.net/wp-content/uploads/2013/07/mashery_jambox-600x450.jpg" width="600" height="450" srcset="http://mattdodge.net/wp-content/uploads/2013/07/mashery_jambox-600x450.jpg 600w, http://mattdodge.net/wp-content/uploads/2013/07/mashery_jambox.jpg 1024w" sizes="(max-width: 600px) 100vw, 600px" /></a>

&nbsp;

I think there is some potential to an idea like this. There are already apps like [Mailbox](http://mailboxapp.com "MailboxApp") and [Boomerang](http://www.boomeranggmail.com/ "Boomerang") that have recognized the need for deferring your email until a later time, the problem is that they require you to use a different app to read your email. By plugging in to the email servers you should be able to get around it. You may have to give out a different email address than you actually check but we all already do that anyway (cough cough...[Mailinator](http://mailinator.com "Mailinator")). It's obviously still in the idea/prototype stage at this point but I thought I'd throw it out there.

To see all of the hacks from that day check out [the page on HackerLeague](https://www.hackerleague.org/hackathons/api-hackday-sf-2013 "Hacker League SF API Hack Day").
