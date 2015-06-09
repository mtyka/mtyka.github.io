---
layout: post
title:  "Interesting combinatorics problem"
date:   2013-05-08 00:03:44
categories: math 
---

I just encountered this interesting combinatorics problem that
seemed easy but actually stumped me for a while.
My combinatorics and statistics has definitely rusted a bit over the years .. ;-)


- Q1)

If you pick N random numbers, what is the probability that they are
sorted in ascending order ? (no dups possible)

```
1/N! = 1/(1*2*3*4*..*N)
```

- Q2)
How would you check that the list is sorted ?

Easy:

{% highlight c %}
issorted = true;
for( i=0; i < (list.size()-1); i++){
 if( list[i+1] < list[i] ){
  issorted = false;
  break;
 }
}
{% endhighlight %}

- Q3)

How many checks will you do in a) best case b) worst case c) avaerage

a) 1
b) N-1
c) aehhhhh… hmmm.

On average, how many checks do you have to do to establish a list being sorted or not ?

This took me a while to work through, i found it deceptive – but
here’s a simple proof i finally came up with.

The expectation value of X, E(X) is the sum of all the possible
Xes times their respective probabilities, right:

```
E(X) = SUM[ X * pX ] for X=1,2,3,...N
```

In this case X is the number of tests needed to be performed, and
take values 1,2,3,…,N where N is the size of the list.
pX, the probability that any given list will require *exactly* X tests.
Now, consider a given list of numbers, [a,b,c,d,e,f,..]
For us to end up needing exactly, say, 3 tests the following 2
things must be true:
a,b and c must be in order while d will NOT be in order. the first two tests pass
while the third fails.

1) the probability that a,b and c are in order is 1/3! (one over 3 factorial)
2) the probability that a,b,c and d are in order is 1/4!

thus the probability that 1 is true but 2 is not true is p3 = 1/3! – 1/4!
or more generally the probability that we will require X tests is

```
pX = 1/X! - 1/(X+1)!
```

So, plugging that into the expectation value formula we get


```
E(X) = SUM[ X * (1/X! - 1/(X+1)! )]
     = SUM[ X/X! ] - SUM [ X/(X+1)! ]
```

now lets write that out so we can collapse some terms:


```
E(X) = 1/1! + 2/2! + 3/3! + 4/4! + ...
            - 1/2! - 2/3! - 3/4! - ...
```

Summing the terms that are above each other we get:


```
E(X) = 1/1! + 1/2! + 1/3! + 1/4! + ... = e - 1 = 1.712.. 
```


THe reason this is actually = e – 1 is that e itself is defined as a
series that looks just like that with an extra 1 at the beginning


```
e = 1 + 1/1! + 1/2! + 1/3! + 1/4! + ....
```

see http://en.wikipedia.org/wiki/E_(mathematical_constant)
P.S. A much simpler way to show this occurred to me later: (it circumvents
the need to calculate the probabilty
that a list required *exactly* X tests, only that it requires *at least X* tests.

Given the set of possible lists:
- Each will require the first test
- 1/2! of them will have the first two numbers in order, and thus require a second test
- 1/3! of them will have the first 3 numbers in order, and thus require a third test
- etc.

Thus the average will be `1 + 1/2! + 1/3! + 1/4! + … = e – 1` 


