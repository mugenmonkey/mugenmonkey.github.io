---
layout: post
title: "DS3 Planner Release Notes: Bug Fixes"
date: 2016-07-20 11:27:02 -0600
---

Hey all, today I fixed a few bugs for the Dark Souls 3 planner:

- Fixed a bug that was causing AR for talismans/chimes to show as `NaN` (thanks JavaScript)
- 2 handing dual wielding weapons (like the Brigand Twindaggers) will no longer increase the weapon's AR
- Fixed poise calculation

With regards to that last one, I know poise is useless in game, but I did want the planner
to still be accurate. Thanks to [Wikidot](http://darksouls3.wikidot.com/poise) for the diminishing
returns formula. Also, I did notice that some of the armor had bad poise values while I was testing,
so it might not be 100% accurate still. At some point I'll probably do a full pass of all of the equipment
and make sure it all matches up with what's in game.
