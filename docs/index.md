---
title: "Intro to R workshop" # edit
#subtitle: "optional" 
author: "Emily Nordmann" # edit
date: "2022-07-13"
site: bookdown::bookdown_site
documentclass: book
classoption: oneside # for PDFs
geometry: margin=1in # for PDFs
bibliography: [book.bib, packages.bib]
csl: include/apa.csl
link-citations: yes
description: | # edit
  The accompany book for the PsyTeachR intro to R workshop.
url: https://psyteachr.github.io/workshop-dataviz # edit
github-repo: psyteachr/workshop-dataviz # edit
cover-image: images/logos/logo.png # replace with your logo
apple-touch-icon: images/logos/apple-touch-icon.png # replace with your logo
apple-touch-icon-size: 180
favicon: images/logos/favicon.ico # replace with your logo
---



# Overview {.unnumbered}

Before the workshop it is necessary to work through some prep tasks to ensure that you are ready to go.

-   If you don't currently have R or RStudio installed, please work through Chapter \@ref(installing-r).
-   If you have R and/or RStudio installed, please skip to Chapter \@ref(rstudio-settings) and update to the latest version before the workshop.
-   If you have technical issues and cannot install R and RStudio on your machine (e.g., if you don't have admin rights), please sign-up for an [RStudio Cloud](https://rstudio.cloud/) account.

Once you've done one of the above, work through Chapter \@ref(intro) which will introduce you to some basic programming terminology and install the packages we need for the workshop. Depending upon your familiarity with R, this should take 1-2 hours.

**Regardless of what set-up you need to do, before you join the workshop, please ensure you have completed the Workshop Set-up Check in Chapter** \@ref(workshop-prep)**. If it works, you're good to go.**

## Workshop schedule

One of the joys of teaching R (or anything for that matter) is that people learn at different paces and bring different skills and prior knowledge. The below table sets out the plan for the workshop, but we will go as fast or as slow as we need to so we may not end up covering it all or we may go further than planned. Each session lasts one hour. Session 1 and 2 are delivered on day 1, session 3 and 4 are delivered on day 2.

| Session     | Topic                                      | Skills                                                                                            |
|-------------|--------------------------------------------|---------------------------------------------------------------------------------------------------|
| Prep        | Intro to R and RStudio                     | Installing R and packages, running code                                                           |
| 1.1 | Intro to data visualisation                | Create basic bar charts, histograms, scatter plots, density plots, & violin-boxplots              |
| 1.2 | Data viz continued                         | Customise visualisations and create more advanced plots like raincloud plots & split violin-plots |
| 2.1 | Data summaries                             | Calculate descriptive statistics                                                                  |
| 2.2 | Recap and quiz + choose your own adventure | Consolidate content so far, independent exercises.                  |
| 3.1 | Data wrangling                             | Select and filter data, create new variables, dealing with missing data                           |
| 3.2 | Data relations                               | Join datasets together                                                         |
| 4.1 | Regression                                | Perform and visualise correlation and regression analyses                                             |
| 4.2 | Recap and quiz + choose your own adventure | Consolidate content so far, independent exercises.                  |

The materials from this workshop are adapted from:

-   [Nordmann, E. & DeBruine, L. (2022). Applied Data Skills (1.0). Zenodo. https://doi.org/10.5281/zenodo.6365078](https://psyteachr.github.io/ads-v1/)

-   [Nordmann, E., McAleer, P., Toivo, W., Paterson, H. & DeBruine, L. (accepted). Data visualisation using R, for researchers who don't use R. Advances in Methods and Practices in Psychological Science.](https://psyteachr.github.io/introdataviz/)

-   [Nordmann, E. & McAleer, P. Fundamentals of quantitative analysis (2.0)](https://psyteachr.github.io/quant-fun-v2/index.html)
