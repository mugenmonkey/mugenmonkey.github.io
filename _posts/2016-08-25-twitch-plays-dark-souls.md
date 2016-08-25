---
layout: post
title: "That Time I Did Twitch Plays Dark Souls"
date:   2016-08-25 08:54:11 -0600
comments: true
---

Remember the "Twitch Plays XYZ" fad back in 2014? Everyone was trying to jump on the bandwagon with their own "Twitch Plays Thing". One joke people kept passing around was "what about Twitch Plays Dark Souls hurr durr hurr!". Not being a particularly clever man I decided to actually give this a shot.

Note that this was back in February 2014, a long time before the "Twitch Plays Dark Souls" phenomena that everyone is actually familiar with.

I managed to whip together the basic code to handle it in an afternoon and threw it up on Twitch on a Saturday morning. I intended this as a sort of test stream, and did not start it at the Asylum. Instead, I loaded up a very high level character and placed them in Firelink Shrine.

At its peak there was only about 60-70 people in chat. We got nowhere. (We **almost** made it to the aqueduct once! And somehow accidentally killed a few hollows!) After around 6 hours of the stream I shut it down. The general consensus seemed to be that it was a terrible, completely infeasible idea. "A game like Dark Souls is impossible in that format, particularly with the 20 second Twitch delay!" people said. I shut it down, chalking it up as a fun, but failed experiment.

Given all of that I was super surprised to see the insane attention that 2015's "Twitch Plays Dark Souls" stream got. It quickly climbed to over 10,000 viewers, and was picked up by pretty much every gaming publication. That stream did start in the Aslyum, which I think helped it gain that initial interest ("will we ever make it out of this pool?"). But ultimately I think it shows that success in anything isn't just an idea and its execution, but also timing and luck (as I'm sure the creator of [Infiniminer](http://www.zachtronics.com/infiniminer/) will tell you).

I'll admit I was a bit bummed that I didn't stick with my experiment and that it never achieved even a fraction of the success that this new stream did. However, it was somewhat vindicating to know the original idea did have merit.

(I will say that I think the later controversial decision to turn Dark Souls into what was essentially a turn based game was pretty brilliant.)

# The Code

If you're interested, you can find the code for how my stream worked [here](https://github.com/naiyt/twitchplaysdarksouls). (Before you judge the quality of the code keep in mind that it was thrown together in an afternoon.)

The basic structure was:

- A `Bot` class that monitored the Twitch IRC chat
- The `Bot` object would enqueue commands using the `Queue` class
- If a command had enough votes it would be appeneded to the actual "to execute" queue in the form of an `Action` object
- The `Action` would then invoke an AutoHotKey script with the requested action that would send the command to the game.

It should have been possible to send the commands directly to the game with Python but I could never get that to work, which is the reason I ended up using AutoHotKey. Had I spent more time on it I'm sure I could have found a more elegant solution. (But sometimes the most elegant solution is just the one that works.)
