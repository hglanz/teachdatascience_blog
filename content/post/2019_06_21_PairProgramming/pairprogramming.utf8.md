---
title: "Pair Programming for Data Science and Statistics"
author: "Nicholas Horton"
date: '2019-06-20'
output:
  html_document:
    df_print: paged
tags:
- teaching
- workflow
- teamwork
- education
categories: notR
---



[Pair programming](https://en.wikipedia.org/wiki/Pair_programming) is a technique from software development where two programmers work in tandem to code.  One is designated the *driver*, responsible for typing, while the other, often called the *navigator* or *observer* reviews the code and provides a high-level overview of the task.


![Photo credit: [Esti Alvarez](https://www.flickr.com/photos/esti/4638056301)](/post/pairprogramming/pair.jpg)



Pair programming has been thought to lead to better code, more enjoyable coding, and
higher productivity, with some research findings supporting those conclusions (see some of the references at the end of this entry).

A recent WikiHow article on [How to Pair Program](https://www.wikihow.com/Pair-Program) broke pair programming down into the following steps:

1. Start with a reasonably well-defined task before you start
2. Agree on one tiny goal at a time
3. Rely on your partner, support your partner
4. Talk a lot
5. Sync up frequently
6. Take a moment to celebrate as you complete tasks and overcome problems
7. Switch roles often--at least every half hour

(It is only fair to note that some people find pair programming to be unpleasant, particularly if implemented with an unacceptable partner, see https://www.quora.com/What-is-your-opinion-on-Data-Science-pair-programming.)

### Pair programming for data science

How might pair programming work for data science?  

The [NASEM](https://teachdatascience.com/nasem) report noted the importance of teamwork:

> The ability to work well in multidisciplinary teams is a key component of data science education that is highly valued by industry, as teams of individuals with particular skill sets each play a critical role in producing data products.

In our teaching we often have students work in groups.  How can we help scaffold their workflow to help them team up to solve data challenges?  What background information do they need to start to work effectively together?  We don't have the answers but see pair programming as a part of the training in how to be an effective data scientist.


[Sean McClure](https://medium.com/@seanaaron100/does-pair-programming-work-in-data-science-5a7b277b1485) considered pair programming in data science and found it to depend on the pairing:

> The ideal pair is someone who is similar enough in style to maintain a steady pace, but different enough in experience to allow for mutual learning.


How might we create opportunities for students to learn how to effectively pair program?

One idea might be to have the students
work through an example introducing a new R package vignette (e.g., the vignette for [dbplyr](https://cran.r-project.org/web/packages/dbplyr/vignettes/dbplyr.html) ) where one person is reading and explaining and the other is coding.

Another idea would be to alternate responsibility for steps with an analysis.  
Person A codes the data ingestation and consistency checking, while
Person B would annotate it to include exploratory distribution of the data.

Later, Person B would code the next bit (creation of graphical displays) while
Person A annotates it with interpretation.  

Have you taught pair programming?  How have you organized and assessed their work? We'd welcome your reflections in the comments.


### Learn more

- https://collaboration.csc.ncsu.edu/laurie/Papers/XPSardinia.PDF (Costs and benefits of pair programming)
- http://sunnyday.mit.edu/16.355/williams.pdf (Strengthening the case for pair programming) 
- https://www.wikihow.com/Pair-Program (WikiHow: how to pair program)
- https://medium.com/@seanaaron100/does-pair-programming-work-in-data-science-5a7b277b1485 (Pair programming in data science)
- https://raygun.com/blog/how-good-is-pair-programming-really/ (Pair programming: so how good is it, really?)

### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.

