---
layout: post
title: '24 : A childhood game taken to the next level'
date: 2013-07-19 15:27:49
permalink: /24-a-childhood-game-taken-to-the-next-level/
dsq_thread_id: 1513235818
---

Remember that math game 24 from middle school? It's the one where you are given 4 numbers and you have to do basic math operations (+,-,\*,/) to make the 4 numbers equal 24. For example, if I had the numbers 2,3,4,5 I could get to 24 by doing 2\*(5+4+3) or maybe by doing 4*(5+3-2). You get the idea.

So the game itself is pretty simple and for some reason I find myself playing it with the numbers on a clock or credit card or something else when I am bored. And I was finding that it was normally pretty easy to find a solution given any set of numbers. That made me start to ask a couple of questions.<!--more-->

  1. Exactly how many solutions exist for solving 24, given a set of 4 integers each between 1 and 9?
  2. Is there a reason 24 was chosen? Does it have more solutions than another number, say 20?

The first step was to figure out exactly how many sets of 4 numbers there were. We need unique sets here, the set [1,3,3,7] is fundamentally equivalent to [3,1,3,7] for the purposes of the game. Sadly, this took me longer than it should have, my combinatorics must be rusty. But by using the <a href="http://en.wikipedia.org/wiki/Multiset_coefficient#Counting_multisets" target="_blank">Multiset Coefficient</a> (basically, combinations with replacement) I used the following formula to determine that there are only **495 unique sets**. Much less than I expected given that there are 6,561 total sets of numbers (9^4).

<center>
  <span class='MathJax_Preview'>\(\mathfrak{\mathit{\frac{(n+k-1)!}{k! (n-1)!}}}\)</span>
</center>


  
The next step is to bust out the IDE and get to coding. Fortunately, this part didn't take as long as the math (my coding isn't rusty yet, phew). Here is a quick little Python 24-solver (really an N-solver...we'll get there) I threw together:

<div class="oembed-gist">
  <noscript>
    View the code on <a href="https://gist.github.com/mattdodge/6043083">Gist</a>.
  </noscript>
</div>

So now I've got a solver and a list of 495 unique sets, time to find out how many of those actually have a solution for 24. Ready? Drumroll....

<center>
  <strong>381</strong> out of 495 have a solution!
</center>


  
381?! Wayyyy more than I was guessing. No wonder this game is so easy, pick 4 numbers and you pretty much have a 3 out of 4 shot that there is a solution. So on to part 2, is there possibly an easier or more difficult number to use as a target? I ran it for target solutions from 1 to 100, and the winner (meaning the number with the most solutions) is....more drumroll...

<center>
  <strong>2! </strong>- with 479 solutions out of 495 possible
</center>


  
2 has the most, followed up by the rest of the single-digit numbers interestingly. 24 came in 17th place. 94 and 97 tied for last with 40 solutions each (remember, only checking from 1-100). Here is the full list for your viewing pleasure.

<center>
  [table id=3 /]
</center>One more fun thing; there are actually 2 (and only 2!) sets of 4 that can produce solutions for a target of 314. Here they are, can you come up with the formulas to get there?

<center>
  1 5 7 9<br /> 5 6 8 8
</center>
