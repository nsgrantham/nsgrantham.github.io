---
layout: page
title: What I Use
sidebar_link: true
order: 1
---

I use a number of programs in my data science workflow.

## Programming languages

When I'm exploring a new dataset for the first time, I reach for R. I have used R regularly since my sophomore year at Cal Poly in 2009. Although R was once relatively unknown outside of academic statistics departments, the rise of data science as a new discipline has made R accessible to a much wider audence. Today it is especially welcoming to newcomers to data science, thanks to the contributions of RStudio, tidyverse, and the global community of passionate R developers. I find R most effective for exploratory data analysis and data visualizations with ggplot2.

For data science in production, I depend on Python. I began learning Python in 2015 for an internship in the Crew State Monitoring group at the NASA Langley Research Center. Unlike R, which specializes in statistics, Python is a general purpose language which I find useful for developing end-to-end data analysis pipelines and training machine learning models.

I learned Julia in graduate school after I could no longer stomach the the sluggish convergence of my Markov chain Monte Carlo (MCMC) code in R. Fitting hierarchical Bayesian models with MCMC is a breeze with Mamba.jl. I do not use Julia frequently these days, but with its recent 1.0 release in June 2018, that may change.

Finally, bash scripts are the glue that holds everything together.

## IDEs and text editors

If I'm only working with R, I'll use RStudio, but in most other cases I use Visual Studio Code. I use Neovim with Tmux when a task is terminal heavy.

## Getting things done

I have a personal subscription to GSuite which I use for GMail, Google Drive, Google Docs, and Google Sheets.

In recent months, I have migrated my note keeping and schedule planning to [Notion](https://www.notion.so/) and I couldn't be happier with it. Notion is difficult to describe, but I view it as a productivity platform for my life. I highly recommend trying it out.

## This website

nsgrantham.com is built with [Jekyll](http://jekyllrb.com), a blog-aware, static site generator in Ruby, and is hosted freely with GitHub Pages. Its structure and style is courtesy of Hydeout, a Jekyll 3.x refresh of Hyde, which I have [forked and modified slightly](http://github.com/nsgrantham/hydeout).

## Dotfiles

Dotfiles are configuration files that live in a user's home directory. By keeping [my dotfiles on GitHub](http://github.com/nsgrantham/dotfiles), I can get a new system up-and-running in no time at all, backup changes, and share my settings with others. In particular, I maintain my global Git configuration, Zsh aliases, custom Oh-My-Zsh theme, Neovim and Tmux settings, R profile, and Brewfile for macOS.