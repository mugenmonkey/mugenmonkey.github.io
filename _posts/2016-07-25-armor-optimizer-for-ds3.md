---
layout: post
title: "Dark Souls 3 Armor Optimizer"
date: 2016-07-25 17:03:00 -0600
---

Today I released an oft requested feature: [an armor optimizer for Dark Souls 3](https://mugenmonkey.com/darksouls3)! If you're already familiar with the DS1 armor optimizer you should feel right at home. If not, keep reading this post to get an overview of how the feature works and the cool stuff you can do with it.

<iframe width="420" height="315" src="https://www.youtube.com/embed/dUr168SIgG4" frameborder="0" allowfullscreen></iframe>

First off, it can be accessed by clicking on the armor icon in the "Tools" section in the bottom left of the planner:

![Armor Optimizer Location](/assets/optimizer-location.png)

After clicking that, you'll be greeted with the optimizer proper:

![Optimizer](/assets/armor-optimizer.png)

## Breakpoints

In the first section, you can specify the equip load breakpoint to allow. i.e., what equip load percentage should your chosen armor not exceed. This calculation takes anything you currently have equipped on your build (excluding armor) into account. By default you can choose `70%` or `100%`, but you can also specify a custom breakpoint.

## Sort By

This section allows you to specify what you want the optimizer to optimize for. Physical absorption will probably be the most useful, but you can **finally** optimize your build for poise!

## Allow Naked?

You can specify whether the optimizer should allow any naked options. Please keep in mind that you get a signifiant defense penalty for each piece of armor you don't wear, so it's usually not worth it (except for fashion or other specific circumstances).

## Forced Equipment

![Forced equipment](/assets/forced-equipment.png)

At the top of the optimizer you can "force" equipment. This means that the optimizer will always include your chosen pieces in its results. This is super handy for fashion -- you can ensure you maintain the armor pieces that your fashion requires while min-maxing the rest.

## Find Armor

Once you've configured the settings to your preference go ahead and hit "Find Armor". Depending on the speed of your computer, this can take a bit of time. There are 33,718,464 possible armor combinations in Dark Souls 3, so be patient. (It's optimized in various ways so it doesn't calculate all 33.7 million each time. However, a typical optimization will still run through several hundred thousand combinations.)

When it's finished you'll be presented with the top 10 armor sets that match your specifications.

![Finished](/assets/finished-optimization.png)

At this point you can click on any of them to apply it to your current build.

## Filtering Armor

One last thing -- there is a small "Filter Armor" option at the top of the optimizer.

![Filter Button](/assets/filter-button.png)

If you click on that it'll pop open a list of all the armor sets. This allows you to filter out specific armor pieces that you don't want included in the results -- another boon for fashion if, say, you hate the look of Smough's set. (If you used the DS1 planner this should all be familiar to you.)

![Armor List](/assets/armor-list.png)

--

And that's about it! If you notice any bugs or have any questions go ahead and hit me up [on Twitter](https://twitter.com/mugenmonkey) or [fill out a bug report](http://goo.gl/forms/bLUkmMau5U).

\- naiyt
