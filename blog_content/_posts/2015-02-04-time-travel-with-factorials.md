---
layout: post
title: "Time Travel with Factorials"
description: How long ago is \(n!\) seconds? 
excerpt_separator: <!-- more -->
tags:
- mathematics
- history
---

Introductory calculus courses often give students the following rules-of-thumb when finding limits of ratios of functions:

- Exponential \\(\exp(x)\\) beats a power \\(x^{k}\\)
- Power beats a logarithm \\(\log(x)\\)

For example, this implies \\(\lim\limits\_{x\to\infty} \frac{\exp(x)}{x^{2} + 7x + 42} = \infty\\) or \\(\lim\limits\_{x\to\infty} \frac{\log(x)}{x^{4}+x-12} = 0\\).

However, courses tend not to emphasize just how ridiculous the factorial function is in comparison. Recall that the factorial of a non-negative integer \\(n\\) is defined such that
\\[
n! = n\cdot(n-1)\cdots 2\cdot 1,
\\]
where \\(0! = 1\\) by convention. So, say \\(n = 4\\). Then \\(n! = 4\cdot 3\cdot 2\cdot 1 = 24\\).

Factorials are absolute monstrosities for even moderately-sized \\(n\\). To appreciate just how quickly factorials grow, let's consider looking back \\(n!\\) seconds in the past. <!-- more --> Given a number of `seconds`, the following `how_long_ago` function finds its equivalent formulation in years to weeks all the way to seconds.

{% highlight r %}
how_long_ago <- function(seconds) {
  # given seconds, returns years, weeks, days, hours, mins, and secs
  if (seconds < 0) stop("seconds must be non-negative")
  spm <- 60             # seconds per minute
  sph <- 60 * spm       # per hour
  spd <- 24 * sph       # per day
  spw <- 7 * spd        # per week
  spy <- 52.1775 * spw  # per year
  s <- seconds
  years <- floor(s / spy); s <- s %% spy
  weeks <- floor(s / spw); s <- s %% spw
  days <- floor(s / spd); s <- s %% spd
  hours <- floor(s / sph); s <- s %% sph
  mins <- floor(s / spm); s <- s %% spm
  cat(years, "years,", weeks, "weeks,", days, "days,", 
      hours, "hours,", mins, "minutes, and", s, "seconds.")
}
{% endhighlight %}

## \\(4!\\) seconds is just 24 seconds. 

You were likely reading my brief introduction or clicking on the link to this page \\(4!\\) seconds ago.

## \\(5!\\) seconds is exactly 2 minutes.


{% highlight r %}
how_long_ago(factorial(5))
{% endhighlight %}



{% highlight text %}
## 0 years, 0 weeks, 0 days, 0 hours, 2 minutes, and 0 seconds.
{% endhighlight %}

## \\(6!\\) seconds is exactly 12 minutes.


{% highlight r %}
how_long_ago(factorial(6))
{% endhighlight %}



{% highlight text %}
## 0 years, 0 weeks, 0 days, 0 hours, 12 minutes, and 0 seconds.
{% endhighlight %}

## \\(7!\\) seconds is almost an hour-and-a-half.


{% highlight r %}
how_long_ago(factorial(7))
{% endhighlight %}



{% highlight text %}
## 0 years, 0 weeks, 0 days, 1 hours, 24 minutes, and 0 seconds.
{% endhighlight %}

\\(7!\\) seconds ago you were probably waking up, heading to work, eating some food, or, if you're like me, procrastinating on the computer.

## \\(8!\\) seconds is almost a half-day.


{% highlight r %}
how_long_ago(factorial(8))
{% endhighlight %}



{% highlight text %}
## 0 years, 0 weeks, 0 days, 11 hours, 12 minutes, and 0 seconds.
{% endhighlight %}

\\(8!\\) seconds ago is almost a half-day in the past. I was getting ready for bed (grad students live exciting lives).

## \\(9!\\) seconds is just over 4 days.


{% highlight r %}
how_long_ago(factorial(9))
{% endhighlight %}



{% highlight text %}
## 0 years, 0 weeks, 4 days, 4 hours, 48 minutes, and 0 seconds.
{% endhighlight %}

\\(9!\\) seconds ago from Wednesday at noon (when I published this post) is Saturday morning. Do they still show cartoons on Saturday morning?

## \\(10!\\) seconds is exactly 6 weeks.


{% highlight r %}
how_long_ago(factorial(10))
{% endhighlight %}



{% highlight text %}
## 0 years, 6 weeks, 0 days, 0 hours, 0 minutes, and 0 seconds.
{% endhighlight %}

\\(10!\\) seconds ago from February 4, 2015 (the publish date of this post) yields December 24, 2014. You may have been celebrating Christmas Eve or the last night of Hannukah.

## \\(11!\\) seconds is about 1.25 years.


{% highlight r %}
how_long_ago(factorial(11))
{% endhighlight %}



{% highlight text %}
## 1 years, 13 weeks, 5 days, 18 hours, 10 minutes, and 48 seconds.
{% endhighlight %}

From February 4, 2015, \\(11!\\) seconds ago it was October 31, 2013. Happy Halloween!

## \\(12!\\) seconds is about a decade-and-a-half.


{% highlight r %}
how_long_ago(factorial(12))
{% endhighlight %}



{% highlight text %}
## 15 years, 9 weeks, 2 days, 8 hours, 42 minutes, and 0 seconds.
{% endhighlight %}

\\(12!\\) seconds ago from now we entered the final month of the 20th century: December 1, 1999. With 2000 AD just around the corner, the world was collectively freaking out about the [Y2K bug](http://en.wikipedia.org/wiki/Year_2000_problem).

## \\(13!\\) seconds is nearly two centuries.


{% highlight r %}
how_long_ago(factorial(13))
{% endhighlight %}



{% highlight text %}
## 197 years, 17 weeks, 0 days, 5 hours, 27 minutes, and 36 seconds.
{% endhighlight %}

Holy history Batman! \\(13!\\) seconds ago it was the year of 1817. In the United States, President James Monroe had recently bought Florida from Spain and was in the midst of his nation-wide good-will tour promoting national unity and ushering a relatively young America into the [Era of Good Feelings](http://en.wikipedia.org/wiki/Era_of_Good_Feelings).

## \\(14!\\) seconds is nearly three millenia.


{% highlight r %}
how_long_ago(factorial(14))
{% endhighlight %}



{% highlight text %}
## 2762 years, 29 weeks, 5 days, 5 hours, 9 minutes, and 36 seconds.
{% endhighlight %}

\\(14!\\) seconds ago it was the 8th century BC. [Ancient Greece](http://en.wikipedia.org/wiki/Ancient_Greece) transitioned from the Dark Ages into its Archaic period and held the first-ever Olympic games in 776 BC.

## \\(15!\\) seconds is over 40,000 years.


{% highlight r %}
how_long_ago(factorial(15))
{% endhighlight %}



{% highlight text %}
## 41438 years, 28 weeks, 5 days, 6 hours, 50 minutes, and 24 seconds.
{% endhighlight %}

\\(15!\\) seconds ago it was 40,000 BC in the height of the [Upper Paleolithic](http://en.wikipedia.org/wiki/Upper_Paleolithic). The oldest known cave art in [Cave of El Castillo, Spain](http://en.wikipedia.org/wiki/Cave_of_El_Castillo) dates back to this time.

## \\(16!\\) seconds is over 650,000 years.


{% highlight r %}
how_long_ago(factorial(16))
{% endhighlight %}



{% highlight text %}
## 663016 years, 42 weeks, 4 days, 14 hours, 52 minutes, and 48 seconds.
{% endhighlight %}

\\(16!\\) seconds ago is between 700,000 and 600,000 years ago in the [Lower Paleolithic](http://en.wikipedia.org/wiki/Lower_Paleolithic). During this period, [early humans first settled northern Europe](http://news.nationalgeographic.com/news/2005/12/1216_051216_humans_britain.html). 

## \\(17!\\) seconds is over 11 million years.


{% highlight r %}
how_long_ago(factorial(17))
{% endhighlight %}



{% highlight text %}
## 11271285 years, 46 weeks, 6 days, 9 hours, 18 minutes, and 0 seconds.
{% endhighlight %}

\\(17!\\) seconds ago, [humans, chimpanzees, and bonobos had not yet diverged evolutionarily](http://en.wikipedia.org/wiki/Chimpanzee–human_last_common_ancestor), with the [first hominin](http://en.wikipedia.org/wiki/Sahelanthropus) likely appearing some 3-4 million years later.

## \\(18!\\) seconds is over 200 million years.


{% highlight r %}
how_long_ago(factorial(18))
{% endhighlight %}



{% highlight text %}
## 202883146 years, 9 weeks, 4 days, 2 hours, 16 minutes, and 48 seconds.
{% endhighlight %}

\\(18!\\) seconds ago Earth was transitioning from the Triassic to the Jurassic within the [Mesozoic Era](http://en.wikipedia.org/wiki/Mesozoic). Terrestrial species lived on a single supercontinent, [Pangaea](http://en.wikipedia.org/wiki/Pangaea), though many land- and sea-dwelling organisms disappeared following a [mass extinction event](http://en.wikipedia.org/wiki/Triassic–Jurassic_extinction_event) which paved the way for the domination of Earth by the dinosaurs.

## \\(19!\\) seconds is nearly 4 billion years.


{% highlight r %}
how_long_ago(factorial(19))
{% endhighlight %}



{% highlight text %}
## 3854779777 years, 25 weeks, 4 days, 1 hours, 51 minutes, and 28 seconds.
{% endhighlight %}

\\(19!\\) seconds ago (3.8 billion years ago) the Earth was not yet 1 billion years old. On this very young Earth, [life as we know it began](http://en.wikipedia.org/wiki/Timeline_of_evolutionary_history_of_life). Specifically, the first organisms appeared (cells resembling prokaryotes) and several thousand million years later lived the [last universal ancestor](http://en.wikipedia.org/wiki/Last_universal_ancestor) to which all current life on Earth can be traced back.

## \\(20!\\) seconds is over 77 billion years!


{% highlight r %}
how_long_ago(factorial(20))
{% endhighlight %}



{% highlight text %}
## 77095595549 years, 42 weeks, 0 days, 8 hours, 53 minutes, and 20 seconds.
{% endhighlight %}

\\(20!\\) seconds ago is impossible because the [Big Bang](http://en.wikipedia.org/wiki/Big_Bang) only happened 13.8 billion years ago. In fact, \\(20!\\) seconds is roughly 5.5 times the age of our universe!

[^1]: [This factoid](http://www.reddit.com/r/math/comments/2uekic/10_in_practical_terms_6_weeks_is_exactly_10/) inspired this blog post. :)
