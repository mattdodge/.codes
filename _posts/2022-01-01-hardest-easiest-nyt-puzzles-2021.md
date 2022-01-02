---
layout: post
title: The Hardest and Easiest New York Times Crossword Puzzles of 2021
date: 2022-01-01 15:14:00
header-opacity: dark
header-img: "img/xword/bg.jpg"
permalink: /hardest-easiest-nyt-puzzles-2021/
medium-link: https://medium.com/@that314guy/the-hardest-and-easiest-new-york-times-crossword-puzzles-of-2021-e214eef55f7e
---

2021 is officially behind us. Let's take a look back at the 365 daily New York Times crossword puzzles from the year and see which ones were the hardest and which ones were the easiest. Using the crowd-sourced solve times from hundreds of solvers at [XW Stats](https://xwstats.com){:target="_blank"}, I was able to rank the puzzles in terms of difficulty. I'll describe the method below, but for you with short attention spans here are those rankings, along with links to the puzzles at XWordInfo.

## Hardest Puzzles of 2021

Here are the 5 most difficult/slowest puzzles of 2021, based on comparing solve times to a solver's average solve time on that day of the week.

1. [Friday, Dec 10](https://www.xwordinfo.com/Crossword?date=12/10/2021){:target="_blank"} - Joe DiPietro, Themeless (68.1% harder)
2. [Friday, Mar 19](https://www.xwordinfo.com/Crossword?date=3/19/2021){:target="_blank"} - Kameron Austin Collins, Themeless (60.3% harder)
3. [Wednesday, Oct 6](https://www.xwordinfo.com/Crossword?date=10/6/2021){:target="_blank"} - Jules Markey, Pigeon Holes (49.9% harder)
4. [Tuesday, June 8](https://www.xwordinfo.com/Crossword?date=6/8/2021){:target="_blank"} - Christopher Adams,  Added H (49.4% harder)
5. [Sunday, May 16](https://www.xwordinfo.com/Crossword?date=5/16/2021){:target="_blank"} - Joe DiPietro, A Shot in the Dark (48.2% harder)

Joe DiPietro, a veteran constructor with hundreds of published puzzles, appears twice on the list. His Friday December 10 puzzle had some tricky crosses and difficult cluing. 19% of XW Stats solvers needed to cheat on that puzzle, compared to the 10% average cheat rate on Fridays. Jules Markey clearly caught the solvers off-guard with the rare Wednesday rebus puzzle.

## Easiest Puzzles of 2021

Here are the 5 easiest/fastest puzzles of 2021, based on comparing solve times to a solver's average solve time on that day of the week.

1. [Thursday, Nov 18](https://www.xwordinfo.com/Crossword?date=11/18/2021){:target="_blank"} - Ori Brian, First Second Middle... (46.2% easier)
2. [Thursday, Sep 16](https://www.xwordinfo.com/Crossword?date=9/16/2021){:target="_blank"} - Kevin Patterson, Silver Lining (45.8% easier)
3. [Friday, Oct 29](https://www.xwordinfo.com/Crossword?date=10/29/2021){:target="_blank"} - Aimee Lucido, Themeless (43.69% easier)
4. [Sunday, Dec 5](https://www.xwordinfo.com/Crossword?date=12/5/2021){:target="_blank"} - Chase Dittrich and Jeff Chen, Come Again? (43.67% easier)
5. [Wednesday, Oct 27](https://www.xwordinfo.com/Crossword?date=10/27/2021){:target="_blank"} - Johanna Fenimore, SNL (43.5% easier)

A couple of Thursdays lead the easy list this year. Neither involved any kind of rebus or typical Thursday trickery which probably led to their faster than average solve times. Johanna Fenimore's inclusion of several famous Saturday Night Live quotes also seemed to be a crowd favorite.

## Calculation Method

To compute these lists, I define a puzzle's difficulty to be how much longer a solver takes to solve that puzzle compared to his/her average on that day of the week. Let's get into some specifics.

The first concept to understand that drives this calculation is something I call "Adjusted Solve Time." You can read more about [Adjusted Solve Time on the XW Stats help page](https://xwstats.com/help#adjusted-solve-time){:target="_blank"}, but it is basically an adjusted time it takes to solve a puzzle that takes into account any squares that were checked or revealed. This metric adds a time penalty for checking and revealing squares so that puzzles that require "cheating" show up as harder than those that do not.

Once I have a solver's adjusted solve time, the goal is to get their "average solve time" too. This value is intended to represent an individual solver's average time to solve a puzzle **on that day of the week**. Monday puzzles are obviously easier than Thursdays, so a solver will have different averages for each. But solvers also sometimes get better at puzzles over time. It is relatively safe to assume that a solver, especially a newer one, is probably solving puzzles faster at the end of the year than they were at the beginning of the year. To account for this, the average solve time metric uses a 10 week rolling average for each day of the week. In other words, a solver's average solve time on a random Tuesday is the average of their solve times on the previous 10 Tuesday puzzles before that.

Now I can compare a solver's adjusted solve time for a given puzzle and compare it to their "expected" adjusted solve time (their 10 day average) for that puzzle. I can do this for every solver and for every puzzle throughout the year, then attempt to aggregate this data together. There are some different ways to do this, but I ended up settling on taking the **median** percentage difference of adjusted solve times for each puzzle. Median makes more sense than mean, as the latter can be heavily skewed by outliers or strange performances. I opted for median over a different percentile (e.g., 75th percentile) because I believed it would best capture the experience of both faster and slower solvers.

So, putting this all together gives me percentages of how much slower/harder or faster/easier a puzzle was. When I say that Joe DiPietro's puzzle was 68.1% harder, that means that the median solver took 68.1% longer to solve that puzzle than they did the 10 previous Friday puzzles before that. This includes the time penalties for checking/revealing.

## Raw Data

If you'd like to conduct some of your own analysis, I've provided some of the raw data here. This data has been normalized and anonymized.

[Crossword Difficulty 2021 Full Data.csv](/files/Crossword Difficulty 2021 Full Data.csv){:target="_blank"}

Here is a brief description of each field in the CSV:
* Puzzle Date - the date the puzzle was published
* Constructor - the puzzle's constructor
* Day of Week - the day of the week the puzzle was constructed
* Cheat Pct - the percentage of solvers that checked or revealed a square in the puzzle
* Adjusted Mean - the average/mean of all adjusted solve times compared to average adjusted solve time
* Adjusted Median - the median of all adjusted solve times compared to average adjusted solve time
* Adjusted P75 - the 75th percentile of all adjusted solve times compared to average adjusted solve time
* Adjusted P90 - the 90th percentile of all adjusted solve times compared to average adjusted solve time
* Actual Mean - the average/mean of all non-adjusted solve times compared to average solve time
* Actual Median - the median of all non-adjusted solve times compared to average solve time
* Actual P75 - the 75th percentile of all non-adjusted solve times compared to average solve time
* Actual P90 - the 90th percentile of all non-adjusted solve times compared to average solve time
