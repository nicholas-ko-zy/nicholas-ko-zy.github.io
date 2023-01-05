---
title: Mangrove Forest Research
layout: inner
lead_text: A research project under Assoc. Professor Michiel Van Brugel to understand factors leading to mangrove forest regeneration on Pulau Ubin island.
---

# Mangrove Research
_I worked with Dr Michiel Van Brugel, to understand why certain plots of land have greater diversity of mangrove species than others. I used the `vegan` package to measure the dissimilarity of mangrove species within each research plot on Pulau Ubin Island._

## Goal
Use bootstrap simulations to understand whether elevation and **abundance per plot** or elevation and **abundance across all plots** lead to the observed distribution of mangrove species on Pulau Ubin Island. 

## Data
#### Species abundance per plot

We asked the question, "Does species abundance per plot contribute to the diversity of mangrove species?". The species abundance value has been adjusted based on the elevation and abundance of trees species within each plot. 

![](/img/mangrove/sp_abundance.png)

#### Species abundance across all plot

Species abundance weighted by abundance across all plots and land elevation, not just the abundance within the research plot.

![](/img/mangrove/all_abund.png)

## Results

#### Heatmap 1: Elevation and Abundance
_Dissimilarity of population of mangrove species weighted by their elevation and relative abundance within each plot._

![](/img/mangrove/heatmap_1.png)

#### Checking if results lie within confidence interval

I calculated the 0.05 and 0.95 percentile of a bootstrap resampling of the data. I used R to check if the results from the heatmap above were found inside the interval. The pink squares indicate that the values are outside the confidence interval. 

![](/img/mangrove/confidence_interval_simulation1.png) 


## Summary 
This was an attempt at making sense of the mangrove data. I tried to use bootstrap sampling to create simulations of the mangrove communities. The hypothesis testing using confidence interval showed that the dissimilarity found in the simulated community was not due to randomness (all values were outside the 95% confidence interval). However, present literature suggests a more conventional NMDS (Non-metric Multidimensional Scaling) which was if given more time I would pursue. I have made a preliminary attempt below. 

![](/img/mangrove/nmds.png) 