---
title: "More cloud computing: data science is not done on a laptop"
author: "Nicholas Horton"
date: '2019-07-30'
output:
  html_document:
    df_print: paged
tags:
- cloud computing
- education
- Amazon web services
- cluster computing
- grid computing
- workflow
- high performance computing
- authentication
- SQL
- BigQuery
- Azure
- Google cloud platform
- computing
categories: R
---



Previous blog entries have discussed cloud based servers ([RStudio Server](https://teachdatascience.com/cloud) and [JupyterHub](https://teachdatascience.com/jupyter)) and [parallel/grid/cluster](https://teachdatascience.com/parallel) computing.  Today we will expand upon these ideas to discuss at a high level how data science students can leverage cloud based tools to undertake their analyses in a flexible manner.  


Our discussion is motivated by several recent papers and blog posts that describe how complex, real-world data science computation can be structured in ways that would not have been feasible in past years without herculean efforts.  We've already noted the [fantastic example](https://livefreeordichotomize.com/2019/06/04/using_awk_and_r_to_parse_25tb/) that described multiple iterations needed to parse huge amounts of raw DNA sequencing data to undertake analyses for a given set of genetic locations.  In ["Ambitious data science can be painless"](https://hdsr.mitpress.mit.edu/pub/9mdn32tq) Monajemi et al. describe workflows that take advantage of new software stacks to undertake massive cloud-based experiments.  While a few years older, Chamandy et al.'s [Teaching statistics at Google scale](https://www.tandfonline.com/doi/full/10.1080/00031305.2015.1089790) described three examples where modern data challenges were overcome with creative statistical thinking (see companion report on [Estimating uncertainty for massive data streams](https://ai.google/research/pubs/pub43157) ).  Finally, the NSF-funded workshop report on ["Enabling computer and information science and engineering research and education in the cloud"](https://dl.acm.org/citation.cfm?id=3233928) highlights opportunities as university computing migrates to cloud solutions more and more.

We also really enjoyed reading the recent [Three strategies for working with Big Data in R](https://www.r-bloggers.com/three-strategies-for-working-with-big-data-in-r/) blog post.  

Unfortunately, too few undergraduate students work with massive data streams in existing data science programs.  Before entering the workforce, they would benefit from a foundation (or at least some exposure) in cloud computing, even as the specific tools change.  How can we prepare them? 

## Cloud computing at CMU

[Majd Sakr](https://www.cs.cmu.edu/~msakr/) at Carnegie Mellon University has been teaching an innovative course (cross-listed undergraduate and graduate level) on [cloud computing](http://www.cs.cmu.edu/~msakr/15619-s19/) with the goal of skill building and problem solving using Amazon Web Services (AWS), Microsoft Azure and Google Cloud Platform (GCP).  

> Students will utilize MapReduce, interactive programming using Jupyter Notebooks, and data science libraries to clean, prepare and analyze a large data set. Students will orchestrate the deployment of auto-scaled, load-balanced and fault-tolerant applications using virtual machines (VMs), Docker containers and Kubernetes, as well as serverless computing through Functions as a Service. Students will explore and experiment with different distributed cloud-storage abstractions (distributed file systems and databases) and compare their features, capabilities, applicability and consistency models. In addition, students will develop different analytics applications using batch, iterative and stream processing frameworks.

An attractive feature of the course is that many of the assignments must be completed on two or more cloud environments, as a way for students to learn how to take advantage of these flexible and powerful systems.


## Getting started

We've already discussed how to get started with precursors to larger scale [parallel and grid](https://teachdatascience.com/parallel) computing. What are the next steps to explore cloud-based systems?  Each of the main cloud providers have active educational outreach programs.

- [Google Compute Platform](https://edu.google.com/programs/faculty/?modal_active=none) allows faculty to apply to receive $100 in GCP credits and $50 per student. Credits can be used in courses, student clubs, and other faculty-sponsored events.  (To replicate our example later in this blog, you'll want to set up an account and request credits.)

- [Azure for Education](https://azure.microsoft.com/en-us/education/) provides access for educators to open source content for classes and $200 in Azure credits, plus free services.

- [Amazon Web Services Educate](https://aws.amazon.com/education/awseducate/) provides between $75 and $200 in AWS annual credits per educator (depending on membership status) and between $30 and $100 for students.

Our advice is to sign up and start to explore.  The world of cloud computing is quickly changing. By gaining experience through investment in time in learning these tools will help instructors provide guidance to their students in use of these modern compuational tools.

## An example: BigQuery in Google's GCP

Our earlier discussion of [SQL](https://teachdatascience.com/sql) highlighted the importance of databases and data technologies in data science education.  How might one explore larger datasets?  Let's consider an example using GCP (kudos to [Shukry Zablah](https://www.shukryzablah.com) for his assistance).


[BigQuery](https://cloud.google.com/bigquery/) is Google's serverless, highly-scalable, cloud data warehouse.  A [quickstart](https://cloud.google.com/bigquery/docs/quickstarts) document is available which discusses use of the web user interface and the GCP console as well as access through an API interface.  The [bigrquery](https://github.com/r-dbi/bigrquery) package in R makes it easy to work with data stored in Google BigQuery through queries to BigQuery tables.

I began by requesting GCP credits (see above) and used the online interface to create a project that I called "Test Project for Blog".  Then I created an RMarkdown file which loaded needed packages.



```r
library(dplyr)
library(bigrquery)
library(ggplot2)
library(forcats)
library(purrr)
library(readr)
```


```r
projectId <- "bigquery-public-data"  # replace with your own project
billingId <- "test-project-for-blog" # replace with your own billing ID
datasetName <- "samples"
tableName <- "wikipedia"
```

BigQuery includes a number of [public datasets](https://cloud.google.com/bigquery/public-data/).
We are going to analyze the public dataset of the revisions of Wikipedia articles up to April 2010, hosted in GCP BigQuery. The size of the table is 35.69GB. The queries we will run will take only seconds. 


```r
query <- "SELECT  title, COUNT(title) as n
          FROM `bigquery-public-data.samples.wikipedia` 
          GROUP BY title
          ORDER BY n DESC
          LIMIT 500"
```

For safety, always try to make sure that your queries have the `LIMIT` set on your queries. 


```r
mostRevisions_tb <- 
  bigrquery::bq_project_query(x = billingId, 
    query = query) #creates temporary table
```

When the previous `bq_project_query()` function is run within RStudio, a connection is made to Google (GCP) and an authentication window will open up in a local browser.

All the heavy lifting we perform is done on the database end (note that we are billed for it, though the first 1TB of accesses are free). The local machine only receives the data once we try to display it. Right now `mostRevisions_tb` is just a reference to a temporary table online.  The query accessed 7GB of data.

We can get a copy of the data on our local machine once we are confident that it is what we want. 


```r
mostRevisions <- bq_table_download(mostRevisions_tb) 
```




```r
glimpse(mostRevisions)
## Rows: 500
## Columns: 2
## $ title <chr> "Wikipedia:Administrator intervention against vandalism", "Wi...
## $ n     <int> 643271, 419695, 326337, 257893, 226802, 204469, 191679, 18671...
```


```r
clean <- mostRevisions %>% 
  filter(!grepl("Wikipedia|User|Template|Talk", title)) %>%
  mutate(title = fct_reorder(title, n)) %>% #to sort levels
  glimpse()
## Rows: 272
## Columns: 2
## $ title <fct> George W. Bush, List of World Wrestling Entertainment employe...
## $ n     <int> 43652, 30572, 27433, 23245, 21768, 20814, 20546, 20529, 20225...
```

Let's plot the top 10 entries. 


```r
ggplot(clean %>% head(10), aes(x = title, y = n, fill = n)) + 
  geom_bar(stat = "identity") + 
  labs(x = "Article Title",
       y = "Number of Revisions",
       title = "Most Revised Wikipedia Articles (Up to April 2010)") +
  scale_fill_gradient(low = "darkblue", high = "darkred", guide = FALSE) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 20, hjust = 1)) 
```

<img src="cloud2_files/figure-html/unnamed-chunk-9-1.png" width="672" />

We've obviously just scratched the surface here.  There are lots of other examples out there to consider replicating in your classroom (e.g., [returning tweets on a schedule](https://www.mikejohnpage.com/blog/returning-tweets-on-a-schedule-in-r-using-aws-ec2-rds-and-cron/)).  Hopefully you are intrigued enough to request some credits for you and your students and start to explore.  Not sure where to begin?  Check out the [GCP Essentials Videos](https://www.youtube.com/playlist?list=PLIivdWyY5sqKh1gDR0WpP9iIOY00IE0xL) series.

## A shameless plug for CASI

While we're thinking more broadly about modern methods, we wanted to mention Efron and Hastie's [Computer Age Statistical Inference: Algorithms, Evidence and Data Science](https://web.stanford.edu/~hastie/CASI/) textbook, which is available in hard copy as well as a freely downloadable pdf.  At Amherst College we've used it as a supplementary text for our STAT495 (Advanced Data Analysis) capstone course, which leverages prerequisites in theoretical statistics, applied statistics, and computation. While the book could be described as "great ideas that Efron and colleagues have come up with over the last few decades", it provides an overview of key developments in an accessible framework that merits attention. As they describe:

> The twenty-first century has seen a breathtaking expansion of statistical methodology, both in scope and in influence. "Big data," "data science," and "machine learning" have become familiar terms in the news, as statistical methods are brought to bear upon the enormous data sets of modern science and commerce. How did we get here? And where are we going? This book takes us on a journey through the revolution in data analysis following the introduction of electronic computation in the 1950s.

> Beginning with classical inferential theories – Bayesian, frequentist, Fisherian – individual chapters take up a series of influential topics: survival analysis, logistic regression, empirical Bayes, the jackknife and bootstrap, random forests, neural networks, Markov chain Monte Carlo, inference after model selection, and dozens more. The book integrates methodology and algorithms with statistical inference, and ends with speculation on the future direction of statistics and data science.

Some of the approaches are classical and don't require much computation.  Others (e.g., the bootstrap) make modest computational demands.  A number (e.g., deep learning) require grid or cloud computing.

In his [review](https://rviews.rstudio.com/2016/10/28/book-review-computer-age-statistical-inference/), Joseph Rickert noted:

> My take on Computer Age Statistical Inference is that experienced statisticians will find it helpful to have such a compact summary of twentieth-century statistics, even if they occasionally disagree with the book’s emphasis; students beginning the study of statistics will value the book as a guide to statistical inference that may offset the dangerously mind-numbing experience offered by most introductory statistics textbooks; and the rest of us non-experts interested in the details will enjoy hundreds of hours of pleasurable reading.


### Learn more

- [Three strategies for working with Big Data in R](https://www.r-bloggers.com/three-strategies-for-working-with-big-data-in-r/)

- [Google Compute Platform](https://edu.google.com/programs/faculty/?modal_active=none)

- [Azure for Education](https://azure.microsoft.com/en-us/education/) 

- [Amazon Web Services Educate](https://aws.amazon.com/education/awseducate/) 

- [Teaching statistics at Google scale](https://www.tandfonline.com/doi/full/10.1080/00031305.2015.1089790)

- [Enabling computer and information science and engineering research and education in the cloud](https://dl.acm.org/citation.cfm?id=3233928)

- [GCP Essentials Videos](https://www.youtube.com/playlist?list=PLIivdWyY5sqKh1gDR0WpP9iIOY00IE0xL)

- [Query with a credit card: BigQuery sandbox](https://cloud.google.com/blog/products/data-analytics/query-without-a-credit-card-introducing-bigquery-sandbox)

- [Computer Age Statistical Inference: Algorithms, Evidence and Data Science](https://web.stanford.edu/~hastie/CASI/)

### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.  However, comments on the blog should be constructive, encouraging, and supportive.  We reserve the right to delete comments that violate these guidelines.

