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

We asked the question, "Does species abundance per plot affect the diversity of mangrove species?". The species abundance value has been adjusted based on the elevation and abundance of trees species within each plot. 

![](/img/mangrove/sp_abundance.png)

#### Species abundance across all plot

Species abundance weighted by abundance across all plots and land elevation, not just the abundance within the research plot.

![](/img/mangrove/all_abund.png)

## Results

#### Heatmap 1: Elevation and Abundance
_Dissimilarity of population of mangrove species weighted by their elevation and relative abundance within each plot._

![](/img/mangrove/heatmap_1.png)

_*Heatmap 2 contained an erroneous assumption so it is not featured here._
#### Heatmap 3: Abundance across all plots

_Dissimilarity of population of mangrove species weighted by absolute abundance across all plots._

![](/img/mangrove/heatmap_3.png)

## Summary 
We see that elevation and abundance can predict the presence of mangrove species diversity in the plots, except for plot 10 as indicated by the red cross in heatmap 1. Also simulating the diversity of mangrove trees using "abundance across all plots" instead of "abundance per plot" leads to higher accuracy. 