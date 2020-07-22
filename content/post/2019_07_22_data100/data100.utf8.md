---
title: "Data100: Principles and Techniques of Data Science" 
author: "Jo Hardin"
date: '2019-07-21'
output:
  html_document:
    df_print: paged
tags:
- python
- berkeley
- jupyterhub
- education
categories: R
---



Last week's entries focused on Python included a description of the innovative and popular [data8](https://teachdatascience.com/data8/), today we describe the follow-up course, data100, http://www.ds100.org/ (Principles and Techniques of Data Science) offered by the University of California/Berkeley [Division of Data Sciences](https://data.berkeley.edu).  

<br>

<center>
<figure>
<img alt = 'data100' width='600' src='/post/data100/data100class.png' />
</figure>
</center> <br>

## Course Goals 

The goals of data100 are listed on the [data100 website](http://www.ds100.org/) and reproduced here.  The goals are lofty indeed, but they also address an incredibly important shortcoming in many undergraduate curricula -- a student who is successful in data100 will hit the ground running *doing* data science after graduation.

> * **Prepare** students for advanced Berkeley courses in data-management, machine learning, and statistics, by providing the necessary foundation and context

> * **Enable** students to start careers as data scientists by providing experience working with real-world data, tools, and techniques

> * **Empower** students to apply computational and inferential thinking to address real-world problems

## Prerequisites

The content in data100 is taught at a high level, and students are expected to enroll with a strong background.  Indeed, depending on a student's high school background, they may need to take up to 5 semester long courses before taking data100.   The official prerequisites include:

1. Foundations of Data Science.  Students should know the material covered in [data8](http://Data8.org), see our [data8 blog](https://teachdatascience.com/data8/) entry.

2. Computing.  Background in computing (e.g., for loops, lambdas, debugging, and complexity) in order to have a stronger focus on the related data science concepts.

3. Math. Calculus and Linear Algebra including: linear operators, eigenvectors, derivatives, and integrals


##  Content

Building on a strong prerequisite structure, the topics in data100 include:

* data wrangling (mostly with pandas, see https://teachdatascience.com/pandas/)
* visualization (see https://teachdatascience.com/dataviz/)
* EDA & working with text
* SQL (see https://teachdatascience.com/sql/)
* Dimension reduction
* PCA
* Foundations of statistical inference (see https://teachdatascience.com/infer/)
* Linear regression
* Gradient descent
* Bias-Variance trade-off
* Cross-validation
* Logistic regression
* Classification
* Prediction
* Big Data and Spark (see https://teachdatascience.com/parallel/)

## Resources

For instructors who teach any or all of the content listed above, one of the biggest contributions to the larger data science education community is the freely available data100 textbook, [Principles and Techniques of Data Science](https://www.textbook.ds100.org/) by [Sam Lau](https://www.samlau.me/), [Joey Gonzalez](https://people.eecs.berkeley.edu/~jegonzal/), and [Deb Nolan](https://www.stat.berkeley.edu/~nolan/).   There are great examples and accompanying code.  Additionally, the authors provide both source code and *instructions* for setting up the textbook for local deployment through the [DS100 textbook GitHub repo](https://github.com/DS-100/textbook/blob/master/SETUP.md).

<br>

<center>
<figure>
<img alt = 'book1' width='600' src='/post/data100/data100book1.png' />
</figure>
</center> <br>

As with the course, the text assumes a certain amount of prerequisite knowledge, but the authors also provide a roadmap for working through the topics as well as guidance on the notation that they use.  

<center>
<figure>
<img alt = 'book2' width='600' src='/post/data100/data100book2.png' />
</figure>
</center> <br>

###  Bias-Variance Tradeoff

You may have thought about the bias-variance tradeoff, and maybe you've even taught the concept in your class.  The basic idea is that sometimes a model overfits the training data (i.e., a model is too complicated) and has high variance from training sample to training sample.  But sometimes a model is too simple and has high bias in the sense that it systematically under- or over-estimates the test data.  Certainly, at any level of data science education, students can understand the big idea of the tradeoff between models that are too simple and those that are too complicated.

However, actually delving into the mathematical details of the bias-variance tradeoff is usually left to higher level data science or computational statistics courses.  Because of the strong prerequisite structure of data100, the bias-variance tradeoff is introduced with its mathematical derivation in terms of the expected loss (a risk function).  The data100 textbook is a great resource for data science topics to be taught with mathematical, statistics, or computational details.

<center>
<figure>
<img alt = 'bvtrade' width='600' src='/post/data100/risk.png' />
<figcaption>Mathematical derivation of the bias-variance tradeoff given in the data100 textbook at https://www.textbook.ds100.org/ch/15/bias_modeling.html </figcaption> </figure>
</center> <br>

Does the data100 course inspire thoughts on the role of computing in the statistics and data science curriculum? Consider submitting a paper to the upcoming [special issue of JSE](https://nhorton.people.amherst.edu/JSEFlier2.pdf) devoted to the topic.

### Learn more

- http://www.ds100.org/
- [data100](https://data.berkeley.edu/education/courses/data-100)
- video of [Data 100 Overview](https://youtu.be/HITIm3KoU2U)
- data100 accompanying textbook, [Principles and Techniques of Data Science](https://www.textbook.ds100.org/)

- https://data.berkeley.edu/academics/resources/data-science-education-workshops/2019-national-workshop-data-science-education (Data Science Education 2019 workshop)
- https://data.berkeley.edu/education/connectors (connector courses)
- http://nhorton.people.amherst.edu/JSEFlier2.pdf (Special issue call for papers: computing in the statistics and data science curriculum)

### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.  However, comments on the blog should be constructive, encouraging, and supportive.  We reserve the right to delete comments that violate these guidelines.

