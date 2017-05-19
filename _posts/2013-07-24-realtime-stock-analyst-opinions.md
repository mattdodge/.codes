---
layout: post
title: Realtime Stock Analyst Opinions
date: 2013-07-24 16:11:33
permalink: /realtime-stock-analyst-opinions/
dsq_thread_id: 1528400388
header-img: "img/post-stock.jpg"
---

Two words: WolframAlpha. Or is that one word? I don't know, but what I do know is that it is a lifesaver for any engineering student these days. Unfortunately for me, it only really existed during my senior year of college, which happened to be the year I got the highest GPA. Coincidence? Probably.

Until now, I had really only used <a title="WolframAlpha" href="http://wolframalpha.com" target="_blank">WolframAlpha</a> for its scientific and mathematical solving abilities. But it can do so much more! Featured in this post will be a snippet of its financial knowledge. Head on over and try <a title="WolframAlpha AAPL" href="http://www.wolframalpha.com/input/?i=AAPL" target="_blank">searching for your favorite stock symbol</a>. You will notice a plethora of information from realtime trade data to price projections. You will also notice that they provide a breakdown of analyst opinions on the stock from buy to sell. Wouldn't it be cool if we could see this breakdown for all stocks in the market and how they compare to one another? I'm glad you asked.<!--more-->

Queue the <a title="WolframAlpha API" href="http://products.wolframalpha.com/api/" target="_blank">WolframAlpha API</a>. Thanks to that API, a realtime backend provided by <a title="Firebase" href="https://www.firebase.com/" target="_blank">Firebase</a>, and some nifty visuals courtesy of <a title="HighCharts" href="http://www.highcharts.com/" target="_blank">HighCharts</a>, I was able to put together a basic visual of the overall opinion of all 500 of the stocks on the S&P 500. <a title="Stocky Node" href="http://stockynode.aws.af.cm" target="_blank">So go check it out</a>! More on how it works below, but first a screenshot!

![Stock Analyst Output Screenshot]({{site.url}}/img/stock_opinions.png "Stock Analyst Output Screenshot")


The way it works is rather simple, on my command it takes the current list of the 500 S&P 500 stock companies (<a title="Wikipedia S&P 500" href="http://en.wikipedia.org/wiki/List_of_S%26P_500_companies" target="_blank">courtesy of Wikipedia</a>) and one-by-one (in parallel of course) asks WolframAlpha for the analyst opinions of the stock. It collects and parses that information, then pushes it up to a Firebase. Firebase is nice and will keep all of this data for me and even notify existing connections if any of the data has changed. So if you happen to be looking at the chart when the analyst opinions get updated, the chart will get updated too. The whole app is written in Node and hosted on <a title="AppFog" href="https://www.appfog.com/" target="_blank">AppFog</a>, another first for me. I have to say, their deployment and management process is really straight-forward.

It was a fun little project, I got to experiment with AppFog, and I think it's actually a somewhat useful and meaningful visual. Hope you enjoy! Check it out here: <a title="Stocky Node" href="http://stockynode.aws.af.cm" target="_blank">http://stockynode.aws.af.cm</a>; you can leave feedback in the comments below.
