---
layout: post
title:  "Champernowne constant"
permalink: /:title/
date:   2022-06-02 18:38:40 +0530
weight: 2
categories: algorithms, maths
---

<h1><u>Champernowne constant</u></h1>

Champernowne constant(in base 10) is : 0.12345678910...
One could see the pattern quite easily, it's a real number with the fractional part as the concatenation of successive integers.
Suppose we want to find the Nth digit of this infinite decimal expansion, what can we do?<sub><a href="#1">1</a></sub>
I'd highly recommend the reader to think about the problem before reading further.

<p>
The simplest way would be to iterate over the integers while keeping track of the number of digits so far.

<details>
<summary>Code</summary>
{% highlight python %}
def find_Nth_digit(N):
  cur = 0, num = 1
  while true: 
      d = number_of_digits(num)
      if cur + d < N:
          cur += d
      else:
          return get_ith_digit(i = N - cur, num)
      num += 1
{% endhighlight %}
</details>
</p>

<p>
This approach isn't too favorable when N becomes huge, say, N > 10^18. 
If we look at the natural numbers we can see a pattern which we can exploit to create a better algorithm.
The first 9 numbers have 1 digit each.
The next 90 numbers have 2 digits each.
The next 900 numbers have 3 digits each.
...
And so on.

We can write it out as a polynomial, F(D).

F(D) = number of digits upto 10 ^ D - 1, i.e integers having <= D digits

F(D) = 9 * 1 + 90 * 2 + ... + 9 * 10 ^ (D - 1) * D

We can binary search on the value of D and then binary search again(if needed) to find the Nth digit amongst the (D + 1)th digit numbers. This reduces the complexity of the algorithm by a lot, and we have something in the form of O(log(N)) time complexity.
</p>

<p id="1">
[1] - The problem is a slight variation of the <a
href="https://projecteuler.net/problem=40">project euler problem.</a>
</p>
