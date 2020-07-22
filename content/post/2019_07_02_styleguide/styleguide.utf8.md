---
title: "Style Guides for Coding"
author: "Hunter Glanz"
date: '2019-07-01'
output:
  html_document:
    df_print: paged
tags:
- RStudio
- reproducibility
- collaboration
- style
categories: R
---



# The Importance of Good Coding Style

To begin with another quote from Hadley Wickham: "Good coding style is like using correct punctuation. You can manage without it, but it sure makes things easier to read." In this way, coding style is very much an example of a negative virtue. You are much more likely to be told if you have a bad coding style than you are if you have a good coding style. Coding style is important because while *your* code only has one author, it'll usually have multiple readers.

At UseR! 2018, Jenny Bryan gave a talk on [Code smells and feels](https://www.youtube.com/watch?v=7oyiPBjLAWY&feature=youtu.be) detailing why good coding practice is important.  We recommend everyone listen to her advice.  

As the term "style" usually implies, this can be a very subjective thing. Everyone thinks differently and hence likely codes differently too (i.e. with their own style). So what is meant by "good coding style"? To frame this in a way invariant to the programming language being discussed, you should think of "good coding style" as a set of guidelines to follow which make your code

* easier to read,

* easier to reproduce,

* and easier to contribute to.

As students are learning to code and practicing often throughout coursework, they should be using good coding style. Not only do educators need to lead by example, but also discuss style guides early on and often in students' coding careers. This will have tremendous benefits not only for instructors assessing student-submitted code, but also for fellow students collaborating on projects and such. In fact, the recent [styler package](https://www.tidyverse.org/articles/2017/12/styler-1.0.0/) is a hands-on way to engage students with the use of a consistent coding style.

Besides enforcing their use in assignments, instructors can incorporate coding style into the classroom as the focus of an assignment itself. For example, have students prepare brief presentations on the importance of different sections of a particular style guide. Students could even be asked on an exam what a violation of a particular rule might look like and why it would be bad.

Style guides can vary by language so you should be sure to look around for them whenever using a new language. Because `R` is popular for data science and throughout this blog, the focus of this post will be `R` style guides.

Before we dive into some of the specifics of `R` coding style it should be noted that there can be style guides specific to organizations for internal use. In any case, if you're coding you should have some form of *consistent style* that helps with the above items as much as possible.

# `R` Style Guides

There are multiple `R` style guides out there (some of which are linked at the bottom of this post), but most of them are derived from [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml).

<center>

![](/post/styleguide/googlerstyle.png)

</center>

As you'll notice, the guidelines really are about the cosmetics of your code: naming conventions, line length, spacing, etc. To name a few:

1. The maximum line length is 80 characters

2. Indent your code with two spaces

3. Place spaces around all binary operators (=, +, -, <-, etc.)

Some of you may be wondering how the collection of these "rules" isn't overly cumbersome. The canned response is, "once you get used to following them it's not a big deal." But this often isn't satisfying. When it working with `R` through RStudio, a great number of these things can be built in. Take the first two above:

1. In RStudio click on the Tools menu at the top and then click on "Global Options". Click "Code" and then go to the "Display" tab. 

<center>

![](/post/styleguide/maxlinelength.png)

</center>

In the middle of that tab is an option to specify the "margin column" and check a box for "Show margin". With these things done, you will now see a gray, vertical line in your source panel (where new files show up) at the 80 character mark. So, now you know when to move to the next line of code.

2. In RStudio click on the Tools menu at the top and then click on "Global Options". Click "Code" and then go to the "Editing" tab (default tab).

<center>

![](/post/styleguide/twospacetab.png)

</center>

Here you can specify that when you hit the "Tab" key RStudio will insert some specified number of spaces (2 per the style guide). So, you do not need to worry about hitting the spacebar twice to indent. You can just hit tab.  Interestingly, not everyone agrees that 2 is the right number of spaces.   Roger Peng gives a thoughtful [blog post](https://simplystatistics.org/2018/07/27/why-i-indent-my-code-8-spaces/) on why he indents his code by 8 spaces.

RStudio is a wonderful application and environment for doing `R` work for many reasons, but also because maintaining good coding style can be made so much easier. Many text editors and IDEs (integrated development environments) offer similar features, so be sure to look for them.

At the end of the day, style guides can seem arbitrary but they reinforce practices that improve readability. Good coding style and improved readability are just good computing hygiene that everyone should live by.

## Learn more

* [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml): https://google.github.io/styleguide/Rguide.xml

* Tidyverse Style Guide: https://style.tidyverse.org

* R Coding Style Guide via R-Bloggers: [https://www.r-bloggers.com/%F0%9F%96%8A-r-coding-style-guide/](https://www.r-bloggers.com/%F0%9F%96%8A-r-coding-style-guide/)

* Style tips in *Doing Bayesian Data Analysis* text: [https://rpruim.github.io/Kruschke-Notes/some-useful-bits-of-r.html#style-guide](https://rpruim.github.io/Kruschke-Notes/some-useful-bits-of-r.html#style-guide)

### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.

