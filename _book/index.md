--- 
title: "*Statistical rethinking* with brms, ggplot2, and the tidyverse: Second edition"
subtitle: "version 0.2.0"
author: "A Solomon Kurz"
date: "2021-03-16"
site: bookdown::bookdown_site
output: 
  bookdown::gitbook:
    split_bib: yes
documentclass: book
bibliography: bib.bib
biblio-style: apalike
csl: apa.csl
link-citations: yes
geometry:
  margin = 0.5in
urlcolor: blue
highlight: tango
header-includes:
  \usepackage{underscore}
  \usepackage[T1]{fontenc}
github-repo: ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed
twitter-handle: SolomonKurz
description: "This book is an attempt to re-express the code in the second edition of McElreath's textbook, 'Statistical rethinking.' His models are re-fit in brms, plots are redone with ggplot2, and the general data wrangling code predominantly follows the tidyverse style."
---

# What and why {-}

This ebook is based on the second edition of [Richard McElreath](https://twitter.com/rlmcelreath)'s [-@mcelreathStatisticalRethinkingBayesian2020] text, [*Statistical rethinking: A Bayesian course with examples in R and Stan*](https://xcelab.net/rm/statistical-rethinking/). My contributions show how to fit the models he covered with [Paul Bürkner](https://twitter.com/paulbuerkner)'s [**brms** package](https://github.com/paul-buerkner/brms) [@R-brms; @burknerBrmsPackageBayesian2017; @burknerAdvancedBayesianMultilevel2018], which makes it easy to fit Bayesian regression models in **R** [@R-base] using Hamiltonian Monte Carlo. I also prefer plotting and data wrangling with the packages from the [**tidyverse**](http://style.tidyverse.org) [@R-tidyverse; @wickhamWelcomeTidyverse2019]. So we'll be using those methods, too.

## My assumptions about you {-}

If you're looking at this project, I'm guessing you're either a graduate student, a post-graduate academic or a researcher of some sort, which suggests you have at least a 101-level foundation in statistics. If you're rusty, consider checking out the free text books by [Legler and Roback](https://bookdown.org/roback/bookdown-bysh/) [-@leglerBroadeningYourStatistical2019] or [Navarro](https://learningstatisticswithr.com/) [-@navarroLearningStatistics2019] before diving into *Statistical rethinking*. I'm also assuming you understand the rudiments of **R** and have at least a vague idea about what the **tidyverse** is. If you're totally new to **R**, consider starting with Peng's [-@pengProgrammingDataScience2019] [*R programming for data science*](https://bookdown.org/rdpeng/rprogdatascience/). For an introduction to the **tidyvese**-style of data analysis, the best source I've found is Grolemund and Wickham's [-@grolemundDataScience2017] [*R for data science*](http://r4ds.had.co.nz) (*R4DS*), which I extensively link to throughout this project. Another nice alternative is Baumer, Kaplan, and Horton's [-@baumerMocernDataScienceR2021], [*Modern data science with R*](https://mdsr-book.github.io/mdsr2e/).

However, you do not need to be totally fluent in statistics or **R**. Otherwise why would you need this project, anyway? IMO, the most important things are curiosity, a willingness to try, and persistent tinkering. I love this stuff. Hopefully you will, too.

## How to use and understand this project {-}

This project is not meant to stand alone. It's a supplement to the second edition of McElreath's text. I follow the structure of his text, chapter by chapter, translating his analyses into **brms** and **tidyverse**-style code. However, some of the sections in the text are composed entirely of equations or prose, leaving us nothing to translate. When we run into those sections, the corresponding sections in this project will sometimes be blank or omitted, though I do highlight some of the important points in quotes and prose of my own. So I imagine students might reference this project as they progress through McElreath's text. I also imagine working data analysts might use this project in conjunction with the text as they flip to the specific sections that seem relevant to solving their data challenges.

I reproduce the bulk of the figures in the text, too. The plots in the first few chapters are the closest to those in the text. However, I'm passionate about data visualization and like to play around with [color palettes](https://github.com/EmilHvitfeldt/r-color-palettes), formatting templates, and other conventions quite a bit. As a result, the plots in each chapter have their own look and feel. For more on some of these topics, check out chapters [3](https://r4ds.had.co.nz/data-visualisation.html), [7](https://r4ds.had.co.nz/exploratory-data-analysis.html), and [28](https://r4ds.had.co.nz/graphics-for-communication.html) in *R4DS*; Healy's [-@healyDataVisualization2018] [*Data visualization: A practical introduction*](https://socviz.co); Wilke's [-@wilkeFundamentalsDataVisualization2019] [*Fundamentals of data visualization*](https://serialmentor.com/dataviz/); or Wickham's [-@wickhamGgplot2ElegantGraphics2016] [*ggplot2: Elegant graphics for data analysis*](https://ggplot2-book.org/).

In this project, I use a handful of formatting conventions gleaned from [*R4DS*](https://r4ds.had.co.nz/introduction.html#running-r-code), [*The tidyverse style guide*](https://style.tidyverse.org/) [@wickhamTidyverseStyleGuide2020], and [*R markdown: The definitive guide*](https://bookdown.org/yihui/rmarkdown/software-info.html) [@xieMarkdownDefinitiveGuide2020].

* **R** code blocks and their output appear in a gray background. E.g.,


```r
2 + 2 == 5
```

```
## [1] FALSE
```

* **R** and the names of specific package (e.g., **brms**) are in **boldface** font.
* Functions are in a typewriter font and followed by parentheses, all atop a gray background (e.g., `brm()`).
* When I want to make explicit the package a given function comes from, I insert the double-colon operator `::` between the package name and the function (e.g., `tidybayes::median_qi()`).
* **R** objects, such as data or function arguments, are in typewriter font atop gray backgrounds (e.g., `chimpanzees`, `.width = .5`).
* You can detect hyperlinks by their typical [blue-colored font](https://solomonkurz.netlify.app/).
* In the text, McElreath indexed his models with names like `m4.1` (i.e., the first model of [Chapter 4][Geocentric Models]). I primarily followed that convention, but replaced the `m` with a `b` to stand for the **brms** package.

## **R** setup {-}

To get the full benefit from this ebook, you'll need some software. Happily, everything will be free (provided you have access to a decent personal computer and an good internet connection).

First, you'll need to install **R**, which you can learn about at [https://cran.r-project.org/](https://cran.r-project.org/).

Though not necessary, your **R** experience might be more enjoyable if done through the free RStudio interface, which you can learn about at [https://rstudio.com/products/rstudio/](https://rstudio.com/products/rstudio/).

Once you have installed **R**, execute the following to install the bulk of the add-on packages. This will probably take a few minutes to finish. Go make yourself a coffee.


```r
packages <- c("ape", "bayesplot", "brms", "broom", "dagitty", "devtools", "flextable", "GGally", "ggdag", "ggdark", "ggmcmc", "ggrepel", "ggthemes", "ggtree", "ghibli", "gtools", "loo", "patchwork", "psych", "rcartocolor", "Rcpp", "remotes", "rstan", "StanHeaders", "statebins", "tidybayes", "tidyverse", "viridis", "viridisLite", "wesanderson")

install.packages(packages, dependencies = T)
```

A few of the other packages are not officially available via the Comprehensive R Archive Network (CRAN; https://cran.r-project.org/). You can download them directly from GitHub by executing the following.


```r
devtools::install_github("stan-dev/cmdstanr")
devtools::install_github("EdwinTh/dutchmasters")
devtools::install_github("gadenbuie/ggpomological")
devtools::install_github("rmcelreath/rethinking")
devtools::install_github("UrbanInstitute/urbnmapr")
remotes::install_github("stan-dev/posterior")
```

It's possible you'll have problems installing some of these packages. Here are some likely suspects and where you can find help:

* for difficulties installing **brms**, go to [https://github.com/paul-buerkner/brms#how-do-i-install-brms](https://github.com/paul-buerkner/brms#how-do-i-install-brms) or search around in the [**brms** section of the Stan forums ](https://discourse.mc-stan.org/c/interfaces/brms/36);
* for difficulties installing **cmdstanr**, go to [https://mc-stan.org/cmdstanr/articles/cmdstanr.html](https://mc-stan.org/cmdstanr/articles/cmdstanr.html);
* for difficulties installing **rethinking**, go to [https://github.com/rmcelreath/rethinking#quick-installation](https://github.com/rmcelreath/rethinking#quick-installation); and
* for difficulties installing **rstan**, go to [https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started](https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started).

## We have updates {-}

For a brief rundown of the version history, we have:

### Version 0.1.0. {-}

I released the 0.1.0 version of this project in November 24, 2020. It was the first full-length and nearly complete draft including material from all the 17 chapters in McElreath's source material. All **brms** models were fit with version 2.14.0+. 

### Version 0.1.1. {-}

On December 2, 2020, I released a mini update designed to 

* fix code breaks resulting from updates to the [**broom** package](https://CRAN.R-project.org/package=broom) [@R-broom], caught by [Jenny Bigman](https://twitter.com/jennybigman);
* replace the soon-to-be retired `sample_n()` code with `slice_sample()`, caught by [Randall Pruim](https://github.com/rpruim);
* and correct a few typos along the way.

### Version 0.2.0. {-}

Welcome to version 0.2.0! Major improvements include:

* a corrected workflow for fitting single-level b-spline models ([Section 4.5.2][Splines.]), thanks to [Stephen Wild](https://github.com/sjwild);
* a refined workflow for fitting multilevel b-spline models using the `s()` function ([Section 4.6][~~Summary~~ First bonus: Smooth functions with `brms::s()`]), thanks to [Gavin Simpson](https://github.com/gavinsimpson);
* a new bonus section ([4.7][Second bonus: Group predictors with matrix columns]) on grouping predictors within matrix columns, thanks to insights from @gelmanRegressionOtherStories2020 and [hints](https://discourse.mc-stan.org/t/fit-a-brm-model-with-an-array-of-predictors/20465) from [Paul-Christian Bürkner](https://github.com/paul-buerkner);
* a **brms** solution to the $\sigma = 0.01$ sixth-order polynomial model in [Section 7.1.1][More parameters (almost) always improve fit.], thanks again to Stephen Wild;
* a workflow to reproduce the Metropolis simulation for [Figure 9.3][High-dimensional problems.], thanks to [James Henegan](https://github.com/jameshenegan);
* a corrected workflow for taking `fitted()`-based random draws for the bonus material in [Section 13.5.2.1][Bonus: Let's use `fitted()` this time.], thanks to an exchange with [Ladislas Nalborczyk](https://github.com/lnalborczyk); 
* a **brms** solution to McElreath's bivariate differential equation model in [Section 16.4][Population dynamics], thanks to [Markus Gesmann](https://github.com/mages);
* better use of `geom_area()` throughout the book, thanks to insights from [Randall Pruim](https://github.com/rpruim);
* the adoption of a [CONTRIBUTING](https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/blob/master/CONTRIBUTING.md) section on GitHub, thanks to [Brenton M. Wiernik](https://github.com/bwiernik);
* improved table workflow with the **flextable** package [@R-flextable]; and
* all models have been refit using **brms** version 2.15.0.

### We're not done yet and I could use your help. {-}

Some areas of the book could use some fleshing out. The sections I'm particularly anxious to improve are

* [15.3][Categorical errors and discrete absences], which may someday include a **brms** workflow for categorical missing data; and
* [16.2.3][Coding the statistical model.], which contains a mixture model that McElreath fit directly in Stan and I suspect may be possible in **brms** with a custom likelihood.

If you have insights on how to improve these or any other sections, please share your thoughts on GitHub at [https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/issues](https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/issues). The contribution guidelines for this book are listed at [https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/blob/master/CONTRIBUTING.md](https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/blob/master/CONTRIBUTING.md).

## Thank-you's are in order {-}

I'd like to thank the following for their helpful contributions:

* E. David Aja ([\@edavidaja](https://github.com/edavidaja)),
* Monica Alexander ([\@MJAlexander](https://github.com/MJAlexander)),
* Shaan Amin ([\@Shaan-Amin](https://github.com/Shaan-Amin)),
* Malcolm Barrett ([\@malcolmbarrett](https://github.com/malcolmbarrett)),
* Adam Bear ([\@adambear91](https://github.com/adambear91)),
* Jenny Bigman ([\@jennybigman](https://github.com/jennybigman)),
* Louis Bliard ([\@lbiard](https://github.com/lbiard)),
* Paul-Christian Bürkner ([\@paul-buerkner](https://github.com/paul-buerkner)),
* Markus Gesmann ([\@mages](https://github.com/mages)),
* James Henegan ([\@jameshenegan](https://github.com/jameshenegan)),
* Sebastian Lobentanzer ([\@slobentanzer](https://github.com/slobentanzer)),
* Ed Merkle ([\@ecmerkle](https://github.com/ecmerkle)),
* Ladislas Nalborczyk ([\@lnalborczyk](https://github.com/lnalborczyk)),
* Randall Pruim ([\@rpruim](https://github.com/rpruim)),
* Gavin Simpson ([\@gavinsimpson](https://github.com/gavinsimpson)),
* Richard Torkar ([\@torkar](https://github.com/torkar)),
* Brenton M. Wiernik ([\@bwiernik](https://github.com/bwiernik)),
* Stephen Wild ([\@sjwild](https://github.com/sjwild)), and
* Donald R. Williams ([\@donaldRwilliams](https://github.com/donaldRwilliams)).

Science is better when we work together.

## License and citation {-}

This book is licensed under the Creative Commons Zero v1.0 Universal license. You can learn the details, [here](https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed/blob/master/LICENSE). In short, you can use my work. Just make sure you give me the appropriate credit the same way you would for any other scholarly resource. Here's the citation information:


```r
@book{kurzStatisticalRethinkingSecondEd2021,
  title = {Statistical rethinking with brms, ggplot2, and the tidyverse: {{Second}} edition},
  author = {Kurz, A. Solomon},
  year = {2021},
  month = {mar},
  edition = {version 0.2.0},
  url = {https://bookdown.org/content/4857/}
}
```

## You can do this, too {-}

This project is powered by Yihui Xie's [-@R-bookdown] [**bookdown** package](https://bookdown.org), which makes it easy to turn R markdown files into HTML, PDF, and EPUB. Go [here](https://bookdown.org/yihui/bookdown/) to learn more about bookdown. While you're at it, also check out Xie, Allaire, and Grolemund's [*R markdown: The definitive guide*](https://bookdown.org/yihui/rmarkdown/). And if you're unacquainted with GitHub, check out Jenny Bryan's [-@bryanHappyGitGitHub2020] [*Happy Git and GitHub for the useR*](https://happygitwithr.com/). I've even [blogged](https://solomonkurz.netlify.com/post/how-bookdown/) about what it was like putting together the first version of this project.

The source code of the project is available on GitHub at [https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed](https://github.com/ASKurz/Statistical_Rethinking_with_brms_ggplot2_and_the_tidyverse_2_ed).



