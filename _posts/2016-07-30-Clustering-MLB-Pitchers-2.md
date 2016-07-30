---
layout: post
title:  "Clustering MLB Pitchers (part 2)"
date:   2016-07-30
categories: analysis
---

To check out the motivation/derivation of this analysis, check out [part one]({% post_url 2016-06-25-Clustering-MLB-Pitchers %}).

For those who don't want to read the first post, a brief recap: Taking data from a few years of pitch-by-pitch stats, 
I derived some metrics I thought would be useful in grouping pitchers into different pitcher types using a standard clustering algorithm. 
Here are the results of those groupings.


## Pitcher Examples
First, Let's take a look at two pitchers in particular, and how they stack up in our 6 metrics.
We'll start with Clayton Kershaw, a man few would deny is the best pitcher in baseball during the
past few years.

![Kershaw Radar]({{ site.url }}/pics/Clayton.png)

What we can see is that he's everything we've said he is. He's above average in every statistic (though we could argue if that's necessarily a good thing), 
He is the league leader in IP/App, throws hard, is aggressive, misses a ton of bats, does everything you'd want him to. Clayton passes both the eye test and
the more rigorous analysis. 

Now, let's move to a perhaps more paradoxical pitcher, Nate Eovaldi. 

![Eovaldi Radar]({{ site.url }}/pics/Eovaldi.png)

If we use the area of the radar chart as a proxy for performance, then it appears as those Nate is a pretty good pitcher. 
The scouting report on Nate: big fastball, but straight. He has a triple digit heater, yet led the NL in hits in his last year with the Marlins. 
Naturally, he's toward the top of the MPH stat, but we can see that his WHIFF% is below average (and his aggression well above -- related?). Since he's 
got that hard fastball, his differential is also quite high. The lack of strikeouts means his IP/App is barely above average. Two pitchers, both with talent, 
but significantly different results. This would imply that perhaps there are more variables that are more predictive of performance, or the power of WHIFF 
can mean the world in terms of outcome. The point being here not to bash Nate (I *am* a Yankee fan, after all), but rather to highlight the use of these charts.


## Cluster Results

Shifting focus from individual pitchers, we can think of the clustering as taking large groups of pitchers that have similar
ratings in each of the derived metrics, and "averaging" their ratings to obtain a group of pitchers that behaves similarly, which
hopefully would also create groups that receive similar results.