---
layout: post
title: "Starting bars and histograms at zero in ggplot2"
modified:
categories: blog R
excerpt:
tags: [R, ggplot2]
image:
  feature: feature_insect_2.jpg
date: 2016-09-23T016:14:00+02:00
author: malditobarbudo
share: false
---



When creating histograms or bar plots in `ggplot2` we found that the data is 
placed at some distance from the x axis, which means the y axis starts below zero:


{% highlight r %}
# libraries
library(ggplot2)
library(ggthemes)

# histogram with gap example
ggplot(iris, aes(x = Petal.Length, fill = Species)) +
  geom_histogram(position = 'dodge') +
  scale_fill_solarized() +
  theme_solarized(light = FALSE, base_family = 'Inconsolata')
{% endhighlight %}

<img src="/figure/source/2016-09-23-starting-bars-and-histograms-at-zero-in-ggplot2/gap_plot-1.png" title="Histogram with gap at the bottom" alt="Histogram with gap at the bottom" style="display: block; margin: auto;" />

This is because, internally, `ggplot2` is expanding axis by a multiplicative
or additive constant, see the `expand` parameter in `?scale_y_continuous` for
details. This makes sense in almost all plots, except for the bar and histogram
ones, as we see above.  

## Workaround

To avoid this behaviour, we have to modify the y scale indicating the `expand`
value as well as the `limits` value:


{% highlight r %}
# histogram without bottom gap
ggplot(iris, aes(x = Petal.Length, fill = Species)) +
  geom_histogram(position = 'dodge') +
  scale_y_continuous(expand = c(0,0),
                     limits = c(0,30)) +
  scale_fill_solarized() +
  theme_solarized(light = FALSE, base_family = 'Inconsolata')
{% endhighlight %}

<img src="/figure/source/2016-09-23-starting-bars-and-histograms-at-zero-in-ggplot2/bottom_plot-1.png" title="Histogram without bottom gap" alt="Histogram without bottom gap" style="display: block; margin: auto;" />

As you can see, here we add the following layer to the `gpplot` call:

```r
scale_y_continuous(expand = c(0,0),
                   limits = c(0,30)) +
```

This way we indicate to avoid the y axis expand, but we set (harcoded) the y max
limit to allow for a gap above the data (the y max value depends on the count range,
so you have to take a look at the plot first).  
This way has some disadventages[^a]:

[^a]: It seems an asymmetrical expand argument is on its [way](https://github.com/hadley/ggplot2/issues/1669)

  + You have to provide a hardcoded limit value, so you have to generate the plot
    one time and modify the code to generate the final version. So no
    automatization here, in case you need it.
  
  + It does not play well with `free y` faceted plots.

### Bar plots

For bar plots is the same:


{% highlight r %}
# histogram without bottom gap
ggplot(iris, aes(x = Species, fill = Species)) +
  geom_bar(stat = 'count') +
  scale_y_continuous(expand = c(0,0),
                     limits = c(0,55)) +
  scale_fill_solarized() +
  theme_solarized(light = FALSE, base_family = 'Inconsolata')
{% endhighlight %}

<img src="/figure/source/2016-09-23-starting-bars-and-histograms-at-zero-in-ggplot2/bar_plot-1.png" title="Bar plot without bottom gap" alt="Bar plot without bottom gap" style="display: block; margin: auto;" />


## Summary

<img src="/figure/source/2016-09-23-starting-bars-and-histograms-at-zero-in-ggplot2/cowplot_summary-1.png" title="Summary plot. Default plot, without expand plot and without expand and limits plot" alt="Summary plot. Default plot, without expand plot and without expand and limits plot" style="display: block; margin: auto;" />

As you can see, default plot adds a gap at both ends, up and down. Setting
`expand` to zero removes both gaps, no ideal. Finally setting `expand` to zero and
setting the optimal y max limit does the trick.

## Bits of code

Code for the summary
[cowplot](https://cran.r-project.org/web/packages/cowplot/index.html):


{% highlight r %}
# install cowplot if needed
# install.packages('cowplot')

# libraries
library(cowplot)

# plots
default <- ggplot(iris, aes(x = Petal.Length, fill = Species)) +
  geom_histogram(position = 'dodge') +
  scale_fill_solarized() +
  labs(title = 'default') +
  theme_solarized(light = FALSE, base_family = 'Inconsolata') +
  theme(legend.position = 'none')

wo_expand <- default +
  scale_y_continuous(expand = c(0,0)) +
  labs(title = 'wo expand')

wo_expand_w_limits <- default +
  scale_y_continuous(expand = c(0,0),
                     limits = c(0,30)) +
  labs(title = 'wo expand + limits') +
  theme(legend.position = c(.8, .7))

# cowplot
plot_grid(default, wo_expand, wo_expand_w_limits,
          align = 'h', nrow = 1,
          labels = NULL)
{% endhighlight %}

## Sources

  + [http://stackoverflow.com/questions/20220424/ggplot2-bar-plot-no-space-between-bottom-of-geom-and-x-axis-keep-space-above](http://stackoverflow.com/questions/20220424/ggplot2-bar-plot-no-space-between-bottom-of-geom-and-x-axis-keep-space-above)

  + [https://github.com/hadley/ggplot2/issues/1669](https://github.com/hadley/ggplot2/issues/1669)

***
