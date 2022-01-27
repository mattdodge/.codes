---
layout: post
title: Building a Wordle Bot In Under 100 Lines of Python
date: 2022-01-26 15:14:00
header-opacity: dark
header-img: "img/wordle_photo.jpeg"
permalink: /wordle-bot/
medium-link: https://medium.com/@that314guy/the-hardest-and-easiest-new-york-times-crossword-puzzles-of-2021-e214eef55f7e
---

If you don't know what Wordle is, get out from under your rock and check it out. It's a simple game at surface level but does end up needing some strategy to master it. It didn't take more than a couple of rounds of playing it before my curiosity piqued and I felt the need to build a bot to play the game and learn more about it. Let's get going!

## The Objective

Build a Python program that plays Wordle. It should make guesses of words and try and figure out the ultimate answer word in as few guesses as possible.

## The Basics

The basics of the game are pretty straightforward. We need to be able to keep a list of valid words, make guesses, collect responses, and update our word list based on the data in those responses. This high-level Python code can be the baseline of our solving application.

```python
def solve():
    valid_words = ALL_WORDS
    while True:
        guess = make_guess(valid_words)
        print("Guess: " + guess.upper())
        result = collect_result()
        if result == CORRECT:
            print("I won!")
            break
        valid_words = update_valid_words(valid_words, guess, result)
```

We really only have 3 methods to build here with this simple structure. The first is `make_guess`. This method should take a list of valid words and guess a word out of that list. To start, let's just choose randomly. We'll make this smarter later. For now we just want to get the basic gameplay in place.

```python
import random

def make_guess(valid_words):
    """ Given a list of valid words, make a guess as to the next word """
    return random.choice(valid_words)
```

The `collect_result` method just needs to collect some input from the user about the guess. The user of our program will input the guess into the Wordle website and that will tell us what is right and wrong. The user needs to input those values into our program. We'll use 3 different characters to denote the 3 possible results for each letter:

* `_` = Miss - this letter does not appear in the word
* `?` = Wrong Spot - this letter is in the word but not in this location
* `!` = Correct - this letter appears in the word in this spot

Some straightforward Python user input collection along with some regex matching for data validation gets us on our way.

```python
import re

def collect_result():
    """ Collect the result of our guess from the user """
    result = input("What's the result? (_/?/!) ")
    match = re.match(r'^[!?_]{5}', result)
    if not match:
        print("Invalid response string, try again")
        return collect_result()
    return result
```

Now we need to update our valid words based on our guess and the result, that's what the `update_valid_words` method is for. We'll start with a trivial implementation of this that isn't very performant but should get the job done. We'll iterate through all remaining valid words and see if they would have yielded the same result had we guessed our guess. In order to do this, we'll need a new method called `get_result` that will return what the result for a guess would be if we knew the answer.

```python
def get_result(guess, answer):
    """ Given a guess and a known answer, return what the result would be """
    result = ""
    for pos, ch_guess, ch_answer in zip(range(5), guess, answer):
        if ch_guess == ch_answer:
            result += "!"
        elif ch_guess not in answer:
            result += "_"
        else:
            result += "?"
    return result

def update_valid_words(valid_words, guess, result):
    """ Return a list of valid words based on previously valid words and a new guess/result """
    return [word for word in valid_words if get_result(guess, word) == result]
```

There are a few issues with this, specifically around words with duplicate letters (e.g. ROBOT, SPEED, etc). But we'll sort those out later. At this point we have enough to play the game! We can put all of these methods together and play. Let's see how our game plays with a target word of SNAKE.

```
$ python solve_wordle.py
Guess: WHISK
What's the result? (_/?/!) ___??
Guess: DUSKY
What's the result? (_/?/!) __?!_
Guess: SPOKE
What's the result? (_/?/!) !__!!
Guess: STAKE
What's the result? (_/?/!) !_!!!
Guess: SNAKE
What's the result? (_/?/!) !!!!!
I won!
```

It worked! In 5 guesses even, not too shabby considering the simple approach for a lot of those methods. Since we're relying on a random choice here this will likely be different each time too, 5 seemed to be the median solve amount for SNAKE given this algorithm. Let's see if we can do better.

## Smarter Guesses

A good Wordle player is one who can guess a good word. Astute analysis, am I right? To start we just randomly picked a word from our remaining words, but that's not very smart. What makes a good guess though?

My initial theory was that a good guess would be one that had the most common letters in it. This is a fairly safe assumption, especially early on in the guessing. With many words left we want to learn as much about them all as we can, so guessing common letters is a natural instinct. Surely a first guess of "RATED" would likely teach us more about the word than a guess of "JAZZY" would. Let's enhance our `make_guess` method to employ this strategy.

To do this, the first step is to figure out which letters are the most frequent. We should only consider valid words for these character counts too, no sense counting words we know are not correct. Python's built-in `Counter` object can help with this.

After we get the counts of each character in the remaining words, we want to find the words that have the most of these characters. We also don't want to count duplicates here either. Remember, our goal is to find out about as many letters as possible. So while E may be the most common letter in words, it doesn't make sense to guess EERIE. We don't want to waste those other letter slots.

```python
from collections import Counter

def make_guess(valid_words):
    # Given a list of valid words, make a guess as to the next word

    # Figure out the counts first

    counts = Counter()
    for word in valid_words:
        counts.update(word)

    # Now score the words based on their counts
    # This will be a list of tuples (score, word)
    word_scores = []
    for word in valid_words:
        unique_chars = set(word)
        word_score = sum([counts.get(c) for c in unique_chars])
        word_scores.append((word_score, word))

    # Sort the list and pick the highest score
    return sorted(word_scores, reverse=True)[0][1]
```

Not a terribly complicated method. We can probably make a few enhancements, like ignore letters that we already know are in the word, but this should do for now. Let's play again against the word SNAKE and see if we do better.

```
$ python solve_wordle.py
Guess: LATER
What's the result? (_/?/!) _?_?_
Guess: SHADE
What's the result? (_/?/!) !_!_!
Guess: SUAVE
What's the result? (_/?/!) !_!_!
Guess: SPACE
What's the result? (_/?/!) !_!_!
Guess: SNAKE
What's the result? (_/?/!) !!!!!
I won!
```


We still got the word but it still took us 5 tries. Also, since our guessing algorithm is deterministic now, this will be the result every single time we play, at least for a target word of SNAKE.

One interesting observation here is that this algorithm thinks the best possible starting word is LATER (or any of its anagrams i.e., ALTER, ALERT). If you're looking for a good starting word, these might be good ones to try.

This algorithm will get some words in just a few tries, but 5 tries doesn't cut it for me. I can do that on my own, I want my computer to be smarter than me. Let's do better.

## Put This Machine To Work

Linear-time frequency counting isn't much of an algorithm to write home about. We might even be able to roughly eyeball that ourselves as humans. If we think about what a good guess is, it is one that will tell us the most information based on the response. In other words, it is the one that will yield, on average, the smallest next list of valid words.

We have a computer in front of us, we can simulate this! For each valid word remaining, what would happen if we guessed it? We can run each valid word as a guess against every other valid word as an answer and see what the resulting word list would be. This comes out to an O(N^2) algorithm against a relatively small problem space of just a few thousand words. Let's go!

```python
def make_guess(valid_words):
    """ Given a list of valid words, make a guess as to the next word """
    guess = None
    lowest_worst_score = len(valid_words) + 1
    lowest_total_score = len(valid_words) ** 2
    for possible_guess in valid_words:
        worst_score = 0
        total_score = 0
        for possible_answer in valid_words:
            # reuse our old get_result method from before, nice
            result = get_result(possible_guess, possible_answer)
            num_new_valid_words = len(update_valid_words(valid_words, possible_guess, result))
            worst_score = max(num_new_valid_words, worst_score)
            total_score += num_new_valid_words

        # If this possble guess has a lower total/average number of words remaining, use it instead
        if worst_score < lowest_worst_score:
            guess = possible_guess
            lowest_worst_score = worst_score
            lowest_total_score = total_score

        # If it's a tie in average words remaining, choose the one that doesn't have the worst case
        elif worst_score == lowest_worst_score and total_score < lowest_total_score:
            guess = possible_guess
            lowest_worst_score = worst_score
            lowest_total_score = total_score
    return guess
```

This doesn't look too bad, basically we're just seeing how many words will be remaining after every possible guess and every possible answer. It will definitely be slower, but I think it will be smarter too. However, there's a small gotcha here. We thought our algorithm would run in O(N^2) time, which isn't too bad. But our rather simple implementation of `update_valid_words` comes back to bite us here. That method actually has its own O(N) runtime, so really this guess method will now run in O(N^3). It's still a small data set, but cubic runtime gets big quickly.

Rather than trying to optimize either of those methods, let's just make this strategy the one that runs once we're down to less than 500 words in our valid word list. We can keep the frequency counting algorithm for longer word lists but once we're closer to the end we'll do the deep search.

```python
def make_guess(valid_words):
    if len(valid_words) < 500:
        return make_guess_exhaustive(valid_words)
    else:
        return make_guess_frequency(valid_words)
```

Time to try it out against the word SNAKE again!
  
```
$ python solve_wordle.py
Guess: LATER
What's the result? (_/?/!) _?_?_
Guess: SHADE
What's the result? (_/?/!) !_!_!
Guess: SNAKE
What's the result? (_/?/!) !!!!!
I won!
```

Nice! Only 3 guesses! We might be getting a bit lucky here though. If we know the S, A, and E are in the right places then SNAKE, SUAVE, and SPACE should all be roughly equal. But this algorithm should still be more performant in the general case. Can we test that theory though?

## The Hardest Wordle Word

At this point I got a bit curious, how would this bot do against other wordle words? I had spot-checked a few of the recent games and the bot was consistently beating my mere human brain, exactly what I wanted! Would this be the case for any word? Was there a theoretical maximum number of guesses a bot with this strategy would need to guess a word?

I decided to run a simulator, much like we do for the exhaustive guess algorithm. For every word in the word list, run the bot and see how many guesses it would take to guess it. The code for this wasn't really challenging, but it's a bit beside the point so I won't share it here.

As expected, most of the words were solved in 2 or 3 guesses. The occasional 4 would slip in there too. But there was one word that took 8, yes 8, guesses to get. Without further ado, I present to you the hardest possible Wordle word:

**CATCH**

Catch? Really? Why is that hard? Pretty easy word with pretty common letters. What's going on here? Let's play with our bot and see what happens.

```
$ python solve_wordle.py
Guess: LATER
What's the result? (_/?/!) _!!__
Guess: FATTY
What's the result? (_/?/!) _!!?_
Guess: HATCH
What's the result? (_/?/!) _!!!!
Guess: BATCH
What's the result? (_/?/!) _!!!!
Guess: MATCH
What's the result? (_/?/!) _!!!!
Guess: WATCH
What's the result? (_/?/!) _!!!!
Guess: PATCH
What's the result? (_/?/!) _!!!!
Guess: CATCH
What's the result? (_/?/!) !!!!!
I won!
```

Interesting! The bot quickly got to the _ATCH suffix, but then was stuck spinning its wheels trying to guess the first letter. Because it didn't have that much information, it was really only able to guess one letter at a time and hope that it was right. If we could somehow learn about more than one letter here we could probably solve it faster.

And there it is, a truly marvelous realization after I looked at this for a while:

**It is sometimes advantageous to guess a word that you _know_ is wrong in order to make a better guess on your next word.**

The rules just state the guess has to be a real word, not a word that is valid given the current set of known information. In other words, if we have 6 words that end in _ATCH left in our list, don't just guess them one by one. Instead, guess another word that **isn't** in the remaining valid word list to try and get as much info as possible.

What does this look like in code? It's surprisingly easy. Instead of iterating through valid words looking for possible guesses, we'll iterate through **all** words. This will have an obvious performance hit, so we'll only run this when we're down to 50 words or less. 

Here's the relevant part of our `make_guess_exhaustive` method now
```python
def make_guess_exhaustive(valid_words):
    guessable_words = ALL_WORDS if len(valid_words) < 50 else valid_words
    for possible_guess in guessable_words:
        # rest of the method goes here...
```

Now what happens if we run against CATCH?

```
$ python solve_wordle.py
Guess: LATER
What's the result? (_/?/!) _!!__
Guess: CRIMP
What's the result? (_/?/!) !____
Guess: CATCH
What's the result? (_/?/!) !!!!!
I won!
```

Look at that! After finding out that the A and T were correct, the bot guesses CRIMP with zero intention of winning but instead gathering information. As a result, it knows which letter is the first letter and is able to guess CATCH correctly on the 3rd try. Much better than the 8th guess like before. I call that a win!

## Wrap Up and Other Enhancements

This was a fun little experiment and weekend project. I think it also demonstrates how an idea can start small and with some pretty simple pseudo-code and eventually turn into a rather complex and impactful algorithm. The code is still concise, but it ends up getting the job done.

There were a few gotchas I discovered along the way while building this that I didn't get into too much here, for the sake of keeping the article focused. The final code, shared below, includes these enhancements and realizations though.

* Multiple Letters - Wordle is kind of weird (smart?) about how it handles multiple letters in a word. If the answer is ROUND and your guess is ROBOT, the first O would be green (right spot) but the second O would be blank, rather than yellow. This is a bit of a gotcha and needs to be accounted for in the code
* Concept of "knowns" - The `update_valid_words` method's algorithm and performance wasn't satisfactory to me. I ended up implementing a concept of "knowns" about the answer word. These are basically things that we know about the word, where letters can and can't be, what letters are missing, etc. In the final code you'll see these knowns references flying around.
* Word list - If you've played Wordle you will notice that most words are pretty common. Most of the examples here only referenced a common word list (2,314 5-letter words). The list of 5-letter words in my `/usr/share/dict/words` file was 9,330, a much larger list.

To check out the finished code for this project, head on over to the [mattdodge/wordle_bot GitHub repo](https://github.com/mattdodge/wordle_bot). Feel free to fork, report issues, and/or submit PRs there.

The ultimate file ended up being over 100 lines of code, but that's mainly because it includes the enhancements and other features I mentioned here.

I hope you enjoyed this post and this journey of making a fun little bot for a fun little game. Of course, don't use this bot to cheat, that's not cool. But learning and tinkering with Python for real-world projects like this is always fun - happy Wordle-ing!
