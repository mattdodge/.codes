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

<center>
  <span class='MathJax_Preview'>\(\mathfrak{\mathit{P\ =\ W(1,16)\ [W(8,9)\ W(1,8)\ +\ W(9,8)\ W(1,9)]}}\)</span>
</center>


  
Obviously, this equation will get quite long for the later rounds so it's probably best if you <a title="NCAA Bayes Gist" href="https://gist.github.com/mattdodge/5197813" target="_blank">write or use a program</a> to create these calculations for you. What you end up with is a list of every team and the probability that each team makes it to each round of the tournament.

Here's where it gets fun. ESPN is nice and <a title="ESPN Tournament Pickem" href="http://games.espn.go.com/tournament-challenge-bracket/en/whopickedwhom" target="_blank">puts out a list</a> of the percentage of people that pick each team to make it to each round, so we can directly compare these percentages. The theory behind this being that if a team has a 30% shot of making it to the Elite 8, but 45% of people pick that team to get there, your expected value by picking them is pretty low. We are looking to find (especially in a season full of upsets like we had this year) the teams that have a decent shot that no one is picking for some reason or another. You can check out the full list and breakdown on <a title="2013 NCAA Probabilities" href="https://dl.dropbox.com/u/603003/2013%20NCAA%20Probabilities.xlsx" target="_blank">this spreadsheet</a> but I'll give you the highlights here.

**Final Four - Probability of making it vs. the number of people who think they will**
  
[table id=1 /]

Notice that even though the four 1-seeds have the highest actual probability of making it, they also have some of the highest pick percentages, making them not a very good value for you. If one of them makes it, whoopdie-do, you got it right, so did everyone else. Remember, we're going for **expected value** here. A team like (3) Florida may be a great Final 4 pick, they've got better than a 1 in 4 shot of making it but only 1 in 6 people are picking them to go there. You can do the same thing for the earlier rounds, check out the spreadsheet linked above. Spoiler alert: (7) Creighton and (8) Pitt in the Elite-8.

Now obviously the biggest problem with this method is the reliance on these "actual probabilities." If someone could come up with a fool-proof method of computing these they'd probably be rich and living on an island by now. But that doesn't mean people aren't trying. The beauty of this method is that you can plug in any formula for probability that you like. A <a href="https://www.google.com/search?q=sports+score+predictor" target="_blank">quick Google search</a> will yield you dozens of people that will attempt to do it. For this example, I used <a title="TeamRankings" href="http://www.teamrankings.com/" target="_blank">TeamRankings</a> and their matchup predictor. They take several factors into account and normally make you pay for it, but due to some lazy programming on their end you can pretty much find any matchup you want if you know what you're doing. You can modify <a title="TeamRankings Grabber" href="https://gist.github.com/mattdodge/5198428" target="_blank">the program</a> used to gather all the matchups if you want.

This is just a concept, I have never actually used this (but I will be using it this year, hopefully no one who is in my pool is reading this...). I'm open to suggestions, let's see what happens. Cross your fingers for no 1-seeds in the Final 4!
