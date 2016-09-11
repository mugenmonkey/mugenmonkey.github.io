---
layout: post
title: "Dark Souls 3 Build Randomizer"
date:   2016-09-11 14:24:45 -0600
comments: true
---

This is something I've had several people asking about for awhile. Needed a fun little project to work on today, so I decided to finally implement a build randomizer for the [Dark Souls 3 planner](https://mugenmonkey.com/darksouls3).

In order to use it, just click on the little randomizer button in the "Tools" section in the bottom left of the planner:

![Randomizer button](/assets/randomizer-button.png)

Clicking on that will open up an options box that will let you optionally set a min and max level for your random build. Want the random build to be exactly SL 120? Just put 120 in the min and max input boxes:

![Randomizer - SL120](/assets/sl120.png)

If the "Only useable weapons" checkbox is enabled you will only be assigned weapons that your random build has sufficient stats to use.

This will randomize almost everything in your build: gender, covenant, starting class, all stats, hollowing, armor, weapons, spells, and rings.

## Stats Randomization Algorithm

In case you're curious, the randomization algorithm for stats goes something like this:

- Choose a random starting class
- Choose a random target soul level, that's within the request min and max levels (or 0 and 802 otherwise)
- Set the current soul level to the starting classes starting level
- While the current soul level is less than the target soul level:
  - Randomly choose a stat to level up
  - Calculate the number of levels before that stat reaches 99
  - Calculate the number of overall levels until the build reaches the target level
  - Randomly choose a value between 0 and the minimum of 25, number of levels in current stat until 99, number of levels until the build reaches the target
    - This is done in an effort to try and prevent the stats from being spread out too evenly. The original implementation just increased the random stat by 1, but this led to builds where all of the stats were too close to one another.

In psuedo code this would looke something like:

```
startingClass = chooseRandomStartingClass()
targetSL = randomIntegerInRange(slMin, slMax)
currentSL = startingClass.level
stats = constructStatsFrom(startingClass)

while currentSL < targetSL
  nextStat = randomStat()

  until99 = 99 - startingClass.getVal(nextStat)
  untilTargetSL = targetSL - currentSL

  levelIncrease = randomIntInRange(0, Math.min(25, until99, untilTargetSL))

  stats[nextStat] += levelIncrease
  currentSL += levelIncrease
```
