markdown
================
nicoarias
April 17, 2019

Including Plots
---------------

\`\`\`{r setup, echo=FALSE, out.width = '100%', message = FALSE, warning=FALSE} \# load packages -----------------------------------------------------------

library(readxl) library(tidyverse)

read data ---------------------------------------------------------------
=========================================================================

read the finches data
=====================

finches &lt;- read\_excel("finches\_data.xlsx")

take a quick look at all the variables in the dataset
=====================================================

histogram ---------------------------------------------------------------
=========================================================================

histogram of beak length, grouped by survival, with labels
==========================================================

ggplot( data = finches, \# use the finches dataset mapping = aes(x = beak\_length, \# put beak length on the x axis fill = outcome) \# fill sets the color of the boxes ) + geom\_histogram(bins = 14) + \# add the histogram, use 14 bins facet\_wrap(~ outcome, ncol = 1) + \# outcome is the grouping variable guides(fill = FALSE) + \# don't show a legend for fll color labs( title = "Figure 1.", \# title x = "Beak Length (mm)", \# x-axis label y = "Number of Birds" \# y-axis label )

summarize ---------------------------------------------------------------
=========================================================================

summarize the dataset by outcome (survived vs. died)
====================================================

beak\_length\_grouped\_summary &lt;- finches %&gt;% group\_by(outcome) %&gt;% summarize(mean = mean(beak\_length), sd = sd(beak\_length), n = n()) %&gt;% mutate(sem = sd / sqrt(n), upper = mean + 1.96 \* sem, lower = mean - 1.96 \* sem)

print the results in the console
================================

bar chart ---------------------------------------------------------------
=========================================================================

bar chart of mean beak lengths
==============================

ggplot( data = beak\_length\_grouped\_summary, \# dont use the original finches dataset mapping = aes(x = outcome, \# survival on the x axis y = mean, \# mean beak length on the y axis fill = outcome) \# make died/survived different colors ) + geom\_col() + \# add columns geom\_errorbar( \# add error bars mapping = aes(ymin = lower, \# lower 95% confidence limit ymax = upper), \# upper 95% confidence limit width = .3 \# width of horizontal part of bars ) + guides(fill = FALSE) + \# don't show a legend for fll color labs( title = "Figure 2.", \# title x = "Survival Outcome", \# x-axis label y = "Beak Length (mm)" \# y-axis label )

t-test ------------------------------------------------------------------
=========================================================================

get a vector of beak lengths for birds that died
================================================

beak\_length\_died &lt;- finches %&gt;% \# start with finches dataset filter(outcome == "died") %&gt;% \# only include rows w/ outcome=died pull(beak\_length) \# extract the beak\_length column

print the new object in the console... it is a vector
=====================================================

get a vector of beak lengths for birds that survived
====================================================

beak\_length\_survived &lt;- finches %&gt;% filter(outcome == "survived") %&gt;% pull(beak\_length)

print the results in the console
================================

perform a two-sample t-test assuming unequal variances
======================================================

t.test(beak\_length\_died, beak\_length\_survived) \`\`\`
