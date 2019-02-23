---
layout: post
title: FiveThirtyEight Riddler - Prolog to the Rescue!
date: 2019-02-22 15:14:00
permalink: /prolog-to-the-rescue/
medium-link: https://medium.com/@that314guy/fivethirtyeight-riddler-prolog-to-the-rescue-c622d7fac36e
---

One of my weekly routines is to solve the [FiveThirtyEight Riddler](https://fivethirtyeight.com/tag/the-riddler/) puzzle. If you've never done them before, they are a set of weekly riddles that tend to involve math, logic, and statistics. Sometimes, though, they make you think. That's silly, thinking is what computers are for!

This week's puzzle is a perfect example of a puzzle that can be solved with one of my favorite programming languages, [Prolog](https://en.wikipedia.org/wiki/Prolog). I believe that in order to be a good software engineer or computer scientist you must be versed in many different programming languages. There simply isn't one "best" programming language, they come in all shapes and sizes and some are better than others for a certain problem.

One rather niche programming language is Prolog. If you've never seen it or worked with it before I highly recommend checking out [some](https://learnxinyminutes.com/docs/prolog/) [tutorials](https://en.wikibooks.org/wiki/Prolog/Introduction) and getting familiar with the basics. Essentially, it's a language that allows you to define a set of rules within a problem space and then find the answers, if they exist, that satisfy all of those rules.

In this post, I'll walk through how we can use Prolog to quickly and succinctly arrive at an answer for this week's Riddler puzzle. I'll be using [SWI-Prolog](http://www.swi-prolog.org) for this, but I believe it will work in any flavor of Prolog that you choose.

## Problem Statement

First, let's take a look at [this week's puzzle](https://fivethirtyeight.com/features/dont-trust-anyone-older-than-l/) description:

> We are marooned on an island that has the following curious property: Everyone over a certain age lies all the time. More specifically, there is an age limit L — a positive integer — and all islanders who are younger than age L only tell the truth, while islanders who are at least L years old only tell lies.
>
> We are greeted by five islanders who make the following statements:
>
> - A: “B is more than 20 years old.”
> - B: “C is more than 18 years old.”
> - C: “D is less than 22 years old.”
> - D: “E is not 17 years old.”
> - E: “A is more than 21 years old.”
> - A: “D is more than 16 years old.”
> - B: “E is less than 20 years old.”
> - C: “A is 19 years old.”
> - D: “B is 20 years old.”
> - E: “C is less than 18 years old.”
>
> What is L? And what did we just learn about the ages of the islanders?

So basically we have a group of 5 islanders with certain ages. Some of them are liars and some are not, and they are giving us facts about the other islanders' ages. These "facts" make great rules in a Prolog program. Let's get to work.

## Starting Small

We'll start off with a very small subset of this puzzle. I'm going to show some Prolog syntax below. If you've never seen it before it may look a little strange, I encourage you to read through [this wiki tutorial](https://en.wikibooks.org/wiki/Prolog/Introduction) first, it will give you the basics.

I'm going to start with some simple claims about peoples ages.

```prolog
% Claim that a Person is older than Age
claim_older(Person, Age) :- Person > Age.

% Claim that a Person is younger than Age
claim_younger(Person, Age) :- Person < Age.
```

This sets up 2 types of "claims" that we can make about an islander's age. A claim that a person is older than an age (`claim_older`) is true **only if** that person's age is greater than the specified age. A similar rule is declared for younger. We can load this Prolog program into our interpreter and start making queries against it. 

> Note: We need to specify some "bounds" for the person's age, otherwise Prolog won't know how to assign someone's age and will try infinite possibilities. This is the purpose of the `between` fact in our query.

First, we'll bound A's age between 10 and 40 (reasonable boundaries given the problem statement). Then we'll specify a claim that A is older than 15 and a claim that A is younger than 18. Prolog will tell us possibilities for how old A is.

```prolog
?- between(10, 40, A), claim_older(A, 15), claim_younger(A, 18).
A = 16 ;
A = 17 ;
false.
```

Prolog has determined that A is either 16 or 17 years old - that makes sense! When querying in the interpreter, you can press `;` to see more possible results. In this case, after we hit the 17 result there were no more valid options, so Prolog returned `false`. 

What happens if we give it an impossible claim like A is older than 18 and younger than 15?

```prolog
?- between(10, 40, A), claim_older(A, 18), claim_younger(A, 15).
false.
```

We can even specify claims about multiple people - this time the query is broken up into multiple lines to make it easier to read:

```prolog
?- between(10, 40, A),
   between(10, 40, B),
   claim_older(A, 15),
   claim_younger(A, 17),
   claim_older(B, 35).
A = 16,
B = 36 ;
A = 16,
B = 37 ;
A = 16,
B = 38 ;
A = 16,
B = 39 ;
A = 16,
B = 40 ;
false.
```

Prolog has spit out all of the possible pairs of ages that are valid for this scenario, neat!

## The Invention of Lying

In our first runs we were able to make claims but we always assumed those claims were true. Per our problem statement, claims are only true if the claimant is under a certain age. Let's define some additional rules for when a claim is made but the claimant is lying. 

```prolog
% A claimant is a liar if they are at least a certain age
% We'll hard code this to 20 years old for now
liar(Claimant) :- Claimant >= 20.

% We make two declarations for an older claim, one from a lying
% claimant and one from one telling the truth
claim_older(Person, Age, Claimant) :-
    liar(Claimant),
    Person =< Age.
claim_older(Person, Age, Claimant) :-
    not(liar(Claimant)),
    Person > Age.

claim_younger(Person, Age, Claimant) :-
    liar(Claimant),
    Person >= Age.
claim_younger(Person, Age, Claimant) :-
    not(liar(Claimant)),
    Person < Age.

run(A, B) :-
    between(10, 40, A),
    between(10, 40, B),
    claim_older(A, 18, B),
    claim_younger(A, 15, B),
    claim_younger(B, 21, A).
```

There are a few things to notice about our program now.

1. We are hard-coding the age of lying to 20 for now, we'll variablize this soon.
2. We've added the ability for a claim to have a `Claimant` as well - this the person making the claim.
3. We've added a second set of rules for each claim, now with one that supports a liar. Notice the inverted comparison operators in each set of claims. When Prolog looks through its rules it will find one that matches. We can create an "or" condition by specifying multiple possibilities for each claim - one for a lying claimant and one for one who tells the truth.
4. We have added our claims and bounds to a `run` rule, this saves us some repetitive typing in the interpreter. Now we can just query `run(A, B)` and see the results. Let's do that now!

```prolog
?- run(A, B).
A = 15,
B = 20 ;
A = 16,
B = 20 ;
A = 17,
B = 20 ;
A = 18,
B = 20 ;
false.
```

Prolog has given us some pairs of ages that are valid given the rules we provided. The possibilities boil down to `15 <= A <= 18`, and `B = 20`. If we look back to our original claims we basically had B claiming that A is older than 18 and younger than 15. Since that is impossible, Prolog determined that B must be a liar. But since liars are only at least 20 years old (since we hard coded that rule), B must be at least 20. However, we also had A claiming that B was younger than 21. Since we know A is between 15 and 18 (since B was a liar), we know A must be telling the truth (younger than 20). So B indeed is younger than 21 and at least 20, or, in other words, B is 20. 

## Sexier Ranges

Bet that's the first time you've seen those two words next to each other. 

Prolog is great at giving us multiple solutions to the problem. But we're going to be working a lot with ranges of ages. For the previous example, it would have been great if rather than iterating through every possible set of ages Prolog could have told us the range of ages that were possible in one go. Fortunately, Prolog provides a way to define "constraints" for values using something called [Constraint Logic Programming](https://en.wikibooks.org/wiki/Prolog/Constraint_Logic_Programming). Let's adapt our previous code to use constraints instead now.

```prolog
:- use_module(library(clpfd)).

liar(Claimant) :- Claimant #>= 20.
not_liar(Claimant) :- Claimant #< 20.

claim_older(Person, Age, Claimant) :-
    liar(Claimant),
    Person #=< Age.
claim_older(Person, Age, Claimant) :-
    not_liar(Claimant),
    Person #> Age.

claim_younger(Person, Age, Claimant) :-
    liar(Claimant),
    Person #>= Age.
claim_younger(Person, Age, Claimant) :-
    not_liar(Claimant),
    Person #< Age.

run(A, B) :-
    A in 10..40,
    B in 10..40,
    claim_older(A, 18, B),
    claim_younger(A, 15, B),
    claim_younger(B, 21, A).
```

Check out what's new

1. We import the `clpfd` module at the top of our program rules now, to get access to the constraint logic predicates.
2. We prefix our comparison operators with the `#` symbol. So it's `Person #< Age` now instead of just `Person < Age`. These operators are documented [here](http://www.swi-prolog.org/pldoc/man?section=clpfd#sec:A.9.2) if you'd like to read more about them.
3. We added a `not_liar` rule instead of using `not(liar)`. This is most likely due to my noob-ness with Prolog but I kept having trouble inverting the CLP constraint operators so I wrote it out explicitly.
4. Use `A in 10..40` instead of `between(10, 40, A)` in our `run` rule.

Now check out our output when we run — much more readable!

```prolog
?- run(A, B).
B = 20,
A in 15..18 ;
false.
```

## Variable Liars

Up until now, we've been assuming that everyone over 20 is a liar. In reality, the age of that lying threshold is something we need to solve for as well. We'll do this by making another variable for the lying age and pass that through with our claims.

```prolog
:- use_module(library(clpfd)).

liar(Claimant, LieThreshold) :- Claimant #>= LieThreshold.
not_liar(Claimant, LieThreshold) :- Claimant #< LieThreshold.

claim_older(Person, Age, Claimant, LieThreshold) :- 
	liar(Claimant, LieThreshold),
	Person #=< Age.
claim_older(Person, Age, Claimant, LieThreshold) :- 
	not_liar(Claimant, LieThreshold),
	Person #> Age.

claim_younger(Person, Age, Claimant, LieThreshold) :- 
	liar(Claimant, LieThreshold),
	Person #>= Age.
claim_younger(Person, Age, Claimant, LieThreshold) :- 
	not_liar(Claimant, LieThreshold),
	Person #< Age.

run(A, B, L) :-
	A in 10..40,
	B in 10..40,
	L in 10..40,
	claim_older(A, 18, B, L),
	claim_younger(A, 15, B, L),
	claim_younger(B, 21, A, L).
```

We've pretty much just added an argument to each of our rules that will keep track of the lying age threshold. We can test this by running again with our known threshold of 20.

```prolog
?- run(A, B, 20).
B = 20,
A in 15..18 ;
false.
```

Cool, same result, that makes sense! But what if we want a variable lie threshold?

```prolog
?- run(A, B, L).
A in 15..18,
A#>=L,
L in 10..18,
B in 21..40 ;

A in 15..18,
A#=<L+ -1,
L in 16..20,
B#>=L,
B#>=L,
B in 16..20 ;
false.
```

This syntax is a little weirder but it makes sense if you parse it out. I won't go into too much detail (it won't really matter when we plug in our actual claims) but here's the gist. With a completely variable lie threshold (well, between 10 and 40 per our constraint) there are two solution possibilities. We know B is a liar because of the impossible claim. But what if A is a liar too.

## Tying it all Together

We've got a pretty slick claiming system in a rather succinct format (imagine writing this code in another language!), now it's just time to fill out the actual claims from the problem statement and see what our solution is! (obviously, spoilers ahead...)

```prolog
:- use_module(library(clpfd)).

liar(Claimant, LieThreshold) :- Claimant #>= LieThreshold.
not_liar(Claimant, LieThreshold) :- Claimant #< LieThreshold.

claim_older(Person, Age, Claimant, LieThreshold) :- 
	liar(Claimant, LieThreshold),
	Person #=< Age.
claim_older(Person, Age, Claimant, LieThreshold) :- 
	not_liar(Claimant, LieThreshold),
	Person #> Age.

claim_younger(Person, Age, Claimant, LieThreshold) :- 
	liar(Claimant, LieThreshold),
	Person #>= Age.
claim_younger(Person, Age, Claimant, LieThreshold) :- 
	not_liar(Claimant, LieThreshold),
	Person #< Age.

claim_equal(Person, Age, Claimant, LieThreshold) :-
	liar(Claimant, LieThreshold),
	Person #\= Age.
claim_equal(Person, Age, Claimant, LieThreshold) :-
	not_liar(Claimant, LieThreshold),
	Person #= Age.
	
claim_not_equal(Person, Age, Claimant, LieThreshold) :-
	liar(Claimant, LieThreshold),
	Person #= Age.
claim_not_equal(Person, Age, Claimant, LieThreshold) :-
	not_liar(Claimant, LieThreshold),
	Person #\= Age.

run(Islanders, L) :-
	Islanders ins 10..40,
	L in 10..40,
	[A, B, C, D, E] = Islanders,
	claim_older(B, 20, A, L),
	claim_older(D, 16, A, L),
	claim_older(C, 18, B, L),
	claim_younger(E, 20, B, L),
	claim_younger(D, 22, C, L),
	claim_equal(A, 19, C, L),
	claim_not_equal(E, 17, D, L),
	claim_equal(B, 20, D, L),
	claim_older(A, 21, E, L),
	claim_younger(C, 18, E, L).
```

A few quick things to note about the changes here:

1. I've converted the `run` arguments to a list of islanders now, rather than explicitly saying A, B, etc. Also, notice the `ins` syntax for a list rather than the `in` syntax for a single variable.
2. I've added claims for "equal" and "not equal" ages. I probably could have gotten away with re-writing D's claim that E's age is not 17 in a different way, but I figured I'd be explicit.
3. I've loaded in the 10 claims from the problem statement.

And now if we run it, with our ages and lying threshold as variables, we will see our solution:

```prolog
?- run([A, B, C, D, E], L).
A = L, L = 19,
B = 20,
C = 18,
D in 10..16,
E in 20..40 ;
false.
```

There you have it! The age threshold is 19, making **A** (age 19), **B** (age 20) and **E** (age >= 20) all liars. **C** (age 18) and **D** (age <= 16) are truth tellers.

I hope this post has at least minimally encouraged you to go explore some other programming languages or to scrounge up some interest in Prolog. If you're curious or want to explore more, the completed and commented code from this tutorial is available on a [GitHub gist here](https://gist.github.com/mattdodge/bd2a2a0f885c732328054e9fac6a05c0). Also feel free to reach out to me with any questions, suggestions, or Prolog tips!
