---
layout: post
title: "knitr + jekyll + so simple theme or Why I started this..."
modified:
categories: blog R
excerpt:
tags: [R jekyll so_simple_theme]
image:
  feature: feature_picos_1.jpg
date: 2016-09-23T01:51:00+02:00
author: malditobarbudo
share: false
---



## Why a blog? Why about R?

The hole `R` blog idea is something that have been around of my mind since
several years. But I had always a very good excuse to pospone it and to
procastinate with another *project*.

I love `R` and the rich ecosystem which seems to be growing around it
(*hadleyverse*, *shiny*, *htmlwidgets*...) even in those frustating moments when
struggling with the code to achieve some data analysis or
visualization with no fortune. And after a while, it seems to me the perfect
timing to start writing about *beardy data* and other `R` adventures.

## Data with beards? What the...?

After ten years trying to get some juice from different data sources, I have
never found a perfect data set like the ones in the documentation or the
examples. Sometimes data has beards, and mustaches, and in one ocassion, I met
some data with sideburns.

So, I'm gonna try to use *no so perfect* data whenever I can in this blog, but
no promises, though, `iris` is so damn pretty ;)

## Why knitr, jekyll and so simple theme?

If I'm gonna write about `R`, I want to be able to write in `Rmd` files. This
will allow to show the code and/or the results with no worries. Plus, I'm
confident writing in markdown.  
First option seemed to be
[R markdown websites](http://rmarkdown.rstudio.com/rmarkdown_websites.html),
with its *pros* and *cons*, a subjective ones by the way.

  + **Pros**
  
    1. It uses `rmarkdown v2`, which is nice because it allows you all the
       interactive stuff like `htmlwidgets`.
    
    1. You can use the RStudio IDE to build the pages, the same way when
       building a package or a shiny app.
  
  + **Cons**
  
    1. Early development, only available from the preview version of RStudio.
       But it won't surprise me if all the cons listed below are fixed sooner
       than later.
    
    1. No blog friendly. You can generate the `html` files, but all the blog
       structure and the *goodies* (rss feeds, tags, categories...) are missing.
    
    1. Involve too much tinkering to get something similar to a blog page to my
       pleasure. The idea of only write the `Rmd` files and after that some
       *black magic* creates the pages seemed very attractive for me.
       
Second option is [jekyll](https://jekyllrb.com/):

  + **Pros**
  
    1. It leverages the post-processing after writing the markdown document and
       creates all the necessary blog infrastructure.
    
    1. Lot of themes and templates, easy to find one that matches my taste.
    
    1. Integrates well with web servers and GitHub Pages
  
  + **Cons**
  
    1. Generate static content, so forgot about fancy interactive apps with
       `htmlwidgets`. There is a
       [workaround](https://github.com/yihui/knitr-jekyll/issues/8) for this by
       Brendan Rocks.
    
    1. Works with `md` files, not `Rmd`. Umm, seems like I can not have it all.
    
    1. No integration with RStudio IDE, so I have to run system command to
       build the pages.

But, after a google search, I found
[this](https://brendanrocks.com/blogging-with-rmarkdown-knitr-jekyll/) which
led me to
[this repository](https://github.com/yihui/knitr-jekyll). Here,
Yihui Xie, software engineer at RStudio, explains how to use the `servr`
package to be able to use `Rmd` documents in the jekyll workflow.

Finally I found the perfect combination. It allowed writing in R markdown and working
in the RStudio IDE as a project combined with a git repository for control version,
so I can feel like home. Also, using jekyll opened a world full of possibilities
about themes and templates. After some searching, I found
[so simple theme](https://mmistakes.github.io/so-simple-theme/), which is in
fact an easy theme to use and configure.  
As to `htmlwidgets` and `shiny` integration, if they can not be in the blog
at least a small `shiny-server` can do the trick.

## How to

Instructions on how to set up something similar are better detailed in the links
above, but a summary is given:

1. Fork, clone and rename the so simple theme
   [repository](https://github.com/mmistakes/so-simple-theme).

1. Install jekyll (In archlinux, first install `ruby` and after that use
   `gem` to install jekyll and bundler: `gem install jekyll bundler`).

1. Execute `bundle install` in the blog root folder. This way all theme
   dependencies are installed.

1. Replace all the template info with blog and author info. Replace example
   posts with your posts. Replace icons and logos with your icons and logos...  
   **tl_dr: replace all you need from the theme templates.**

1. Configure `_config.yml`, `_sass/_variable.scss` and `_sass/_syntax.scss` files
   if needed. I did it to get the solarized style.

1. Create a `build.R` script as in the `knitr-jekyll` repository, and install
   `servr` package. Now `servr::jekyll()` is available.

After executing `servr::jekyll()` a `_site` folder is created in the root of
the project. This folder contains all the structure and the pages of
the blog and can be exported to a web server in production mode.  

## And that is all

After the install and configuration now I only have to write an R markdown
post and execute `servr::jekyll()`. After checking everithing is ok I can
push the `_site` folder to the server and the new post is online.  
If want to pick a look to the code used to generate this site, you can find it
[here](https://github.com/MalditoBarbudo/beardy_data).
