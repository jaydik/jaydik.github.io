---
layout: post
title:  "Clustering MLB Pitchers (part 1)"
date:   2016-06-25
categories: analysis
---

In late December 2015, I participated in TrueMedia's baseball [hackathon](http://www.trumedianetworks.com/hackathon/).
My submission can be found [here](http://www.trumedianetworks.com/analysts/jon-dickerson).
As that reads a bit dry, I wanted to condense it a bit and focus on the fun.

For those interested, all the code used to generate this can be found on my [github](https://github.com/jaydik/mlb-hackathon).

## Background

In the hackathon, participants were given a few years of [pitchfx](http://www.sportvision.com/baseball/pitchfx%C2%AE) data,
 and asked to answer one specific question. The question I decided to ask, was simple.

 > Can we find clusters of pitchers based on their attributes, rather than resulting statistics?

 When we talk about ''the ace of the staff'', or say things like ''he really pounds the zone'', what are we describing? My goal was to find out.

What I wanted to avoid was merely saying all pitchers with 18+ wins were ''aces'', and those that had high ERA were ''back of the rotation''.
There has been debate in the MVP/Cy Young balloting, with many analysts questioning the value of those traditional statistics
(or [accepting](http://www.fangraphs.com/blogs/the-cy-young-award-and-the-wins-barrier/) that it is what it is), and
really most of the field of sabermetrics is trying to find better ones, better able to isolate the individual performance of a given player.
With all that said, let's dive in.


------


## Data

The data given to the analysts was pitch-level data for three years. There were many variables, listed below.

|`seasonYear`|`gameString`|`gameDate`|`gameType`|
|`visitor`|`home`|`visitingTeamFinalRuns`|`homeTeamFinalRuns`|
|`inning`|`side`|`batterId`|`batter`|
|`batterHand`|`pitcherId`|`pitcher`|`pitcherHand`|
|`catcherId`|`catcher`|`timesFaced`|`batterPosition`|
|`balls`|`strikes`|`outs`|`manOnFirst`|
|`manOnSecond`|`manOnThird`|`endManOnFirst`|`endManOnSecond`|
|`endManOnThird`|`visitingTeamCurrentRuns`|`homeTeamCurrentRuns`|`pitchResult`|
|`pitchType`|`releaseVelocity`|`spinRate`|`spinDir`|
|`px`|`pz`|`szt`|`szb`|
|`x0`|`y0`|`z0`|`vx0`|
|`vy0`|`vz0`|`ax`|`ay`|
|`az`|`paResult`|`runsHome`|`battedBallType`|
|`battedBallAngle`|`battedBallDistance`|`atbatDesc`|

Most of those are self-explanatory, but the more obscure ones are actually the most interesting, they are the positional
measurements for the pitch, both for locating the pitch in 2-D space on the plane of the plate, but also its angle and movement.
For a full explanation of the pitchfx variables, check out [this guide](https://fastballs.wordpress.com/category/pitchfx-glossary/)

## Deriving Metrics

Like I mentioned above, I wanted to avoid using too many ''outcome'' variables, meaning I didn't want to use WHIP, era, wins, etc.
What should I use instead? To answer that, I decided to dig deeper in what we meant by common qualitative phrases.

#### Innings Pitched Per Appearance (IP/App)
The  first metric I chose could be considered an outcome variable, as a pitcher who pitches well pitches longer, but I decided to use it
to capture the idea of an ''innings-eater'' -- a little on the nose, I'll admit -- who isn't necessarily an elite pitcher (think [Mark Buehrle](http://www.baseball-reference.com/players/b/buehrma01.shtml)
or [CC Sabathia](http://www.baseball-reference.com/players/s/sabatc.01.shtml) later in their careers).


#### Fastball Percentage (FB%)

The next variable is fastball percentage. This is a simple metric derived using the following formula

$$
FB\% = \frac{fastballs}{total Pitches}
$$

Where _fastballs_ includes the following game day labels: fastball, two-seam fastball, four-seam fastball, cutter, sinker.
This variable is a simple usage stat, how often does a given pitcher use his fastball? Conversely, how often does he go offspeed?


#### Average Fastball Speed (MPH)

Average fastball speed is again a very clear metric. The speed of the pitcher's fastball (defined the same as above)
is averaged first over game then over years. The purpose of this variable is clear: how hard can you throw the ball?
Baseball literature is chock-full of references to ''flamethrowers'', ''hard-throwing lefties'' and the like, so this is an obvious variable to include in the analysis.

#### Pitch Differential (DIFF)
I wanted to get a sense of how offspeed a pitcher's secondary pitches are. I define DIFF as:

$$
DIFF = avg(fastball_{mph}) - avg(nonFastball_{mph})
$$

This variable has one potential issue, which is that all pitches are aggregated in the nonFastball calculation, more accurate maybe would
be the average fastball minus the average speed of the slowest pitch type a pitcher throws.


#### Aggression (AGG)
This is perhaps the fuzziest of the variables, and the toughest to describe. When we call a pitcher aggressive,
often we mean big fastball, throwing right down the middle, saying ''my best vs. your best''. How do you define that though?

Pardon a brief technical aside.

There are 4 important variables for this: `px`, `pz`, `szb`, and `sbt`. `px` describes the horizontal location of the
ball relative to the center of the plate, e.g., a pitch with `px` = 0 would literally be ''right down the middle''
(with respect to the horizontal plane). Horizontal is easy, I give the pitchers 2 baseball's width deviation from dead
center to be labeled down the middle. Vertically is a bit tougher. The top and bottom of the strike zone (supposedly)
are determined batter by batter. Those are meant to be captured in the `szt` and `szb` variables, respectively.
Lastly, `pz` is the height of the ball above the ground. Thus, I can use `szb`, `szt`, and `pz` to calculate ''down the middle''
for a pitches' vertical location. After all is said and done, what I get is pitches labeled ''aggressive'' as shown in the picture below.
Also plotted is the rulebook strikezone, in dashed line. The data used in the plot is taken from the 2015 World Series between the Royals and Mets.

![aggression plot]({{ site.url }}/pics/aggplot.svg)


#### Whiff Percentage (WHIFF%)
The last variable in this analysis is whiff percentage. It is calculated as

$$
WHIFF = \frac{swinging strikes}{induced swings}
$$

In other words, of all the times the batter swung the bat, how many times did he miss the ball completely?
This is my (rather uninspired) way of measuring pitchers who have ''swing-and-miss stuff''.

In [part two]({% post_url 2016-07-30-Clustering-MLB-Pitchers-2 %}), I look at two pitchers specifically and how they stack up in our metrics, then analyze some of the clusters that resulted from my K-means analysis.