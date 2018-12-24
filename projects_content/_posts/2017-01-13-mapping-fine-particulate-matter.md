---
layout: post
title: Mapping fine particulate matter
image: https://lh3.googleusercontent.com/ueZS-SXibFfNLJ5AVgpbMIrjUQhkSJV3HBsaa2ISuN5lUDcINJWPGnwA8P-S11z7v-1CWxtdRhO_iDtYpz_1keMtvK2lBdd2ZSB5wGrRJdj2CVDi5zkHdwtHvSqCcvPDaba5tv7PebdOm7CjWarOSPt4Dga6Mc1Fd8MSdiStmc8C84rv58Q6kdLY9D_SNkUIBr2OD3kQPFcY7mng5lrK3J3F7djf0_HzQ6Q7TPQ9FKl5rQMz0OV31GXM4nsV4Ar3bXgh2RlEZoDGhmf0-wn4wtf6EeffDwI-eHnHbCS2BPBIcCIT82ux1DNYize57YDC2xH9bguvCkmpPTVoF_r2t85c9yv8CnXiPr63971E4dx2xIwnfE8kU1KZSnoA5KdJctOjH8N504gK1eFGeLPnG6oyfg5-fqioqZ6yyQLZSYt-kettMEMapsPJpBiRU6amQBRlBBiiGU7R388AZosncz47mXCYXMnf2vaTWtf-laGS4NLbePAQVBmnDVpdH6peqlQ5KMRrGBu-VLMeTb4Asytzn7HC54dKyqns8PL67F_oMckKhqB_ugR9maKy7jzz0CiW_lOFrbgJ-LmfOrX5E45-LxZTk9VWRQ737ROABuuPZyWrl58n7RUPO7ZNkCzEcnYUxkkC-fEFhSDjjKY-B-4C=s400-no
description: Spatial regression with an informatively-missing covariate
---

Mapping fine particulate matter (PM2.5) requires a number of spatial and spatiotemporal covariates, one of which is aerosol optical depth (AOD) captured by NASA satellites. However, AOD is often missing, and it is likely the conditions that lead to missing AOD are also conducive to high AOD (pictured below).

![](https://lh3.googleusercontent.com/2utOcVlmri_KrY4H12TrYg-OpSRVtyAbgoXngDXm982aWYBA0LYsmz6x-FvhcQAYNDYcZcArDawczZKVV1364Ofn8QphNmA45FAB_simh_be6agZIPa8GPSWXCEkQT-2frAZzfMrvgJ7UgobLNNdgx_0Fh-0cJ8wuVtzGr9h8QFTAgR5UPL3nVONYko7qccYrYqc3B-1m-h-2kOd4HT0tNMOi7fFwiv4O2jyLN_8Nc729eHyPZ5zKsE-rbGPVj7xT0N75Mskt63L-Zqixd7M-vV_i7UcIuJeG7xxyv2dsGnMfeLw6sN-3v4aYwSXXrATlDYgp_GPP-m8NBsQLibeq7DwKwcTXdDRf01w-RDwS-zY_H_ke7ChkiSKEtt6mlpHVqwaR_2uMqWhBfR-KBw1I0VlA257uqPZye3wYrNKB1B4HycnsB8lN54l6IzSk66aipNIy1L_P_gneuxDnb3oMoOzTUcg05fYTYt69OwrxvSsp_2hdGiwkOP5h8nXQ007B3b1OFifKzBFAJ8-j2rbrTSvyI_R0rKtVCfWfF4HlxYsQdN_rA41eo0Prk5PUH1-TzqGWEeTGSKxrE4iguTT8gBaxvtOaxFfSNPBdzcThVDdz_J1zMEVLq1xgEXb20j3pN86iL-Y7KLsdk9C14NZoX1x=w800-h400-no)

The following paper develops a Bayesian hierarchical model for predicting PM2.5 when a spatial covariate such as AOD exhibits informative missingness. Read more in Environmetrics:

Grantham, NS, Reich, BJ, Liu, Y, Chang, HH (2018). [_Spatial Regression with an Informatively-Missing Covariate: Application to Mapping Fine Particulate Matter_](https://onlinelibrary.wiley.com/doi/full/10.1002/env.2499). _Environmetrics_. Code available at [github.com/nsgrantham/spatial-covariate-informative-missingness](http://www.github.com/nsgrantham/spatial-covariate-informative-missingness).