---
    layout: post
    title: "Horse Race"
    permalink: /:title/
    date:   2023-03-18 01:14:27 +0530
---
<h1> Horse Race </h1>
<i>This blog is part of the <a href="{% post_url 2022-06-25-problem-of-the-month%}">POTM series.</a></i>

Shelly has a beautiful, muscular horse named as "The Big Dawg", and she is invited to the annual equestrianism competition. Being a smart person, she does not want to waste time going for competitions where she does not have any chance of winning, unfortunately she is very busy at the moment completing her farm project, and she turned for your help. Can you help Shelly out?
![Big Dawg](/assets/horse.jpeg){: width="600"}

The problem goes like this...


<p>
There are $N$ horses taking part in the competition. Each horse has a certain strength, denoted by a positive integer. A horses' strength is a unique integer between $1$ and $N$, and each of the horse is assigned a unique track number between $1$ and $N$. 

The competition is divided into $\log_2 N$ rounds (It's guaranteed that $\log_2 N$ is a positive integer). 
The rounds are a bit atypical, in the i<sup>th</sup> round, the horse at track number $1$ and $2$ would compete against each other, and $3$ vs $4$, $5$ vs $6$, ... and so on.
If $K$ horses qualified for the i<sup>th</sup> round, then there will be $K / 2$ winners and $K / 2$ losers, and the winners move on to the (i+1)<sup>th</sup> round. The last round will be played against 2 horses, and one of them would be crowned the champion. 

The winning criteria for each round is one of the two possibilities, either the stronger horse(horse which has the higher strength) defeats the weaker horse, or vice-versa.
Fortunately for the equestrians, the winning conditions of each round is given before the tournament starts, and is in the form of a binary string, i.e. strings which have either '1' or '0'(like "101"). If the i<sup>th</sup> letter is '1', then the stronger horse wins the race, and if the i<sup>th</sup> letter is '0' the weaker horse takes the win.

Given this information, can you figure out if Shelly could likely be crowned as the champion?
</p>

<br>
<br>

Before diving into the solution, I'd like the reader to take some time off to savor the problem and try cracking it.
<p>
<!-- TODO: hints? -->
<h3> Solution </h3>
We can make couple of basic observations right off the bat: <br>
<!-- Since Shelly needs to win the tournment We can make some observations: <br> -->
1. Shelly wants to win the tournament, so her horse has to win <b>every</b> round. <br>
2. Between two horses, only <b>relative</b> strength matters.
</p>

Let's characterize every other horse' strength relative to Big Dawg's(Shelly's horse) strength. Let "B" be for horses which have "bigger" strength and "S" for horses with "smaller" strength, and let Big Dawg's strength be as "x".

What more can we do? Hmm, we want Shelly to win the tournament, so she must win the last round. Let's backtrack our way from the end of the tournament to the start and see what goes on.

<br>
Given:-
```
N = 8 (number of horses/equestrians)
winning_condition = "001"
```
<br>

There will be 3 rounds in this tournament ...

--------------------------------
For the 3rd round, the winning condition is '1' i.e., the horse with the "bigger" strength wins.
So for Shelly to win the 3rd round, her horse must fight with a horse with smaller strength.

3rd round: 
<br>
```
      x   S   - 3rd round (x vs S)
       \ /
        x
```

<br>

--------------------------------
Let's see what happens for the 2nd round, winning condition is '0', so horse with smaller strength wins.

2nd round:
<br>

```
   x    B  S    O - 2nd round (x vs B, S vs O)
    \  /    \  / 
      x      S   - 3rd round
       \    /
         x
```
<br>
Shelly must fight with a "bigger" horse to win the round. 
Note that the "S" horse in the 3rd round, must win the 2nd round to reach the 3rd round. The "S" horse has an advantage though, since it's smaller it can fight against "B" or "S"(another horse with smaller relative strength) and in both cases the smaller horse wins. It does not matter which horse amongst the smaller ones win, we just want one of the small ones to win. We denote such a horse with "O" to signify either a bigger or smaller horse.

--------------------------------
For the 1st round, the winning condition is '0', so the smaller horse wins.
1st round:
<br>
```
 x    B  B    B  S    O  O    O - 1st round (x vs B, B vs B, S vs O, O vs O)
  \  /    \  /    \  /    \  /
    x       B       S       O - 2nd round
    \    /           \    / 
       x               S   - 3rd round
       \              /
         \          /
           \      /
              x
```
<br>
Notice that for the "O" in the 2nd round, in the 1st round the opponent for "O" is also "O" as it does not matter who wins the round and similary for the "S". But for "B", it has to fight against any other bigger horse in the 1st round to guarantee its place in the 2nd round.

<p>
There are a few observations we can make from the simulation we just did:

1. In the first round we end up with 3 "B"s and 1 "S" ("O"s don't matter), which implies that for Shelly to win the tournament there must be atleast 3 equesterians who have horses with bigger strength than Shelly's horse and atleast 1 equestrian with a weaker horse. <br>

2. When the winning condition is '0' for the $i$<sup>th</sup> round, we can see that in the round $(i - 1)$: <br>
    - For x to win, "x vs B" always has to happen
    - For B to win, "B vs B" always has to happen 
    - For S to win, "S vs O" 
       (S will always win so opponent does not matter)

3. Similary for the winning condition '1', following happens:
    - For x to win, "x vs S" always has to happen
    - For B to win, "B vs O" 
       (B will always win so opponent does not matter)
    - For S to win, "S vs S" always has to happen 
</p>
Looks like we get a new "B" in the $(i - 1)$<sup>th</sup> whenever the winning condition is '0'.
We can write it formally as...

$$
    F(x) = F(x - 1) + F(x - 1) + 1 
$$
$$
    F(x) = 2 \cdot F(x - 1) + 1
$$

Where $F(x)$ is the number of Bs at a particular round having winning condition as '0', and $F(x - 1)$ is the number of Bs at the previous round which had the same winning condition. 
Since, each B in the previous round would produce a new B, we get $2\cdot F(x - 1)$ and the 1 is for the B which is produced by x.

It follows that, for Shelly to win the tournament: the total number of Bs required is atleast $2^L - 1$, where $L$ stands for the number of rounds having winning condition as "0".
And similarly for S, the total number of Ss required is $2^R - 1$, where $R$ stands for the number of rounds having winning condition as "1".

Hence, Shelly can win the tournament if there are atleast $2^L - 1$ stronger horses and $2^R - 1$ weaker horses.

So for the given winning_condition "001", Shelly must have atleast $2^2 - 1 = 3$ stronger horses and $2^1 - 1 = 1$ weaker horse. 
From which it follows that, if Big Dawg's strength is one of {2, 3, 4, 5} then Shelly can possibly win the race!

<br>
Bonus problem: What is the expected probability of Shelly winning the tournament?

    
