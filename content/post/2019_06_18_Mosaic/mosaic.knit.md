---
title: "Less Volume More Creativity in R"
author: "Nicholas Horton"
date: '2019-06-18'
output:
  html_document:
    df_print: paged
tags:
- R Markdown
- visualization
- modeling
- statistics
categories: R
---



In 2016, [GAISE](https://teachdatascience.com/gaise) enunciated the importance of *multivariate thinking* and *technology* when teaching introductory statistics and data science courses. A big challenge is how to do this using R and RStudio without running into cognitive overload with our students.  

The [mosaic](https://cran.r-project.org/web/packages/mosaic) package was created by Randall Pruim, Danny Kaplan, and Nicholas Horton with the goal of introducing a *Less Volume, More Creativity* approach to introductory statistics that could simplify the use of technology.  In their 2017 RJournal paper entitled ["The mosaic Package: Helping Students to ‘Think with Data’ Using R."](https://journal.r-project.org/archive/2017/RJ-2017-024/index.html) they write:

> The mosaic package provides a simplified and systematic introduction to the core functionality related to descriptive statistics, visualization, modeling, and simulation-based inference required in first and second courses in statistics. This introduction to the package describes some of the guiding principles behind the design of the package and provides illustrative examples of several of the most important functions it implements. These can be combined to help students "think with data" using R in their early course work, starting with simple, yet powerful, declarative commands.

The package builds on the formula object in R, which allows the specification of models in a compact symbolic form.  As an example, consider the `drugrisk` measure of drug related risk behaviors from the `mosaicData::HELPmiss` Health Evaluation and Linkage to Primary Care (HELP) study.


```r
library(mosaic)
favstats(~ drugrisk, data = HELPmiss)
##  min Q1 median Q3 max     mean       sd   n missing
##    0  0      0  1  21 1.869658 4.313479 468       2
```

The `favstats()` function takes a formula as an argument to display the summary statistics for 
the `drugrisk` variable.

Alternative the `df_stats()` function provides a similar display (but returns a dataframe).


```r
df_stats(~ drugrisk, data = HELPmiss)
##   min Q1 median Q3 max     mean       sd   n missing
## 1   0  0      0  1  21 1.869658 4.313479 468       2
```


As [Chris Wild](https://www.stat.auckland.ac.nz/people/cwil119) has noted, comparisons are more interesting than descriptions of a single group.  So we can modify the command to calculate the summary values by homeless status (`housed` or `homeless`).  Note that `favstats` has taken advantage of the formula syntax in R which is given by  `response variable ~ explanatory variable`. 


```r
favstats(drugrisk ~ homeless, data = HELPmiss)
##   homeless min Q1 median   Q3 max     mean       sd   n missing
## 1   housed   0  0      0 0.75  21 1.712000 3.948089 250       1
## 2 homeless   0  0      0 1.00  21 2.050459 4.700449 218       1
```

If only the means are needed, they can be calculated instead.


```r
mean(drugrisk ~ homeless, data = HELPmiss, na.rm = TRUE)
##   housed homeless 
## 1.712000 2.050459
```

The mosaic modeling language approach is attractive because the same syntax used above for the calculation of the mean can be used to fit a linear model (in this case, equivalent to an equal-variance t-test).


```r
msummary(lm(drugrisk ~ homeless, data = HELPmiss))
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        1.7120     0.2729   6.274 8.07e-10 ***
## homelesshomeless   0.3385     0.3998   0.846    0.398    
## 
## Residual standard error: 4.315 on 466 degrees of freedom
##   (2 observations deleted due to missingness)
## Multiple R-squared:  0.001535,	Adjusted R-squared:  -0.0006073 
## F-statistic: 0.7165 on 1 and 466 DF,  p-value: 0.3977
```

Finally, the [`ggformula`](https://cran.r-project.org/web/packages/ggformula/vignettes/ggformula-blog.html) package (automatically loaded with the `mosaic` package) can be used to create ggplot style side by side boxplots, again, using the same `response variable ~ explanatory variable` syntax.  



```r
gf_boxplot(drugrisk ~ homeless, data = HELPmiss)
```

<img src="mosaic_files/figure-html/unnamed-chunk-6-1.png" width="672" />

The key insight is the dramatic skew for the risk score: describing the distribution with means (as done with the `lm()` call) may be misleading.

### A more complicated example

What if we were also interested in a third variable?  The formula interface accommodates 
*multivariate thinking*.


```r
gf_point(pcs ~ drugrisk, data = HELPmiss, color = ~ homeless) %>%
  gf_lm()
```

<img src="mosaic_files/figure-html/unnamed-chunk-7-1.png" width="672" />

```r
lm(pcs ~ drugrisk + homeless, data = HELPmiss)
## 
## Call:
## lm(formula = pcs ~ drugrisk + homeless, data = HELPmiss)
## 
## Coefficients:
##      (Intercept)          drugrisk  homelesshomeless  
##          49.5654           -0.3484           -1.8132
```

Time spent learning the formula interface is worthwhile in the long-run as it is the basis of the `lm()` and `glm()` functions.  When students move into upper level courses, it's not hard for them to move from the scaffolding of `ggformula` to full-blown `ggplot2`.  


### Replicating the above analyses without mosaic

It's certainly possible to undertake the same analysis using base R or using `ggplot2`
for graphics.  But it's more complicated to have beginning students run commands like:


```r
tapply(HELPmiss$drugrisk, HELPmiss$homeless, mean)
##   housed homeless 
##       NA       NA
```

or


```r
tapply(HELPmiss$drugrisk, HELPmiss$homeless, mean, na.rm = TRUE)
##   housed homeless 
## 1.712000 2.050459
```

when they realize that there are missing values for the `drugrisk` variable.

The `dplyr` functions `group_by()` and `summarize()` can be used, but some people find the suite of `tidyverse` functions too challenging for a first course in statistics:


```r
HELPmiss %>%
  group_by(homeless) %>%
  summarize(meanval = mean(drugrisk, na.rm=TRUE))
## `summarise()` ungrouping output (override with `.groups` argument)
## # A tibble: 2 x 2
##   homeless meanval
##   <fct>      <dbl>
## 1 housed      1.71
## 2 homeless    2.05
```

It's also not a huge stretch to get students to use `ggplot2` to generate the boxplots comparing `homeless` and `drugrisk`.  


```r
ggplot(data = HELPmiss, aes(x = homeless, y = drugrisk)) + geom_boxplot() 
```

However, all of the alternative approaches require new idioms and learning outcomes.  They add complexity.  The mosaic authors argue that teaching just *one* approach to modeling can help reduce the [cognitive load](https://www.instructionaldesign.org/theories/cognitive-load) of the course and allow the instructor to focus on getting students to explore data.

### Notes

- Different instructors will make different pedagogical choices depending on the courses and their students.  For instructors hesitant to use modern tools, the `mosaic` package may simplify the process of getting students started while minimizing cognitive load.

- If R was rewritten from scratch, functions such as `mean()` would probably support a formula interface as well as a `data=` option.  Unfortunately, the core R interface is (wisely) not being modified so additional packages such as `mosaic` or the tidyverse are needed to provide a more coherent and consistent experience.

- The `mosaic` package *masks* certain functions from other packages when it loads (for example, the `mosaic::mean()` function has augmented functionality related to formulas and the `data=` option). The `find()` function can be used to identify which function is referenced in the user's environment.


```r
find("mean")
## [1] "package:mosaic" "package:Matrix" "package:base"
```

### Learn more

- https://journal.r-project.org/archive/2017/RJ-2017-024/index.html ("The mosaic Package: Helping Students to ‘Think with Data’ Using R." RJournal paper)
- https://cran.r-project.org/web/packages/mosaic/vignettes/mosaic-resources.html (mosaic teaching resources)
- https://cran.r-project.org/web/packages/mosaic/vignettes/MinimalRgg.pdf (minimal R commands)
- https://cran.r-project.org/web/packages/ggformula/vignettes/ggformula-blog.html (ggformula: another option for teaching graphics in R to beginners)
- https://nhorton.people.amherst.edu/is5/about.html (Intro Stats 5th edition in R with mosaic)

### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.

