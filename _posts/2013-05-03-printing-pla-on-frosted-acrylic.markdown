---
title: Printing PLA on frosted acrylic
layout: post
author: jcdoll
tags:
- 3d printing
---

The adhesion between my PLA and heated glass bed suddenly degraded about a week ago. Before then, adhesion had been fantastic running the extruder and bed at 185C and 60C, respectively. I was using borosilicate glass from McMaster-Carr mounted on top of the heater PCB with clips. Two other things changed in the same time frame that might have led to the adhesion problem: I started using colored PLA filament from new sources (Amazon and 3D Printer Hub) and I started running the air conditioner.

The sudden degradation in PLA-glass adhesion has been reported by other people (e.g. [here](http://forums.reprap.org/read.php?1,127861,127873), [here](https://groups.google.com/forum/?fromgroups=#!topic/makergear/tCh0z7scAoQ) and [here](http://forums.reprap.org/read.php?262,124416,125417)). I'm not sure exactly what caused it in my situation (build up on the glass? moisture in the PLA? black magic?) but I found a better alternative to heated glass in the process of debugging. I tried a few things initially that didn't solve the problem that are worth mentioning.

First I tried modifying the bed temperature (60C -> 90C) and obtained a slight improvement in adhesion but prints would still fail quite frequently. Next I tried cleaning experiments. For the first month I had been using nail polish remover (acetone + lots of other crap) and was concerned that contaminants might be building up on the glass. So I consulted the forums and tried cleaning the glass with hot water and detergent, vinegar, [lemon juice](http://forums.reprap.org/read.php?262,181891,181891), isopropyl alcohol and acetone (separate experiments). None of them solved the problem and I was starting to get frustrated with glass.

So I started looking around at alternative bed materials. Many people use kapton tape or blue painters tape with good results, but I like having a continuous and smooth printing surface. [Polycarbonate](http://reprap.org/wiki/PLA) has also been reported as working, but it scratches more easily than acrylic and is more expensive. The main problem with acrylic seems to be that it bonds too strongly to PLA even when unheated, but I remember seeing that [sandblasted](http://hardwired.cc/?p=335) or [frosted](https://groups.google.com/forum/#!msg/deltabot/cTt4PXiblIQ/m7WMmECFzQoJ) acrylic can give the right level of adhesion. So I picked up a 6" x 6" x 1/8" piece of acrylic from Tap Plastics and frosted it by hand with 400 grit sandpaper.

The frosted acrylic solved my issues and has a few significant advantages over a heated glass bed with just a few downsides. The advantages are that acrylic is incredibly cheap ($1 for my piece), it can be refinished using sandpaper in case it gets dirty or scratched, it can be easily drilled rather than held in place with clips, and it doesn't require any heating (faster prints, one less variable to control).

The first disadvantage is that you can't print ABS anymore, but I have no interest dealing with warping and rafts so this wasn't an issue. The second disadvantage, perhaps more significant, is that you no longer have separate control of first layer temperature and adhesion. So far this hasn't been an issue, but I can imagine a scenario where good first layer adhesion required a higher than ideal temperature which would cause other issues.

The main risk seems to be that the adhesion might be *too* good, although this hasn't been a problem with my first few prints on the acrylic. I also bought a sheet of HDPE while I was at the store; it is more chemically stable than acrylic and I haven't seen anyone write about trying PLA + HDPE so it's something that I'll try out in the next week or two as an experiment.

**Updates:**
* The adhesion problem described in this post caused by the filament. You get what you pay for. I only use Diamond Age (high quality, every color under the sun, expensive) or Printrbot (local, cheap, good) filament now. Skip the random vendors on Amazon.

* Printing on unheated, frosted acrylic is a bad idea. The roughness combined with the lack of thermal expansion makes over-adhesion a very real problem.

* If you have trouble printing on heated glass, something else is wrong in your system. If no heater is available, use blue tape from Amazon or Printrbot.
