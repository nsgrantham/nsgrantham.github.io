---
layout: post
title: "Spacewalks: A Fifty-Year History"
excerpt_separator: <!-- more -->
tags:
- data visualization
- space
---

The first-ever spacewalk was performed by cosmonaut Alexei Leonov on March 18, 1965. Since then, humankind has logged over 2,000 hours of extravehicular activity (EVA) beyond Earth's appreciable atmosphere. Three nations have led spacewalks: Russia, USA, and, most recently, China, following the success of Shenzhou 7 on September 27, 2008.

I developed the following graphic after stumbling upon NASA's data portal and their dataset on [US and Russia's Extra-vehicular Activity (EVA)](https://data.nasa.gov/Raw-Data/Extra-vehicular-Activity-EVA-US-and-Russia/9kcy-zwvn). The processing and visualization of these data are fully reproducible at [github.com/nsgrantham/spacewalks](https://www.github.com/nsgrantham/spacewalks). 

<!-- more -->

The data visualization ([full-size available here](http://i.imgur.com/E0bOYHe.png)) was produced with `R` packages `ggplot2` and `gridExtra`. The left-hand plot ranks total EVA duration per astronaut/cosmonaut/taikonaut whereas the right-hand plot depicts the durations of mission-specific EVAs over time.

![spacewalks](http://i.imgur.com/E0bOYHe.png)

