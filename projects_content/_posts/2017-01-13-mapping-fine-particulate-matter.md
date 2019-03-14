---
layout: post
title: Mapping fine particulate matter
image: https://drive.google.com/uc?export=view&id=1vt90eZUalxvw7cIWh2ChNPJBWz0dur1j
description: Spatial regression with an informatively-missing covariate
tags:
- spatial statistics
- Julia
---

Mapping fine particulate matter (PM2.5) requires a number of spatial and spatiotemporal covariates, one of which is aerosol optical depth (AOD) captured by NASA satellites. However, AOD is often missing, and it is likely the conditions that lead to missing AOD are also conducive to high AOD (pictured below).

![](https://drive.google.com/uc?export=view&id=1mT_hX1KeWbg8uM4jNrRKmNZ9sDJ0BAAe)

The following paper develops a Bayesian hierarchical model for predicting PM2.5 when a spatial covariate such as AOD exhibits informative missingness. Read more in Environmetrics:

Grantham, NS, Reich, BJ, Liu, Y, Chang, HH (2018). [_Spatial Regression with an Informatively-Missing Covariate: Application to Mapping Fine Particulate Matter_](https://onlinelibrary.wiley.com/doi/full/10.1002/env.2499). _Environmetrics_. Code available at [github.com/nsgrantham/spatial-covariate-informative-missingness](http://www.github.com/nsgrantham/spatial-covariate-informative-missingness).
