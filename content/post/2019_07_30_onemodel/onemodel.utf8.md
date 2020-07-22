---
title: "One model to rule them all"
author: "Nicholas Horton"
date: '2019-07-29'
output:
  html_document:
    df_print: paged
tags:
- R
- confounding
- causal inference
- modeling
- inference
- statistics
- python
categories: R
---



As we near the end of our summer posts, we've started to think more broadly about statistics as well as data science courses.  Today's post considers a broad question relevant for many courses: how can we teach statistical thinking without having to resort to introducing a profusion of tests?  

[Jonas Kristoffer Lindeløv](https://lindeloev.net) proposed an elegant approach using the idea that [common statistical tests are linear models](https://lindeloev.github.io/tests-as-linear/).

<br>

<center>
<figure>
<img alt = 'Common statistical tests are linear models' width='400' src='/post/onemodel/common.png' />
</figure>
</center> <br>

Cheatsheets in [R](https://lindeloev.github.io/tests-as-linear/linear_tests_cheat_sheet.pdf) and [Python](https://eigenfoo.xyz/tests-as-linear/cheatsheets/linear_tests_cheat_sheet.pdf) describe how standard statistical tests (e.g., the one-sample t-test) can be undertaken using the `lm()` and `glm()` functions.

As an example, Jonas demonstrates that an equal variance two-sample t-test can be carried out using either of the following commands:


```r
t.test(mpg ~ am, var.equal = TRUE, data = mtcars)
```

```
## 
## 	Two Sample t-test
## 
## data:  mpg by am
## t = -4.1061, df = 30, p-value = 0.000285
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -10.84837  -3.64151
## sample estimates:
## mean in group 0 mean in group 1 
##        17.14737        24.39231
```

```r
summary(lm(mpg ~ am, data = mtcars))
```

```
## 
## Call:
## lm(formula = mpg ~ am, data = mtcars)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -9.3923 -3.0923 -0.2974  3.2439  9.5077 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   17.147      1.125  15.247 1.13e-15 ***
## am             7.245      1.764   4.106 0.000285 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.902 on 30 degrees of freedom
## Multiple R-squared:  0.3598,	Adjusted R-squared:  0.3385 
## F-statistic: 16.86 on 1 and 30 DF,  p-value: 0.000285
```

The results are fully equivalent.

Similarly, a Kruskal-Wallis test can be undertaken to compare multiple groups using the ranks with nearly equivalent results.  Here the results are comparable for all but the smallest sample sizes.


```r
kruskal.test(mpg ~ cyl, data = mtcars)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  mpg by cyl
## Kruskal-Wallis chi-squared = 25.746, df = 2, p-value = 2.566e-06
```

```r
kruskallm <- lm(rank(mpg) ~ as.factor(cyl), data = mtcars)
anova(kruskallm)
```

```
## Analysis of Variance Table
## 
## Response: rank(mpg)
##                Df  Sum Sq Mean Sq F value   Pr(>F)    
## as.factor(cyl)  2 2262.75 1131.38  71.056 6.64e-12 ***
## Residuals      29  461.75   15.92                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Further examples are given for count models and other non-parametric procedures.

One resource which promotes the idea of teaching linear models first and then looping back to specific tests such as the two-sample t-test is Danny Kaplan's [Statistical Modeling: A Fresh Approach](http://project-mosaic-books.com/?page_id=13).  

Several things about the approach are attractive:

1. Students can focus on learning one or two commands (e.g., `lm()` and `glm()`) rather than a different procedure for each test.  

Jonas notes:

> This needless complexity multiplies when students try to rote learn the parametric assumptions underlying each test separately rather than deducing them from the linear model.

2. Rather than being stuck with a two-sample t-test, students can consider more sophisticated multiple regression models including possible [confounders](https://en.wikipedia.org/wiki/Confounding) of the relationship between the group variable and the outcome.  

The second point here is huge: rather than being paralyzed by their introductory statistics course (which teaches them that if the grouping variable wasn't randomly assigned they can't make any causal conclusions) students can start to disentangle multivariate relationships (see the related post [here](http://teachdatascience.com/gaise/)).  How to teach confounding and related topics merit their own post, but for now, here are some modern references devoted to causal inference:

- [Book of Why](http://bayes.cs.ucla.edu/WHY/)
- [Causal diagrams: draw your assumptions before your conclusions](https://www.edx.org/course/causal-diagrams-draw-your-assumptions-before-your-conclusions)
- [review of Imbens and Rubin](https://imai.fas.harvard.edu/research/files/ImbensRubin.pdf)

## Closing thoughts 

Jonas closes his [post](https://lindeloev.github.io/tests-as-linear/) with a description of teaching materials and a course outline that focuses on the fundamentals of regression then outlines special cases.  His list includes:

1. Build from OLS
2. Extend to multiple regression
3. Add rank-transformations
4. Teach three assumptions: independence of residuals, normality of residuals, and homoscedasticity.
5. Describe how to generate confidence/credible intervals
6. Introduce the idea of $R^2$

Overall it is a promising approach.


###  Learn more

- [Common tests are linear models](https://lindeloev.github.io/tests-as-linear/)
- [There is still only one test](http://allendowney.blogspot.com/2016/06/there-is-still-only-one-test.html)
- [Statistical Modeling: A Fresh Approach](http://project-mosaic-books.com/?page_id=13)


### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.  However, comments on the blog should be constructive, encouraging, and supportive.  We reserve the right to delete comments that violate these guidelines.

