---
layout: post
title: "DS3 Planner Release Notes: Equipment Effects Added and Updated"
date: 2016-07-19 12:24:36 -0600
---

Today I finished re-working how the planner handles equipment effects. This has made it possible
to display and calculate effects from armor and weapons (e.g., Symbol of Avarice adding to Item Discovery)
as well as the ability to better handle rings with multiple effects (e.g., the Clutch Rings decreasing
absorption as well as increasing AR for their corresponding damage types).

I may have missed a few effects (let me know if you notice any), but here are the main equipment pieces
that should now be properly working:

- All of the Clutch Rings
- Carthus Bloodring
- Fleshbite Ring
- Yhorm's Greatshield
- Dragon Tooth
- Symbol of Avarice.

This has been some time in the making, so I'm glad to finally get it working. As always, let me know
if you see bugs or miscalculations.

If you're interested in a technical overview of these changes you can take a look
[at this post]({% post_url 2016-07-19-technical-overview-of-effect-handling %})
