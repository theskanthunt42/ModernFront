---
layout: post
title: I made something to monitor my Raspberry Pi 3 at home.
categories: posts
tags: thoughts projects
author: the42game
description: And you could have a peek at it as well! Yes I am still using the OG Raspberry Pi 3b
---
# Context
So I had this little Raspberry Pi 3 sitting around which I bought back in 2016.  
Yes I am still using it, why not?  
So recently I decided to let it run something instead of biting dust at home I installed DietPi on it and use it as a real server for some thing that I would actually use everyday, being slow is okay too, it's fun innit?  
Anyway, in order to see how bad CSG is doing here I've decided to attach a [mini UPS](https://github.com/linshuqin329/UPS-18650) to it, which pretty much is a block that sits underneath your Pi with two 18650 batteries connected to a [MAX17041](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17040-MAX17041.pdf).  
And what it brings is some I2C connection to my Pi for some stats to read, and with that plus A HUGE MERGER OF MY OTHER THREE PREVIOUS PROJECTS with something more blended in it, I've created:  
[SimpUPSMon](https://gist.github.com/theskanthunt42/02de49d7190eb4a0a867d66c2e532e10) - *Some readings from my Raspberry Pi and it's "UPS"*(sic)  
Which read some status from the Pi, some status from your browser, and *bang*！  
Oh and I hand wrote a basic HTTP server for this, just becauz I am lazy... so I just grab socket and do some rough TCP send thing... and called it a server.
# TL;DR
Go see for yourself at(and your IP address + headers): [SimpUPSMonSample](https://front.the42.info/rpimon)