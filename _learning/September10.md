---
title: "September 10, 2024"
collection: learning
type: "journaling"
permalink: /learning/September10
venue: ""
date: 2024-09-10
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.


Reading and applying...
==
Finding Ghosts in Your Data: Anomaly Detection Techniques with Examples in Python
==================

A little bit of a rough morning today as far as making significant progess on the coding from the book. I've been preoccupied with brainstorming ways to incorporate my scrappy Intel feed into the site. This book has introduced FastAPI to me, so that's a possible route I might pursue.

As far as the book goes, even though I wasn't overly "feeling it" this morning I wanted to get some of it done, it's not about whether or not I want to its about what I said I was going to do. 

I read along with the intent of rereading once again when I actually implement the code blocks. The content today introduced combined the three individual tests to check for normality and establish the conditions to trigger each test - less than 5k observations for Shapiro, gte 8 for D'Agostino, and always run the Anderson-Darling test.

It also got into how in certain conditions we can use the Box-Cox method to transform non-normal data into something that looks more normally distibuted. Using the boxcox method from scipy.stats this is easily established, the book highlights that sometimes the boxcox method can "smother" outliers, which is obviously not something that will help detection outliers. To combat this the boxcox method is applied to the middle 80% of the data as it is not expected to have many outliers in this range. To obtain the middle 80% the math.floor function was used.

```
col_length = len(col)
col_sort = sorted(col)
col80 = col_sort[ math.floor(.1 * col_length) + 1 : math.floor(.9 * col_length) ]
```

I wanted to get that snippet, because I can see how how having that one in the toolbox would be effective.



Looking ahead
======

Go back over and implement the coding for this portion of the book. Also, still poking at using the univariate notebook I mentioned a couple days ago to pick up network connections of interest, at work I'm applying it to different features of firewall data outside of work, I've collected a bunch of PCAP files from malicious repos and converted them the Zeek logs, and I'm using a similar approach to see if anything stands out.

