---
layout: post
title: "Who Reviews the Pitchfork Reviewers?"
excerpt_separator: <!-- more -->
tags:
- data visualization
- indie music
- web scraping
---


[Pitchfork](http://pitchfork.com) is the largest indie music site on the Internet (in the English-speaking world, at least), updating its pages daily with the latest indie music rumblings, interviews with budding artists, sneak previews of new albums and artist collaborations, and, most notably, a suite of music reviews by dedicated music critics forming Pitchfork's staff. I follow Pitchfork's album reviews religiously and I am not alone in feeling that their 'Best New Music' category routinely captures the best that modern music has to offer.

Since its creation in 1999, Pitchfork has gained quite the following among music fanatics and its widely-read album reviews can have a [demonstrable impact on an album's success](http://the.honoluluadvertiser.com/article/2005/May/08/il/il22p.html). Indeed, Pitchfork is credited with propelling Arcade Fire into the limelight after bestowing a glowing 9.7 'Best New Music' review on their 2004 release _Funeral_. Conversely, they are responsible for striking a devastating blow to Travis Morrison's solo career after his 2004 release _Travistan_ received a controversial 0.0. This make-or-break phenomenon has appropriately been dubbed [The Pitchfork Effect](http://archive.wired.com/wired/archive/14.09/pitchfork.html?pg=3&topic=pitchfork&topic_set=).

Simply put, an album's commercial fate can rest squarely in the hands of the Pitchfork staff member responsible for its review. But what goes into a review? Are staff members consistent in their reviewing behavior? Do biases exist? If so, what kinds? In the following post, I analyze Pitchfork's data in an attempt to answer these questions. After all, who reviews the reviewers?  

<!-- more -->

## All the reviews that are fit to parse

To download Pitchfork's full catalogue of 16080 album reviews I wrote a `python` script that scrapes and parses Pitchfork's pages using `urllib2` and `BeautifulSoup`, respectively, and saves the data into a `SQLite` database via `sqlite3`. I then load these data into `R` and analyze them using a handful of packages including `dplyr`, `magrittr`, and `ggplot2`. For more details on the mechanics behind the data collection and analysis[^1], please visit my `pitchfork-reviews` [GitHub repository](https://github.com/nsgrantham/pitchfork-reviews).


These album reviews include both new releases as well as reissues. Reissues are typically multi-CD re-releases from already successful artists, often with extra artwork, remastered audio, remixes, or bonus tracks added to sweaten the deal. For example, see [David Bowie's recent 'Nothing Has Changed' Deluxe Edition](http://pitchfork.com/reviews/albums/20004-david-bowie-nothing-has-changed/) or the highly anticipated [remastered My Bloody Valentine album set](http://pitchfork.com/reviews/albums/16605-isnt-anything-reissue-loveless-reissue-eps-1988-1991/). In my analysis of album scores by reviewer, I am interested in initial album releases rather than reissued content from well-established artists. 

I therefore attempt to remove reissues from my data set, but this is not straightforward because albums are not explicitly labeled as "new" or "reissued." I can, however, confidently remove the most successful reissues designated as 'Best New Reissue' by Pitchfork. Eliminating these 235 reissues leaves a remaining 15845 reviews comprised of all initial album releases as well as, unfortunately, reissues not distinguished by 'Best New Reissue.' This is not ideal, and the following analyses ought to be given a certain degree of wiggle room as a result, but I imagine the effect of these lingering reissues is relatively negligible. It is my personal experience that a large proportion of Pitchfork reviews pertain to initial album releases over reissues.

## A brief history of album scores

Pitchfork published its first-ever album review on January 05, 1999. 16 years later, Pitchfork boasts reviews of 16080 total albums from 7502 distinct artists and 3845 music labels. For the following analyses, we consider a smaller subset of these reviews (15845) after removing 235 'Best New Reissue' albums. 

Reviews consist of a brief discussion of an album's musical content (e.g., does the album succeed in breaking new ground, where does it fall short, what are the must-listen-to tracks, etc.) and a numeric score assigned to the album ranging from 0.0 to 10.0 in incremements of 0.1. The higher the score, the higher the perceived quality of the album. Most often, an album's review is conducted by a single member of Pitchfork's staff.

We can numerically summarize Pitchfork's history of album scores.


{% highlight text %}
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    6.40    7.20    6.95    7.80   10.00
{% endhighlight %}

One quarter of all reviews earn scores that fall at or below 6.4 and half of all scores land in a narrow interval from 6.4 to 7.8. The remaining quarter albums are fortunate to earn 7.8 or above. On average, the mean score is 6.95 and the median score is 7.2. A visual depiction paints a much richer picture of scoring history.

![histogram of all scores](https://lh3.googleusercontent.com/YP7wM4_O18Q0mGUlXiS287gjv0QMuP3ktV4t_9T6AdnS8EzHiaz2dqsd-ZmLPE0K2DrTHZqxvC3rhDQ4ugU5dMiFeWr_AKWWn6TQyiKHBFkscMjxTGt_ebcSDPK4hsECJpd07zNMAyyPL-ufzbtas_HlP40RYgIDc8sXkw6tm_HRhw897skQfVbBmFgYnoJmAbVzzSwzrJkauSc8suWZMhLV1iy1z39noVpcvXfEK86hCY7GYLKgsZbKFpjpUbEYWbYzAiyVrY4eEP26lZz6YlQyQDUsIwOd8fd4hV_SjobdopsKHThWPhKSWE1IMs3-HzSAHpJxfqZ7BcEgMYnY5a2uL-ln0n8pjnSQF6S0pthTMkcW_VB1rimSJSjcSpnjdKXKHeF5LwyEDLGAiaBOf3m8alBt2ZxApykDnmDGo5fDyoRR-9tgH_uUpOTsJ7aUsNAaD3dKzUkN9LwaFm95dYhR0PapuBF5hGxD3lYzGVuDcdmYqysoRmSUl2GI-iq6WhN1yCs4t4hWXVr2jVLtFAkEdWHGyfPw_tFrdpAv42KAk03dlYTgVR0lXyGtyvPdphPbf2_vscbxLxjRjg9I5_C5wotO7lHjcjR3T9TVRCcMQHUO8XdxdRI46RhO1AWjgEN0rgBjn8KxteNMMkHlHa6qaTavMPPC4-E=w1102-h661-no)


The five most common scores are 7, 7.8, 7.5, 8, 7.4. There is a clear preference for whole number scores (5.0, 6.0, 7.0, 8.0) as well as half number scores (5.5, 6.5, 7.5). Among highly acclaimed albums, 32 have received a perfect score of 10.0 and 34 have come very close (9.6 to 9.9).

### By month

Do scores depend on the month in which they're reviewed?

![density of scores by month](https://lh3.googleusercontent.com/qRlBugNTWIE_ARg-xvq7yeNB3bfUnSSrrkbNr4lnzSNV4E-xOyGoi-BKSXzULLScdRY8_tFZHMsv9KvTfbTf3GsIBIbBdEOTwiE6C-nvJ37x4RiGBXYUfMZ7hY4vHHYUupAaIYjzX1BuPQzneGa0wvTngWlkwHOZWsyp6TDYcs5X-KwaS2Aa7ZjXeE26wLHGLbV4S3gcs-V5OS-PG4HtLevMQTG_DA9_YwgIMMPe6ZCZLzHdlUEj5XA0OwY1L7tVJgsKLYZJ5BwjuPHdbRQyoAelycySpB73yEaYoxoC149FcKtMNbDJ3F6F3UVCp5Yx-ZfVbml_3yIOSR5HqMA6HRvGs9IbOIxcpQH2MA0Dt8RL2vbtUdw6n0PSDr-yMBgobVKoQOqFcIJX-fyIbNM2HhMNJ04jCZCYpaj-rpqoVT53_Uj4n1tOGWXKmv1g8K-dEk6Jbtm2AbVDkVS9bnI_nFM7JV4kxnddx0nDU7Aa43wIgd1bbCSl3Fag5voqM3GpfYd7GDN7l5nWvu4P686A5ealRj2xjACF_tEICU3dvBSKSTZgn0MoCWuxe7kjzA7S4Q9YLO4oJ9miOhetKgB_qbZmVJs8dgdK6b4i-94-x6gTj7ABq4Da5t8S0I6HwvaNXeSJR7AxuSMjBDZwVdctRJ1bClhQ9F-KkxA=w2000-h1200-no)

It doesn't seem so, there is no discernable difference in score distribution by month. 

### By year

Score distributions may also vary from year to year.

![density of scores by year](https://lh3.googleusercontent.com/mbr3F_SBqVu12MBlGcPuoKuUbT2PHwqc8Dt5bWzffCoiSuHhWm0lD1vaJ1RKtqP0oJ0aIDp03lB1dz3suAnV6NGFncFoKz-IIdl2Fx7IiO8kU6vPExqxmfjmRTnB6sEl_m0st4AyQBavjw1ZZWQ2P0GcEru57mxN6S8VTZol_gGx8kzVQhy5B7Q-w4RpVfzQ8d1cBDC9nTUHtxtfj15-8Wc7vngi1g4XYP3PFse-2m1yJuySqj37s38rfBND6xBuML0NdTgxwbzv76sRdPBYacGFHF-e3FUA7jW7bfnPJybk6qCuuXDkHiHvtLErZW2weM7Qejdg_TMI8GQGP5txtqoBDz4KnaKyF4IiMlUqZSLVXgk8746dhaWkYUJ8_Kd1ZW4ez-kI_59G4FQQplQQR_tHsJixLQROKam-tZ9oJ90BTiSjis-3GpWSX58zLKaK0CiA2fnTxnVqkZBSbEUeq8WkPEB47DBEomGQZ-ruUX41A0Nlh_IKI9jKYCP5o532hdlUgHiWMQ0hRcBN8zrz6mBBW7FKTTK75Ih74UPuZSj2eaJuh3mwYhWdBQGKs5hp41KQE6qCvRR6-E_md5lEGfDFkTgsLpyz_noYsKkBB5dLaaSu9p8-9GedthfjOInUuPqV273Zvt9Km68vp2hil7guJY1q9ymidgk=w2000-h1200-no)

It appears the yearly score distribution has grown over time to closely mimic the overall scoring pattern above. The mildly uncharacteristic score distributions in the earlier years are likely a result of two factors. First, without a historical index of album scores to consult, Pitchfork reviewers were tasked with determining their own scoring rubrics. Second, Pitchfork reviewed fewer albums during its formative years so a higher variability in scores is to be expected. 

![number of albums reviewed per year](https://lh3.googleusercontent.com/dxGwVQn3hYrbvvH8w1bEgtWgZImkC_KXHcIKDRMbBFtzRrOWg6sxPySL00mhFsW3NA9mSIC-nEtNvuahB01yetNhgFUUl56YD_HxyMrrFtBwx0nC9clKWZYIzWdGO5encvf27CYe_0i9syhfugduYrHefFMq3dpPbxsnQsv306yNE05l4n5NnVI0IYCX3_1mVLFdnpJfGyMAhxilHeDkf7hmEKusIJst38odWi1Q0fdMkgLMgUJFSXISKuICKFAAcDOi4ErNnqNYt_OrijMPWzZgdR7kCCpztX00G5_L7byxz4vOIHhepIa8-n9-RyFAihpZOq3-HUZSGp8fz8haB2VDnUbu_hTEbL40TQF96RL9Nf9_myN-kq1Auy5HxjFnhyqiCETKTDlPIx9AGVPpvtdV2-o44jVkNZpHNEeE3PE8FcmK1FBeHS5Qn1exZ3Z05817Wvl5PlSunk6wOiN8s0-T41qUaNSnm8zPVTZfu-lFH9ZlDOBNpxaz2ibV8VEhEjxt0h0qwPKtyiCeo8YOeN6HVSq4TJ-4J8LcuGwt4a_mnioGC2AXJSQ3O0akoQI2YKaiJbidUdqFNZDK53_ayZta68My5R_6KEMnD7ut29hIexnoXaeuRBi4RXBxbeXIkPyDWctmGzRPoJyZlmLxXDnyAwqtdOXyh8I=w2000-h1200-no)

Indeed, it appears Pitchfork did not reach a visible "steady state" of about 1100 - 1200 reviews per year until around 2004/2005. Note the drop in total reviews from 2008 to 2009 is a direct result of our eliminating 'Best New Reissue' albums from the data set. While Pitchfork has been reviewing reissues for many years, the 'Best New Reissue' designation was not introduced until 2009.

### By 'Best New Music'



In addition to the numeric score attributed to every album, Pitchfork reviewers can distinguish exceptional new releases by labeling them as 'Best New Music' (BNM). Since the introduction of BNM in 2003, just [539 new releases of 13825 total albums](http://pitchfork.com/reviews/best/albums/) (3.9%) have been deemed worthy of the distinction. Despite its regular use and the clear honor associated with receiving it, the BNM accolade was not formally defined until 2012 when Pitchfork [responded to a reader's question]((http://pitchfork.com/features/inbox/8780-welcome-to-inbox/)) of "How does Pitchfork's 'Best New Music' system work?":

> The truth of it is breathtakingly simple: Editors choose Best New Music albums based on the records that we think are the cream of the crop. These are excellent records that we feel transcend their scene and genre. When an album gets Best New Music, we think there's a very good chance that someone who doesn't generally follow this specific sphere of music will find a lot to enjoy in it.

Calling this definition "breathtakingly simple" is a bit of a misnomer considering its high subjectivity. Regardless, what kind of scores can we expect from BNM albums?

![histogram of scores for best new music](https://lh3.googleusercontent.com/zPaRK5n14jADwn4romT7G0w_sny9Jr1F9-MqTed_nU34Wf0_UPjlUeTWL1YS_f7hyI0vMk35GC9ZxT4NuehhnVbNthankE4CBqsayttvEtxvTUTLhhg792Ut5KrdlnnrizSSIRq1iRHSkoJk12voz-6zaW00ubyJy7CY_Z1oFbfeiLLj8UW4l_7M03vyP3rSO3n0xR3pupffShNLjAUZOhPxCJjXfGT3F0RTNk5xcNX-_pTQcMUoQSI0yCSM8KJ3wkgBOb-r_nTeZjiMAQiRta7sPIc91ungkQ6SWYquaf9nKXIf_Lkck_jdjZqWXiP8wV5_J4PoM504srpUcwcKYBPxWwMBzWO08m0dgK8G9ctyhlQSaZ2Dcii-LRh84VlAA2SFRFmkyukCnp5QjwyY0BS08oKlTEZ5Bb7aoo05ve5Fw0ex14SRqRZJvV4E34hiWe5F2eBLPCQwVT3ehuzvb9_h3hQmbqUXl_HNS-S5eOFYyp43aWJFiKMXXDOob35Q_CtQi8MhYKa4sbtb1UwDR-F1yd70y0-ZzNyrLBv4jlgMSCqkk6fDzSsehm3Ur3UxICbo6IONiqo1llX6e95Zz-j_k5voCCEPPMxuUurPNtlPApGC4OLQ4VOKC-gAT9DqNo0Eu5z4oCydJBgK69mqCNnaS49BFmrWbvA=w2000-h1200-no)

BNM album scores average a mean score of 8.619666, median of 8.5, with standard deviation 0.3236971. The five most common BNM scores are 8.5, 8.4, 8.3, 8.6, 8.7. Only 2 BNM albums have scored below 8.0.

## The reviewer effect

My primary interest in this analysis is the effect of reviewer on album score. Of course, without performing a controlled experiment I cannot safely conclude that "reviewer x has effect y" on the score of any given album. I am simply looking to characterize scoring patterns across a wide range of possible reviewers on the Pitchfork staff.

There are 319 total reviewers, a term I use loosely here because a very small handful of these reviews are collaborations among two or more reviewers. How many reviews have each of these reviewers contributed?


{% highlight text %}
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1.00    3.00   13.00   53.53   48.00  805.00
{% endhighlight %}

Half of all reviewers have contributed 13 or fewer reviews while the 25% most prolific reviewers have contributed 48 or more reviews. There are 74 reviewers with at least 50 reviews and 45 reviewers with at least 100 reviews. Leading the pack is Joe Tangari with a whopping 805 reviews.  Go Joe!

We must ensure our analysis is not adversely influenced by reviewers for which we have insufficient data. Thus, for the sake of the following analyses, we will restrict our attention to only those reviewers with 100 or more reviews. These 45 reviewers account for a staggering 70.21% of all reviews on Pitchfork.  

How does score distribution vary from reviewer to reviewer?

![scores by reviewer](https://lh3.googleusercontent.com/uRA6-JT83RQaAqHmYjO92eJ1l4tnkNFuac-PF27IIUcy1bZMtuLy7-_V99u4tW7wViv45d3teOkwguqgkwQArG6eOBSHLJjeuFqPPSQpvwjKmo2Zaqkl1_mJHeiVK4TTEZpbGfaLYfPZyq1nJx0gOCEz5iQySafLEf75pSLzu9NE3ggAdU2wo9o85WApFLunyjHx6SATxYqc4D8TOvaet2KjLmoOxMM0fsplvVFvXGcp7o8uEsBTi0k8fStEZQZvfp20Fc8_N8Iz0AnFAOiYAIB3jIKHes-xcJ6JKSN07lj_zl1KFRia9h-pAu9s6zuSAVJ-w25KLqDEvPnw06i1ONVgpHnqzV5klCfzn8H_9DRV-ai8mDk6jgbpSMZ07MiVH8hkRsGY_qP20mTQW3ttQOHhfKC-_AqxavpejpK2FRkShANHaYhXy3xgEPqTIpv3VQqdLMEFphHHyCgSONHBmzJjM0z5Gl-s2sb2kOiqpkdpgF0f31XNheHmWdhcZWvAroE3vEQa7n9PkbqC3jo6YiK23s6XfsSWARrfxKtUEw2-z2NXZmZX-Y_UY1z70ToKJr9fIbj5FB93z1_FN3V6sKmox6XNNBMJlt-m1UFiQRTijLCEKH4rpdBv6hniJsqz8MnxOMUOyrlKNcysNxc5_ljAn3Lh6r4nEn8=w1102-h661-no)

Yikes! There is a surprising lack of continuity in score distribution from reviewer to reviewer. Some reviewers assign scores very frequently around 7.0, 7.5, or 8.0. Other reviewers are much more variable in their scores. We even identify a handful of bimodal distributions in the bunch, suggesting some reviewers have go-to scores to rate _good_ and _bad_ albums. 

### By 8.0+

Pitchfork defines high-scoring albums as those receiving scores of 8.0 or greater (8.0+) by their respective reviewer. [High-scoring albums get special attention](http://pitchfork.com/best/high-scoring-albums/), so a score of 8.0 is much more valuable than a 7.9. We therefore investigate differences in reviewer preference for assigning high scores. For each reviewer with at least 100 published reviews, we plot their proportion of 8.0+ album scores against their approximate number of album reviews published per year, a measure of their activity as a reviewer.

![proportion of 8.0+ scores versus reviews per year by reviewer](https://lh3.googleusercontent.com/UyCl5jBNaDChFVdQey12s_OsJajIMbHB7rzDffyCCwaWzpC9GSM82lMQhCt_dFUxxMIgoMY-6Z9VklbbcCXYj1P8qOIYao0lNrWUV_fAhQcSXmlMiXbrZp88Mu38nbPCs6Jj8_DsAeynfVzT2fK6vxw_Z-onD1Uu4i1qR06XSn-_7_tnhofCsOss-K2r6HIlwq6WIqbQT3pKhEyOiWzKNzYjzAVhCTFULN984h3-6Dj5l-LgPjcWiHoD0KvJSCG0Rq4fo0HrYISXy1SMuXJhq0bysxI82mZnDi7XppUcuUt0VYu6iRFX-5pSey2-86_WPnvmo9JI_oOlnqaf50F_ULX6JfaWf0JVVmaPWCLzM3tmZFwVUClxa4egTYYKxtLmHPqK3OMEKEQGlz2NGMJ1LpVWm5cbPBg425elsFIlc1MTAkx3FuTnpc8pfVqAOrs14vDj7wTun-UTv4oGWY4wnLhFcAk7_LmIyMkHFdfApzMY2b3Eq4tl7Q6Sf6ERzqPv2v7sx0RpB6OoOPEI7gZh_S0PlYngah_xhB0Yaj-7bmzv8uYFqZzVI4_b1XX-2amiMDjZ6NU8XVnqYVTvwuuGfTVbvzXBiNF2fGZ6ZRnlEzHLAV8alKrcfk5pJtQKspMl4Ey4ewQfjwPJyf8D8_5BAqvyZ3bn0LzNs5k=w2000-h1200-no)

Ian Cohen wins the award for most active reviewer with over 80 reviews per year on average, 13.7% of which regularly receive an 8.0+. After Cohen, the next most active reviewers are Stephen M. Deusner (with 17.2% of his album reviews earning 8.0+), Sam Ubl (21.7%), and Pitchfork's most prolific reviewer Joe Tangari (24.7%).

Some reviewers are very conservative with assigning high scores, such as Nick Neyland (5.1%), Joshua Love (3.8), and Zach Kelly (2.7%). Adam Moerder is notoriously difficult to please; a paltry 1.9% of albums he has reviewed have earned an 8.0+ despite his reviewing an average of 45.41 albums per year.

On the liberal end of the spectrum, Dominique Leone publishes 44.28 reviews per year, a rate very close to Moerder's, but 44.1% of albums he reviews earn a prestigious 8.0+. Scott Plagenhoef is similarly liberal at 43.1%, though he is not quite so active. Other liberal reviewers include Matt LeMay (38%), Mark Richardson (36.8%), and Brandon Stosuy (36.7%).

### By 'Best New Music'

Now, high scores are great, but [BNM albums get _extra_ special attention](http://pitchfork.com/reviews/best/albums/), particularly from rabid music aficionados like myself. Only albums scored 8.0+ are considered for BNM by their reviewer (with very few exceptions). But how drastically do reviewers differ in awarding their 8.0+ albums a head-turning 'Best New Music' accolade? Because BNM was not introduced by Pitchfork until 2003, we restrict ourselves to the 13825 album reviews published in 2003 or later.

![proportion of best new music versus reviews per year by reviewer](https://lh3.googleusercontent.com/1cxMlnTrw0vFbOK5IpVvRl5pQYNc3i9jFe93ywpl4VhihD32Lb_L03dGuc3N5buEoCavsdxS3yK1xzADTcsuPXId3vl-ABm4rDAP-lWP1TLpFmcesJd4ENWhL3L6MPjq6pzPCuJD9wxwoHQP4jiAYwnzjNDnxBtdAgHkY0hYecjErG5cq92gRTKi-BUpJzNsVYwEYNnFrkwIwOhdyhUo8Y8huj_YBE3a5Jz7IAal3fCealtUmpWqQZn1cHHAggT5mUoEO41KurPUkwglufLhCag9Nx3zerRX-SrYDsyes6Y7ZHYUORgM7Ugiw-MqTJCj9-zPPWBs5LHiNE58Ej9e94mrQosstSsiaqJOhRjsiPbLfegqBrMxJ7dqx1NEnduxOSGt9fRMBPv-FmRLayH2pEP22tCSfuxg-eysR2DEmNbdKqcIST-jZ0NJpAP7k4gBAi0vanQBWo5eFg3hNgb-ECtxhW0_WH3Aa5rGuMQm2oOIpjNgJvP5GUWA8dIVm1SUN3S5749LLmodgMSPbDWu-VoM6kc3gSXjy4MIl0rXg6nahHlglWpFfh9itLFK0VS_-z64fExPZXZ4pwpY96BVR4pXja3h9DVWTxVplcQgGITbK6ufi4tgVOPj5lXXZuKxOMX3JV-nm-Zu_0wmHbK3eqhZY5-Pz7PmjDg=w2000-h1200-no)

Ian Cohen, Pitchfork's most active reviewer, awards BNM to a high 57% of his 8.0+ reviews. The other active reviewers are considerably less generous: Stephen M. Deusner (11%), Sam Ubl (18.2%) and Joe Tangari at a miniscule 3.5%.

Reviewers noted for commonly assigning high scores tend to be relatively moderate with further awarding BNM to their 8.0+ albums. This includes Mark Richardson (31.9%), Brandon Stosuy (27.4%), and Scott Plagenhoef (26%). A notable exception is Dominique Leone. Despite having the highest proportion of 8.0+ reviews, he awards BNM to a rather low 11.1% of these albums.

By comparison, some reviewers can't seem to get enough BNM! Lindsay Zoladz awards BNM to a staggering 69.6% of her 8.0+ scores. Ryan Dumbai has a slightly lower BNM percentage (59.1%) but he is also more likely to assign a high score to the albums he reviews. Also of note is Larry Fitzmaurice at 47.6%. 

Conservative reviewers, those reluctant to assign high scores to their album reviews, range from drastically low to moderate in their use of BNM. Higher variability is to be expected here as the strict scoring habits of these reviewers allow very few albums to be eligible for BNM. That being said, Nick Neyland awards BNM to 30% of his 8.0+ album reviews, Zach Kelly (25%) and Adam Moerder (25%). In his history of 106 published album reviews, Joshua Love has never once labeled an album as BNM. Love is not alone; Joshua Klein and David Raposa have also never awarded BNM in their respective 217 and 155 published reviews.

### By date published

Perhaps the best way to visualize reviewer behavior is to plot a reviewer's scoring history over time. Each plot includes the following information:



#### Scores over time

A line graph in grey illustrates a reviewer's scoring history from their first published Pitchfork album review to their most recent. We see a high variability in scores assigned by Pitchfork's most active reviewers, owing presumably to the high frequency of albums that hit their desk for review.

![ian cohen](https://lh3.googleusercontent.com/LVbriZAV4fPh9xUBc6Fe6hqAmN3IWXqUB4km0oaAHcJarw_mCkj86EmDZDviFAgz3tLpG_tnru1O_HDYBOTPjdhoIZ5Zke8hXImVm4myTkL3DKI7VCLXgf0JaLE-k5dbO5YL3BvGY56EMycT5B9HgsBqHfw_uobMBaLis0IalDxtJBxlxB9MT1ZCWbeWHygtR5Ys-hTm0Q4XZ7LVeuHjARKT7RijdRSiq45H9HY5-ebC2L69lQXM3k0TxFxh90nlnpr_bMA4DezeJGKBJs3mNIF7u_4-an_0WWuDTYpQkBE9cIVBXGnJGMv45qq6wopkbXZSPtD7HJfULambsn09Roc1crmp9BbCtkZkFjlAEQwlwDMsgsiFGW9AMsn5w7PcDdBa2gxp_-G7xLHBRs67nPKuU89yQsfUWt2nwxYg8neaUgiwiBLQxLgnNRyIKMUUhzreOlhmFO_BtPCRNbfoclNdgvVkC-dIJPhzAPXqJLeiyiMst7yk8PxSIjEU1-7KmtPiFgbWeJYP_8Osc2l_Y363F2G9bEcsdx45CeFJ4J9CZDHL0Jis8FlJh0Si7OU48K7Vkj8ELaCFcLhkwH5S7j1LezX28JaRMCbPHE63SWSBase0jTULfZoynAIcFQFzUe3SotQxtLvLIsuD_a5ljW7C-k8QigVyBpE=w2000-h1200-no)

![joe tangari](https://lh3.googleusercontent.com/nvbpAt2bFXANa8HkgOuUUbiug6PoZouVpZ8J3cvZPBKZiA9-jwzK12yU5huIRMqmmrRx3Rnogz9UVH8FOXvNXVEaeGHvFuGuGudY_fufeg95L1Qqrs265lb0NX7-V6os1QzD0qr5hLxa1PT5D7SmHP4w50-YRmJ9yD6eHFi4C8qFlHrIazD7TgXKQ64ncWdX6P0B9OtI5wMX1bPFRhlrqQP0RzLK0u28KLnebVMI66d2BRZs3f4UkgYb5HCE9AOionNa_32VRwnR6PFsQFUF29VMNrhVRbEDaFgYSQRRqUUNo5xLNs5Z-FLLe1bvUSryJAe6G2kSydSJ-Yw6-hHs72UNuk2IQtGUDOFjf0ACaJx2zg3E10HesTq9p5RXgSM7Cimin1g_4gM3e1qTQa3BMUa972K7vhb6iFtQx95xNrNqEuJMWZNlJ7vION-1h1uXb0C9vHhXnNbgJvxIo0nN2tr4tikoPgmdGYdXv0G4TyIls2_jMIJXJPwIFNtbM3as1FFE-pMc1W97xE5Rt2KtEJxTJ-9laZW7vR1ce8mQPO3litdt_FJrvGYXV-5qT-6_aROWCExp5_uftnvKs-7VrXJ90skq0NKuz90z3o91q4pgkjmFT3cfsqV6NMrdQoIDVGYGYR0BIhPLRPq2NgC98rxHqGOspjIT85U=w2000-h1200-no)

![stephen deusner](https://lh3.googleusercontent.com/bFxziQi9LNkrOV7av3AvTof1z3U7rJSdazHL-Q4-T5k2jXPplqtnlVsNLv_djDZw8aLvzzVfQh5wcgQvLXdigkbyNt5H70ZBopA9lHzSeJTTlFRrkGi-B7y-vcgdbVNEZBovFpKXQkUfmmjmLAkDANHOTnPx0YFdsQ-YB8JhkvIGoTNOD6xRRliN0ufrX9ziEZAzJCkP8r9Qwa_Krv0ATWavdcdaKKk6P9rEYHqCssjYiRQmCAVdgNee9X3YvegIzomlZg8dklD7av8LRZEo2sHzRAk-uwl9lIfrF4r_Uq2eszmXzqyvNPKoynHPj_ep3Ec3aW9PuH8eKmDKVmk0m7mRz-qe9EBG2-bI54J6Gsga75qr56ztOn5C9juqSCIfriJJYWom_uf0heMjCaudoZ6r_xOZSaCTWzdxskGom895YqjHd2kG_bC5O4aahRs1YjqUohLNvqdZyiSFmaNTNJwxwr0XAx7ppJvSbnk98fDjz241ecGZwaJKq8Xd-lpWr6YiWqnXALDuzExFLC0foKMqSfBpRWHN10LEhAz_xQeW60Jv6iURz2dtmiEETLSGSmdf9x6qe5ugtzYHxKzIyWn34tq7MlhGnDKMvJd1mIQKvZD1zAUMDOtoqBipq6zBit5K8gKCHzooQKbDBlGZaJjQ7DP4oz8Xu58=w1102-h661-no) 


This stands in stark contrast to reviewers like Nick Neyland, Zach Kelly, and Marc Masters who, while less active, show very low variance in the scores they assign.

![nick neyland](https://lh3.googleusercontent.com/Dn191x6PXsWdo1TB4ihf61u3pVAPF-HJNdspfrQz32f-M5JT9bs4mMMaANHYMamUGFm5i4M0KSUoEaHY006LDAAkVFHh5_muUoIHWxKJDFmhexgh0SwNfAvSTJKgnNb-_ufqOBYO3nRH5UsXVUu-WHtCmdEB1QF7GOBzyUsTHoJ9cJ9UuhXgfiBBVsRT6j8kMhvOWo3mAGNYLhW_luEp7h4i8E9DMu0QjT0wnI_GWjhq8qmPBELpLSHy9ccqETb63TT3UHjXjtI9eXI84TpFw-PBPOA86EIHAUO7u_QOg1L_moPTaBvdDT3u3aPKzawHIiGYvJAdJj7YbAByGMvYD6R8MT1nhiQA3booZHhjbe0FibQa_h8KRGwS6alfrkS7Bao5-WCNzhC3a9hdlZrjsE_EsBxeimT4EMgubHn84T0KtzW2g7x-ruOzr5DCfryI47rppBxeDB_mvBa0DKetIiOMnofkvaGJPAXFNGHw64Z5Z9kcHXqtTBRJKxg-6h85jhKdkz0XWOxdXOJZEMR8kvzuBXRzP5LUSCdV-iCtreQB1Djm4HBzDDkimzcB3iTOZgjh08MbrovZOuRELn-uQBc0k2bStMp9iRGhSuhWylcNIoGimXdAjNKo6VjD5Bl5ZPocB-7ydvpUH1feRzaw5JbklplKFsY6PRI=w1102-h661-no)

![zach kelly](https://lh3.googleusercontent.com/U18dDE5HPWusFH6YWtIeH9V0DkTZ6PIJuhxvhXGy3uy7SpaRkox7HMY1g25Eo9tW4Aq_mCk4JS0_XVfZ1XUDmXe87CQO8G6o9ZTHP5iJYXr5PaJr5RA-Ut39iHBHEy9yooAafTgb1C26hIQA7Npuv2VgdhkI9F-dE7nT99NSH0XXxZBNQ8JHEVg1SSXR_l3pAno3thkToKSSmE6BJc6LHW7r6WFM3ngYI_vgDaPuE8IPzHdnoZASjUaq9etsN_3ykP7Z60jC_SpvrrYG7RCdb89ngAWxbG7HkZedQYHVwJFA1p0pTpi9md_wM5sPsJn4Dkx_lli2aXtOJPE9YMnudSQEFwKwnhr_IdqET9KfZAU6HOMim0xRGmHdWAgjTr-mP4pEh_TsHsz5BI8jSp2od-LxL_68g4IJpYk8RhVUFtf_-ELYpd4TqAiO2Jiaqnil2QMRrw2Vke6-gcbGQVkd-b7K4P6z9ZsyupqJ-dZ1pGvd8kfJ95poteu249AiO9KCmpPHuWWlz-eXod8XILnydCnjLto47eChMMAUt1fiZlOpZw8SRV05fZBQve6Gax4qb4awJRAWmQ1ROuBGOcOOPoT3zdwjZJ1ih2Xe8s-euneV-5Ffsd8HdUF2RKCi1WIZ4E3Fb2SA-NE5vIrPj_vLlpTRduvBUkZtNOY=w1102-h661-no)

![marc masters](https://lh3.googleusercontent.com/QIcxJzbguk8C4PIxpK5i7itJFLinTeMxqfMnIAA_NxtjMHw09FViyKJojSAnrw3cXzFdVwBTJeGnggnWfCA7tk5dDZV7VnRC99H4VPpzQYWKL_pbbUYegxmB-1sJIKHkvYrtWTk7OCf0iqVpRkRftthPeukDOQoJasPwtI00pxVFZtK7CusPWWeFRiEo8XB8_bsLqXcG7hnyy1zaj5OvD8-O5y_h5z2ue4h0hGc7YYuvEfBGUyQ96KLS51sPUnYVhdCKj5-tNF4i2v0Wmtin9pcwRUO_yrrdkF_oqUw-j_O1QXHA6yqoTLdpVzL2FCntfbF4hTX1zEmR0TyLNmX2TCumNNUf5-pk86VfeSIBr7L0sIuPlwcMulMp6nuYHvi28Bgrr-yoqI3fAXM8RMk5L69_yKcCTT8vVBXvzGR72QF-70Odn8CBWJ_pjUF-nYSj8eBOCkRewNEqnoziEki74c4Ge3XZN00bkrvk5hs6sujcwgMDuNd29wzvdouGD7iftYVI-sEAnEF_uDfD8zGQrM0dyzRqW49w2fyIsgWNL7ZWis3e5sIVBKsGI_GZES_3axmRttgWH1HxpGH5nr62qF6qHQt8tXitciU7KIRt3w9w6m_sb40BAoJlwAk4D0Rq7qaGR2-ts91xcQj_Dk6EqOtCqSFzVBRm8Wo=w1102-h661-no)

#### Average score over time

A [LOESS smoother](http://en.wikipedia.org/wiki/Local_regression) in blue approximates a reviewer's average score over time. Most reviewers consistently average around 7.0 or 6.5. Reviewers identified as liberal with high scores show averages that hover closer to 8.0.

![scott plagenhoef](https://lh3.googleusercontent.com/uasZuReQhbcxqgVbRlHxtER6QXiz_oObBse5ZhjjlIjkquZwQsfbEvybhW8VDugyOCjKBJdzI1Cv6-SHcLG7mbTwgeOBOFtYW3lCPLTH15MuIgrmm8HwaGwWcDTTRNXPzZ5nKJXSqaBmiErGbHlChOTS7tnFaGnXrVHlx6LDzi8sGbAv4VT-4gjw7cB97QK12wNo7brDF3fDWPXeICAGnmBihmz90F9Cm_JkilT_hGz4f-rONnrGdwPkOTUZMghsXayg9Si4RaYwdGAXX_3-cN4kAU9jk4o08TJ4_s19bVB4ACF2LXj4b1kGqLJ179nuwp4YEm5Lihl6YW7H6d42Cw_ZKk0eYsoL5D2A1gFow4BhUkMMQBoGdYzlwC1BTjNI3eGr0tq3yhQB1YmULsmmRz5ZxYs9uzMWWyLXQV1Wc0yBa6r6SgLS-f8Ub0HA1CUXD3ysfSUY8KDr0ZAWDE_T_lwgydCHvH0fgEmwtkcH3gWaSqiGtcPvuHV7boJxgz-bLh7l_YtnsT9dMkuBLVsjo7fXj4bVOgeRsVhcy2XOnKW1WRrVBltjY6XszGItV9PZEnRnm1MKZ7lzaL3jM949RuJtbkjTZ6kUVn0G2EkoegvQ5CVYz6Is29UQtgriF9mNq5-Jn-fC-h3BMocUA6gaWy-MHLkYeZne7Bo=w1102-h661-no)

![dominique leone](https://lh3.googleusercontent.com/UEOsD9Fh4h70SBFujkULfZe5GMeCSNJu1jvD1zX6akyen9AGnVlEfb4XMpO-IOotEwLidK86Lzjp0BkcUCB-tuH3lJBFVdBwIHvkjyi_coXPLLfGuKllbpfXjkVywXfonocqkSCafzs15BGtbjoU1PWUmVgjZCxYztAy2dauk7NsfZZLFVzfoAazmFd3zBN2Z-Io72vZy_EWMn9Uc-sReonsRVuiaroUBN5rmQuvuhY-k8FQiCOkmZkcmF9j8TG0dCJPXDoztCxWZFGYiQ4d_MrBk0yoO10cAi5UHvwsTPCw7oFhKA1Bnidf1SH7bSdnP5G1njLU-1IAcV66YpHN37FuqVPmtoO4LoGMXsaUwXOSjqKg-Q2dhszXkxF9SWZdprypPxCmm6DMRwPrEbQ6-n1PKNZm9cqwGybc2R6ya7TfRZkwDjogFYC6_kbZ3i1KsDMWcwD0zhYHqNRD-xdOedpfANzpv9CFOLf3qoft6DjiDSWia41aS7kTiX-2FDLmjI2Fh5dhU35IRe6sR7t0ZfytG6QFYwN4YUZc3PdFjEkcC8Z8EEx2W6wo-TLPZOEbgLML7buLTAZkpvPUqsavIc3ULnNSzZt7uBm3TACa-kXUVecZyTcjY6Re0rEg70xykDMPHLQiqDOmdDWe8cYZP-v50rNdsg53eBU=w1102-h661-no)

Conservative reviewers, including Ian Cohen above, attain averages as low as 6.0

![adam moerder](https://lh3.googleusercontent.com/9A_TDsEA-58OLypvG37mosa93BtAE0sPufsyOJWCqphC64ZTHLdHYhJc7Y3P2IMkLYwL5oXNLEkB-neeqR-NN4HcsM-NDChEvCrOWG6zie979wSABLDmIFLSwNY_Ja6KdhjergtU_fzvIBIXO9hukCbY1p_CokQYEqI4Wm5-YyO8-4Jx1yJ0DkQnH3p-BN9W0X6TW1w7PG_k_-y1TUgqk3URcwIEHlO5xkrAWRtHJiZnmUX62_fkcSfcyPflHRBvfAqAXoomdBsrV0yaSiYqKNJ6mCMPR_qt0tuOP054oKrH0oB2OqozicJOId-BiXWLjLzrMenKVGDHstMP82GyVNP_53w_YX4B_zRz1MfmQEshXgXfoWD24S8mltMZUxxYxk7cF4A4Vhd9nMo_FdQl75HMs5ZDSgfvm9iX3rNYaHCXfk-WsarKn1HOdxHzk4FwJ4o0wq-M4fC3EogMQzo3OUhI9WopD00rlPksHrFA3VHRRMKh-QxDTBkIHFo23cmMD1S37FQMnti1N0sVHUW02txgJN-qTQsulmXSDza-mlMQUBTE6Zaah1a1bvzoX9rHsv8tpw6mJei0u5cR9JpLVI5jlqRXmHInqxaN-q65aDfnp1CQ213kAYlhJj0dgnG_JvonF0GtmZ0hKcQ9dSpcr8X8XBsejzoxNzk=w1102-h661-no)

![joshua love](https://lh3.googleusercontent.com/o-b7H0EWEYhp9FukCsYZKsZucr19RkBa9G_cHnC4eBH6mmgl9ysYxFnUnIILqIe_ueTC7sDXvcBqesxe6I-Zkx-dEqMvBx5YI2XVU0WwIAS8pYslXJ6ZgRjmLIl09ijia2gjTEGO7Yi3wXfj50pUE8hYzNhA1hKv2yhLbp0CiRVHGbH19fTO7vkn-Im5s07wxA5idE3KidmWtl58TyB8EMZ01x6ifHJcELofy7Kz_kNPMENptui-PeBnsw2rF4ygSGJ_-dwAdLtojO6RofRPdmsK3gi2gQVwSvhcrNOQoEsw4egv59r2eSvcHiVOi1aBtFM3DfQWul7iKcllrlJYuVrsAJ_9360qePw3_3G-umrKheLqlA1mNAQpCpXFX-s_BU27PQ_mMi_gI7SkW5kpFeQbnrWK6HR6zowqFcktgLF60QfvsrutRopmrML_whOBOziZy9BkLAFp7lsuGHyxEBsqdekDzkOxbz9Ky7mH0lbubMKOG-O0LGgxWkMKdmlVxRlxYQiErmvW38M32618zaD1kbFIWJL6-5MhFE_pmTAnGwmUsEHEyk8dr0pFJ_qU14gSMjyx5BlgKcQhIozFLKgkLEp2KQNk9n6TpofvlYDcoAULylMR8upr6q96o7PDDCARUkvAH2np3l4fHGv65nOshwzSRDTFM1c=w2000-h1200-no)

![david raposa](https://lh3.googleusercontent.com/1y3Pnbi2CGejAq0G01tfo5BDhlg9kN-lrPMBfowz-ZYsoNLrYbbLk3PcQjENGztIoGNVmoaWFlybJKBuEVirmcxdrbIgTQVJUH1aPZtglLh8-v3xpezMenbNusU5pNlmm76UwiLwNFOlyXpXdGCuoTyOMmSEDP9Pdgiwug7De6xJG7W1Vquu-fvVmF13I_zfjdCKpLdIKZwOa58bYZWwYe7smqTxRyCY3E60HhhIQN1TSHDO6lA7V3TKOyr9agcj-gJqhKzYjRY6FIYZuUc_OXfXW48vI-QVAL14pvRr0z2N8430d1kTGOOcM1KRO2YcCAyU_230n0dw8yYV_DYIjhVg3MAIZEhyizZ_HN3dcJF8zoqtn26-jDipHS_s_hQCHF2h_kryUmt5J4FRchexzhbg_E_roMTIyXS1vsrgexDYcB06pEmz7tyofSCerYPkjNXCNRR-5w_VwQEqLWgruuG3urRDvhGebFGs0JfANAKf8aPI141I8r5vei-1xorRGJNVHAzXQLgfZpOXBv8envfiJ3nF8f6C7a52CPkbZX7hV0Mc1xggm2UD350HLUGYzhLkli6gpV1gt3PotwcBdOmrEyTbtlHBnleEqkV0iySTHWpMmKqFJ_EDQrD7IlDwrk_zPCmMmNEnn1LWCJ3b2g5BJI2c57qn8AI=w1102-h661-no)


#### 8.0+ and 'Best New Music'

Colored points identify album reviews earning 8.0+ (purple) and BNM (pink), an accolade that did not exist prior to 2003. A vertical dotted line (pink) in some plots marks BNM's first recorded use on January 15, 2003.  Some reviewers exhibit a strong tendency to assign high scores to the albums they review.

![mark richardson](https://lh3.googleusercontent.com/qB6dWEu7D9Y46h4DhVRiBzn8ltztVJvtFQiCUBon2Vcitd_a1A7v48jP7SVhyRYyC7-6YL9zaVoTGEk-YMpFiKGkV4bcraJuvLK7UXdPWEVRN2DZX4z9Ao4YJFNsV6mg_nuEWdcNFd2A7AwiWeVkMoETx9yMOtWt34P1DOdlS5TDQ1i7rW3FPBKP1RdPYc_YV_HVonVqgZ9AYPT5mNHgejdsa5OAuWoZ8N6xHLEwVNIfCglghS4RPPLnZpZ3SZz0s-ulL-8iU2HO10zrPAWeWgwkwtQV7fUyJNrdeAs4npN68p66997Qf0B12er_hcwZENTyZq8nS1MFe4ambNXfii5E72lKk62mbGCett8CITEHgNSw-LyFqELstOPFJCex2vustQKLyyJ8R8Lmy7xfyhvr7aB6VRg_jcqfM7VyPLnofJ2RU_AYb_wynI2sy6YUZtkRqm9ud30smt_YsrLtbSFHIWxKTBqDwDkHIKAbPs3RlFff1RiTgZfNAmz1zWEEhAMwZgPgF8zGkQMXgGUf82ZI4kRiuY5YkjqKuyi54Pd9tgd0WkxlXyYsXczrdd1op3Y9GRcgRphQB9vOWIByNcIGE0LAd2dPG-8dSQVQXZXPA3OKK2c7OGLzBQY-EpKoUFPItIYgqPKl58NEQq76V750yKKdmGyxaOU=w1102-h661-no)

![brandon stosuy](https://lh3.googleusercontent.com/MyGk29UTFgzVvrYQWV6TUKxTyyg4yJq0TSGCHahBKtk1wcn1TXk8WgQH8oeWbm8ONHm6lHGlt2e7wIXhHR1pWejoU7LvUUrqtGq1vlLPW3K7_4Fp3ftY1U5ckInIRDdjG63Q05BYfV4BbFz_SQiO3GwbrKBYiWq1OSEVSXjLdkjfS-3OetnRTrvBarFw11sw8O6HfVuB761J60FQ9QnG-4fOJ6FKX_kVla6D6NwvzkBkuFTRdUxweSoJJ3l_Yizf-2zT3kVWXP-PcNd-tAc1vLV2TNuy8FuItSE4o2eNRq2VzoRy_Y2_g39nJ6nP6i3W5MDYWEYab32aRUUdfDJ_GMZ6r0IBupxo5dRjfWRva_NYEZdGaKNg525jXacJgR1PQSPjn10Zff0o1orhb1-W4bW3WY1x4z28cV40jKPA0c-moHuSGcdE-SS4PtJeSVG5fZTfRKW-zsLPVgPTUKdv-dRXsDdaWvwSjCs8ntoZ3UWHJr19--xgqj-VxpEQUOt3B0_WQtJX6reFf2y4oTTwWW34Vc3_AlDeUIye74nnp-DW0RcTUHV_6GNu2X-P_MJ7l5DXo-LW_OwIoMqjRnLoytI9M0094wplPLNj7vMkF003Eljn-ngjlHLPbh1Ew3_I7cYyzyIddKOeuvs_mQK5rwzX4P2E25Q-P3s=w2000-h1200-no)

Meanwhile, other reviewers are slightly more reluctant to assign high scores, but when they do they often further distinguish the album as BNM.

![ryan dombal](https://lh3.googleusercontent.com/PAPLfACjWgESw658fkXqKB-QGhuaM9axiBjYvS1azxvaF_pKzJVistCPUiwB3umckCm_r_SaGS3HEwJlhsMDS6t2BsS7Amh7iC2j04RkWL0cjnyRNF4CXXkXetSCQ3f7iWb90nfgesISiCtY-4CnoR4kuqgxb8BtfXbu9E8OKJVzvQT0E7OweowBjtMCEBb6Qhnbl9y0dek-wjCesP23q0vG7UoXFBVsj7yzUTSDpnkQNL--01SKTLdKR-vVZAU2Gxtvz9ciITTZH2rdHIttl71xCVCgqIZy1uDRa6xBRcewVZvFrImehbIRnIwk16HMAeApQd5l4RCEmDpAyS3gIC5Jckf1y8QYWYi4pzmY1L-M05BSotNkvMh_pK64Ufn6IMQ_0aCGXSpdZHrO5c8VA6XG9tGIkpVDaS4qiQiiHzILHNFTjg8NhLWzbqHMTAyDm5GOq0ONK3zHXdJv6lrhHzsKgv50YAXKAZOUwELlUcdBNzKYXGuaoiv4UECiiuY7IZLKbbvLtLUENQPXAZWc8YuJ2P5-ZsfHZ-a5yXjTS0PSi0tfHtU_SZhpMdgQChJpt8je8uTTBWK4aH-VQbkedMsihlGMq2m28kvnf9MXcw1ZlzQ6LbHKkMW5ggkLOLPLRbC1kmrrug0kbM9O2VKY5Qv8dnNjA4T_qsY=w1102-h661-no)

![lindsay zoladz](https://lh3.googleusercontent.com/c17KI5UHzGNs3g24CNUY6JulHiHjcRB_ws_ZH_1LCBYaxD9Jl5zDSKBegjWtsswII0fjd8-9M4CZu8vuIIgqDfLm9x07rLEIk5GDr1sT28TEEAisdGbbNll2xq_P-OcJjSyqE4xIuFcE7VxTCdKsKaQfpuoaxLwVlPPGmQliBwI5DVXf42sG8k_GgW4to59IOKQmZuTbkkZQ-qo3HY_SoC9ztcKhlkOAlfrTljblRkJ7dXRq4UJKDYd0RzfIYm_wv-BzGbK6aUsJKl7c1kwIoBeLvakwSe5lMPqv5yVrqpUGyDBP1PPjxclyyphzggyZWSUEISdFmcRjzrINxibfibyQ05GHiCoBpyvEa6lCK7NFjQi07jNPvRmtoDGa4_43Xtav5sRaf5bMM9lpKK_Mmq8YP12CJFpJVuZpwcC-0mrb3V3nl8kECcxZ7iu_YnBq1JU2DuiMqeCUdDFI-TXKcUTE0pZWIzWigTIwIpuluKk4rSk_OjP7pWMYm2i5g8tOv9BMTndBCCV6eFLGiEYpWLIzsHXNDafH7Glvsv5oBqaNgOvb1XatwLwvzbmhG6RK8SR77jqEPLsdRUb8aKJYzSpdl4QF8fXfM5FTvvzFIECMOqQpbO8GMYUXgkEopwwadWzmcFtaLqXUtYeW83kk7MS6Dsapnp7lwng=w2000-h1200-no)

![larry fitzmaurice](https://lh3.googleusercontent.com/mtnyPcAywpCFW1LTuQ3aT0f2rvJrs4ybZ1PTtkSJoZml-kQQMUrG0c6ud7plFQ16oVZkd_zFxUhimoJerQLOagZ-AqbpOxa5liOBprEl-JU8JYWrQRByXfxdgPsrld9iLUtDZuDjPtCvfac-ElMhMvLR5p8Y0pn9G9YhSx5lG7uvHFFsOFoD8QLKj6DT7YoX4p28IsP7rD90fWYswDfZuLlo8UySEOXDATLR_OpNrTZ2BAaO3zfkiAx8Sn7qEwEBYzfdsL_rOqEMm-90WzM3ehhAAbALBoDtw0Kpj3kz2PeidOVQtFvXdFlZgyGg0dW_SDkG_1jB5Zi4d1zwhm_VxFwaWikT37PgW8KWk8FyE4jcPtvS2kr0mRWh-3EpNJOlIof_Ml2wftnQ-gr3LYmq32jVoJtW_hpkrGe6g4xweojUWaKc6gBCClCeW-oaU4MHGkBr9ZVfcjBPBrfYTYzVT84JWZi2VU-tRHXAGsLhGnzD2CslO0iEkHrhOKuyM5VFgklejlSn7jY_T32yUJ4Pz7WFXts9KTM1bT_HlTHxr6gpW5NRSmcgBvJCTIHUzUtl2Lg302J45KPWFCxvHthGMGywF6s30FwSNSugFEBKkwAGpVyzAQ9SI4vvXkbtic6bXUGS4sXoLmFeeq2PkGCbReN6GY70Nre-4HI=w1102-h661-no)

## Discussion

I do not pretend to know how Pitchfork reviewers come to be responsible for reviewing particular albums. Are they randomly assigned? I highly doubt it, this seems way too inefficient as you will end up assigning, say, a death metal album to a reviewer who is better versed in the nuances of lo-fi folk music. Perhaps reviewers choose for themselves which albums to review? While more plausible, this system is not without its flaws. For example, who bears the responsibility for albums nobody has selected for review? Maybe it's simply a mish-mash of anything and everything. Sometimes the reviewer selects a specific album to review. Other times they are asked to review something wildly off their radar. 

Whatever the process, it seems likely that every staff member at Pitchfork sees albums from all walks of the music world: groundbreaking new music, atrocious flops, and all shades inbetween. However, despite relative uniformity in aggregate album quality from reviewer to reviewer, the members of the Pitchfork staff differ drastically in their reviewing behaviors. Where some reviewers assign high and low scores with impunity, others seldom venture beyond a comfortable 6.0 - 8.0 range. Certain reviewers have reviewed hundreds of albums without once acknowledging a single album as 'Best New Music,' while others appear to assign high scores and BNM like they're going out of style.

In the absence of a well-defined scoring system, Pitchfork reviewers have developed their own personal rubrics for judging new music. Now, this might be innocuous if not for phenomena like the Pitchfork Effect. An album that reaches Mark Richardson's desk, say, likely has a much better shot at high praise than if that same album hit Adam Moerder's desk. With fame and fortune hanging in the balance, Pitchfork's willful neglect of acknowledging reviewer variability seems irresponsible at best. 

A solution to this may be to allow multiple reviews per album. That is, why not allow _three_ members to review an album and assign it the average of their scores? This is not the cheapest option for Pitchfork, surely, but an average of three scores is resistant to the whims of an individual biased reviewer and ought to better capture the musical novelty of a newly released album. For a website that thrives on controversy, however, I would not hold my breath.

[^1]: I am not the first person to attempt an analysis of Pitchfork album reviews. In recent years, Johnny Stoddard identified [the top-rated artists and albums of 2000 - 2010](http://blog.import.io/post/analysing-music-data-from-pitchfork), Alex Pounds attempts (but doesn't complete) an investigation of the [relationship between an artist's Pitchfork popularity and their number of listeners](http://ethicsgirls.com/pitchforkeffect/), and Jim Duke summarized [album scores overall, over time, and, very briefly, by reviewer](http://jmduke.com/posts/analysing-pitchfork-using-pandas/) using data obtained by Christopher Marriott's [Pitchfork web scraper](https://classic.scraperwiki.com/scrapers/pitchfork_review_data/). 
