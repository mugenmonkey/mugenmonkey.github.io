---
layout: post
title: "How to Calculate AR for any Weapon and Infusion in Dark Souls 3 - Rough Draft"
date:   2016-07-22 10:34:38 -0600
comments: true
published: false
---

In the last couple of months there have been some big steps forward in terms of mining out the scaling
and infusion data from Dark Souls 3. This has allowed Mugenmonkey to be able to calcluate accurate
AR for any infusion type (which would have been nearly impossible were we manually calculating each
scaling coefficient). In this post I'll be presenting the canonical way to calculate AR (along with
all of the data you need). In a subsequent post I'll go over how these values can be extracted from the
game's files.

## Attack Rating

First off, a quick definition of AR, for those not familiar. AR stands for "Attack Rating" and it's
just what the Soulsborne community has taken to calling your weapons total damage. It's just the
value that gets displayed on your stats screen, as shown below:


## The Formula

Note: this formula only calculates AR for fully upgrade weapons. It's possible to calculate for other
upgrade levels, but that is out of scope for this post.

This'll get a bit complex, so get ready to head down the rabbit hole with me.

AR is:

Base damage + Scaling Bonuses

Base damage is:

Base Physical + Base Magic + Base Fire + Base Lightning + Base Dark

Scaling bonuses are the complex part. The basic scaling bonus calculation for a particular damage type
is:

(Base Damage * Weapon Coefficient) - Base Damage

The weapon coefficients are where things start to get complex.

The weapon coefficients are calculated as follows:

1 + (0.01 * Scaling Coefficient * Diminishing Return Value)

Scaling Coefficient:

These are the special numbers that correspond to the weapon's scaling value. Take note that the scaling modifiers are NOT
equal between weapons -- i.e., a strength scaling of "C" for one weapon is not necessarily the same
as another.

Diminishing Return Values (also called "saturation" by some people):

These values are where your stats are taken into account, and can be thought of as how long
it takes to reach a weapon's full potential. Each level for a stat has a corresponding value
that can sort of be thought of as the "percentage" of the total bonus possible for that damage
type and that stat.

The fun part about this? There isn't just one set of diminishing return values -- there are
actually *11*. The one you have to use depends entirely on the weapon, infusion, and bonus type being
calculated. (Luckily these values can be extracted out of the game's files -- finding all of
these manually would probably be a nearly insurmountable task).

--

That's at least how you would get the scaling coefficient if the bonuses only took one stat type
into account. But of course it wouldn't be that easy, now would it? Different stats can affect
the same bonus type, which makes this even more fun.

## Physical Coefficient:

Normal Weapon:

1 + (0.01 * Strength Scaling Coefficient * Physical Diminishing Return Value) + (0.01 * Dex Scaling Coefficient * Physical Diminishing Return Value) + (Luck Scaling Coefficient * Physical Diminishing Return Value)

Blessed Weapon, Anri's Straight Sword, Saint Bident, Lothric's Holy Sword, Wolnir's Holy Blade, Morne's Great Hammer:

1 + (0.01 * Strength Scaling Coefficient * Physical Diminishing Return Value) + (0.01 * Dex Scaling Coefficient * Physical Diminishing Return Value) + (Luck Scaling Coefficient * Physical Diminishing Return Value) + (0.01 * Faith Scaling Coefficient * Physical Diminishing Return Value)

## Magic Coefficient

1 + (0.01 * Intelligence Scaling Coefficient * Magic Diminishing Return Value)

## Fire Coefficient

1 + (0.01 * Intelligence Scaling Coefficient * Fire Diminishing Return Value) + (0.01 * Faith Scaling Coefficient * Fire Diminishing Return Value)

## Lightning Coefficient

1 + (0.01 * Faith Scaling Coefficient * Lightning Diminishing Return Value)

## Dark Coefficient

1 + (0.01 * Intelligence Scaling Coefficient * Dark Diminishing Return Value) + (0.01 * Faith Scaling Coefficient * Dark Diminishing Return Value)


With all of this, we can construct the full formula:

```
Base Physical + Base Magic + Base Fire + Base Lightning + Base Dark + 1 +
(Base Physical * (1 + (0.01 * Strength Scaling Coefficient * Physical Saturation)
+ (0.01 * Dex Scaling Coefficient * Physical Saturation)
+ (Luck Scaling Coefficient * Physical Saturation)
+ (0.01 * Faith Scaling Coefficient * Physical Saturation))) - Base Physical)
+ (Base Magic * (1 + (0.01 * Intelligence Scaling Coefficient * Magic Saturation))
   - Base Magic)
+ (Base Fire * (1 + (0.01 * Intelligence Scaling Coefficient * Fire Saturation)
+ (0.01 * Faith Scaling Coefficient * Fire Saturation)) - Base Fire)
+ (Base Lightning * (1 + (0.01 * Faith Scaling Coefficient * Lightning Saturation))
   - Base Lightning)
+ (Base Dark * (1 + (0.01 * Intelligence Scaling Coefficient * Dark Saturation)
+ (0.01 * Faith Scaling Coefficient * Dark Saturation)) - Base Dark)
```

I've created a spreadsheet that you can view [here](https://docs.google.com/spreadsheets/d/1nGXbJ5DEaWCtXHhj46Wws4HM0KkV85nyczJemqmtDF8) that lists all of the coefficients, base damages, and saturation values.

To use it, find the weapon with the infusion type you want. That'll give you the scaling coefficients and base damages. Then, take a look at the "Saturation Values Functions" section. For your weapon you'll find the function index for the damage type you're calculating. From there, hop over to the second sheet called "Saturation Values". Find the row that matches the "Function Index" that you found in the last step. Then you just find the column that matches your level in the given stat.
