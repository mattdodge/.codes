---
layout: post
title: How to win your NCAA bracket with math!
date: 2013-03-18 20:59:14
permalink: /how-to-win-your-ncaa-bracket-with-math/
dsq_thread_id: 1149731240
---

I love sports. I watch a lot of sports. But there is no way I'm going to sit here and tell you I've even seen half of these teams in the NCAA tournament play, and yet I will fill out a bracket and swear up and down that I have it right. I am probably describing a lot of bracket-filler-outers here, and I am proposing a new way to fill one out to maximize your chances of winning your office pool. I'm not picking by seed, not picking by mascot color, not even picking against the schools that those annoying kids in high school went to. We're talking math here folks.

Math, specifically <a title="Bayesian Statistics Wikipedia" href="http://en.wikipedia.org/wiki/Bayesian_statistics" target="_blank">Bayesian Statistics</a>. Assuming you have a machine that can pit any two teams against each other and determine a probability of winning (this is a very large assumption, I know, more on this later), you can actually determine the probability that any team makes it to any round of the tournament.

**Example**: The odds that a 1 seed makes it to the Sweet 16 (these might be lower than you think).
  
What needs to happen for this is a couple of things, first of all, the 1 seed has to win it's opening matchup (1 vs 16). Then, the 1 seed needs to win its second round matchup, which will be either against the 8 seed or the 9 seed. So in this case, we will need to run two probabilities, the one that 1 beats 8 and the one that 1 beats 9, and then weight them with the probability that each opponent makes it there (we know this because we know P(8 beats 9)). So we end up with the following formula for the probability (_P_) that the 1 seed makes it through the opening weekend to the Sweet 16 (let W(X,Y) be the probability that X beats Y):

```
P = W(1,16) [W(8,9) W(1,8) + W(9,8) W(1,9)]P = W(1,16) [W(8,9) W(1,8) + W(9,8) W(1,9)] 
```

Obviously, this equation will get quite long for the later rounds so it's probably best if you <a title="NCAA Bayes Gist" href="https://gist.github.com/mattdodge/5197813" target="_blank">write or use a program</a> to create these calculations for you. What you end up with is a list of every team and the probability that each team makes it to each round of the tournament.

Here's where it gets fun. ESPN is nice and <a title="ESPN Tournament Pickem" href="http://games.espn.go.com/tournament-challenge-bracket/en/whopickedwhom" target="_blank">puts out a list</a> of the percentage of people that pick each team to make it to each round, so we can directly compare these percentages. The theory behind this being that if a team has a 30% shot of making it to the Elite 8, but 45% of people pick that team to get there, your expected value by picking them is pretty low. We are looking to find (especially in a season full of upsets like we had this year) the teams that have a decent shot that no one is picking for some reason or another. You can check out the full list and breakdown on <a title="2013 NCAA Probabilities" href="https://dl.dropbox.com/u/603003/2013%20NCAA%20Probabilities.xlsx" target="_blank">this spreadsheet</a> but I'll give you the highlights here.

**Final Four - Probability of making it vs. the number of people who think they will**
  
|Team|Actual Probability|Pick Percentage|Difference|
| :--- | :--- | :--- | :--- | :--- | 
|(1) Louisville|38.87|51.50|-12.63|
|(1) Indiana|37.47|44.80|-7.33    |
|(1) Gonzaga|29.98|26.90|3.08     |
|(1) Kansas|27.08|34.50|-7.42     |
|(3) Florida|26.43|17.40|9.03     |
|(2) Ohio State|21.31|43.10|-21.79|
|(2) Duke|20.12|24.10|-3.98       |
|(2) Miami (FL)|17.80|36.60|-18.80|
|(2) Georgetown|16.29|24.70|-8.41 |
|(4) Syracuse|13.39|8.50|4.89     |
|(4) Michigan|12.85|14.00|-1.15   |
|(3) Michigan St|12.50|15.10|-2.60|
|(3) New Mexico|10.96|9.30|1.66   |
|(5) Wisconsin|8.79|9.60|-0.81    |
|(3) Marquette|8.77|3.80|4.97     |
|(4) Saint Louis|6.98|3.50|3.48   |
|(8) Pittsburgh|6.78|0.90|5.88    |
|(6) Arizona|6.31|2.20|4.11       |
|(4) Kansas State|6.15|4.10|2.05  |
|(5) UNLV|5.05|0.80|4.25          |
|(5) VCU|4.75|3.60|1.15           |
|(5) Oklahoma St|4.36|1.30|3.06   |
|(7) Creighton|4.33|0.70|3.63     |
|(6) Butler|4.14|2.40|1.74        |
|(8) NC State|3.91|1.00|2.91      |
|(6) Memphis|3.53|1.30|2.23       |
|(7) Notre Dame|3.27|1.50|1.77    |
|(8) UNC|3.25|2.30|0.95           |
|(7) Illinois|3.22|0.70|2.52      |
|(7) San Diego St|2.81|0.30|2.51  |
|(9) Missouri|2.47|0.50|1.97      |
|(6) UCLA|1.98|1.10|0.88          |
|(11) Minnesota|1.96|0.60|1.36    |
|(10) Colorado|1.96|0.20|1.76     |
|(10) Iowa State|1.96|0.50|1.46   |
|(11) MTSU/SMC|1.91|0.10|1.81     |
|(8) Colorado St|1.73|0.20|1.53   |
|(10) Cincinnati|1.66|0.30|1.36   |
|(9) Wichita State|1.54|0.30|1.24 |
|(11) Bucknell|1.26|0.20|1.06     |
|(12) Ole Miss|1.22|0.90|0.32     |
|(12) Oregon|1.22|0.70|0.52       |
|(9) Temple|1.20|0.20|1.00        |
|(12) California|1.08|0.20|0.88   |
|(10) Oklahoma|1.05|0.20|0.85     |
|(11) Belmont|0.99|0.20|0.79      |
|(9) Villanova|0.93|0.50|0.43     |
|(13) BSU/LaSalle|0.58|0.10|0.48  |
|(14) Davidson|0.56|0.20|0.36     |
|(12) Akron|0.48|0.10|0.38        |
|(14) Valparaiso|0.18|0.10|0.08   |
|(15) Pacific|0.13|0.10|0.03      |
|(13) N Mexico St|0.12|0.10|0.02  |
|(15) Iona|0.09|0.10|-0.01        |
|(13) SD State|0.08|0.10|-0.02    |
|(14) Harvard|0.07|0.20|-0.13     |
|(13) Montana|0.05|0.10|-0.05     |
|(15) Florida Gulf|0.03|0.10|-0.07|
|(15) Albany|0.02|0.10|-0.08      |
|(14) NW State|0.02|0.10|-0.08    |
|(16) LIU|0.01|0.30|-0.29         |
|(16) NCAT/Lbrty|0.00|0.30|-0.30  |
|(16) W Kentucky|0.00|0.40|-0.40  |
|(16) Southern U|0.00|0.30|-0.30  |

Notice that even though the four 1-seeds have the highest actual probability of making it, they also have some of the highest pick percentages, making them not a very good value for you. If one of them makes it, whoopdie-do, you got it right, so did everyone else. Remember, we're going for **expected value** here. A team like (3) Florida may be a great Final 4 pick, they've got better than a 1 in 4 shot of making it but only 1 in 6 people are picking them to go there. You can do the same thing for the earlier rounds, check out the spreadsheet linked above. Spoiler alert: (7) Creighton and (8) Pitt in the Elite-8.

Now obviously the biggest problem with this method is the reliance on these "actual probabilities." If someone could come up with a fool-proof method of computing these they'd probably be rich and living on an island by now. But that doesn't mean people aren't trying. The beauty of this method is that you can plug in any formula for probability that you like. A <a href="https://www.google.com/search?q=sports+score+predictor" target="_blank">quick Google search</a> will yield you dozens of people that will attempt to do it. For this example, I used <a title="TeamRankings" href="http://www.teamrankings.com/" target="_blank">TeamRankings</a> and their matchup predictor. They take several factors into account and normally make you pay for it, but due to some lazy programming on their end you can pretty much find any matchup you want if you know what you're doing. You can modify <a title="TeamRankings Grabber" href="https://gist.github.com/mattdodge/5198428" target="_blank">the program</a> used to gather all the matchups if you want.

This is just a concept, I have never actually used this (but I will be using it this year, hopefully no one who is in my pool is reading this...). I'm open to suggestions, let's see what happens. Cross your fingers for no 1-seeds in the Final 4!
