---
layout: post
title: "Technical Overview of DS3 Planner Effect Handling"
date: 2016-07-19 12:45:07 -0600
---

In the [previous post]({% post_url 2016-07-19-release-notes-equipment-effects %})
I mentioned how the Dark Souls 3 planner now supports effects on all equipment types,
as well as multiple effects per item. Here's a brief technical overview of how that was done
for those that might find that interesting.

# Background: Storing Equipment Data

Each of the other planners on Mugenmonkey store equipment data (weapons, armor, rings, etc)
in JSON. This works perfectly well for the most part -- except for when it comes to saving
builds and doing any sort of data processing on them. A few features I'd eventually
like to release for the DS3 planner are better sorting options (e.g., "give me the top
rated SL120 builds with the Greatsword") and build stats (e.g., the number of builds with
certain rings equipped). Querying for that info when all of the weapon data is in JSON
is difficult.

So for the Dark Souls 3 planner, I decided to actually create SQL tables for each equipment
type (weapons, armor, spells, rings). This allows me to do easy queries like this one, which
retrieves all builds with the Symbol of Avarice equipped:

```
SELECT * FROM ds3_builds
INNER JOIN ds3_armors ON ds3_armors.id = ds3_builds.head_id
WHERE ds3_armors.name = "Symbol of Avarice"
```

# Original Effects Setup

The original setup for effects had the "business logic" stored in three columns in
the rings table: `alters`, `alter_value`, `alter_method`. An example usage would be Havel's Ring,
which would look something like this:

```
+---------+-------------+--------------+
| alters  | alter_value | alter_method |
+---------+-------------+--------------+
| equip   |        1.15 | multiply     |
+---------+-------------+--------------+
```

That data is presented to the frontend through the Mugenmonkey API, and the frontend stat calculation
code can just look at those field to determine what to do.

# The Problems

This solution came with two main issues:

- It doesn't work for rings that affect multiple stats
- If I wanted to do the same for other equipment types I would have to add those same columns to all of them (nasty)

The natural solution to something like this is to create an effects table and have a
"many-to-one" association between effects and equipment. In particular, I went with a
Rails-style [polymorphic](http://guides.rubyonrails.org/association_basics.html#polymorphic-associations)
association so that each effect could point to either a ring, piece of armor, or spell.

This all worked perfectly well for solving both of those problems.

# One Last Thing

The one thing that the JSON strategy has over the database table strategy
is the ability to more easily update and view equipment data and to track those changes in source
control. In order to sort of get the "best of both worlds" I wrote two scripts to allow
me to modify equipment in YAML files rather than through the database directly.

The first script will dump the current state of the database equipment columns into their
respective YAML files, while the second will do the reverse. The second one is now run
on every deploy so that the production database always has equipment matching exactly
what is in source control. The YAML representation of a ring, for example, looks like this:

```
- name: Ring of Favor
  rank: 0
  weight: 1.5
  effects:
    - alters: hp
      alter_value: 1.03
      alter_method: multiply
      description: Increases max HP by 3%
    - alters: stamina
      alter_value: 1.085
      alter_method: multiply
      description: Increases stamina by 10.85%
    - alters: equip
      alter_value: 1.05
      alter_method: multiply
      description: Increases equip by 5%
```

Now I can have my cake and eat it too!

\- naiyt
