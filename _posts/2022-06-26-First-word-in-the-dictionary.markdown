---
layout: post
title: "First word in the dictionary"
permalink: /:title/
date:   2022-06-26 19:21:50 +0530
---
<h1> <u> First word in the dictionary </u> </h1>

## Problem

Given two strings find the first word occuring in the dictionary made from the given strings.<sub><a href="#1">1</a></sub>
A word is said to be in the dictionary if it can be formed using the following operations:
```
1. Pick one of the first character from either of the string
2. Delete that character from the string 
3. Repeat the above steps until all the characters are taken
```
```
Example: 
String A = "JA"
String B = "RA"
Words = ["JARA", "JRAA", "RAJA", "RJAA"]
```
Amongst the words, ```"JARA"``` is the first word in the dictionary.

Can you think of a way to find the first word in the dictionary given two arbitrary strings?

## Solution

The naive way to find the first word is to generate all possible words, sort the list and take the first word.
But that's boring and time consuming, let's think of a simpler way to find it. 
One trivial(yet important) observation is that, we always have atmost two choices when picking the ```i```<sup>th</sup> letter and it's always better to take the letter which comes first in the alphabetical order. For example, let's say the choice is between ```A``` and ```C```, if we take ```A``` at this position all the subsequent letters which would be taken would come after ```A``` and since all the words with ```A``` would come before ```C``` we took the best possible letter at the ```i```<sup>th</sup> position.
So, whenever we have a choice between two dissimilar letters we always take the one which comes first in the alphabetical order. 

But what about when we've a choice between two ```A```s? 
Well in that case, we've to "cheat" a little bit, we'd go down both the strings and look at the position(let's call it the ```j```<sup>th</sup> position) where we hit the first dissimilar pair of letters and take the ```A``` from the string which has the least of the two values at the ```j```<sup>th</sup> position and if there's no such ```j``` we can pick ```A``` from either of the strings.<sub><a href="#2">2</a></sub>
It's like when we hit a crossroad and we don't know which way to go so we step out of the car walk a few metres and ask one of the villagers which road to take.
The reason this technique works is because when we hit the first choice of dissimilar letters we took the fastest route to that position.
We repeat the above given algorithm as long as there are letters in the strings, and our algorithm terminates when we've no more choices left and at this point we would have successfully generated the first word in the dictionary. :D 

<details>
<summary> Code(in Haskell)</summary>
{% highlight haskell %}

get_first_word [] y = y
get_first_word x [] = x
get_first_word (x:xs) (y:ys) = if x < y 
                     then x : get_first_word xs (y:ys)
                     else if x > y 
                     then y : get_first_word (x:xs) ys 
                     else (if peek_forward xs ys == 0 
                           then x : get_first_word xs (y:ys)
                           else y : get_first_word (x:xs) ys)


peek_forward [] [] = 0
peek_forward x [] = 0
peek_forward [] y = 1
peek_forward (x:xs) (y:ys) = if x < y then 0
                           else if x > y then 1
                           else peek_forward xs ys

-- Example : 
-- get_first_word "JA" "RA"
-- "JARA"
{% endhighlight %}
</details>


<p id="1">
[1] - The problem is a slight variation of the <a
href="https://www.hackerrank.com/challenges/morgan-and-a-string/problem">hackerrank problem.</a>
</p>
<p id="2">
[2] - We can formally prove this using the exchange argument which I'd like to leave as an exercise for the reader.
</p>
