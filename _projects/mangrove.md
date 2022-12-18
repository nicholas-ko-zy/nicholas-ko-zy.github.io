---
title: Mangrove Forest Research
layout: inner
---

# Mangrove Research
_I worked with Dr Michiel Van Brugel, to understand why certain plots of land had more mangrove species than other. I used the `vegan` package to measure the range of different species within a research plot on Pulau Ubin Island._

## Goal
Use bootstrap simulations to understand whether elevation or abundance or a combination of both elevation **and** abundance lead to the observed distribution of mangrove species on Pulau Ubin Island. 

## Data
#### Species abundance per plot.

The species abundance value is already adjusted based on the elevation and abundance of trees species within each plot. 

![](/img/mangrove/sp_abundance.png)

#### Species abundance across all plot

Species abundance weighted by abundance across all plots, not just the abundance within the research plot.

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
We see that elevation and abundance can predict the presence of mangrove species diversity in the plots, except for plot 10 as indicated by the red cross in heatmap 1. This indicates that there might be a third factor aside from ground elevation and species abundance which might contribute the diversity of mangrove species in those research plots.