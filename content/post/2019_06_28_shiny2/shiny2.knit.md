---
title: "Using Shiny in the Classroom"
author: "Hunter Glanz"
date: '2019-06-28'
output:
  html_document:
    df_print: paged
tags:
- R Shiny
- workflow
- visualization
- dynamic visualization
- rstudio
- reactive
categories: R
---




## Shiny Recap

<center>

![](/post/shiny2/shinyhex.jpg)

</center>

Yesterday we introduced [R Shiny](https://shiny.rstudio.com/) and discussed how it allows you to build interactive web applications straight from R. We saw a few examples highlighting the wondrous interactivity of exploratory data analysis, data visualization, and data models that it enables. If you didn't catch yesterday's post, check it out at [https://teachdatascience.com/shiny1/](https://teachdatascience.com/shiny1/) and be sure to go play around with some Shiny apps at RStudio's gallery at [https://shiny.rstudio.com/gallery/](https://shiny.rstudio.com/gallery/). 

`shiny` is powerful and should be used in as many ways as possible. Today's post will detail a few of the major ways it can be integrated into the classroom.  

## Shiny Apps for Student Users

With `shiny`, instructors with knowledge of R have the ability to create their own "applets" or demos to connect students with myriad datasets and data science topics. The pedagogical importance of tools like the Rossman/Chance [applets](http://www.rossmanchance.com/applets/) and [statkey](http://www.lock5stat.com/StatKey/) cannot be overstated, but curriculum and activities no longer need to conform to existing applets and web-based tools. As educators and R users, most classroom activities that involve the computer in some way can be converted into an interactive `shiny` app.

`shiny` activities are not limited to introductory topics. To a very large extent, if it can be done in R then it can be incorporated into a `shiny` app in some fashion. Although `shiny` apps use R on the back end, remember that they can be deployed to a website to be viewed and used within any browser with internet access.  If your home institution/organization does not have access to a Shiny server, [shinyapps.io](https://www.shinyapps.io/) by RStudio will host your `shiny` app for free. If the resources accompanying the free usage tier are not sufficient for your use, remember that many of RStudio's pro products are free for educators.

Especially cool is the unprecedented ability to connect students' novel methods and research via `shiny` In a 2018 TAS paper, Lee Fawcett [outlines](https://amstat.tandfonline.com/doi/full/10.1080/10691898.2018.1436999#.XQ7XIetKiHt) a collaborative and interactive environment for students to learn about and apply new methods via `shiny` apps.

Beyond statistical concepts like data visualization and analysis, `shiny` apps are a great way to teach R skills themselves. The [`learnr`](https://blog.rstudio.com/2017/07/11/introducing-learnr/) package facilitates the learning process explicitly by making the construction of custom R tutorials easier than ever. Each `learnr` tutorial is a `shiny` interactive document. Keep an eye out for a future blog post all about the `learnr` package!

## Shiny Apps By Students

Give students a `shiny` app and they will exhaust what the app can do. Give students the ability and opportunity to *build* a `shiny` app and the sky's the limit!

Equipping students with statistical computing and programming skills empowers them to own and drive their work with data. Add to that the creation of a `shiny` app and there's nothing they'll feel they can't do with data. We've had great success asking teams of students (~3) to create a `shiny` app as a final project. Not only does the assignment give students fantastic practice with R, but the result is a **product** that they can literally show off to peers and prospective advisers, employers, etc.

### Student NASA Shiny App

The NASA app was created by a team of three students as a final project.  The goal was exploratory data analysis with a dataset of their choice. The app can be found [here](https://mschroth.shinyapps.io/lisinkershinyapp/). 

<center>

![](/post/shiny2/nasaapp.png)

</center>

Based on the apps in the RStudio gallery, you know that the students who created the NASA app had the enthusiasm to customize the theme of the page.  The students not only found an interesting dataset and exciting ways to explore it, but they even added a gif on the last tab. It's clear they had fun creating it!

### Student Classification Shiny App

The Classification app was also created by a team of three students as a final project, and they chose to build a tool for exploring different classification methods. The app can be found [here](https://mschroth.shinyapps.io/classificationapp/)

<center>

![](/post/shiny2/classifyapp.png)

</center>

The app is fun for its simultaneous simplicity and complexity. The classification methods included are somewhat advanced and nice to visualize.  However, the coup de grace is the choice of datasets which provides stunning clarity to how the different methods handle different data situations.

## Beyond the Classroom

Whether it's exposure to `shiny` as a learning tool or the process of building an app from scratch, there is tremendous pedagogical value from using in the classroom.  However, beyond the classroom, fluency with a product like `shiny` sets-up students with valuable skills for almost any employer. Interactive visualizations and dashboards are all but ubiquitous in industry for both monitoring and gaining insight from a company's data.

## Learn more

* `shiny` Home: https://shiny.rstudio.com/

* RStudio `shiny` Gallery (there are others): https://shiny.rstudio.com/gallery/


### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.

