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

### Aces

![Aces Radar]({{ site.url }}/pics/SP4.png)

Naturally, we begin with the best of the best, the Aces cluster. As we can see, they throw a ton
of innings, and miss a ton of bats, using their above average fastballs. They are more aggres-
sive than your average pitcher. These guys are the ones you want at the front of your rotation.
What’s interesting is not so much that this cluster contains Bumgarner, Scherzer, and Kershaw, 
but the Samardzija and Quintana inclusions can give you pause, since they were based on data prior to this year,
where both pitchers have been impressive. When you look at Samardzija and 
Quintana against the others by the given metrics, they actually stack up favorably. 
Also included in this cluster are other Aces such as Jose Fernandez,
Masahiro Tanaka, Yu Darvish, Sonny Gray, Gerrit Cole, among many others.
It’s interesting to see that the elite pitchers were grouped together by these metrics. How-
ever, with the inclusion of some tier 2 and 3 pitchers, it’s clear that these are not sufficient
conditions for elite pitcher status.

### Almost Aces

![Almost-Aces Radar]({{ site.url }}/pics/SP5.png)

The next group, is what I call “Almost-Aces”. They have above average fastballs, and they use
them more than average, with a good arsenal of off-speed pitches, but they get below average
whiffs, and pound the zone a bit more than average. What is interesting about this group
compared to the Ace group is that this group has significantly higher off-speed differentials,
but pitch significantly fewer innings on average. Additionally, they have lower whiff rates.
This suggests they pitch to contact a bit more, and luck being what it is, a certain percentage
of these hit balls fall, leading to both their reduced performance metrics and inning counts.


### Go Ahead, Hit It

![Hit-it Radar]({{ site.url }}/pics/SP3.png)

This is an interesting cluster. They use their fastball significantly more than average, but it is
about average in terms of velocity. They’re about average in terms of aggression. They don’t
generate many swings and misses, prompting me to give them the name I did. Due to the
high amount of contact they induce, they pitch slightly fewer innings than average. Some
of these guys are borderline belonging to other groups, such as Mark Buehrle, Who pitches
more innings and throws the fastball less often and less hard than a lot of the pitchers in this
segment. The prototypes of this group are pitchers like Bartolo Colon, Dan Haren, Lance Lynn, Doug Fister, and Jake Peavy,
among others.

## Conclusion

These three clusters represented for me a very interesting set. The other clusters 
generated were a bit more average, almost creating an 'other' bucket for pitchers that
didn't really fit anywhere. What I find so cool about this analysis is that if I 
were to ask *a priori* which pitchers you'd put into the three clusters mentioned above,
the lists of your pitchers, and the pitchers who came out would be very close to one another.
Again, we used no data on outcome, Wins/Losses, ERA, whip, etc. It could be argued that IP/App
is a proxy for that, but we avoided explicitly outcome-based statistics, yet still 
received reasonable clusters of pitchers that fit well with our intuition. 