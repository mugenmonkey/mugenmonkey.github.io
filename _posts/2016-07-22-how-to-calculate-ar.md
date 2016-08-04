---
layout: post
title: "How to Calculate AR for all Weapons and Infusions in Dark Souls 3"
date:   2016-07-22 10:34:38 -0600
comments: true
---

In this post I'll be presenting the canonical way to calculate AR (along with giving you all of the scaling data you need). In a subsequent post I'll go over how the scaling and saturation values can be extracted from the game's files.

Credit: a lot of this post expands on info originally posted by rbfrosty [on Reddit](https://www.reddit.com/r/darksouls3/comments/4gqrpy/how_to_calculate_attack_rating_of_any_weapon_at/). The ability to mine the scaling data found in the spreadsheet listed in this post is thanks in large part to the work done by [monrandia and pireax](https://www.reddit.com/r/darksouls3/comments/4j3o40/spreadsheet_with_full_ar_calculation/).

## Attack Rating

First off, a quick definition of AR. AR stands for "Attack Rating" and it's what the Soulsborne community has taken to calling your weapon's total damage. It's just the value that gets displayed on your stats screen, as shown below:

![Attack Power in the stats screen](/assets/ar.jpg)

## The Formula

Note: this formula only calculates AR for fully upgrade weapons. Calculating for different upgrade levels is outside of the scope of this post.

In order to scare you as much as possible I'm going to start with the final horrifying formula:

```
AR = Base Physical + Base Magic + Base Fire + Base Lightning + Base Dark +
Base Physical * (Strength Scaling Coefficient * Physical Saturation
+ Dex Scaling Coefficient * Physical Saturation
+ Luck Scaling Coefficient * Physical Saturation
+ Faith Scaling Coefficient * Physical Saturation)
+ Base Magic * (Intelligence Scaling Coefficient * Magic Saturation)
+ Base Fire * (Intelligence Scaling Coefficient * Fire Saturation
+ Faith Scaling Coefficient * Fire Saturation)
+ Base Lightning * (Faith Scaling Coefficient * Lightning Saturation)
+ Base Dark * (Intelligence Scaling Coefficient * Dark Saturation
+ Faith Scaling Coefficient * Dark Saturation)
```

Before you run off screaming just know that once we break it down it won't seem *quite* so bad.

# So Let's Break it Down!

A simplified version of the formula is as follows:

```
AR = Base Damage + Physical Scaling Bonus + Magic Scaling Bonus + Fire Scaling Bonus
     + Lightning Scaling Bonus + Dark Scaling Bonus
```

# Calculating Scaling Bonuses

The basic formula for the scaling bonus for a particular damage type is:

```
Base Damage For That Damage Type * Weapon Coefficient(s)
```

# Calculating Weapon Coefficients

Now things start to get fun. The weapon coefficient is the multiplier that is determined by your weapon's scaling and your level in the corresponding stat. Keep in mind that the scaling *letters* don't have consistent scaling *values*. i.e., two weapons that have a strength scaling of "C" do not necessarily have the same scaling multipliers.

Weapon coefficients are calculated as:

```
Stat Scaling Coefficient * Saturation Value
```

# Stat Scaling Coefficient

These are set values for each weapon and stat type. They can be found on [this spreadsheet](https://docs.google.com/spreadsheets/d/1nGXbJ5DEaWCtXHhj46Wws4HM0KkV85nyczJemqmtDF8/) that will be discussed in more depth below. Keep in mind that those values are **percentages** while this formula expects the **decimal** value -- thus, you'll need to divide them by 100.

# Saturation Value

These can sort of be thought of as the "percentage" of the total potential AR of a weapon. These are set values for each level in the corresponding stat. This is where the increase in AR from leveling up actually comes from (and also how the "diminishing returns" are calculated.)

The fun part, however, is there isn't just one set of saturation values -- there are actually *11*. Which one you use is determined by the weapon, infusion, and damage type. These are all included [in the spreadsheet](https://docs.google.com/spreadsheets/d/1nGXbJ5DEaWCtXHhj46Wws4HM0KkV85nyczJemqmtDF8/) mentioned above (and we'll go more in depth in the example section). Keep in mind that those values are **percentages** while this formula expects the **decimal** value -- thus, you'll need to divide them by 100.

# Physical Bonus

Some damage types are affected by more than one stat, physical damage being one of them. This means we actually have to factor in the bonus damage received from each relevant stat. Physical damage is normally modified by strength, dex, and luck (for weapons with luck scaling). The formula is:

```
Strength Coefficient = Strength Scaling Coefficient * Physical Saturation
Dex Coefficient = Dex Scaling Coefficient * Physical Saturation
Luck Coefficient = Luck Scaling Coefficient * Physical Saturation

Physical Bonus = Base Physical * (Strength Coefficient + Dex Coefficient + Luck Coefficient)
```

Blessed Weapons, Anri's Straight Sword, Saint Bident, Lothric's Holy Sword, Wolnir's Holy Blade, and Morne's Great Hammer also receive physical damage from Faith scaling:

```
Strength Coefficient = Strength Scaling Coefficient * Physical Saturation
Dex Coefficient = Dex Scaling Coefficient * Physical Saturation
Luck Coefficient = Luck Scaling Coefficient * Physical Saturation
Faith Coefficient = Faith Scaling Coefficient * Physical Saturation

Physical Bonus = Base Physical * (Strength Coefficient + Dex Coefficient + Luck Coefficient +
Faith Coefficient)
```

# Magic Bonus

Magic is only affected by Intelligence:

```
Int Coefficient = Int Scaling Coefficient * Magic Saturation
Magic Bonus = Base Magic * Int Coefficient
```

The one exception is the Golden Ritual Spear which receives Magic Damage from Faith Scaling:

```
Faith Coefficient = Faith Scaling Coefficient * Magic Saturation
Golden Ritual Spear Magic Bonus = Base Magic * Faith Coefficient
```

# Fire Bonus

Fire is affected by Intelligence and Faith:

```
Int Coefficient = Int Scaling Coefficient * Fire Saturation
Faith Coefficient = Faith Scaling Coefficient * Fire Saturation
Fire Bonus = Base Fire * (Int Coefficient + Faith Coefficient)
```

# Lightning Bonus

Lightning is affected by Faith:

```
Faith Coefficient = Faith Scaling Coefficient * Lightning Saturation
Lightning Bonus = Base Lightning * Faith Coefficient
```

# Dark Bonus

Dark is affected by Intelligence and Faith:

```
Int Coefficient = Int Scaling Coefficient * Dark Saturation
Faith Coefficient = Faith Scaling Coefficient * Dark Saturation
Dark Bonus = Base Dark * (Int Coefficient + Faith Coefficient)
```

# Example

I realize this all feels like quite a mess by now, so an example should help clear things up a bit.

Let's calculate the AR for a **+10 Dark Falchion** with the following stats: **15 str**, **17 dex**, **40 int**, **35 faith**.

First off, let's pop open [the scaling data spreadsheet](https://docs.google.com/spreadsheets/d/1nGXbJ5DEaWCtXHhj46Wws4HM0KkV85nyczJemqmtDF8) to get our base damages:

| Damage Type | Damage |
|-------------|--------|
| Physical    | 112    |
| Magic       | 0      |
| Fire        | 0      |
| Lightning   | 0      |
| Dark        | 145.6  |

This means our **total base damage** is:

```
Base Damage = 112 + 145.6 = 257.6
```

Our Physical bonus would be:

```
Physical Bonus = 112 * Physical Coefficients
```

To get our strength coefficient, we need to find the "strength scaling coefficient" and the "physical saturation". Going back to the spreadsheet we'll find that the strength coefficient for the Dark Falchion is `30`.

For the saturation value, we look at the `Physical` column in the `Saturation Curvs` section. We'll see that we want curve `0`. So, we open the "Saturation Curves" tab. From there, we find the `Level 15` (15 is our current strength) column for the `Curve Index - 0` row -- this gives us `19.80409249`.


```
Strength Coefficient = 30/100 * 19.80409249/100 =~ 0.0594
```

We follow a similar pattern for the dex coefficient, this time getting `54` for our scaling coefficient and `23.24584203` for our saturation (this was the value in the `Level 17` column in the `Curve Index - 0` row):

```
Dex Coefficient = 54/100 * 23.24584203/100 =~ 0.1256
```

The weapon does not have any Luck or Faith Scaling, so we skip them (but if it did, the pattern would be the same).

```
Total Physical Bonus = 112 * (0.0594 + 0.1256) = 20.71
```

Given that the Dark Falchion has no Magic, Fire, or Lightning damages, we can skip those (but if it did, we'd just follow the same pattern above, but for the correct stat and damage types).

The Dark bonus would be:

```
Dark Bonus = 145.6 * Dark Coefficients
```

Our int scaling coefficient is `77` and the saturation value is `75` (the "Level 40" column for stat curve 0):

```
Int Coefficient = 77/100 * 75/100 =~ 0.5775
```

The faith coefficient is `77` and the saturation value for level 35 in curve 0 is `66.55058206`:

```
Faith Coefficient = 77/100 * 66.55058206/100 =~ 0.5124
```

This gives us:

```
Dark Bonus = 145.6 * (0.5775 + 0.5125) =~ 158.705
```

Now we can calculate our total AR:

```
AR = 257.6 + 20.71 + 158.704 = 437.014
```

Since Dark Souls rounds down, our final AR is `437`! This matches up with the value [Mugenmonkey displays](https://mugenmonkey.com/darksouls3/58970). Keep in mind that there's potential room for rounding or floating point precision errors, so there's always the small possibility that your final value may be 1 off from the in game value.

# Addendum

AR isn't too complex once you have all the necessary data. All of this information is out there, but I wasn't aware of any single location that had all of the neccessary formulas AND data. Hopefully this will help somebody, or at the very least be interesting!
