---
layout: post
title: "R markdown"
modified:
categories: blog
excerpt:
tags: [rmarkdown]
image:
  feature:
date: 2016-09-22T14:50:00+02:00
author: malditobarbudo
share: false
---
## R markdown dummy text

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```r
cars %>%
  summary()
```

## Including Plots

You can also embed plots, for example:

```r
# plotting of R objects
plot <- function (x, y, ...)
{
  if (is.function(x) && 
      is.null(attr(x, "class")))
  {
    if (missing(y))
      y <- NULL
    
    # check for ylab argument
    hasylab <- function(...) 
      !all(is.na(
        pmatch(names(list(...)),
               "ylab")))
    
    if (hasylab(...))
      plot.function(x, y, ...)
    
    else 
      plot.function(
        x, y, 
        ylab = paste(
          deparse(substitute(x)),
          "(x)"), 
        ...)
  }
  else 
    UseMethod("plot")
} 88
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
