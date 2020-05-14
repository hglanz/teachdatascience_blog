---
title: "Creating R data packages for teaching"
author: "Kelly McConville"
date: '2019-07-22'
output:
  html_document:
    df_print: paged
tags:
- R Markdown
- packages
- roxygen2
- data ingestation
- leaflet
- mapping
- visualization
- cherry tree
- Portland
categories: R
---




Today's guest entry by [Kelly McConville](https://mcconville.rbind.io) (Reed College) describes the creation of data packages in R by instructors and students.  

## Sharing data with students

The beginning of the data analysis cycle involves **ingesting data**. For novice students in the first week or two of an introductory course, this can be a tricky step.  If I am new to R and I can't even load the data, then my first impressions of R are not going to be too great. Therefore, instructors may work to make data importation as seamless as possible for students, especially early on in a course.  That means we want to AVOID asking our students to load data this way:


```r
dat <- read.csv("scary/long/path/to/file.csv")
```

where everyone has a different file path and the probability of a small typing error is high.  A simple solution is to save the data on a website so that everyone uses the same file path:


```r
dat <- read.csv("https://mcconville.rbind.io/workshop/trees_OHSU.csv")
```

If you go that route, it is also very important to share a data dictionary with the students.  

A better solution might be to create an R *data package*.  Under this set-up, the documentation and data are kept together and data are loaded with `library(package_name)`. The following packages provide useful scaffolding for package creation:

- [`devtools`](https://www.rstudio.com/products/rpackages/devtools/): supports the development and dissemination of the package
- [`usethis`](https://usethis.r-lib.org/index.html): automates steps of package creation, such as constructing the data file
- [`roxygen2`](https://cran.r-project.org/web/packages/roxygen2/index.html): simplifies writing documentation


In this post, I will give you a quick introduction to creating R data packages.   I highly recommend you check out the resources at the bottom of the blog for more thorough instructions.

## Nuts and Bolts

Nick and I  recently taught a workshop and I wanted to use [data collected as part of the Urban Forestry Tree Inventory Project](https://www.portlandoregon.gov/parks/article/433143).  Urban Forestry collected data on the trees in over 200 Portland, OR parks. While these data are fairly clean, they still need a bit of data wrangling that I didn't want the workshop participants to have to do at that point in time.  Therefore I decided to create a data package called `pdxTrees`. Let's walk through the steps I took to create the package.   

**Step 1**: Create a version control R package in RStudio by checking the box "Create a git repository."

<br>

<center>
<figure>
<img alt = 'r_package' width='400' src='/post/datapackage/r_package.png' />
</figure>
</center> <br>

**Step 2**: Use Jenny Bryan's [https://happygitwithr.com/](https://happygitwithr.com/) to create a public [GitHub](https://teachdatascience.com/github/) [repository](https://github.com/mcconvil/pdxTrees) and to link it to the new package R Project.

**Step 3**: Add the raw data and transform it into the clean form I want to share in the package.

- I ran the function `usethis::use_data_raw()` to create a `data-raw` folder in the R project and the file `DATASET.R`.  
- I put the raw data files in the `data-raw` folder.
- I used the file `DATASET.R` to load and wrangle the raw data.
- At the bottom of the wrangling file, I included the following code to create a clean `.Rda` file:


```r
# Swap pdxTrees for the name of your data
usethis::use_data(pdxTrees, overwrite = TRUE)
```

- I ran the code in `DATASET.R`, which created a new folder called `data` that contains the cleaned data file.



**Step 4**: Create the documentation/help file for the data.

- Within the R folder, I created a script file, `pdxTrees.R`.
- I then added [`roxygen`](https://cran.r-project.org/web/packages/roxygen2/index.html) comments to the file.  See the [Object Documentation Chapter](http://r-pkgs.had.co.nz/man.html) of [R packages](http://r-pkgs.had.co.nz/).  
- I ran `devtools::document()`, which created the `man` folder and an `Rd` help file.  
- I ran `?pdxTrees` in the console to view the help file.  

<br>

<center>
<figure>
<img alt = 'roxygen_help_file' width='400' src='/post/datapackage/roxygen_help_file.png' />
</figure>
</center> <br>


- Notice that I included the dataset name as a string in the bottom of the R script.  If you forget this, the help file for that dataset won't be created!


**Step 5**: Edit the [DESCRIPTION file](http://r-pkgs.had.co.nz/description.html) and consider creating a Readme file.  The code `usethis::use_readme_md(open = interactive())` generates a skeleton Readme file.  

**Step 6**: Delete any extraneous files, such as `hello.R` and run checks with `devtools::check(document = FALSE)` to make sure the package compiles without errors or warnings.

**Step 7**: Push the changes to the public GitHub repository and now `pdxTrees` is ready to share with the world!

## Load the data

If you are teaching with an [RStudio Server](https://teachdatascience.com/tags/rstudio-server/), packages can be installed for all users.  If your students are using local installations of R or RStudio, then they will need to do the following commands once to install the data package `pdxTrees` (one time only).


```r
install.packages("devtools")
devtools::install_github("mcconvil/pdxTrees")
```

Once the package is installed, students load the library every time they want to access it.  


```r
library(pdxTrees)
dplyr::glimpse(pdxTrees)
## Rows: 15,856
## Columns: 34
## $ user_id                    <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1...
## $ inventory_date             <chr> "5/9/17", "5/9/17", "5/9/17", "5/9/17", ...
## $ species                    <chr> "PSME", "PSME", "CRLA", "QURU", "PSME", ...
## $ common_name                <chr> "Douglas-fir", "Douglas-fir", "Lavalle h...
## $ dbh                        <dbl> 37.4, 32.5, 9.7, 10.3, 33.2, 32.1, 28.4,...
## $ condition                  <chr> "Fair", "Fair", "Fair", "Poor", "Fair", ...
## $ tree_height                <dbl> 105, 94, 23, 28, 102, 95, 103, 105, 97, ...
## $ crown_width_ns             <dbl> 44, 49, 28, 38, 43, 35, 40, 45, 56, 35, ...
## $ crown_width_ew             <dbl> 57, 45, 27, 31, 44, 39, 40, 29, 45, 33, ...
## $ crown_base_height          <dbl> 4, 4, 3, 5, 4, 12, 6, 13, 5, 17, 6, 6, 4...
## $ collected_by               <chr> "staff", "staff", "staff", "staff", "sta...
## $ park                       <chr> "Gammans Park", "Gammans Park", "Gammans...
## $ scientific_name            <chr> "Pseudotsuga menziesii", "Pseudotsuga me...
## $ family                     <chr> "Pinaceae", "Pinaceae", "Rosaceae", "Fag...
## $ genus                      <chr> "Pseudotsuga", "Pseudotsuga", "Crataegus...
## $ functional_type            <chr> "CE", "CE", "BD", "BD", "CE", "CE", "CE"...
## $ mature_size                <chr> "L", "L", "S", "L", "L", "L", "L", "L", ...
## $ native                     <chr> "Yes", "Yes", "No", "No", "Yes", "Yes", ...
## $ edible                     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ nuisance                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ structural_value           <dbl> 10101.06, 7900.55, 1111.03, 525.26, 8224...
## $ carbon_storage_lb          <dbl> 2578.6, 1968.6, 248.2, 320.1, 2026.7, 18...
## $ carbon_storage_value       <dbl> 167.26, 127.69, 16.10, 20.76, 131.46, 12...
## $ carbon_sequestration_lb    <dbl> 29.4, 25.0, 11.8, 4.8, 25.6, 24.7, 21.4,...
## $ carbon_sequestration_value <dbl> 1.91, 1.62, 0.77, 0.31, 1.66, 1.60, 1.39...
## $ stormwater_ft              <dbl> 136.9, 121.1, 32.3, 34.9, 105.2, 77.7, 9...
## $ stormwater_value           <dbl> 9.15, 8.09, 2.16, 2.33, 7.03, 5.19, 6.07...
## $ pollution_removal_value    <dbl> 24.44, 21.62, 5.76, 6.22, 18.78, 13.87, ...
## $ pollution_removal_oz       <dbl> 44.8, 39.7, 10.6, 11.4, 34.5, 25.4, 29.7...
## $ total_annual_benefits      <dbl> 35.50, 31.34, 8.69, 8.86, 27.47, 20.66, ...
## $ origin                     <chr> "North America - from British Columbia s...
## $ species_factoid            <chr> "Bracts on cones look like a mouse's fee...
## $ longitude                  <dbl> -122.6936, -122.6938, -122.6942, -122.69...
## $ latitude                   <dbl> 45.57491, 45.57489, 45.57493, 45.57490, ...
```



* Encourage students to check out the help file to learn about the different variables in the data set.


```r
?pdxTrees
```



## Playing with `pdxTrees`

Here's an example of a [leaflet](https://teachdatascience.com/leaflet) visualization of the trees.  In this example, I ask my students to create a map of the [Japanese flowering cherry trees](https://en.wikipedia.org/wiki/Prunus_serrulata) and to label the park that contains the tree.  If you zoom in along the waterfront, you will notice that the Gov Tom McCall Waterfront Park is a great place to experience the cherry blossoms!


```r
# Load libraries
library(tidyverse)
library(leaflet)
library(pdxTrees)

# Find the Japanese flowering cherry tree
cherry_blossoms <- pdxTrees %>%
  filter(common_name == "Japanese flowering cherry")

# Create a map of the trees and label the park
m <- leaflet(cherry_blossoms) %>% 
  addTiles() %>% 
  addMarkers(~ longitude, ~ latitude, popup = ~ park) 
m
```

<!--html_preserve--><div id="htmlwidget-59740d898863724b7ede" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-59740d898863724b7ede">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addMarkers","args":[[45.48515536,45.4852421,45.4859786,45.55144196,45.55172914,45.55157543,45.55150591,45.47360554,45.47357267,45.47360441,45.47357136,45.58787896,45.48584822,45.51920541,45.51915671,45.51918263,45.5927627,45.59270112,45.59273288,45.59271447,45.59275362,45.59277587,45.59280065,45.5163242,45.51628187,45.52663243,45.52666915,45.52671312,45.52671287,45.58997564,45.59002079,45.53122241,45.53119626,45.53026402,45.52726797,45.52727435,45.52655122,45.52760578,45.52718833,45.52723133,45.53023241,45.53026294,45.53026587,45.5302293,45.53023725,45.53024697,45.53027585,45.53020791,45.52726464,45.5302653,45.5302862,45.53024678,45.52711521,45.53023281,45.53027513,45.53025247,45.56882623,45.56917072,45.56901001,45.5687856,45.56875779,45.56941078,45.56971054,45.56947339,45.56935854,45.56764304,45.47846861,45.57773193,45.57767263,45.55147084,45.58738717,45.58767105,45.49118435,45.53267465,45.53342264,45.5334611,45.47731244,45.47721315,45.49629085,45.49632261,45.50986241,45.5204397,45.52406338,45.52420037,45.5241474,45.52340177,45.52416062,45.52422068,45.52330015,45.5241094,45.52425315,45.52332106,45.52348453,45.52345387,45.52427136,45.52433876,45.52408855,45.52354066,45.52342185,45.52335538,45.52401847,45.57837321,45.56832415,45.52046645,45.50154736,45.51940315,45.51940872,45.51942148,45.5204338,45.52184331,45.52364294,45.52520174,45.5253251,45.52544694,45.52364835,45.52656099,45.52637661,45.52593779,45.52461994,45.52449628,45.52635013,45.52627477,45.52521213,45.52539286,45.52606005,45.52551762,45.52570344,45.52493859,45.52575541,45.5237022,45.5238213,45.52387901,45.52371175,45.52506411,45.5256267,45.52499865,45.5256873,45.52574108,45.52642139,45.52636276,45.52468383,45.52632745,45.52604127,45.52634337,45.52629559,45.52627274,45.52627115,45.52629753,45.52449937,45.52462695,45.52617005,45.52611635,45.52467475,45.52558144,45.52376582,45.52525404,45.52381348,45.52375873,45.525379,45.5255714,45.52658247,45.52654943,45.52582228,45.52479976,45.52598953,45.52455605,45.52632849,45.52436953,45.52457048,45.52526601,45.52622289,45.52533506,45.52481368,45.5259411,45.52550787,45.52664443,45.52493888,45.52486984,45.52640085,45.52586816,45.52475442,45.5261057,45.52616199,45.52632711,45.52443857,45.52443651,45.52545616,45.52475441,45.52600202,45.52588402,45.52564511,45.52488251,45.52582952,45.5250036,45.52507466,45.52589638,45.54079586,45.49742116,45.49741712,45.48662355,45.49742204,45.50448385,45.50462712,45.50496817,45.50459337,45.50476037,45.50504198,45.50485192,45.50505321,45.50493435,45.50500385,45.54883963,45.54888171,45.55918601,45.4775523,45.47755854,45.49479955,45.49477738,45.49482299],[-122.5710536,-122.5711347,-122.571985,-122.6721049,-122.6714354,-122.6721161,-122.6721233,-122.6240687,-122.62397,-122.6236574,-122.6233552,-122.7618313,-122.5714361,-122.5445972,-122.544855,-122.5446987,-122.6561664,-122.6563726,-122.6562906,-122.6560222,-122.6559865,-122.6560616,-122.6560877,-122.5535621,-122.5535561,-122.6923282,-122.6923111,-122.6921681,-122.6922572,-122.7268749,-122.7268992,-122.6527415,-122.6528304,-122.6527689,-122.5783496,-122.5782692,-122.5776297,-122.5789053,-122.578028,-122.5780624,-122.6534067,-122.6534399,-122.6537376,-122.6537143,-122.6535151,-122.6535681,-122.6536497,-122.6537403,-122.5788626,-122.6533731,-122.6535705,-122.6536129,-122.5788703,-122.6534414,-122.6535134,-122.6537858,-122.6747315,-122.6747316,-122.6747442,-122.6747046,-122.674733,-122.6724695,-122.6724699,-122.6724766,-122.6724718,-122.6747489,-122.6365257,-122.712244,-122.7121893,-122.5187641,-122.7105878,-122.7108739,-122.6299185,-122.7028955,-122.7030259,-122.7029305,-122.6367473,-122.6359204,-122.6167572,-122.6165606,-122.5273912,-122.6258677,-122.669511,-122.669601,-122.6695997,-122.669583,-122.6695189,-122.6695259,-122.6696324,-122.6695171,-122.6696046,-122.6697032,-122.6695984,-122.6695497,-122.6695322,-122.669555,-122.6695932,-122.6695493,-122.6696419,-122.6696175,-122.6695011,-122.712495,-122.6221345,-122.6288015,-122.6799143,-122.624178,-122.6243088,-122.62502,-122.6260338,-122.6286095,-122.6694117,-122.669846,-122.6698872,-122.669925,-122.6694987,-122.6704702,-122.6704964,-122.67009,-122.6697094,-122.6696961,-122.6702239,-122.6702654,-122.669773,-122.6698283,-122.670076,-122.6698686,-122.6699234,-122.6697155,-122.6699538,-122.6694186,-122.6694395,-122.6694645,-122.6695122,-122.6698089,-122.6699963,-122.6697962,-122.6700169,-122.670032,-122.6705951,-122.6705587,-122.6697175,-122.670533,-122.6701443,-122.6704569,-122.6704608,-122.6704035,-122.67035,-122.6701983,-122.669602,-122.6696309,-122.6701416,-122.6701032,-122.6696477,-122.6698917,-122.6694404,-122.6698718,-122.6695314,-122.6695133,-122.6699152,-122.6699758,-122.6703817,-122.6705126,-122.6700454,-122.6697513,-122.6701206,-122.6696822,-122.6703112,-122.6696689,-122.669627,-122.6697902,-122.6701539,-122.6698074,-122.6696873,-122.6700126,-122.6699406,-122.6704006,-122.6697859,-122.6697447,-122.670532,-122.6700593,-122.6697366,-122.6701726,-122.6702028,-122.6703854,-122.6696564,-122.6695851,-122.6698425,-122.6696654,-122.6700328,-122.6699943,-122.6699057,-122.6696918,-122.6699699,-122.6697182,-122.6697491,-122.6733099,-122.5451172,-122.6162398,-122.6158558,-122.5437494,-122.6160096,-122.6822712,-122.6822361,-122.6822933,-122.6822374,-122.6822194,-122.6823635,-122.6822286,-122.6823941,-122.6822564,-122.6823184,-122.5307069,-122.530623,-122.5858895,-122.5263028,-122.5264373,-122.5841394,-122.5840355,-122.5840719],null,null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},["Lents Park","Lents Park","Lents Park","DeNorval Unthank Park","DeNorval Unthank Park","DeNorval Unthank Park","DeNorval Unthank Park","Berkeley Park","Berkeley Park","Berkeley Park","Berkeley Park","Cathedral Park","Lents Park","Ventura Park","Ventura Park","Ventura Park","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Columbia Childrens Arboretum","Floyd Light Park","Floyd Light Park","Couch Park","Couch Park","Couch Park","Couch Park","Northgate Park","Northgate Park","Holladay Park","Holladay Park","Holladay Park","Montavilla Park","Montavilla Park","Montavilla Park","Montavilla Park","Montavilla Park","Montavilla Park","Holladay Park","Holladay Park","Holladay Park","Holladay Park","Holladay Park","Holladay Park","Holladay Park","Holladay Park","Montavilla Park","Holladay Park","Holladay Park","Holladay Park","Montavilla Park","Holladay Park","Holladay Park","Holladay Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Peninsula Park","Crystal Springs Rhododendron Garden","Columbia Park","Columbia Park","Argay Park","University Park","University Park","Kenilworth Park","Wallace Park","Wallace Park","Wallace Park","Crystal Springs Rhododendron Garden","Crystal Springs Rhododendron Garden","Creston Park","Creston Park","Lincoln Park","Laurelhurst Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Columbia Park","Fernhill Park","Laurelhurst Park","Lair Hill Park","Laurelhurst Park","Laurelhurst Park","Laurelhurst Park","Laurelhurst Park","Laurelhurst Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Gov Tom McCall Waterfront Park","Lan Su Chinese Garden","Knott Park","Creston Park","Creston Park","Raymond Park","Creston Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Duniway Park","Luuwit View Park","Luuwit View Park","Sacajawea Park","Gilbert Primary Park","Gilbert Primary Park","Essex Park","Essex Park","Essex Park"],null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[45.47357136,45.59280065],"lng":[-122.7618313,-122.5187641]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Summary

Data packages are straightforward ways for instructors to make data and codebooks available for their students.  They help model an appropriate level of data curation by integrating the codebooks and other meta data related to the dataset.  They also facilitate the use of rich, real-world data sources.  

Because data packages are often simpler than full-fledged R packages, they provide an accessible way for instructors, and even students, to develop a package as a way to organize and document data that they might be ingesting and curating. 

The `devtools`, `roxygen2`, and `usethis` packages facilitate and automate much of the package creation process.  

[Kelly McConville](https://mcconville.rbind.io) is an assistant professor of statistics at [Reed College](https://www.reed.edu/) in Portland, OR.  At Reed, she teaches a variety of statistics and data science courses.  As a firm believer that undergraduate research enhances the educational experience, she involves students in forestry data science research and co-chairs two national programs: the [Undergraduate Statistics Project Competition (USPROC)](https://www.causeweb.org/usproc/) and the [Electronic Undergraduate Statistics Research Conference (eUSR)](https://www.causeweb.org/usproc/eusrc/2019).  She'd love to see your students submit their work to USPROC and eUSR!


##  Learn more

- [pdxTrees package home](https://github.com/mcconvil/pdxTrees)
- [RStudio Package Development Cheat Sheet](https://github.com/rstudio/cheatsheets/raw/master/package-development.pdf)
- [R packages](http://r-pkgs.had.co.nz/) by Hadley Wickham
    + In particular, the chapter on [Data](http://r-pkgs.had.co.nz/data.html)
- [Package Development Tutorial](https://github.com/jennybc/pkg-dev-tutorial)
- [Creating a package for your dataset](https://www.r-bloggers.com/creating-a-package-for-your-data-set/)
- [Another great example of creating a data package](https://www.erikhoward.net/how-to-create-an-r-data-package/)



### About this blog 

Each day during the summer of 2019 we intend to add a new entry to this blog on a given topic of interest to educators teaching data science and statistics courses. Each entry is intended to provide a short overview of why it is interesting and how it can be applied to teaching. We anticipate that these introductory pieces can be digested daily in 20 or 30 minute chunks that will leave you in a position to decide whether to explore more or integrate the material into your own classes. By following along for the summer, we hope that you will develop a clearer sense for the fast moving landscape of data science. Sign up for emails at https://groups.google.com/forum/#!forum/teach-data-science (you must be logged into Google to sign up).

We always welcome comments on entries and suggestions for new ones.  However, comments on the blog should be constructive, encouraging, and supportive.  We reserve the right to delete comments that violate these guidelines.

